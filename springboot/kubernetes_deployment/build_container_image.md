# 如何构建容器镜像

## 构建容器镜像

可以选择基于Dockefile构建镜像，也可以使用OpenShift S2I构建镜像。

构建工具可以使用Docker，也可以使用Podman。

Dockerfile例子：

```dockerfile
FROM registry.access.redhat.com/ubi8/openjdk-8

ENV TZ=Asia/Shanghai

COPY target/*.jar /opt/spring-petclinic.jar

CMD java -jar /opt/spring-petclinic.jar

EXPOSE 8080
```

说明：
- 假设你已经构建了一个应用程序的jar包，并将其打包到了target目录下
- 基础镜像为Red Hat OpenJDK 8 UBI镜像构建(根据情况，你也可以选择Dockerhub上的OpenJDK镜像为基础镜像)
- 可选设置时区为Asia/Shanghai
- 将应用程序jar文件复制到容器的/opt目录中
- 使用CMD指令启动应用程序。
- 暴露容器的8080端口

其它：
- 在Kubernetes中部署时，不要在Dockerfile中指定JAVA_OPTS参数，因为在Kubernetes中，容器的CPU和内存是可以动态调整的，而在Dockerfile中指定JAVA_OPTS参数，会导致容器的CPU和内存无法动态调整。
- 在Kubernetes中部署时，不要在Dockerfile中将application.properties复制到容器镜像中，而是应该使用Kubernetes ConfigMap和Secret来管理应用程序的配置文件。

生产部署的基础镜像建议使用Red Hat Container Catalog中经过安全认证的基础镜像。

参考：
- [Red Hat Container Catalog](https://catalog.redhat.com/software/containers/search)
- [Dockerhub | OpenJDK](https://hub.docker.com/_/openjdk)
- [Podman](https://podman.io/getting-started/)

## 容器镜像构建最佳实践

- 采用认证的安全基础镜像
- 让镜像以非root用户身份运行
- 设置组所有权和文件权限
- 使用多阶段构建镜像或复制构建好的制品到容器镜像中，避免在容器镜像中添加构建应用程序的工具和库
- 在镜像中包含最新的安全更新
- 维护原始基础镜像层
- 限制镜像中的层数

参考：
- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
- https://developers.redhat.com/articles/2021/11/11/best-practices-building-images-pass-red-hat-container-certification
