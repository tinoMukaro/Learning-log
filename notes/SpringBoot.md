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

# 🔐 JWT (JSON Web Token) – Spring Security Notes

## 📌 What is JWT?

JWT (JSON Web Token) is a way to:

- **Authenticate users without sessions**
- Send **secure information between client and server**

👉 It is a **stateless authentication mechanism**

---

## 🧠 Simple Idea

Instead of:

- Server storing session (JSESSIONID)

We do:

- Server gives client a **token**
- Client sends token on every request

---

## 📦 Structure of JWT

A JWT has 3 parts:

```
HEADER.PAYLOAD.SIGNATURE
```

---

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

Example:

```json id="1k6k7r"
{
  "sub": "tino",
  "role": "USER",
  "exp": 1712345678
}
```

---

### 3. Signature

- Ensures token is **not tampered**
- Created using:
  - secret key
  - header + payload

---

## 🔄 JWT Flow in Spring Boot

### 1. User Logs In

- Sends username + password
- Spring authenticates using:
  - `AuthenticationManager`

---

### 2. Generate JWT

If authentication is successful:

- Create token
- Include user info

---

### 3. Send Token to Client

Example response:

```json id="c5o0is"
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

---

### 4. Client Stores Token

Usually:

- localStorage OR
- httpOnly cookies

---

### 5. Client Sends Token on Requests

```http id="9k6r3h"
Authorization: Bearer <token>
```

---

### 6. Server Validates Token

- Extract token from header
- Verify signature
- Check expiry
- Authenticate user

---

## ⚙️ JWT Generation (Spring Example)

Using a utility class:

```java id="v0qj6m"
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

## 🔑 Using JWT in Login

```java id="b7u9xz"
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

---

## 🛡️ JWT Filter (Core Part)

Purpose:

- Intercepts every request
- Validates token

Steps:

1. Read `Authorization` header
2. Extract token
3. Validate token
4. Set authentication in context

---

## ⚠️ Important Concepts

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

## 🚫 Common Mistakes

- ❌ Storing sensitive data in payload
- ❌ Not setting expiration
- ❌ Weak secret key
- ❌ Forgetting "Bearer " prefix
- ❌ Not validating token on each request

---

## ⚖️ JWT vs Session

| Feature      | Session | JWT    |
| ------------ | ------- | ------ |
| Storage      | Server  | Client |
| Scalability  | ❌      | ✅     |
| Stateless    | ❌      | ✅     |
| React/Mobile | ❌      | ✅     |

---

## 🧠 Mental Model

Login:
→ verify user
→ give token

Requests:
→ send token
→ verify token

---

## 🏁 Summary

- JWT replaces sessions
- Used for stateless authentication
- Generated after login
- Sent in Authorization header
- Verified on every request

---
