# Spring Boot 3集成OpenAPI 3 (Swagger 3)

## 引入springdoc-openapi v2依赖

```xml
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.0.4</version>
</dependency>
```

## 访问Swagger UI

http://server:port/context-path/swagger-ui.html

比如：
http://localhost:8080/swagger-ui.html

## 访问OpenAPI spec

http://server:port/context-path/v3/api-docs

比如：
http://localhost:8080/v3/api-docs

也可以访问YAML格式的OpenAPI spec

http://localhost:8080/v3/api-docs.yaml


## Demo

- [restservice-h2](https://github.com/xdevops-caj-lab-cloudnative-tk/restservice-h2)

## References

- https://springdoc.org/v2/
- https://stackoverflow.com/questions/74701738/spring-boot-3-springdoc-openapi-ui-doesnt-work