# 用RestTemplate方式调用RESTful Web服务

## 要访问的RESTful Web服务

克隆[quoters](https://github.com/xdevops-caj-lab-cloudnative-tk/quoters) 到本地。

运行应用程序：
```bash
mvn clean install
mvn spring-boot:run
```

API说明：
- 获取一个随机的quotes：`GET /api/quotes/random`
- 获取一个指定的quotes; `GET /api/quotes/{id}`
- 获取全部quotes: `GET /api/quotes`

## 注入restTemplate bean

在Application类中注入restTemplate bean


## 定义API资源类

定义Quote和Value类来对应API资源。

## 调用REST API

使用`restTemplate.getForObject()`或`restTemplate.getForEntity()`方法以`GET`方式访问REST API。

## 设置调用REST API超时

设置resttemplate的connection timeout和read timeout。

专门定一个响应慢的服务`/api/random/slow`来进行测试，可以看到超时时间到后resttemplate会中止调用，并返回超时错误。


## Demo

- [restservice/consumingrest](https://github.com/xdevops-caj-lab-cloudnative-tk/restservice-h2/tree/main/src/main/java/com/example/restservice/consumingrest)

## References

- https://spring.io/guides/gs/consuming-rest/
- https://www.baeldung.com/rest-template
- https://stackoverflow.com/questions/13837012/spring-resttemplate-timeout

## Troubleshooting


# Intellij报关于RestTemplate的依赖注入错误

Intellij识别的问题，可以忽略。

参见：
- https://stackoverflow.com/questions/26889970/intellij-incorrectly-saying-no-beans-of-type-found-for-autowired-repository
