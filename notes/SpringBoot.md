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

## todo

- Build login endpoint
- Connect to database (JPA)
- Add JWT generation
- Protect routes with roles
- Test with Postman

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
