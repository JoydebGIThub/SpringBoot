## Developing the First Spring Boot Application
### Setting Up the Project With Maven
- We need to start by creating a Maven pom.xml file. The pom.xml is the recipe that is used to build your project. Open your favorite text editor and add the following:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example</groupId>
	<artifactId>myproject</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>3.4.5</version>
	</parent>

	<!-- Additional lines to be added here... -->

</project>
```
- You can test it by running `mvn package` (for now, you can ignore the “jar will be empty - no content was marked for inclusion!” warning).
- At this point, you could import the project into an IDE (most modern Java IDEs include built-in support for Maven). For simplicity, we continue to use a plain text editor for this example.

### Adding Classpath Dependencies
- Spring Boot provides a number of `starters` that let you `add jars to the classpath`. Starters provide `dependencies` that we are likely to need when `developing a specific type of application`.
#### Maven
- Most Spring Boot applications use the `spring-boot-starter-parent` in the `parent section of the POM`. The `spring-boot-starter-parent` is a `special starter` that provides `useful Maven defaults`. It also provides a `dependency-management section` so that we can omit `version tags` for “blessed” dependencies.
- Since we are developing a web application, we add a spring-boot-starter-web dependency.
```shell
$ mvn dependency:tree
[INFO] com.example:myproject:jar:0.0.1-SNAPSHOT
```
- The `mvn dependency:tree` command prints a `tree representation of the project dependencies`. We can see that `spring-boot-starter-parent` provides `no dependencies by itself`. To add the necessary dependencies, edit your pom.xml and add the `spring-boot-starter-web` dependency immediately below the parent section:
```xml
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</dependency>
</dependencies>
```
- If you run `mvn dependency:tree` again, we can see that there are `now a number of additional dependencies`, including the `Tomcat web server` and `Spring Boot itself`.

## Writing the Code
- To finish our application, we need to create a `single Java file`. By default, Maven and Gradle `compile sources` from `src/main/java`, so we need to create that `directory structure` and then `add a file named` `src/main/java/com/example/MyApplication.java` to contain the following code:
```java
package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@SpringBootApplication
public class MyApplication {

	@RequestMapping("/")
	String home() {
		return "Hello World!";
	}

	public static void main(String[] args) {
		SpringApplication.run(MyApplication.class, args);
	}

}
```
### The annotation we use here:
#### The @RestController and @RequestMapping Annotations:
##### @RestController
- The first annotation on our MyApplication class is `@RestController`. This is known as a **stereotype annotation**. It provides hints for people reading the code and for Spring that the class plays a specific role. In this case, our class is a web `@Controller`, so Spring considers it when handling incoming web requests.
- A convenience annotation that is itself annotated with` @Controller` and `@ResponseBody`.
- Types that carry this annotation are treated as controllers where `@RequestMapping` methods assume `@ResponseBody` semantics by default.
###### @Controller
- Indicates that an annotated class is a "Controller" (for example, a web controller).
- This annotation serves as a specialization of `@Component`, allowing for implementation classes to be `autodetected through classpath scanning`. It is typically used in combination with `annotated handler methods` based on the `RequestMapping annotation`.
###### @ResponseBody
- Annotation that indicates a `method return value` should be bound to the `web response body`. Supported for annotated handler methods.

##### @RequestMapping:
- The `@RequestMapping` annotation provides “routing” information. It tells Spring that any `HTTP request` with the `/` path should be mapped to the `home method`. The `@RestController` annotation tells Spring to `render the resulting string` `directly back to the caller`.
- Annotation for **mapping web requests** onto `methods in request-handling classes` with flexible method signatures.
- Both `Spring MVC` and `Spring WebFlux` support this annotation through a `RequestMappingHandlerMapping` and `RequestMappingHandlerAdapter` in their respective modules and package structures. For the exact list of supported handler method arguments and return types in each.
- This annotation can be used both at the **class and at the method level**. In most cases, at the **method level applications** will prefer to use one of the **HTTP method specific variants** `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, or `@PatchMapping`.
- This annotation cannot be used in `conjunction with other @RequestMapping annotations` that are declared on the same element (class, interface, or method). If **multiple** `@RequestMapping` annotations are detected on the **same element**, a `warning will be logged`, and only the **first mapping will be used**. This also applies to **composed** `@RequestMapping` annotations such as `@GetMapping`, `@PostMapping`, etc.
- When using `controller interfaces` (for example, for AOP proxying), make sure to consistently put `all your mapping annotations` — such as `@RequestMapping` and `@SessionAttributes` — on the `controller interface` rather than on the `implementation class`.

###### path:
- The path mapping URIs — for example, "/profile".
- This is an alias for `path()`. For example, `@RequestMapping("/profile")` is equivalent to `@RequestMapping(path="/profile")`.

###### value:
- Ant-style path patterns are also supported (for example, "/profile/**"). At the method level, `relative paths` (for example, "edit") are supported within the `primary mapping` expressed at the `type level`. **Path mapping URIs may contain placeholders** (for example, "/${profile_path}").
- **Supported at the type level as well as at the method level**! When used at the type level, all method-level mappings inherit this primary mapping, narrowing it for a specific handler method.
- A handler method that is not mapped to any path explicitly is effectively mapped to an empty path

###### method
- The HTTP request methods to map to, narrowing the primary mapping: GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE.
- **Supported at the type level as well as at the method level**! When used at the type level, all method-level mappings inherit this HTTP method restriction.

**1. @GetMapping**
- Annotation for mapping `HTTP GET` requests onto specific handler methods.
- Specifically, `@GetMapping` is a `composed annotation` that acts as a shortcut for **@RequestMapping(method = RequestMethod.GET)**.

###### headers
- The headers of the mapped request, narrowing the primary mapping.
- Also supports media type wildcards (*), for headers such as Accept and Content-Type. For instance,
```
@RequestMapping(value = "/something", headers = "content-type=text/*")
``` 
- will match requests with a Content-Type of "text/html", "text/plain", etc.

###### Consumes:
- Narrows the primary mapping by media types that can be `consumed by the mapped handler`. Consists of one or more media types one of which must match to the request Content-Type header. Examples:
```
 consumes = "text/plain"
 consumes = {"text/plain", "application/*"}
 consumes = MediaType.TEXT_PLAIN_VALUE
``` 
- If a declared media type contains a parameter, and if the "content-type" from the request also has that parameter, then the parameter values must match. Otherwise, if the media type from the request "content-type" does not contain the parameter, then the parameter is ignored for matching purposes.
- Expressions can be negated by using the "!" operator, as in "!text/plain", which matches all requests with a Content-Type other than "text/plain".

#### The @SpringBootApplication Annotation:
- The second class-level annotation is `@SpringBootApplication`. This annotation is known as a **meta-annotation**, it combines `@SpringBootConfiguration`, `@EnableAutoConfiguration` and `@ComponentScan`.
##### @EnableAutoConfiguration:
- `@EnableAutoConfiguration` tells Spring Boot to “guess” how you want to configure Spring, based on the jar dependencies that you have added. Since `spring-boot-starter-web` added Tomcat and Spring MVC, the `auto-configuration` assumes that you are developing a `web application` and sets up Spring accordingly.
- Enable auto-configuration of the Spring Application Context, attempting to guess and configure beans that you are likely to need. Auto-configuration classes are usually applied based on your classpath and what beans you have defined. For example, if you have tomcat-embedded.jar on your classpath you are likely to want a `TomcatServletWebServerFactory` (unless you have defined your own `ServletWebServerFactory` bean).
- When using `@SpringBootApplication`, the `auto-configuration of the context` is automatically enabled and adding this annotation has therefore no additional effect.

##### @SpringBootConfiguration:
- Indicates that a class provides Spring Boot application `@Configuration`. Can be used as an alternative to the Spring's standard `@Configuration` annotation so that configuration can be found automatically (for example in tests).
- Application should only ever include  one `@SpringBootConfiguration` and most idiomatic Spring Boot applications will inherit it from `@SpringBootApplication`.

##### @ComponentScan:
- Configures component scanning directives for use with @Configuration classes.
- Provides support comparable to Spring's `<context:component-scan>` XML namespace element.
- Either `basePackageClasses()` or `basePackages()` (or its alias value()) may be specified to define specific `packages to scan`. If specific `packages are not defined`, scanning will occur `recursively beginning with the package of the class` that declares this annotation.
- Note that the `<context:component-scan>` element has an `annotation-config` attribute; however, this annotation does not. This is because in almost all cases when using `@ComponentScan`, default annotation config processing (for example, processing @Autowired and friends) is assumed. Furthermore, when using `AnnotationConfigApplicationContext`, annotation config processors are always registered, meaning that any attempt to `disable them at the @ComponentScan level` would be ignored.

### The “main” Method
The final part of our application is the `main` method. This is a standard method that follows the Java convention for an `application entry point`. Our main method delegates to Spring Boot’s `SpringApplication` class by calling `run`.
`SpringApplication bootstraps` our application, starting Spring, which, in turn, starts the `auto-configured Tomcat web server`. We need to pass `MyApplication.class` as an `argument to the run method` to tell `SpringApplication` which is the `primary Spring component`. The `args` array is also passed through to expose any command-line arguments.

### Run:
```
mvn spring-boot:run
```






