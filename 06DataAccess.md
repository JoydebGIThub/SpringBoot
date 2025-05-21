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
```properties
app.datasource.url=jdbc:mysql://localhost/test
app.datasource.username=dbuser
app.datasource.password=dbpass
app.datasource.pool-size=30
```
