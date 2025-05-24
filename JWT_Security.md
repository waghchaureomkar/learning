
# Spring Security + JWT - Interview Prep

## ✅ Covered Topics

1. JWT Token Generation
2. AuthenticationController
3. JwtFilter for Validation
4. Spring Security Config

## 🔤 Definitions (English)

- **JWT**: JSON Web Token to secure APIs without session.
- **Filter**: Checks JWT in headers.
- **SecurityConfig**: Manages protected/unprotected routes.

## 💡 Hindi Explanation

- JWT: Token system jisme server session maintain nahi karta.
- Filter: Har request me JWT check karta hai.
- Config: Kis route ko secure karna hai decide karta hai.

## 🧪 Code Snippet

```java
public class JwtFilter extends OncePerRequestFilter {
    protected void doFilterInternal(...) {
        // Check JWT from request and validate
    }
}
```

## ❓ Interview Qs
- JWT vs Session?
- How to expire token and refresh it?
