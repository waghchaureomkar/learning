
# Java 8 Features - Interview Prep

## ✅ Covered Topics

1. Lambda Expressions
2. Functional Interfaces
3. Stream API
4. Method References
5. Default and Static Methods in Interfaces
6. Optional Class
7. Date and Time API

## 🔤 Definitions (English)

- **Lambda Expression**: Anonymous function to simplify code.
- **Functional Interface**: Interface with one abstract method, used with lambdas.
- **Stream API**: Processes collections with operations like map, filter, reduce.

## 💡 Hindi Explanation

- Lambda: Function ko variable ki tarah treat karna.
- Functional Interface: Sirf ek abstract method wali interface.
- Stream: Collection pe fast aur expressive operation.

## 🧪 Code Example

```java
List<String> names = Arrays.asList("Amit", "Ravi", "Neha");
names.stream()
     .filter(name -> name.startsWith("A"))
     .forEach(System.out::println);
```

## ❓ Interview Qs
- What is a Functional Interface?
- Difference between map() and flatMap()?
