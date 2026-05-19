# spring security

## What is Spring Security?

Spring Security is a framework used to:

Authenticate users
Authorize access
Protect applications

### Filter Chain

Every request passes through security filters
Handles:

Login
Token validation
Session management

## Custom Security Configuration

Spring allows you to have custom config, in other words configure or change how the filter chain works

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(customizer -> customizer.disable)
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()
                .anyRequest().authenticated()
            )
            .formLogin(Customizer.withDefaults())
            .httpBasic(Customizer.withDefaults())
            .sessionManagement(session -> session.sessionCreationPolicy(sessionCreationPolicy.STATELESS));

        return http.build();
    }

}
```

this config:
Disable default csrf
Define public vs protected routes
Customize behavior on login form and for apis like postman to work

## 🔑 Custom Login (Manual Authentication)

Instead of Spring handling login, you create your own endpoint:

### Example Controller

```java
@RestController
@RequestMapping("/api/auth")
public class AuthController {

    @Autowired
    private AuthenticationManager authenticationManager;

    @PostMapping("/login")
    public String login(@RequestBody LoginRequest request) {

        Authentication authentication = authenticationManager.authenticate(
            new UsernamePasswordAuthenticationToken(
                request.getUsername(),
                request.getPassword()
            )
        );

        return "Login successful";
    }
}
```

---

## How it works

1. User sends username + password
2. `AuthenticationManager` verifies credentials
3. If valid → returns authenticated object
4. You can:
   - Generate JWT
   - Return response
   - Store session

---

## UserDetailsService

Spring will need a way to fetch users from DB:

```java
@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Override
    public UserDetails loadUserByUsername(String username) {
        // fetch user from DB
        return new User("user", "password", new ArrayList<>());
    }
}
```

---

## Password Encoding

`plain password → hashed password`

- Stored in database as a **hash**

---

### 🔑 Why BCrypt?

- One-way hashing (cannot be reversed)
- Automatically adds a **salt**
- Same password ≠ same hash every time
- Built into Spring Security

---

### Password Encoder Bean

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder(12);
}
```

### Registering a User

```java
@Service
public class UserService {

    @Autowired
    private UserRepo repo;

    @Autowired
    private PasswordEncoder encoder;

    public Users register(User user) {
        user.setPassword(encoder.encode(user.getPassword()));
        return repo.save(user);
    }
}
```

## 🔐 Login / Authentication Flow

```java
encoder.matches(rawPassword, storedHashedPassword);
```

### Login Service

```java
@RestController
@RequestMapping("/api/auth")
public class AuthController {

    @Autowired
    private AuthenticationManager authenticationManager;

    @PostMapping("/login")
    public String login(@RequestBody LoginRequest request) {

        Authentication authentication = authenticationManager.authenticate(
            new UsernamePasswordAuthenticationToken(
                request.getUsername(),
                request.getPassword()
            )
        );

        return "Login successful";
    }
}
```

# JWT (JSON Web Token)

## What is JWT?

JWT (JSON Web Token) is a way to:

- Authenticate users without sessions
- Send secure information between client and server

## Structure of JWT

A JWT has 3 parts:

### 1. Header

Contains:

- Algorithm (e.g. HS256)
- Token type

---

### 2. Payload

Contains user data (claims):

- username
- roles
- expiry time

---

### 3. Signature

- Ensures token is **not tampered**
- Created using:
  - secret key
  - header + payload

---

## JWT Generation

Using a utility class:

```java
@Component
public class JwtUtil {

    private String secret = "mysecretkey";

    public String generateToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60))
                .signWith(SignatureAlgorithm.HS256, secret)
                .compact();
    }
}
```

---

## Using JWT in Login

```java
@PostMapping("/login")
public ResponseEntity<?> login(@RequestBody LoginRequest request) {

    authenticationManager.authenticate(
        new UsernamePasswordAuthenticationToken(
            request.getUsername(),
            request.getPassword()
        )
    );

    String token = jwtUtil.generateToken(request.getUsername());

    return ResponseEntity.ok(new JwtResponse(token));
}
```

## Important Concepts

### Stateless

- Server does NOT store sessions
- Every request must include token

---

### Expiration

- Tokens expire (e.g. 1 hour)
- Improves security

---

### Secret Key

- Used to sign token
- Must be kept secure

---

# DTO & Kafka

## 1. DTO (Data Transfer Object)

### What is it?

- A simple **object** that carries data between processes (e.g., client ↔ server, layers like Controller ↔ Service).
- Typically **no behavior** (no business logic) — only fields, getters, setters.
- Used to **decouple** internal domain models from external APIs.

### Why use DTO?

- ✅ Control which data is exposed (hide sensitive fields like passwords).
- ✅ Reduce number of network calls (aggregate multiple values into one DTO).
- ✅ Decouple internal entities from API contracts (can change DB model without breaking clients).
- ✅ Serialize easily to/from JSON/XML.

### Example (Java):

```java
public class UserDTO {
    private String name;
    private String email;
    // constructor, getters, setters
}
```

## Apache Kafka

What is it?
Distributed event streaming platform used for high-throughput, low-latency messaging.

Based on publish-subscribe model.

Core Concepts:
Concept Description
Producer Sends messages to a topic.
Consumer Reads messages from a topic.
Topic Logical channel to organize messages (like a table).
Partition Topic split into ordered logs for parallelism.
Broker A Kafka server (stores partitions).
Offset Unique position of a message within a partition.
Consumer Group Multiple consumers sharing load; each partition assigned to one consumer in group.
ZooKeeper/KRaft Manages cluster metadata (Kafka 3.x+ uses KRaft).
Key Features:
Durable (messages stored on disk).

Fault-tolerant (replicate partitions).

Scalable (add brokers/partitions).

At-least-once / Exactly-once semantics configurable.

Common Use Cases:
Log aggregation.

Real-time analytics (Kafka Streams, ksqlDB).

Event sourcing / CDC (Change Data Capture).

Decoupling microservices.
