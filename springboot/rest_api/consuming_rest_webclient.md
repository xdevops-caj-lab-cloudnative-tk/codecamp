# 使用WebClient访问RESTful Web服务

RestTemplate是同步阻塞式调用，而WebClient是异步非阻塞式调用。


## 引入WebFlux依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

## 使用WebClient来访问RESTful Web服务

使用WebClient来访问RESTful Web服务。

## 设置全局

定义WebclientTimeoutCustomizer类，实现WebClientCustomizer接口。

在HTTPClient实例上设置超时，并修改webClientBuilder。


## 测试访问API

```bash
# 取随机quotes
http :8080/webclient/quotes/random

# 取第一个quotes
http :8080/webclient/quotes/1

# 取全部quotes
http :8080/webclient/quotes

# 访问一个会超时的服务
http :8080/webclient/quotes/random/slow
```

## Demo

- 【restservice/consumingrest](https://github.com/xdevops-caj-lab-cloudnative-tk/restservice-h2/tree/main/src/main/java/com/example/restservice/consumingrest)

## References

- https://www.baeldung.com/spring-webflux-timeout
- https://www.baeldung.com/spring-5-webclient
- https://spring.io/guides/gs/reactive-rest-service/