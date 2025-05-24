
# Java Multithreading - Interview Prep

## ✅ Covered Topics

1. Thread class and Runnable interface
2. ExecutorService
3. Synchronization & Volatile
4. Callable & Future
5. ThreadPool
6. Thread Communication (wait/notify)
7. CountDownLatch

## 🔤 Definitions (English)

- **Thread**: Independent path of execution.
- **Runnable**: Interface to define a task.
- **ExecutorService**: Framework to manage thread pools.

## 💡 Hindi Explanation

- Thread: Ek background kaam karne wala unit.
- Runnable: Kaam define karne wali interface.
- Executor: Threads ko manage karne wala system.

## 🧪 Code Example

```java
ExecutorService executor = Executors.newFixedThreadPool(2);
executor.submit(() -> System.out.println("Thread running"));
executor.shutdown();
```

## ❓ Interview Qs
- What is difference between Thread and Runnable?
- How to handle thread synchronization?
