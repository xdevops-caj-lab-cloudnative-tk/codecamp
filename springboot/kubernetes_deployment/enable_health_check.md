# 开启健康检查


## 引入Spring Actuator依赖

在pom.xml中引入Spring Actuator依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

## 配置健康检查端点

在application.properties中配置健康检查端点：

暴露全部actuator端点：
```properties
management.endpoints.web.exposure.include=*
```

可以访问http://localhost:8080/actuator查看全部端点。

只暴露heath端点：
```properties
management.endpoints.web.exposure.include=health
```


## 配置Kubernetes检查探针

在Deployment中配置Kubernetes检查探针。

### Liveiness探针
`livenessProbe`是Kubernetes用来检查容器是否处于运行状态的机制。livenessProbe的作用是确保容器正在运行，并在容器不可用时重启容器。

```yaml
          livenessProbe:
            httpGet:
              path: /actuator/health
              port: 8080
              scheme: HTTP
            initialDelaySeconds: 30
            timeoutSeconds: 1
            periodSeconds: 10
            successThreshold: 1
            failureThreshold: 3
```

说明：
- `initialDelaySeconds`字段指定在容器启动后等待多少秒开始检查健康状态，以确保应用程序完全启动。在这个例子中，我们设置为30秒
- `periodSeconds`字段指定多少秒检查一次健康状态。在这个例子中，我们设置为10秒
- `successThreshold` 字段指定多少次检查成功后，认为容器是健康的。在这个例子中，我们设置为1次
- `failureThreshold` 字段指定多少次检查失败后，认为容器是不健康的。在这个例子中，我们设置为3次
- `timeoutSeconds` 字段指定多少秒内检查必须返回，否则认为检查失败。在这个例子中，我们设置为1秒


需要根据应用程序的启动时间来调整`initialDelaySeconds`和`periodSeconds`的值。如果应用程序启动时间较长，可以适当增加这两个值。

### Readiness探针

在Kubernetes中，可以使用readinessProbe来检查容器是否已经准备好处理请求。如果容器不可用，则Kubernetes会停止将流量路由到该容器，并将其标记为不健康。这可以确保请求只发送到已准备好处理它们的容器。

```yaml
          readinessProbe:
            httpGet:
              path: /actuator/health
              port: 8080
              scheme: HTTP
            initialDelaySeconds: 30
            timeoutSeconds: 1
            periodSeconds: 10
            successThreshold: 1
            failureThreshold: 3
```

说明：
- `initialDelaySeconds`字段指定在容器启动后等待多少秒开始检查健康状态，以确保应用程序完全启动。在这个例子中，我们设置为30秒
- `periodSeconds`字段指定多少秒检查一次健康状态。在这个例子中，我们设置为10秒
- `successThreshold` 字段指定多少次检查成功后，认为容器是健康的。在这个例子中，我们设置为1次
- `failureThreshold` 字段指定多少次检查失败后，认为容器是不健康的。在这个例子中，我们设置为3次
- `timeoutSeconds` 字段指定多少秒内检查必须返回，否则认为检查失败。在这个例子中，我们设置为1秒

需要根据应用程序的启动时间来调整`initialDelaySeconds`和`periodSeconds`的值。如果应用程序启动时间较长，可以适当增加这两个值。

