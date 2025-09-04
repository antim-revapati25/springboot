# Spring Boot Request Mappings in Depth

In Spring Boot, you handle HTTP requests using mapping annotations. These define how the backend methods are triggered when specific HTTP methods (GET, POST, PUT, DELETE) are called.

---

## 1. `@GetMapping`
- Used for **retrieving data**.
- Example:

```java
@RestController
public class UserController {

    // URL: http://localhost:8080/user/1
    @GetMapping("/user/{id}")
    public String getUser(@PathVariable int id) {
        return "User ID: " + id;
    }
}
```

✅ Uses **@PathVariable** to capture values from the URL.

---

## 2. `@PostMapping`
- Used for **creating data**.
- Example:

```java
@RestController
public class UserController {

    // URL: http://localhost:8080/user
    // Body: {"name":"Leo","age":25}
    @PostMapping("/user")
    public String createUser(@RequestBody User user) {
        return "Created user: " + user.getName() + ", Age: " + user.getAge();
    }
}

class User {
    private String name;
    private int age;

    // getters & setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}
```

✅ Uses **@RequestBody** to accept JSON data.

---

## 3. `@PutMapping`
- Used for **updating data**.
- Example:

```java
@RestController
public class UserController {

    // URL: http://localhost:8080/user/1
    // Body: {"name":"UpdatedLeo","age":26}
    @PutMapping("/user/{id}")
    public String updateUser(@PathVariable int id, @RequestBody User user) {
        return "Updated user with ID: " + id + " to Name: " + user.getName();
    }
}
```

✅ Combines **@PathVariable** (identify which resource) + **@RequestBody** (new data).

---

## 4. `@DeleteMapping`
- Used for **deleting data**.
- Example:

```java
@RestController
public class UserController {

    // URL: http://localhost:8080/user/1
    @DeleteMapping("/user/{id}")
    public String deleteUser(@PathVariable int id) {
        return "Deleted user with ID: " + id;
    }
}
```

✅ Uses **@PathVariable** to select the resource to delete.

---

# Summary of Data Passing in Spring Boot

| Mapping        | Purpose          | Example URL / Body                 | Annotation Used |
|----------------|------------------|------------------------------------|----------------|
| `@GetMapping`  | Retrieve data    | `/user/1`                          | `@PathVariable` |
| `@PostMapping` | Create data      | `/user` + JSON body                | `@RequestBody`  |
| `@PutMapping`  | Update data      | `/user/1` + JSON body              | `@PathVariable`, `@RequestBody` |
| `@DeleteMapping` | Delete data    | `/user/1`                          | `@PathVariable` |

---

✅ With this, you can handle **all CRUD operations** in Spring Boot!

Next step: Learn about **query parameters (`@RequestParam`)** and how they differ from **path variables**.
