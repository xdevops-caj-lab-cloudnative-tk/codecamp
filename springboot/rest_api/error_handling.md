# 全局异常处理和统一错误响应数据格式


## 参数校验

引入校验依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

在Request DTO中设置校验规则。

在Controller的方法参数中使用`@Valid`注解表示要对该参数进行校验。

## 定义错误时返回数据的统一格式

Example:
```json
{
    "code": 400,
    "errors": [
        "name: Name is mandatory",
        "address: Address is mandatory"
    ],
    "message": "Constraint violations",
    "status": "BAD_REQUEST"
}
```

说明：
- code: HTTP status code
- status: HTTP status
- errors: 错误信息列表
- message: 错误信息概述


## 定义全局异常处理类

使用`@ControllerAdvice`定义全局异常处理类，捕捉异常，并返回统一格式的响应数据。

捕捉的常见异常包括：
- ConstraintViolationException – This exception reports the result of constraint violations.
- MethodArgumentTypeMismatchException – This exception is thrown when method argument is not the expected type.
- MethodArgumentNotValidException – This exception is thrown when an argument annotated with `@Valid` failed validation.

还可以捕捉自定义的异常，比如：
- ResourceNotFoundException
- ResourceAlreadyExistsException

还可以捕捉通用的异常作为“兜底”：
- Exception
- RuntimeException



## 测试访问API

### 新增Customer

Reuest:
```bash
http post :8080/customers name="boc" address="fotan" id=1
```

Response:

HTTP Status Code: `200`

```json
{
    "address": "fotan",
    "id": 1,
    "name": "boc"
}
```

### 处理ConstraintViolationException

Reuest:
```bash
http post :8080/customers industry="fsi"
```

Response:

HTTP Status Code: `400`

Captured exception: `ConstraintViolationException.class`

```json
{
    "code": 400,
    "errors": [
        "name: Name is mandatory",
        "address: Address is mandatory"
    ],
    "message": "Constraint violations",
    "status": "BAD_REQUEST"
}
```

### 处理MethodArgumentTypeMismatchException

Reuest:
```bash
http get :8080/customers/abc
```

Response:

HTTP Status Code: `400`

Captured exception: `MethodArgumentTypeMismatchException.class`

```json
{
    "code": 400,
    "errors": [
        "id should be of type java.lang.Long"
    ],
    "message": "The method argument is not the expected type",
    "status": "BAD_REQUEST"
}
```

### 处理MethodArgumentNotValidException


Reuest:
```bash
http post :8080/customers industry="fsi"
```

Response:

HTTP Status Code: `400`

Captured exception: `MethodArgumentNotValidException.class`

```json
{
    "code": 400,
    "errors": [
        "name: Name is mandatory",
        "address: Address is mandatory"
    ],
    "message": "The method argument is not valid",
    "status": "BAD_REQUEST"
}
```

### 处理CustomerNotFoundException

Reuest:
```bash
http get :8080/customers/99
```

Response:

HTTP Status Code: `404`

Captured exception: `CustomerNotFoundException.class`

```json
{
    "code": 404,
    "errors": [
        "Could not find customer 99"
    ],
    "message": "Could not find customer 99",
    "status": "NOT_FOUND"
}
```

## Demo

- [restservice/exception](https://github.com/xdevops-caj-lab-cloudnative-tk/restservice-h2/tree/main/src/main/java/com/example/restservice/exception)
- [restservice/cutomer](https://github.com/xdevops-caj-lab-cloudnative-tk/restservice-h2/tree/main/src/main/java/com/example/restservice/customer)

## References

- https://www.baeldung.com/global-error-handler-in-a-spring-rest-api
- https://salithachathuranga94.medium.com/validation-and-exception-handling-in-spring-boot-51597b580ffd




