## spring security

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

## 🧠 How it works

1. User sends username + password
2. `AuthenticationManager` verifies credentials
3. If valid → returns authenticated object
4. You can:
   - Generate JWT
   - Return response
   - Store session

---

## 👤 UserDetailsService (Important)

Spring needs a way to fetch users from DB:

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

## 🔒 Password Encoding

Never store plain passwords:

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```

---

## 🔗 AuthenticationManager Bean

Needed for manual login:

```java
@Bean
public AuthenticationManager authenticationManager(
        AuthenticationConfiguration config) throws Exception {
    return config.getAuthenticationManager();
}
```

---

## 🚫 Disable Default Features (Common)

```java
http
    .csrf().disable()
    .formLogin().disable()
    .httpBasic().disable();
```

---

## 🧱 Roles & Authorization Example

```java
.authorizeHttpRequests(auth -> auth
    .requestMatchers("/admin/**").hasRole("ADMIN")
    .requestMatchers("/user/**").hasAnyRole("USER", "ADMIN")
    .anyRequest().authenticated()
)
```

---

## 🔄 Typical Flow (Real App)

1. User registers → password encoded
2. User logs in → authenticated
3. Server generates JWT
4. Client stores token
5. Every request → token verified

---

## ⚡ Common Mistakes

- ❌ Forgetting password encoder
- ❌ Not exposing AuthenticationManager
- ❌ Using default login unintentionally
- ❌ Not permitting auth routes
- ❌ Mixing session + JWT incorrectly

---

## 🧪 What You Should Practice

- Build login endpoint
- Connect to database (JPA)
- Add JWT generation
- Protect routes with roles
- Test with Postman

---

## 💡 Simple Mental Model

Think of Spring Security as:

Request → Filter → Authentication → Authorization → Controller

---

## 🏁 Summary

- Spring Security handles auth + protection
- You can override defaults
- Custom login = manual authentication
- JWT usually used in modern apps
- Everything revolves around the filter chain

---
