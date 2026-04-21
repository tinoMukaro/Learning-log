## 1. Application Properties & Git

**Mistake:** Thought `application.properties` works like `.env` in Node.js

| .env (Node.js)        | application.properties (Spring Boot) |
| --------------------- | ------------------------------------ |
| Never in Git          | Can be in Git (as template)          |
| Only environment vars | Gets packaged inside JAR             |
| Secrets safe outside  | Secrets DANGEROUS if committed       |

**Correction:**

```gitignore
# Add to .gitignore
application.properties
application-local.properties


**Mistake:** Assumed PostgreSQL runs on default port 5432

What I did:

net start | findstr postgres     # Found Postgres running
netstat -an | findstr 5432       # Nothing found - wrong port
Correction: Checked actual port

netstat -an | findstr 2015       # Found it on port 2015
Fix in application.properties:

properties
# Wrong
spring.datasource.url=jdbc:postgresql://localhost:5432/auth_db

# Correct
spring.datasource.url=jdbc:postgresql://localhost:2015/auth_db
Lesson: Never assume default ports. Always verify what port your database is actually listening on.
```

## 2. Spring Security still generating auto one-time password despite custom login

Cause: Spring Boot's SecurityAutoConfiguration automatically creates a default in-memory user and generates a random password on startup when no explicit UserDetailsService is found.

Solution: Exclude the auto-configuration completely:

properties
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration

## 3. JWT Authentication Errors

Error 1: ClassNotFoundException: javax.xml.bind.DatatypeConverter
What happened: JJWT 0.9.1 tried to use a Java class that was removed in Java 9+
Why: Old JJWT version incompatible with modern Java
Fix: Upgrade JJWT to 0.11.5+ (uses java.util.Base64 instead)

Error 2: ClassNotFoundException: io.jsonwebtoken.security.SecureRequest
What happened: Mixed different JJWT versions (API 0.11.5 + IMPL 0.12.6)
Why: API and IMPL versions must match exactly
Fix: All three JJWT dependencies must be the SAME version

```xml
<!-- ALL must match -->
jjwt-api:0.11.5
jjwt-impl:0.11.5
jjwt-jackson:0.11.5
```

Error 3: Circular Dependency in Spring Beans

What happened: Beans referencing each other in a loop
JwtAuthFilter → UserService → PasswordEncoder → SecurityConfig → JwtAuthFilter
Why: Field injection + bean creation in multiple places
Fix: Use constructor injection, don't create beans that already have @Component

```java
// BAD
@Autowired
private UserService userService;

// GOOD
private final UserService userService;
public JwtAuthFilter(UserService userService) {
    this.userService = userService;
}
```
