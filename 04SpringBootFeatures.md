## SpringApplication
- The `SpringApplication` class provides a convenient way to `bootstrap a Spring application` that is started from a `main()` method. In many situations, you can delegate to the static `SpringApplication.run(Class, String…​)` method, as shown in the following example:
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyApplication {

	public static void main(String[] args) {
		SpringApplication.run(MyApplication.class, args);
	}

}
```
- When your application starts, you should see some information in the console:
- By default, `INFO` logging messages are shown, including some relevant `startup details`, such as the user that launched the application.
- If you need a log level other than `INFO`, you can set it, as described in Log Levels. The application version is determined using the implementation version from the main application class’s package.
- Startup information logging can be `turned off` by setting `spring.main.log-startup-info` to `false`. This will also turn off logging of the application’s active profiles.

### Startup Failure
- If your application fails to start, registered `FailureAnalyzer` beans get a chance to provide a dedicated error message and a concrete action to fix the problem. For instance, if you start a web application on port `8080` and that port is already in use, you should see something similar to the following message.

### Lazy Initialization
- SpringApplication allows an application to be `initialized lazily`. When `lazy initialization` is `enabled`, beans are created as they are `needed` rather than during `application startup`.
- As a result, `enabling lazy initialization` can `reduce` the time that it takes your application to start. In a `web application`, enabling lazy initialization will result in many `web-related beans not being initialized` until an `HTTP request is received`.

#### Disadvantage:
- A `downside of lazy initialization` is that it can `delay the discovery of a problem` with the application. If a `misconfigured bean` is `initialized lazily`, a failure will no longer occur during startup and the problem will only become apparent when the bean is initialized.
- Care must also be taken to ensure that the JVM has sufficient memory to accommodate all of the application’s beans and not just those that are initialized during startup. For these reasons, lazy initialization is not enabled by default and it is recommended that fine-tuning of the JVM’s heap size is done before enabling lazy initialization.

-> Lazy initialization can be `enabled` programmatically using the `lazyInitialization method` on `SpringApplicationBuilder` or the `setLazyInitialization method` on `SpringApplication`. Alternatively, it can be enabled using the `spring.main.lazy-initialization` property

```properties
spring.main.lazy-initialization=true
```
```yaml
spring:
  main:
    lazy-initialization: true
```
- If you want to disable lazy initialization for `certain beans` while using lazy initialization for the rest of the application, you can explicitly set their lazy attribute to false using the `@Lazy(false)` annotation.

## Externalized Configuration
- Spring Boot lets you `externalize your configuration` so that you can work with the `same application code` in `different environments`. You can use a `variety of external configuration sources` including Java `properties files`, `YAML files`, `environment variables`, and `command-line arguments`.
- `Property values` can be `injected` directly into your beans by using the `@Value` annotation, accessed through `Spring’s Environment abstraction`, or be bound to structured objects through `@ConfigurationProperties`.
- Annotation for `externalized configuration`. Add this to a class definition or a `@Bean` method in a `@Configuration` class if you want to bind and validate some external Properties (e.g. from a .properties file).
```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class MyBean {

	@Value("${name}")
	private String name;
}
```
```properties
spring.application.name=myapp
```
```yaml
spring:
  application:
    name: "myapp"
```


```java

import java.net.InetAddress;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties("my.service")
public class MyProperties {

	private boolean enabled;

	private InetAddress remoteAddress;

	private final Security security = new Security();

	public boolean isEnabled() {
		return this.enabled;
	}

	public void setEnabled(boolean enabled) {
		this.enabled = enabled;
	}

	public InetAddress getRemoteAddress() {
		return this.remoteAddress;
	}

	public void setRemoteAddress(InetAddress remoteAddress) {
		this.remoteAddress = remoteAddress;
	}

	public Security getSecurity() {
		return this.security;
	}

	public static class Security {

		private String username;

		private String password;

		private List<String> roles = new ArrayList<>(Collections.singleton("USER"));

		public String getUsername() {
			return this.username;
		}

		public void setUsername(String username) {
			this.username = username;
		}

		public String getPassword() {
			return this.password;
		}

		public void setPassword(String password) {
			this.password = password;
		}

		public List<String> getRoles() {
			return this.roles;
		}

		public void setRoles(List<String> roles) {
			this.roles = roles;
		}

	}

}
```
---------------------------------------------------------------
### @ConstructorBinding 
- In this setup, the presence of a single parameterized constructor implies that constructor binding should be used. This means that the binder will find a constructor with the parameters that you wish to have bound.
- If your class has multiple constructors, the `@ConstructorBinding` annotation can be used to specify which constructor to use for constructor binding.
- To opt out of constructor binding for a class with a `single parameterized constructor`, the constructor must be annotated with `@Autowired` or made private. Constructor binding can be used with records. **Unless your record has multiple constructors, there is no need to use @ConstructorBinding**.
- Default values can be specified using `@DefaultValue` on `constructor parameters` and record components. The conversion service will be applied to coerce the annotation’s String value to the target type of a missing property.
- Referring to the previous example, if `no properties are bound to Security`, the MyProperties instance will contain a `null value for security`. To make it contain a `non-null instance of Security` even when `no properties` are bound to it (when using Kotlin, this will require the username and password parameters of Security to be declared as nullable as they do not have default values), use an empty `@DefaultValue` annotation:
```java
public MyProperties(boolean enabled, InetAddress remoteAddress, @DefaultValue Security security) {
		this.enabled = enabled;
		this.remoteAddress = remoteAddress;
		this.security = security;
}
```
- To use `constructor binding` the class must be enabled using `@EnableConfigurationProperties` or `configuration property scanning`. You cannot use constructor binding with beans that are created by the regular Spring mechanisms (for example @Component beans, beans created by using @Bean methods or beans loaded by using @Import)
```java
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Configuration;

@Configuration(proxyBeanMethods = false)
@EnableConfigurationProperties(SomeProperties.class)
public class MyConfiguration {

}
```

```java
import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties("some.properties")
public class SomeProperties {

}
```

- To use configuration property scanning, add the `@ConfigurationPropertiesScan` annotation to your application. Typically, it is added to the main application class that is annotated with `@SpringBootApplication` but it can be added to any `@Configuration class`. By default, scanning will occur from the package of the class that declares the annotation.
```java
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.context.properties.ConfigurationPropertiesScan;

@SpringBootApplication
@ConfigurationPropertiesScan({ "com.example.app", "com.example.another" })
public class MyApplication {

}
```
--------------------------------------------------------------------------------------------
## @Profiles
- Spring `Profiles` provide a way to segregate parts of your application configuration and make it be available only in certain environments. Any `@Component`, `@Configuration` or `@ConfigurationProperties` can be marked with `@Profile` to limit when it is loaded, as shown in the following example:
```java
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;

@Configuration(proxyBeanMethods = false)
@Profile("production")
public class ProductionConfiguration {
}
```
- If `@ConfigurationProperties` beans are registered through `@EnableConfigurationProperties` instead of automatic scanning, the `@Profile` annotation needs to be specified on the `@Configuration` class that has the `@EnableConfigurationProperties` annotation. In the case where `@ConfigurationProperties` are scanned, `@Profile` can be specified on the `@ConfigurationProperties` class itself.
- You can use a `spring.profiles.active` Environment property to specify which profiles are active.
```properties
spring.profiles.active=dev,hsqldb
```
- If no profile is active, a default profile is enabled. The name of the `default profile` is `default` and it can be tuned using the `spring.profiles.default` Environment property
```properties
spring.profiles.default=none
```
- `spring.profiles.active` and `spring.profiles.default` can only be used in `non-profile-specific` documents. This means they cannot be included in `profile specific files or documents` activated by `spring.config.activate.on-profile`.
```properties
spring.profiles.active=prod
#---
spring.config.activate.on-profile=prod
spring.profiles.active=metrics
```


