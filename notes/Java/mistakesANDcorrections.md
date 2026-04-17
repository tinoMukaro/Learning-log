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
