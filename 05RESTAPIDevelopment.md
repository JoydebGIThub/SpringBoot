## REST API Development
### Web
- `Spring Boot` is well suited for `web application development`. You can create a `self-contained HTTP` server by using `embedded Tomcat`, Jetty, Undertow, or Netty.
- Most web applications use the `spring-boot-starter-web` module to get up and running quickly.
- You can also choose to build `reactive web applications` by using the `spring-boot-starter-webflux` module.

## Servlet Web Applications
- If you want to `build servlet-based` web applications, you can take advantage of `Spring Boot’s auto-configuration` for `Spring MVC` or Jersey.

### The "Spring Web MVC Framework"
- The `Spring Web MVC framework` (often referred to as “Spring MVC”) is a rich **“model view controller”** web framework. Spring MVC lets you create special `@Controller` or `@RestController` beans to handle `incoming HTTP requests`. Methods in the `controller` are `mapped to HTTP` by using `@RequestMapping` annotations.
- The following code shows a typical `@RestController` that **serves JSON data**:
```java
import java.util.List;

import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/users")
public class MyRestController {

	private final UserRepository userRepository;

	private final CustomerRepository customerRepository;

	public MyRestController(UserRepository userRepository, CustomerRepository customerRepository) {
		this.userRepository = userRepository;
		this.customerRepository = customerRepository;
	}

	@GetMapping("/{userId}")
	public User getUser(@PathVariable Long userId) {
		return this.userRepository.findById(userId).get();
	}

	@GetMapping("/{userId}/customers")
	public List<Customer> getUserCustomers(@PathVariable Long userId) {
		return this.userRepository.findById(userId).map(this.customerRepository::findByUser).get();
	}

	@DeleteMapping("/{userId}")
	public void deleteUser(@PathVariable Long userId) {
		this.userRepository.deleteById(userId);
	}

}
````
### Spring MVC Auto-configuration
- Spring Boot provides `auto-configuration` for Spring MVC that works well with most applications.
- It `replaces` the need for `@EnableWebMvc` and the two cannot be used together.
- In addition to `Spring MVC’s defaults`, the `auto-configuration` provides the following features:
  1. Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.
  2. Support for serving `static resources`, including support for `WebJars`.
  3. Automatic registration of `Converter`, `GenericConverter`, and `Formatter beans`.
  4. Support for `HttpMessageConverters`.
  5. Automatic registration of `MessageCodesResolver`
  6. Static index.html support.
  7. Automatic use of a `ConfigurableWebBindingInitializer` bean.
- If we want to keep those `Spring Boot MVC customizations` and make more `MVC customizations` (interceptors, formatters, view controllers, and other features), we can add our own `@Configuration` class of type `WebMvcConfigurer` but `without @EnableWebMvc`.
- If you do not want to use the auto-configuration and want to take `complete control of Spring MVC`, add your own `@Configuration` annotated with `@EnableWebMvc`. Alternatively, add your own `@Configuration-annotated` `DelegatingWebMvcConfiguration` as described in the `@EnableWebMvc` API documentation.

### Spring MVC Conversion Service
- Spring MVC uses a different `ConversionService` to the one used to convert values from your `application.properties` or `application.yaml` file.
- It means that `Period`, `Duration` and `DataSize` converters are not available and that `@DurationUnit` and `@DataSizeUnit` annotations will be ignored.
- If you want to `customize` the `ConversionService` used by `Spring MVC`, you can provide a `WebMvcConfigurer` bean with an `addFormatters method`.
- From this method you can `register` any converter that you like, or you can `delegate` to the static methods available on `ApplicationConversionService`.
- Conversion can also be customized using the `spring.mvc.format.*` configuration properties.

### HttpMessageConverters
- Spring MVC uses the `HttpMessageConverter` interface to `convert HTTP requests and responses`.
- Sensible defaults are included out of the box.
- For example, objects can be automatically `converted to JSON` (by using the Jackson library) or XML (by using the Jackson XML extension, if available, or by using JAXB if the Jackson XML extension is not available).
- By default, strings are encoded in UTF-8.

### Path Matching and Content Negotiation
- Spring MVC can map incoming `HTTP requests` to handlers by looking at the request path and matching it to the mappings defined in your application (for example, `@GetMapping` annotations on Controller methods).
- Spring Boot chooses to disable `suffix pattern` matching by default, which means that requests like "GET /projects/spring-boot.json" will not be matched to @GetMapping("/projects/spring-boot") mappings. This is considered as a best practice for Spring MVC applications.

### @ControllerAdvice
- You can also define a class annotated with `@ControllerAdvice` to customize the JSON document to return for a particular controller and/or exception type:
```java
import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.http.HttpServletRequest;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

@ControllerAdvice(basePackageClasses = SomeController.class)
public class MyControllerAdvice extends ResponseEntityExceptionHandler {

	@ResponseBody
	@ExceptionHandler(MyException.class)
	public ResponseEntity<?> handleControllerException(HttpServletRequest request, Throwable ex) {
		HttpStatus status = getStatus(request);
		return new ResponseEntity<>(new MyErrorBody(status.value(), ex.getMessage()), status);
	}

	private HttpStatus getStatus(HttpServletRequest request) {
		Integer code = (Integer) request.getAttribute(RequestDispatcher.ERROR_STATUS_CODE);
		HttpStatus status = HttpStatus.resolve(code);
		return (status != null) ? status : HttpStatus.INTERNAL_SERVER_ERROR;
	}

}
```







