# 100 - 【Lab】將 Spring Boot 專案部上容器運行

#### Step 1
隨便寫個簡單的 Spring Boot 專案並用 Maven 打包，如何打包可以參考下面網址。
* https://dotblogs.com.tw/zjh/2018/09/20/maven_3
* https://www.baeldung.com/spring-boot-run-maven-vs-executable-jar

#### Step 2
接下來建立 Dockerfile。
```docker
FROM openjdk:11-jdk-alpine
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar # 書
ENTRYPOINT ["java","-jar","/app.jar"]
```

`ENTRYPOINT` 的指令相當於：
```
java -jar target/helloSpringBoot-0.0.1-SNAPSHOT.jar
```

## 參考
* https://github.com/spring-guides/gs-spring-boot-docker#initial