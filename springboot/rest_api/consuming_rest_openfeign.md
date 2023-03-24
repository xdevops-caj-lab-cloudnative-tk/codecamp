# 使用OpenFeign访问RESTful Web服务

## 引入依赖

引入Spring OpenFeign依赖：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

并需要添加Spring Cloud依赖到dependencyManagement：
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring-cloud.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```


## 启用FeignClients

在Applicaiton类中使用`@EnableFeignClients`注解启用FeignClient。

## 定义FeignClient接口

定义FeignClient接口来声明要调用的API。

需要指定FeignClient的url。

```java
@FeignClient(value = "quotes", url = "${myapp.quotes.host}")
public interface QuotesFeignClient {
    // ...
}
```

url在应用配置文件中定义:
```yaml
myapp:
  quotes:
    host: http://localhost:8081
```

对于Spring Boot 3 (OpenFeign 4)也可以直接在applicaiton.yaml中定义`spring.cloud.openfeign.feignName.url`，而无需在FeignClient中再指定url绑定的属性。



## 使用FeignClient访问API

像调用本地方法一样调用FeignClient来访问API。

## 设置OpenFeign超时

Spring Boot 2 (OpenFeign 3):

```yaml
feign:
  client:
    config:
      feignName:
        connectTimeout: 5000
        readTimeout: 5000
```

参见：
- https://cloud.spring.io/spring-cloud-openfeign/reference/html/


Spring Boot 3 (OpenFeign 4):

```yaml
spring:
  cloud:
    openfeign:
      client:
        config:
          default:
            connectTimeout: 5000
            readTimeout: 5000
```

参见： 
- https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html/#spring-cloud-feign


## 测试访问API

```bash
# 取随机quotes
http :8080/feign/quotes/random

# 取第一个quotes
http :8080/feign/quotes/1

# 取全部quotes
http :8080/feign/quotes

# 访问一个会超时的服务
http :8080/feign/quotes/random/slow
```

## Demo




## References

- https://docs.spring.io/spring-cloud-openfeign/docs/current/reference/html/#spring-cloud-feign
- https://www.baeldung.com/spring-cloud-openfeign
