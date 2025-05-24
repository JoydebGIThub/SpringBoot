## 400 - Bad Request
### When it occurs:
- Invalid JSON input
- Failing validation constraints (`@NotEmpty`, `@Email`)
- Type mismatch in query/path params.

### 💡 How to handle:
```java
@ExceptionHandler(MethodArgumentNotValidException.class)
public ResponseEntity<?> handleValidationErrors(MethodArgumentNotValidException ex){
  Map<String, Object> errors = new HashMap<>();
  ex.getBindingResult().getFieldErrors().forEach(err ->
    errors.put(err.getField(), err.getDefaultMessage())
  );
  return new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);
}
```

