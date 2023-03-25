# Deploy Sprng Boot Application to OpenShift

## Lab Env

OpenShift 4.10

## Create project

```bash
oc new-project will-petclinic
```

## Deploy MySQL database

Deoloy MySQL ephemeral via OpenShift template on service catalog.

Input parameters:
- MySQL Connection Uername: `petclinic`
- MySQL Connection Password: `petclinic`
- MySQL root user Password: `petclinic`
- MySQL Database Name: `petclinic`

The MySQL database connection url is `jdbc:mysql://mysql:3306/petclinic`

编辑资源限制：
- CPU
  - 要求：1 cores
  - 限制：1 cores
- 内存
  - 要求：512 Mi
  - 限制：512 Mi

确认MySQL Pod运行正常。

## Deploy application

Deploy Java application from GitHub repository with OpenShift S2I image build method.

Create Java application with Red Hat OpenJDK builder image.

- IST: `openjdk-17-ubi8`
- Git Repo URL: `https://github.com/xdevops-caj-lab-cloudnative-tk/spring-petclinic.git`
- 应用程序名称：`spring-petclinic-app`
- 名称：`spring-petclinic`


Edit the deployment to configure env vars to connect the MySQL database.

单值（env）：
- Name: `SPRING_PROFILES_ACTIVE`, Value: `mysql`
- Name: `MYSQL_URL`, Value: `jdbc:mysql://mysql:3306/petclinic`

说明：
- 表示使用`application-mysql.properties`应用配置文件，该配置文件内容如下：

```properties
# database init, supports mysql too
database=mysql
spring.datasource.url=${MYSQL_URL:jdbc:mysql://localhost/petclinic}
spring.datasource.username=${MYSQL_USER:petclinic}
spring.datasource.password=${MYSQL_PASS:petclinic}
# SQL is written to be idempotent so this is safe
spring.datasource.initialization-mode=always
```

编辑资源限制：
- CPU
  - 要求：1 cores
  - 限制：1 cores
- 内存
  - 要求：512 Mi
  - 限制：512 Mi

确认应用Pod运行正常。

## 访问应用

在浏览器中开应用的路由URL。

在FIND OWNERS页面，点击Find Owners按钮中查看Owners记录。

## Access the MySQL database

进入MySQL Pod，然后运行命令：

```bash
mysql -u petclinic -h mysql -p

petclinic

use petclinic;
show tables;
select * from owners;
```

## References

- https://github.com/xdevops-caj-lab-cloudnative-tk/spring-petclinic