## AMQP
- The **Advanced Message Queuing Protocol** (AMQP) is a platform-neutral, wire-level protocol for message-oriented middleware.
- The Spring AMQP project applies core Spring concepts to the development of AMQP-based messaging solutions.
- Spring Boot offers several conveniences for working with AMQP through RabbitMQ, including the `spring-boot-starter-amqp` starter.

### RabbitMQ Support
- `RabbitMQ` is a `lightweight`, `reliable`, `scalable`, and `portable message broker` based on the `AMQP protocol`. Spring uses `RabbitMQ` to communicate through the AMQP protocol.
- RabbitMQ configuration is controlled by external configuration properties in `spring.rabbitmq.*`. For example, you might declare the following section in application.properties
```java
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=admin
spring.rabbitmq.password=secret
```








