# 构建RESTful Web服务

## 定义API

### 定义资源

```json
{
    "content": "Hello, World!",
    "id": 1
}
```

说明：
- `content`为`Hello, ${name}!`，`name`默认值为`World`，并可接受传入的参数值来覆盖默认值
- `id`从1开始，每次递增

### 定义API端点

#### Greeting

**Example 1**

Request:
```bash
greeting
```

Response:
```json
{
    "content": "Hello, World!",
    "id": 1
}
```
**Example 2**

Request:
```bash
greeting?name=William
```

Response:
```json
{
    "content": "Hello, William!",
    "id": 2
}
```

**Example 3**

Request:
```bash
greeting?name=John
```

Response:
```json
{
    "content": "Hello, John!",
    "id": 3
}
```

## 确保添加了Spring Web依赖
检查pom.xml，确保添加了Spring Web依赖：
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

## 创建资源表示类

创建一个资源表示类（POJO类）对应要返回的JSON消息体。

可以使用Lombok来简化POJO类的代码。

## 创建资源控制器

创建一个资源控制类（使用`@RestController`），来暴露REST API端点，接受HTTP请求并返回数据。

## 测试API

### 运行程序

```bash
./mvnw spring-boot:run
```

## 测试访问API

使用[HTTPie](https://github.com/httpie/httpie)来访问API：

```bash
http get :8080/greeting
http get :8080/greeting?name=William
http get :8080/greeting?name=John
```

或者使用Postman来访问API。

## Demo

- [hello/restservice/greeting](https://github.com/xdevops-caj-lab-cloudnative-tk/hello/tree/main/src/main/java/com/example/hello/restservice/greeting)

## References

- [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)