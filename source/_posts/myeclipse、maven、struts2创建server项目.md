---
title: myeclipse、maven、struts2、Jetty创建server项目，为以后的学习提供后台服务。
date: 2018-08-04 23:41:56
tags: [myeclipse,Maven,struts2,Jetty]
summery: 在myeclipse中使用Group Id = org.apache.maven.archetypes Artifact Id = maven-archetype-webapp创建简单的webapp项目，添加Servlet支持，添加struts2支持，添加jetty plugin支持，一步步搭建简单的Struts服务器端项目，供以后学习使用。例如：OkHttp
---
### 一、参考文档

[maven webapp目录结构]

[Maven权威指南笔记]


[maven webapp目录结构]:https://www.cnblogs.com/plxx/p/5256979.html
[Maven权威指南笔记]:Maven权威指南笔记.md

### 二、maven webapp目录结构
```
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── mygroup
    │   │       ├── controller
    │   │       │   ├── HomeController.java
    │   │       │   └── PersonController.java
    │   │       ├── dao
    │   │       │   └── PersonDao.java
    │   │       └── model
    │   │           └── Person.java
    │   ├── resources
    │   │   ├── db.properties
    │   │   ├── log4j.xml
    │   │   ├── struts.xml
    │   │   └── META-INF
    │   │       └── persistence.xml
    │   └── webapp
    │       ├── index.jsp
    │       ├── META-INF
    │       │   ├── context.xml
    │       │   └── MANIFEST.MF
    │       ├── resources
    │       │   └── css
    │       │       └── screen.css
    │       └── WEB-INF
    │           ├── spring
    │           │   ├── app
    │           │   │   ├── controllers.xml
    │           │   │   └── servlet-context.xml
    │           │   ├── db.xml
    │           │   └── root-context.xml
    │           ├── views
    │           │   ├── edit.jsp
    │           │   ├── home.jsp
    │           │   └── list.jsp
    │           └── web.xml
    └── test
        ├── java
        │   └── mygroup
        │       ├── controller
        │       │   ├── DataInitializer.java
        │       │   ├── HomeControllerTest.java
        │       │   └── PersonControllerTest.java
        │       └── dao
        │           └── PersonDaoTest.java
        └── resources
            ├── db.properties
            ├── log4j.xml
            ├── test-context.xml
            └── test-db.xml
```

### 三、Eclipse中使用maven-archetype-webapp创建maven web项目

>File-->New-->Project-->maven Project-->选择maven-archetype-webapp-->填写Group Id、Artifact Id、packageName等信息后-->完成

### 四、定义webapp的contextPath

在pom.xml文件中<build>下增加如下内容,其中mobile-server就是我们定义的contextPath

```
<finalName>mobile-server</finalName>
```

### 五、添加Servlet支持

pom.xml文件中添加：

```
<dependency>
  <groupId>javax</groupId>
  <artifactId>javaee-api</artifactId>
  <version>7.0</version>
  <scope>provided</scope>
</dependency>
```

### 六、修改index.jsp

在src/main/webapp下添加/修改 index.jsp，内容如下：

```
<!DOCTYPE html>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Basic Struts 2 Application - Welcome</title>
  </head>
  <body>
    <h1>Welcome To Struts 2!</h1>
  </body>
</html>
```

### 七、添加Jetty maven plugin
pom.xml文件中添加如下内容：

```
<plugin>
    <groupId>org.eclipse.jetty</groupId>
    <artifactId>jetty-maven-plugin</artifactId>
    <version>9.4.7.v20170914</version>
    <configuration>
        <webApp>
            <contextPath>/${project.build.finalName}</contextPath>
        </webApp>
        <stopKey>CTRL+C</stopKey>
        <stopPort>8999</stopPort>
        <scanIntervalSeconds>10</scanIntervalSeconds>
        <scanTargets>
            <scanTarget>src/main/webapp/WEB-INF/web.xml</scanTarget>
        </scanTargets>
    </configuration>
</plugin>
```
运行 mvn jetty:run，然后输入: <http://localhost:8080/mobile-server/index.jsp> 访问页面,页面显示：Welcome To Struts 2! 则配置成功。

### 八、添加Struts2和Log4j支持

1. 在\<properties\>下添加struts2和log4j的版本属性

```
<properties>
	<struts2.version>2.5.14.1</struts2.version>
	<log4j2.version>2.10.0</log4j2.version>
</properties>
```

2. 在src/main/resources下创建log4j2.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
            <PatternLayout pattern="%d %-5p [%t] %C{2} (%F:%L) - %m%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.opensymphony.xwork2" level="debug"/>
        <Logger name="org.apache.struts2" level="debug"/>
        <Root level="warn">
            <AppenderRef ref="STDOUT"/>
        </Root>
    </Loggers>
</Configuration>
```

3. 配置dependencies

```
</dependencies>
	<dependency>
	    <groupId>org.apache.struts</groupId>
	    <artifactId>struts2-core</artifactId>
	    <!-- <version>${struts2.version}</version> -->
	</dependency>
	<dependency>
	    <groupId>org.apache.logging.log4j</groupId>
	    <artifactId>log4j-core</artifactId>
	     <!--<version>${log4j2.version}</version> -->
	</dependency>
	<dependency>
	    <groupId>org.apache.logging.log4j</groupId>
	    <artifactId>log4j-api</artifactId>
	     <!-- <version>${log4j2.version}</version> -->
	</dependency>
</dependencies>
```

4. 增加dependencyManagement配置

```
<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-bom</artifactId>
			<version>${struts2.version}</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-bom</artifactId>
			<version>${log4j2.version}</version>
			<scope>import</scope>
			<type>pom</type>
		</dependency>
	</dependencies>
</dependencyManagement>
```

5. 修改web.xml文件

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app id="WebApp_ID" version="2.4"
	xmlns="http://java.sun.com/xml/ns/j2ee" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
	<display-name>Basic Struts2</display-name>
	<welcome-file-list>
		<welcome-file>index</welcome-file>
	</welcome-file-list>

	<filter>
		<filter-name>struts2</filter-name>
		<filter-class>org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter</filter-class>
	</filter>

	<filter-mapping>
		<filter-name>struts2</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

</web-app>
```

6. 在src/main/resources下创建struts.xml文件

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
    "http://struts.apache.org/dtds/struts-2.5.dtd">

<struts>

    <constant name="struts.devMode" value="true" />

    <package name="basicstruts2" extends="struts-default">
        <action name="index">
            <result>/index.jsp</result>
        </action>
    </package>

</struts>
```
7. 执行 mvn jetty:run 然后访问<http://localhost:8080/mobile-server/index.action>出现和之前<http://localhost:8080/mobile-server/index.jsp>一样的界面，则说明struts2配置成功。

### 九、根据maven webapp目录结构，补全必要的目录
