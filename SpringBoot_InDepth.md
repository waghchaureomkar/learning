
# Spring Boot In-Depth - Interview Prep

## ✅ Covered Topics

1. Starter Dependencies
2. @RestController, @Service, @Repository
3. application.properties / yml
4. Validation & Exception Handling
5. DTO Mapping
6. Logging (SLF4J)

## 🔤 Definitions (English)

- **Spring Boot**: Simplifies Spring setup with auto-config and embedded server.
- **Controller**: Handle HTTP requests.
- **Service**: Business logic.
- **Repository**: DB operations.

## 💡 Hindi Explanation

- Spring Boot: Spring ka shortcut jisme config kam karna padta hai.
- Controller: User se request lene wala part.
- Service: Logic handle karta hai.
- Repository: DB ka interaction karta hai.

## 🧪 Code Example

```java
@RestController
@RequestMapping("/api")
public class UserController {
    @GetMapping("/hello")
    public String hello() {
        return "Hello World";
    }
}
```

## ❓ Interview Qs
- What is @SpringBootApplication?
- How auto-configuration works?
