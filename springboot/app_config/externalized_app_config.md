# Spring Boot配置的加载顺序以及如何实现多环境的Spring Boot应用配置

## Spring Boot配置的加载顺序

Spring Boot 使用一种非常特殊的PropertySource顺序，旨在允许合理地覆盖值。后面的属性源可以覆盖前面定义的值。来源按以下顺序考虑：

1. 默认属性（由设置指定SpringApplication.setDefaultProperties）。
2. @PropertySource类上的注释@Configuration。Environment请注意，在刷新应用程序上下文之前，不会将此类属性源添加到。现在配置某些属性（例如logging.*和spring.main.*在刷新开始前读取）为时已晚。
3. 配置数据（例如application.properties文件）。
4. RandomValuePropertySource仅在 中具有属性的random.*。
5. 操作系统环境变量。
6. Java 系统属性 ( System.getProperties())。
7. JNDI 属性来自java:comp/env.
8. ServletContext初始化参数。
9. ServletConfig初始化参数。
10. 属性来自SPRING_APPLICATION_JSON（嵌入在环境变量或系统属性中的内联 JSON）。
11. 命令行参数。
12. properties测试的属性。@SpringBootTest在和测试注释上可用，用于测试应用程序的特定部分。
13. @TestPropertySource测试注释。
14. $HOME/.config/spring-boot当 devtools 处于活动状态时，目录中的Devtools全局设置属性。

配置数据文件按以下顺序考虑：
1. 打包在 jar 中的应用程序属性application.properties（和 YAML 变体）。
2. 打包在您的 jar（和 YAML 变体）中的特定于配置文件的应用程序属性application-{profile}.properties。
3. 打包的 jar（和 YAML 变体）之外的应用程序属性application.properties。
4. 打包的 jar（和 YAML 变体）之外的特定于配置文件的应用程序属性application-{profile}.properties。

建议使用同一种配置数据文件格式。如何同时有`.properties`和`.yml`格式，则`.properties`格式优先。

## 使用profile-specific应用配置来实现多环境的应用配置


除了application属性文件，Spring Boot 还将尝试使用命名约定加载特定于配置文件的文件`application-{profile}`。通过指定`spring.profiles.active`来激活指定的profiles。
例如，如果您的应用程序激活一个profile名为`prod`并使用 YAML 文件，那么`application.yml`和`application-prod.yml`都会被考虑。

特定于配置文件的属性从与标准相同的位置加载`application.properties`，**特定于配置文件的文件始终覆盖非特定文件**。如果指定了多个配置文件，则应用**最后获胜**的策略。例如，如果在`spring.profiles.active`配置`prod,live`，则 中的值`application-prod.properties`可以被`application-live.properties 中的值覆盖`。

## 使用属性占位符

在application.properties中可以使用属性占位符`${name}`。
属性占位符还可以指定一个默认值，例如`${name:default}`。

例子：
```properties
spring.datasource.url=${MYSQL_URL:jdbc:mysql://localhost/petclinic}
```

当应用程序从环境变量中获取`MYSQL_URL`的值后，就会覆盖原来的默认值`jdbc:mysql://localhost/petclinic`。



## References

- https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config.files
