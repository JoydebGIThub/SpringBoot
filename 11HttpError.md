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

## 401 - Unauthorized
### When it occurs:
- Invalid/missing credentials(Spring Security)

### 💡 How to handle:
- Handled via Spring Security or custom logic in filters/controllers.
```java
@ExceptionHandler(SecurityException.class)
public ResponseEntity<?> handleUnauthorized(SecurityException ex){
  return buildErrorResponse(HttpStatus.UNAUTHORIZED, "Unauthorized access");
}
```

## 403 - Forbidden
### When it occurs:
- Authenticated user trying to access forbidden resource.

### 💡 How to handle:
- Spring Security `AccessDeniedException`
```java
@ExceptionHandler(AccessDeniedException.class)
public ResponseEntity<?> handleAccessDenied(AccessDeniedException ex){
  return buildErrorResponse(HttpStatus.FORBIDDEN, "Access Denied")
}
```

## 404 - Not Found
### When it occurs:
- Invalid URL (endpoint not defined)
- Resource not found(custom case)

### 💡 Custom exception:
```java
public class ResourceNotFoundException extends RuntimeException{
  public ResourceNotFoundException(String message){
    super(message);
  }
}
```

### 💡 Global handler:
```java
@ExceptionHandler(ResouceNotFoundException.class)
public ResponseEntity<?> handleResourceNotFound(ResourceNotFoundException ex){
  return buildErrorResponse(HttpStatus.NOT_FOUND, ex.getMessage());
}
```

## 405 - Method Not Allowed
### When it occurs:
- Hitting a URL with wrong HTTP method(POST on a GET-only endpoint)

### 💡 Global handler:
```java
@RxceptionHandler(HttpRequestMethodNotSupportedException.class)
public ResponseEntity<?> handlerMethodNotAllowed(HttpRequestMethodNotSupportedException ex){
  return builderErrorResponse(HttpStatus.METHOD_NOT_ALLOWED, ex.getMessage());
}
```

## 415 - Unsupported Media Type
### When it occurs:
- Sending wrong content type(plain text instead of JSON)

### 💡 Global handler:
```hava
@ExceptionHandler(HttpMediaTypeNotSupportedException.class)
public ResponseEntity<?> handleUnsupportedMediaType(HttpMediaTypeNotSupportedException ex)
{
  return buildErrorResponse(HttpStatus.UNSUPPORTED_MEDIA_TYPE, ex.getMessage());
}
```

## 500 - Internal Server Error
### When it occurs:
- NullPointerException
- Uncaught runtime exception
- DB failures, etc

### 💡 Custom internal error exception:
```java
public class InternalServerErrorException extends RuntimeException{
  public InternalServerErrorException(String msg){
    super(msg);
  }
}
```

### 💡 Global handler:
```java
@ExceptionHandler(InternalServerErrorException.class)
public ResponseEntity<?> handleInternalServerError(InternalServerErrorException ex) {
    return buildErrorResponse(HttpStatus.INTERNAL_SERVER_ERROR, ex.getMessage());
}

@ExceptionHandler(Exception.class)
public ResponseEntity<?> handleAllOtherErrors(Exception ex) {
    return buildErrorResponse(HttpStatus.INTERNAL_SERVER_ERROR, "Unexpected error: " + ex.getMessage());
}
```

### Common @ControllerAdvice Utility
```java
private ResponseEntity<?> buildErrorResponse(HttpStatus status, String message) {
    Map<String, Object> body = new HashMap<>();
    body.put("timestamp", LocalDateTime.now());
    body.put("status", status.value());
    body.put("error", status.getReasonPhrase());
    body.put("message", message);
    return new ResponseEntity<>(body, status);
}

```


































