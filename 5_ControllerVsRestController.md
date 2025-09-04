# Spring Boot Controllers — A Deep Dive

## 🌐 What is a Controller?

A **Controller** in Spring Boot is a class where you define **endpoints (URLs)**.  
It handles **incoming HTTP requests** and returns **responses**.

When the application starts:

* Spring Boot scans your package (`@SpringBootApplication` enables component scanning).
* It automatically finds classes annotated with `@RestController` or `@Controller`.
* Each controller can define its own endpoints (`@GetMapping`, `@PostMapping`, etc.).
* They are **independent**, unless you give them the same path.

If you give the **same path to different controllers**, you will get an error like:

```
java.lang.IllegalStateException: Ambiguous mapping. Cannot map 'helloController' method
```

Because Spring Boot won’t know which controller to map.

---

## 🔎 Controller vs RestController

### `@RestController`

* Combines: `@Controller + @ResponseBody`.
* Every method returns data **directly into the HTTP response body**.
* Used for:
  * REST APIs
  * JSON/XML responses
  * Raw strings/HTML snippets

Example:

```java
@RestController
public class RestControllerDemo {
    @GetMapping("/rest")
    public String rest(){
        return "<h1>I will directly be mapped as raw data</h1>";
    }
}
```

### `@Controller`

* Used for **Spring MVC (server-side rendering)**.
* Returns a **view name** (HTML/JSP/etc.) instead of raw data.
* If you want raw data, you must add `@ResponseBody`.

Example:

```java
@Controller
public class ControllerDemo {
    @GetMapping("/home")
    public String home() {
        return "index.html"; // will look for a file named index.html
    }

    @GetMapping("/raw")
    @ResponseBody
    public String raw() {
        return "<h1>This is raw data from Controller</h1>";
    }
}
```

---

## ⚖️ Key Differences

| Feature              | `@Controller`                         | `@RestController`                      |
| -------------------- | ------------------------------------- | -------------------------------------- |
| Default behavior     | Returns a **view name**               | Returns **raw data/JSON**              |
| Need `@ResponseBody` | ✅ Yes (for raw data)                  | ❌ No (already included)                |
| Use case             | Web pages, JSP, Thymeleaf, HTML views | REST APIs, JSON/XML, raw HTML snippets |

---

## 📌 Public vs Non-Public Classes in Java

* **Only one public class per file**.
* The file name must match the **public class name**.

### Why?

* A `public` class is visible across all packages.
* Compiler & JVM need a guaranteed **unique filename** to locate the class.
* Keeps the ecosystem predictable.

### Non-Public Classes

* Classes with no modifier (package-private) are only visible **within the same package**.
* Since they are not globally visible, the compiler doesn’t enforce file-name rules.

### Private/Other Access Modifiers

* Even if a class is `private` or package-private, Spring Boot can still detect it.
* Why? Because **Spring Boot uses reflection and classpath scanning** (ignores modifiers).
* So even a non-public controller can be detected.

✅ **Best practice**: Keep each class in a separate file.

---

## 📝 RestController vs Controller Summary

* `@RestController = @Controller + @ResponseBody`
* `@RestController` → every method directly returns data in HTTP response body.
* `@Controller` → returns a view name (HTML/JSP/etc.). If you want raw data, add `@ResponseBody`.

Examples of return values:

* With `@RestController`: return "Hello" → directly returns `Hello` as text.
* With `@Controller`: return "index" → looks for a file `index.html`.

⚠️ **Note**: You cannot reverse return values. A `@RestController` cannot directly return a view name like `index.html` for rendering.

---

## ✅ Final Example (Both Together)

```java
package com.antim.fromBasics.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class controllerVsResController {
    @GetMapping("/restcontroller")
    public String rest(){
        return  "<h1>I will directly be mapped to HTML</h1>";
    }
}

@Controller
class ControllerDemo {
    @GetMapping("/controller")
    public String hello(){
        return "index.html"; // looks for index.html file in templates/static
    }

    @GetMapping("/rawController")
    @ResponseBody
    public String raw(){
        return "<h1>This is raw data from Controller</h1>";
    }
}
```

---

## 🧠 Recap

* **Controller**: returns view names, used in MVC.
* **RestController**: returns raw data/JSON, used in REST APIs.
* Only one **public class per file** (file name must match public class name).
* Multiple non-public classes can exist in the same file, but not recommended.
* Spring Boot detects controllers via **classpath scanning and reflection**.
* Best practice → separate files for clarity.
