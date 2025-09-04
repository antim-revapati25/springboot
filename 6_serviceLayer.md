# Spring Boot Greeting App — Detailed Explanation

This document explains your Spring Boot Greeting App, covering **Service** and **Controller layers**, along with all key concepts.

---

## 1️⃣ Service Layer (`GreetingService.java`)

```java
package com.antim.fromBasics.service;

import org.springframework.stereotype.Service;

@Service
public class GreetingService {
    public String greet(String name){
        if(name == null || name.isEmpty()){
            return "Hello, Guest";
        } else {
            return "Hello, " + name;
        }
    }
}
```

### Explanation:

1. **`@Service` annotation**

   * Marks this class as a **Spring-managed service bean**.
   * Detected automatically during **component scanning**.
   * Enables **dependency injection** into controllers.

2. **Business Logic**

   * The `greet` method handles greeting logic.
   * Returns `"Hello, Guest"` if the name is missing.
   * Otherwise, returns `"Hello, " + name`.

3. **Spring Boot Flow**

   * Application starts with the main class annotated with `@SpringBootApplication`.
   * Scans for `@RestController`, `@Service`, etc., and creates beans.
   * When a URL like `/greet/abc` is visited, it calls the appropriate controller method, which uses the service.

4. **Key Concepts:**

| Concept              | Description                                        |
| -------------------- | -------------------------------------------------- |
| Controller           | Handles HTTP requests (`GreetController`)          |
| Service              | Handles business logic (`GreetingService`)         |
| Dependency Injection | `@Autowired` injects service into controller       |
| Routing              | `@RequestMapping` + `@GetMapping` define endpoints |
| Path Variables       | Dynamic URL segments captured with `@PathVariable` |
| Optional Handling    | Separate method handles requests without a name    |

---

## 2️⃣ Controller Layer (`GreetController.java`)

```java
package com.antim.fromBasics.controller;

import com.antim.fromBasics.service.GreetingService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/greet")
public class GreetController {

    @Autowired
    private GreetingService greetingService;

    @GetMapping("/{name}")
    public String sayHello(@PathVariable String name){
        return greetingService.greet(name);
    }

    @GetMapping
    public String sayHelloDefault(){
        return greetingService.greet(null);
    }
}
```

### Explanation:

1. **`@RestController`**

   * Marks this class as a Spring MVC controller.
   * Combines `@Controller` + `@ResponseBody`.

2. **`@RequestMapping("/greet")`**

   * Defines the base URL for all endpoints in this controller.
   * Example: `/greet/abc` or `/greet/xyz`.

3. **`@Autowired`**

   * Injects the `GreetingService` bean.
   * Spring manages the service object as a singleton.
   * Avoid using `new` keyword here (Spring won’t manage it).

4. **Endpoints:**

   * `@GetMapping("/{name}")` → handles GET requests with a path variable.
   * `@GetMapping` → handles GET requests without any path variable (default greeting).

5. **Flow:**

   ```
   ```

HTTP Request --> GreetController --> GreetingService --> Response

```
- `/greet/Leo` → Controller captures `Leo` → Service generates greeting → Response sent.
```
---

