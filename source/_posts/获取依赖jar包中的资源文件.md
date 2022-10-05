---
title: 获取依赖jar包中的资源文件
date: 2022-10-05 18:24:25
updated: 2022-10-05 18:24:30
tags:
- Java
---

最近在公司的项目中所有业务逻辑都写在单独的一个项目中输出为Jar包在框架项目中引用就可以了。

在编写业务逻辑的过程中，需要使用freemarker生成固定格式的字符串，那么就需要将模板存放在`resources`下，
这里就会产生一个疑问，框架项目（war部署）怎么样获取到依赖Jar包中的资源文件。

首先创建一个maven项目**jar-resource**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>fun.lovexy</groupId>
    <artifactId>jar-resource</artifactId>
    <version>1.0-SNAPSHOT</version>

    <packaging>jar</packaging>
</project>
```

在resources文件夹中添加tpl目录并创建一个任意内容的文本文件。

创建一个maven项目**web-war**，并引入**jar-resource**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>fun.lovexy</groupId>
    <artifactId>web-war</artifactId>
    <version>1.0-SNAPSHOT</version>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.2</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.11.0</version>
        </dependency>
        <dependency>
            <groupId>fun.lovexy</groupId>
            <artifactId>jar-resource</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
    <packaging>war</packaging>
</project>
```

编写一个资源文件获取的测试接口：

```java
@RequestMapping("/")
public void getFile(@RequestParam("r") String r, HttpServletResponse response) throws IOException {
    ClassLoader classLoader = ResourceController.class.getClassLoader();
    InputStream inputStream = classLoader.getResourceAsStream(r);
    response.setHeader("Content-Type", "text/plain");
    OutputStream outputStream = response.getOutputStream();
    IOUtils.copy(inputStream, outputStream);
}
```

main方法启动或者Tomcat启动，接着访问我们的测试地址`http://localhost:9090/web_war/?r=tpl/a.xml`

得到我们预先在资源文里的内容：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<site>
    <name>a</name>
</site>
```