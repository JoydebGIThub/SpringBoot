## Installation Instructions for the Java Developer
- We can use Spring Boot in the same way as any `standard Java library`.
- To do so, include the appropriate `spring-boot-*.jar` files on `the classpath`.
- Spring Boot does not require any special tools integration, so we can use any `IDE or text editor`.
- Also, there is nothing special about a Spring Boot application, so you can run and debug a Spring Boot application as you would any other Java program.
- Although we could `copy Spring Boot jars`, we generally recommend to use a **build tool** that **supports dependency management (such as Maven or Gradle)**.

### Maven Installation
- Spring Boot is compatible with `Apache Maven 3.6.3` or later.
- Maven can be installed with a package manager. Windows users with Chocolatey can `run choco install maven` from an `elevated (administrator) prompt`.
- Spring Boot dependencies use the `org.springframework.boot` group id.
- Typically, the `Maven POM file` inherits from the `spring-boot-starter-parent` project and declares dependencies to one or more starters. Spring Boot also provides an optional Maven plugin to create executable jars.

### Installing the Spring Boot CLI
- The `Spring Boot CLI (Command Line Interface)` is a command line tool that you can use to quickly prototype with Spring.

#### Manual Installation
- We can download the Spring CLI distribution from one of the following locations:
  - spring-boot-cli-3.4.5-bin.zip
  - spring-boot-cli-3.4.5-bin.tar.gz
- Once downloaded, follow the following instruction
##### CLI instalation
Prerequisites
-------------
- Spring Boot CLI requires Java JDK v1.8 or above in order to run. Groovy v${groovy.version}
is packaged as part of this distribution, and therefore does not need to be installed (any
existing Groovy installation is ignored).
- The CLI will use whatever JDK it finds on your path, to check that you have an appropriate version you should run:
  - java -version
- Alternatively, you can set the JAVA_HOME environment variable to point a suitable JDK.

Environment Variables
---------------------
- No specific environment variables are required to run the CLI, however, you may want to
set `SPRING_HOME` to point to a specific installation. You should also add `SPRING_HOME/bin`
to the PATH environment variable.

Shell Completion
----------------
- Shell auto-completion scripts are provided for BASH and ZSH. Add symlinks to the appropriate
location for your environment. For example, something like:
  - ln -s ./shell-completion/bash/spring /etc/bash_completion.d/spring
  - ln -s ./shell-completion/zsh/_spring /usr/local/share/zsh/site-functions/_spring

Checking Your Installation
--------------------------
- To test if you have successfully installed the CLI you can run the following command:
  - spring --version

### Installation with SDKMAN!
- SDKMAN! (The Software Development Kit Manager) can be used for managing multiple versions of various binary SDKs, including Groovy and the Spring Boot CLI. Get SDKMAN! from `sdkman.io` and `install Spring Boot` by using the following commands:
```shell
$ sdk install springboot
$ spring --version
Spring CLI v3.4.5
```
- If you develop features for the CLI and want access to the version you built, use the following commands:
```shell
$ sdk install springboot dev /path/to/spring-boot/spring-boot-cli/target/spring-boot-cli-3.4.5-bin/spring-3.4.5/
$ sdk default springboot dev
$ spring --version
Spring CLI v3.4.5
```
