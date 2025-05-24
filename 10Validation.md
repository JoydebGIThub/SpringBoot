## Validation
- When we want to save some data through the `@Entity` then before saving we validate the data like I don't want to save the `empty` or `null` value.
- So, we add validation before saving the data.
- Add the dependency to active the `validation`

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```
- In a Spring Boot application, both `@Valid` and `@Validated` annotations are used for `validating input data` in **controllers or service layers**.
1. **@Valid**
   - Purpose: Used for validating method parameters or request bodies in controllers.
   - Scope: Works with `Java Bean Validation` (JSR-380) annotations like `@NotNull`, `@Size`, `@Email`, etc.
   - Usage: Commonly applied to method parameters in controllers to validate incoming data.

```java
@PostMapping("/createUser")
public ResponseEntity<String> createUser(@Valid @RequestBody UserDto userDto, BindingResult result) {
    if (result.hasErrors()) {
        return ResponseEntity.badRequest().body("Validation failed: " + result.getAllErrors());
    }
    return ResponseEntity.ok("User created successfully!");
}
```
2. **@Validated**
   - Purpose: Used for `group-based validation` or `validating method parameters` in service layers.
   - Scope: Works with `Spring's validation` framework and supports validation groups.
   - Usage: Typically applied at the `class level` or `method level` in service layers.

```java
@Validated
@Service
public class UserService {

    public void registerUser(@Validated(OnCreate.class) UserDto userDto) {
        // Validation logic for the "OnCreate" group
    }
}
```
- Use `@Valid` for simple validation in controllers.
- Use `@Validated` when you need group-based validation or validation in service layers.
-------------------------------------------------------------------------
## Now handling the exception we can create Global Exception.

```java
@RestControllerAdvice
public class GlobalExceptionHandler{
  @ExceptionHandler(MethodArgumentNotValidException.class)
  public ResponseEntity<?> handleValidationException(MethodArgumentNotValidException ex){
    Map<String, String> errors = new HashMap<>();
    ex.getBindingResult().getFieldErrors().forEach(
      error -> errors.put(error.getField(), error.getDefaultMessage()));
    ErrorResponse errorResponse = new ErrorResponse(
      LocalDateTime.now(),
      HttpStatus.BAD_REQUEST.value(),
      "Validation Error",
      errors.toString()
    );
    retrun new ResponseEntity<>(errorResponse, HttpStatus.BAD_REQUEST);
  }
}
```
---------------------------------------------------------------------------
## Annotation in Entity class

```java
@Entity
public class User{

  @NotEmpty(message ="The name is required")
  @Size(min = 2, max = 100, message ="The length will be more than one character")
  private String name;
}
```
----------------------------------------------------------------------------

