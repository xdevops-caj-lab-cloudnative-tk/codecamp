# 使用SDKMAN安装和管理JDK


## Install JDK

```bash
# list avaiable jdk
sdk list java

# install jdk 17
# From SDKMAN, choose the AdoptOpenJDK JDK which is now provided by Adoptium and is called Eclipse Temurin. Install the latest JDK 17 LTS version
sdk install java 17.0.6-tem
```

参考：
- [How to Manage Your JDKs With SDKMAN](https://mydeveloperplanet.com/2022/04/05/how-to-manage-your-jdks-with-sdkman/)

## Switch JDK

```bash
# list curent used jdk
sdk current java

# swith to java 17
# after restart terminal, the settings is resumed
sdk use java 17.0.6-tem

# set java 17 as default
sdk default java 17.0.6-tem
```