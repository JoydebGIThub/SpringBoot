## Configure a Custom DataSource:
- To configure your own `DataSource`, define a `@Bean` of that type in your configuration.
- Spring Boot reuses your `DataSource` anywhere one is required, including `database initialization`. If you need to externalize some settings, you can bind your DataSource to the environment.
```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration(proxyBeanMethods = false)
public class MyDataSourceConfiguration {

	@Bean
	@ConfigurationProperties(prefix = "app.datasource")
	public SomeDataSource dataSource() {
		return new SomeDataSource();
	}

}
```
```properties
app.datasource.url=jdbc:h2:mem:mydb
app.datasource.username=sa
app.datasource.pool-size=30
```
```yaml
app:
  datasource:
    url: "jdbc:h2:mem:mydb"
    username: "sa"
    pool-size: 30
```
- Assuming that SomeDataSource has `regular JavaBean` properties for the URL, the username, and the pool size, these settings are `bound automatically` before the `DataSource is made available to other components`.
- Spring Boot also provides a `utility builder class`, called `DataSourceBuilder`, that can be used to create one of the standard data sources (if it is on the classpath). The builder can `detect` which one to use based on what is available on the classpath. It also `auto-detects` the `driver based on the JDBC URL`.
```java
import javax.sql.DataSource;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration(proxyBeanMethods = false)
public class MyDataSourceConfiguration {

	@Bean
	@ConfigurationProperties("app.datasource")
	public DataSource dataSource() {
		return DataSourceBuilder.create().build();
	}

}
```
-To run an app with that DataSource, all you need is the connection information. Pool-specific settings can also be provided.

```properties
app.datasource.url=jdbc:mysql://localhost/test
app.datasource.username=dbuser
app.datasource.password=dbpass
app.datasource.pool-size=30
```
- However, there is a catch due to the method’s `DataSource` return type.
- This `hides` the `actual type` of the `connection pool` so `no configuration property metadata` is generated for the `custom DataSource` and `no auto-completion` is available in the IDE.
- To address this problem, use the `builder’s type(Class) method` to specify the `type of DataSource` to be built and update the method’s return type.
```java
import com.zaxxer.hikari.HikariDataSource;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration(proxyBeanMethods = false)
public class MyDataSourceConfiguration {
	@Bean
	@ConfigurationProperties("app.datasource")
	public HikariDataSource dataSource() {
		return DataSourceBuilder.create().type(HikariDataSource.class).build();
	}
}
```
- Unfortunately, this basic setup `does not work` because `Hikari has no url` property. Instead, it has a jdbc-url property which means that you must rewrite your configuration as follows:
```properties
app.datasource.jdbc-url=jdbc:mysql://localhost/test
app.datasource.username=dbuser
app.datasource.password=dbpass
app.datasource.pool-size=30
```
- To address this problem, make use of `DataSourceProperties` which will handle the `url` to `jdbc-url` translation.
- You can `initialize a DataSourceBuilder` from the state of any `DataSourceProperties` object using its `initializeDataSourceBuilder()` method.
- You could `inject` the `DataSourceProperties` that Spring Boot creates automatically, however, that would `split` your `configuration` across `spring.datasource.*` and `app.datasource.*`. To avoid this, define a custom `DataSourceProperties` with a `custom configuration properties prefix`.
```java
import com.zaxxer.hikari.HikariDataSource;

import org.springframework.boot.autoconfigure.jdbc.DataSourceProperties;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;

@Configuration(proxyBeanMethods = false)
public class MyDataSourceConfiguration {

	@Bean
	@Primary
	@ConfigurationProperties("app.datasource")
	public DataSourceProperties dataSourceProperties() {
		return new DataSourceProperties();
	}

	@Bean
	@ConfigurationProperties("app.datasource.configuration")
	public HikariDataSource dataSource(DataSourceProperties properties) {
		return properties.initializeDataSourceBuilder().type(HikariDataSource.class).build();
	}
}
```
- This setup is equivalent to what Spring Boot does for you by default, except that the pool’s type is specified in code and its settings are exposed as `app.datasource.configuration.*` properties. DataSourceProperties takes care of the `url to jdbc-url` translation.
```properties
app.datasource.url=jdbc:mysql://localhost/test
app.datasource.username=dbuser
app.datasource.password=dbpass
app.datasource.configuration.maximum-pool-size=30
```
---------------------------------------------------------------------------------
## Configure Two DataSources
- To define an `additional DataSource`, an approach that’s similar to the previous section can be used. A key difference is that the `DataSource` `@Bean` must be `declared` with `defaultCandidate=false`. This prevents the auto-configured DataSource from backing off.
- To allow the `additional DataSource` to be injected where it’s needed, also annotate it with `@Qualifier` as shown in the following example:
```java
import com.zaxxer.hikari.HikariDataSource;

import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration(proxyBeanMethods = false)
public class MyAdditionalDataSourceConfiguration {
	@Qualifier("second")
	@Bean(defaultCandidate = false)
	@ConfigurationProperties("app.datasource")
	public HikariDataSource secondDataSource() {
		return DataSourceBuilder.create().type(HikariDataSource.class).build();
	}
}
```
- The auto-configured and additional data sources can be configured as follows:
```properties
spring.datasource.url=jdbc:mysql://localhost/first
spring.datasource.username=dbuser
spring.datasource.password=dbpass
spring.datasource.configuration.maximum-pool-size=30

app.datasource.url=jdbc:mysql://localhost/second
app.datasource.username=dbuser
app.datasource.password=dbpass
app.datasource.max-total=30
```
- More advanced, `implementation-specific`, `configuration of the auto-configured DataSource` is available through the `spring.datasource.configuration.*` properties. You can apply the same concept to the additional DataSource as well
```java
import com.zaxxer.hikari.HikariDataSource;

import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.autoconfigure.jdbc.DataSourceProperties;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration(proxyBeanMethods = false)
public class MyCompleteAdditionalDataSourceConfiguration {

	@Qualifier("second")
	@Bean(defaultCandidate = false)
	@ConfigurationProperties("app.datasource")
	public DataSourceProperties secondDataSourceProperties() {
		return new DataSourceProperties();
	}

	@Qualifier("second")
	@Bean(defaultCandidate = false)
	@ConfigurationProperties("app.datasource.configuration")
	public HikariDataSource secondDataSource(
			@Qualifier("secondDataSourceProperties") DataSourceProperties secondDataSourceProperties) {
		return secondDataSourceProperties.initializeDataSourceBuilder().type(HikariDataSource.class).build();
	}

}
```





