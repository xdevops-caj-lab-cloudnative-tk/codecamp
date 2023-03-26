# 部署Spring Boot应用到Kubernetes

## 部署步骤

1. 需要将Spring Boot应用打包成容器镜像，然后将镜像推送到容器镜像仓库进行管理。
2. 准备一个Kubernetes Deployment的YAML文件，用于描述应用的部署信息。
3. 准备一个Kubernetes Service的YAML文件，用于描述应用的服务信息。

## 构建和管理容器镜像

参见 [如何构建容器镜像](./build_container_image.md)

容器镜像仓库可以选择Quay，Harbor或Nexus。


## Kubernetes Deployment

在Kubernetes Deployment中的关键配置：

- 指定容器镜像的名称和版本
- 指定容器的端口，与容器镜像中定义的EXPOSE端口一致
- 指定容器环境变量，环境变量可以来自单独的变量，或来自ConfigMap和Secret
- 指定容器资源限制
    - 指定CPU的最小要求和最大限制
    - 指定内存的最小要求和最大限制
- 指定副本数

## Kubernetes ConfigMap

准备一个Kubernetes ConfigMap的YAML文件，用于描述应用的配置信息。

## Kubernetes Secret

准备一个Kubernetes Secret的YAML文件，用于描述应用的Secret信息。

## Kubernetes Service

准备一个Kubernetes Service的YAML文件，用于描述应用的服务信息。

## 多环境的配置管理和Secret管理

可以使用Kustomze或Helm来简化多环境的Kubernetes YAML管理。

Kubernetes原生支持Kustomize，而无需引入额外组建。

Kustomize的目录结构例子：

```bash
spring-petclinic
├── base
│   ├── deployment-spring-petclinic.yaml
│   ├── kustomization.yaml
│   ├── route-spring-petclinic.yaml
│   └── service-spring-petclinic.yaml
└── overlays
    ├── dev
    │   └── kustomization.yaml
    └── test
        └── kustomization.yaml
```

说明：
- base目录下的YAML文件是通用的，不同环境的YAML文件都会引用这些文件
- overlays目录下的YAML文件是环境特定的，会覆盖base目录下的YAML文件
- base目录下的文件包括：
    - kustimization.yaml - 用于指定base目录下的YAML文件
    - 其他yaml为应用的Kubernetes资源文件
- overlays目录下的文件包括：
    - kustimization.yaml - 用于指定base目录下的YAML文件，以及指定环境特定的YAML文件。比如：
        - 定制Deployment的镜像版本、副本数、资源限制等
        - 定制ConfigMap的内容，比如指定`SPRING_PROFILES_ACTIVE`环境变量的值
        - 定制Secret的内容，比如指定数据库用户名和密码


用Kustomize部署应用与部署Kubernetes YAML非常一致，只是需要将`-f`改为`-k`。

例子：
```bash
# 部署到dev环境
oc apply -k spring-petclinic/overlays/dev

# 部署到test环境
oc apply -k spring-petclinic/overlays/test
```

## Demo

- [spring-petclinic kustomize manifests](https://github.com/xdevops-caj-lab-cloudnative-tk/spring-petclinic/tree/main/kubernetes/kustomize-manifest/spring-petclinic)

## References

- https://kubernetes.io/zh-cn/docs/tasks/manage-kubernetes-objects/kustomization/

