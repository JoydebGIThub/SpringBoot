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






