# 构建RESTful Web服务 - CRUD

## 定义API

### 定义资源

Employee:
```json
{
    "id": 1,
    "name": "William",
    "role": "Developer"
}
```

### 定义API端点

- 查找全部Employee：`GET /employees`
- 新增Employee： `POST /employees/{id}`
- 查找Employee（如果找不到，则返回404和错误信息）： `GET /employees/{id}`
- 更新Employee（如果不存在则新增）： `PUT /employees/{id}`
- 删除Employee： `DELETE /employees/{id}`

## 创建资源表示类

创建一个资源表示类（POJO类）对应要返回的JSON消息体。

该类也是JPA Entity类。

可以使用Lombok来简化POJO类的代码。

## 创建Repository

创建一个Repository接口继承自JpaRepository接口，用于数据访问。


## 创建RestController

创建一个资源控制类（使用`@RestController`），来暴露REST API端点，接受HTTP请求并返回数据。


## 自定义异常

自定义异常类，用以表示查找不到指定的资源。



## 异常处理

另外需要自定义ControllerAdvice类来处理该异常，返回HTTP Status Code为`404`和错误信息。

## 测试API

### 新增Employee

- API Endpoint: `/employees`
- HTTP Method: `POST`
- HTTP Status Code: `200`

Request:
```bash
http post :8080/employees name="William" role="Developer"
```

Response:
```json
{
    "id": 1,
    "name": "William",
    "role": "Developer"
}
```

新增其他Employee：
```bash
http post :8080/employees name="Lucy" role="Tester"
http post :8080/employees name="Joe" role="Product Owner"
```

### 查询所有Employee

- API Endpoint: `/employees`
- HTTP Method: `GET`
- HTTP Status Code: `200`

Request:
```bash
http get :8080/employees
```

Response:
```json
[
    {
        "id": 1,
        "name": "William",
        "role": "Developer"
    },
    {
        "id": 2,
        "name": "Lucy",
        "role": "Tester"
    },
    {
        "id": 3,
        "name": "Joe",
        "role": "Product Owner"
    }
]
```

### 查找Employee

- API Endpoint: `/employees/{id}`
- HTTP Method: `GET`

**查找存在的Employee**

- HTTP Status Code: `200`

Request:
```bash
http get :8080/employees/1
```

Response:
```json
{
    "id": 1,
    "name": "William",
    "role": "Developer"
}
```

**查找不存在的Employee**

- HTTP Status Code: `404`

Request:
```bash
http get :8080/employees/99
```

Response:
```txt
Could not find employee 99
```


### 更新Employee

- API Endpoint: `/employees/{id}`
- HTTP Method: `PUT`

**更新已经存在的Employee**

- HTTP Status Code: `200`

Request:
```bash
http put :8080/employees/1 name="William" role="Dev Lead"
```

Response:
```json
{
    "id": 1,
    "name": "William",
    "role": "Dev Lead"
}
```

**更新不存在的Employee**

- HTTP Status Code: `200`

Request:
```bash
http put :8080/employees/99 name="Dudu" role="Developer"
```

Response:
```json
{
    "id": 4,
    "name": "Dudu",
    "role": "Developer"
}
```

### 删除Employee

- API Endpoint: `/employees`
- HTTP Method: `DELETE`
- HTTP Status Code: `200`

Request:
```bash
http delete :8080/employees/4
```

Response is empty.


## Demo

- [restservice/employee](https://github.com/xdevops-caj-lab-cloudnative-tk/restservice-h2/tree/main/src/main/java/com/example/restservice/employee)

## References

- [Building REST services with Spring](https://spring.io/guides/tutorials/rest/)

