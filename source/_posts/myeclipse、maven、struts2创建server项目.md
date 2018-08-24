---
title: myeclipse、maven、struts2、Jetty创建mobile-server项目，为移动端开发提供后台服务。
date: 2018-08-04 23:41:56
tags: [myeclipse,Maven,struts2,Jetty,HTTPS, SSL, TLS, Http2,mobile-server,server]
summery: 在myeclipse中使用Group Id = org.apache.maven.archetypes Artifact Id = maven-archetype-webapp创建简单的webapp项目，添加Servlet支持，添加struts2支持，添加jetty plugin支持，一步步搭建简单的Struts服务器端项目，供以后学习使用。例如：OkHttp
---
### 一、参考文档及源码下载

[maven webapp目录结构]

[Maven权威指南笔记]

源代码下载地址：[mobile-server]

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

### 九、添加gson支持

为了方便的构建JSON字符串，模拟接口请求返回，引入Gson。

```
<dependency>
  <groupId>com.google.code.gson</groupId>
  <artifactId>gson</artifactId>
  <version>2.8.5</version>
</dependency>
```

### 十、根据maven webapp目录结构，补全必要的目录

创建相关代码资源目录和package

```
src/test/java
src/test/resource
src/main/webapp/resources

com.farubaba.mobile.server.action
com.farubaba.mobile.server.service
com.farubaba.mobile.server.dao
com.farubaba.mobile.server.modle
```

### 十一、编写login.jsp、LoginAction、LoginService、LoginDao并配置

1. pom.xml添加jstl支持

```
<dependency>
    <groupId>jstl</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
```
1. index.jsp中添加访问login.action的超链接

```html
 <ol>
    <li>通过 login.action 访问 (没有给userName和password参数) : <a href="<s:url action='login'/>">[mobile-server/WEB-INF/resources/jsp/login.jsp]</a></li>
 </ol>
    
```
2. 在login.jsp中使用带参数的login.action

```
<!DOCTYPE html>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@ page import="com.google.gson.GsonBuilder"%>
<%@ page import="com.google.gson.Gson" %>
<%@ page import="com.farubaba.mobile.server.model.User" %>
<%@ taglib prefix="s" uri="/struts-tags" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>


<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
String pageName = pageContext.getPage().getClass().getSimpleName();
%>

<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Hello World! Struts2</title>
    <style type="text/css">
    	.button{
    		width:300px;
    		height:30px;
    		text-size:26pt;
    		text-align:center;
    		background-color:#2bbc8a;
    	}
    </style>
  </head>
  <body>  	
  	<h2>
  		<%= pageName.replace("_002d", "-").replace("_", ".") %> :		
  	</h2>
  	<h4>查看地址栏Url，如果没有给userName和password参数，则并不会返回User对象，下面也不会有值显示：</h4>
  	<h4>JSON String User: [jsonUser] = <s:property value="jsonUser" /></h4>
  	<h4>Object User: [user] = <s:property value="user" /></h4>
  	<p>getServletName = <%= getServletName() %></p>
  	<p>getServletInfo = <%= getServletInfo() %></p>
  	<p>getServletContext.getAttribute() = <%= getServletContext().getAttribute("jsonUser") %></p> 
    <!--
    定义url action 和 param 
     -->
     <s:url var="loginWithNoParam" action="login"></s:url>
   	
   	<s:url var="login" action="login">
   		<s:param name="userName">farubaba</s:param>
   		<s:param name="password">123456</s:param>
   	</s:url>
   	 
   	<s:url var="userlogin" action="userLogin" >
   		<s:param name="userName">farubaba</s:param>
   		<s:param name="password">123456</s:param>
   	</s:url>
   	
   	<s:url var="userlogin" action="userLogin" >
   		<s:param name="userName">farubaba</s:param>
   		<s:param name="password">123456</s:param>
   	</s:url>
   	
    <ul>
    	<li>没有登陆参数，调用 <a href="${loginWithNoParam}">login.action -> return null String</a></li>
    	<li>提供正确登录参数，调用 <a href="${login}">login.action -> return JSON String User</a></li>
    	<li>提供正确登录参数，调用<a href="${userlogin}">userLogin.action -> return Object User </a></li>
    	<li>
    	使用form表单提交登录：(输入：<font color="red">farubaba</font> 和 <font color="red">123456</font> 可登录成功!登录成功后，JSON String User 和 Object User其中之一会有值)
	    <s:form action="login">
		 	<s:textfield name="userName" label="Your name" />
		 	<s:textfield name="password" label="Your password" />
		 	<s:submit value="登录" cssClass="button" />
		</s:form>
    	</li>
    </ul>
    <p><p><p><s:include value="../../road-map.jsp"></s:include>
   
  </body>
</html>
```
### 十二、服务器端编程细节摘要

|功能|解决方案|
|----|----|
|读取request中postbody|1.使用request.getInputStream()|
|struts action中读写文件|1.获得文件路径：String path = ServletActionContext.getServletContext().getRealPath("/resources/files/test.md");<br>生成绝对路径如：/environment/git-repo/mobile-server/src/main/webapp/resources/files/test.md|

### 十三、编写服务端Action、Service、Dao，完整的代码已上传至github [mobile-server]


[maven webapp目录结构]:https://www.cnblogs.com/plxx/p/5256979.html
[Maven权威指南笔记]:Maven权威指南笔记.md
[mobile-server]:https://github.com/Farubaba/mobile-server.git



