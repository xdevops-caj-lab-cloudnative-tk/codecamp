# 如何创建Spring Boot项目

## Spring Boot与JDK的版本映射

- Spring Boot 2 (Java 8+)
- Spring Boot 3 (Java 17+)


## 在IDE中创建

### 在Intellj IDEA中创建

1. 打开File -> New -> Project
2. 选择Spring Initializr
3. 输入项目信息：
    - Group: 制品包的组，对应POM文件中的`groupId`
    - Artifact：制品包名，对应POM文件中的`artifactId`
    - Type：构建工具类型，选择Maven
    - Language：默认为Java
    - Packaging：默认为Jar
    - Java version：选择合适的Java版本，对应POM文件中的`java.version`属性
    - Version：制品包的版本，对应POM文件中的`version`
    - Name: 项目名称，对应POM文件中的`name`
    - Description: 项目描述，对应POM文件中的`description`
    - Package: 包路径
4. 选择Spring Boot版本和项目的依赖
5. 确认项目名称，并选择项目存放位置
6. 创建项目

参考文档：
- <https://www.jetbrains.com/help/idea/your-first-spring-application.html>


### 在Intellij Community中创建

需要先安装相关插件（可以通过`spring`关键字搜索)，再参考上面的过程来创建Spring Boot项目。

注意：需要安装与你当前Intellij版本兼容的插件。


另外也可以通过Spring官方的Spring Initializr网站生成Spring Boot项目后，再导入到IDE中。

## 在Spring官方的Spring Initializr网站创建

1. 访问<https://start.spring.io/>
2. 选择Project为Maven
3. 选择Language为Java
4. 选择Spring Boot版本
5. 输入Project Metadata
    - Group: 制品包的组，对应POM文件中的`groupId`
    - Artifact：制品包名，对应POM文件中的`artifactId`
    - Name: 项目名称，对应POM文件中的`name`
    - Description: 项目描述，对应POM文件中的`description`
6. 添加Dependencies
7. 点击Generate按钮生成并下载项目文件zip包
8. 将项目文件zip包放到指定位置并解压
9. 在IDE中导入或打开该项目

## 运行Spring Boot应用

### 以Maven方式运行

```bash
mvn clean install
mvn spring-boot:run
```

或在IDE的Maven面板上操作。

### 以IDE方式运行

在IDE中找到Spring Boot Application类，运行该类的`main`方法。


## References

- [Spring Boot Getting Started](https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html)

## Troubleshooting

### 安装JDK17
如果遇到JDK版本的问题，可以[使用SDKMAN安装和管理多个版本的JDK](./install_jdk.md)。

重启IDE，添加新安装的JDK。

### Maven无法编译JDK17

[用SDK将当前JDK设置为JDK 17](./install_jdk.md)

检查Maven使用的JDK版本：
```bash
mvn --version
```

如果没有使用SDKMAN，可以参考下面的文章：
- [Maven error – invalid target release: 17](https://mkyong.com/maven/maven-error-invalid-target-release-17/)


