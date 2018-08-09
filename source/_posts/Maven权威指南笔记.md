---
title: Maven权威指南速查笔记
date: 2018-08-03 18:34:26
tags: [OkHttp,Maven,struts]
summery: 本文是《Maven权威指南》的书读笔记，抽取出maven最关键的基础知识，帮助初学者快速入门。了解maven是什么，maven能做什么，maven是如何工作的，以及我们在开发中如何利用maven来管理我们的项目。也是一个maven核心备忘录。
---
### 参考文档

>1. [Linux命令大全]
>1. Maven权威指南
>1. [Maven plugins列表]
>1. [Maven常用插件]

### Maven关键概念汇总

>1. 插件 Maven Plugin
>1. 目标 goal 和 目标前缀 goal Prefix
>1. 坐标(coordinates) 由 groupId，artifactId, packaging, version 共同确定
>1. 构建生命周期 build Lifecycle
>1. 生命周期阶段 phase
>1. 标准目录结构 Maven Standard Directory Layout
>1. 项目对象模型 Project Object Model
>1. 约定优于配置 Convention Over Configuration
>1. Maven仓库 (repositories)
>1. 依赖管理 (dependency management)
>1. 使用maven help插件的mvn help:describe来查看其它插件、目标或者命令的描述：
>>1. mvn help:describe -Dcmd=install
>>2. mvn help:describe -Dcmd=help:describe
>>3. mvn help:describe -Dplugin=compiler   <font color="#DC524A">compiler是一个插件</font>
>>4. mvn help:describe -Dcmd=compile  <font color="#DC524A">compile是compiler插件的一个目标</font>
>>5. mvn help:describe -Dplugin=comp iler -Dmojo=compile -Dfull
>>6. mvn help:describe -Dplugin=help 
>>7. mvn help:describe -Dplugin=org.apache.maven.plugins:maven-help-plugin
>>8. mvn help:describe -DgroupId=org.apache.maven.plugins -DartifactId=maven-help-plugin
>>9. 查看help插件的描述: mvn help:describe -Dplugin=help
>>10. 查看更详细的细节+1: mvn help:describe -Dplugin=help -Dfull
>>11. 查看更详细的细节+2: mvn help:help -Ddetail=true
>>12. 查看更详细的细节+2: mvn help:describe -Dplugin=help -Ddetail=true
>>13. Dmojo指定插件目标：mvn help:describe -Dplugin=compiler -Dmojo=compile -Dfull
>1. "-D<name>=<value>"这种格式不是Maven定义的，它其实是Java用来设置系统属性的方式，可以通过“java -help”查看 Java的解释。Maven的bin目录下的脚本文件仅仅是把属性传入Java而已
>1. 传递性依赖(transitive dependencies)
>1. 依赖范围(dependency scope) : test、compile、provided
>1. 站点生成和报告 (Site Generation and Reporting) : mvn site
>1. Maven Assembly 是一个用来创建你应用程序特有分发包的插件。

### POM.xml标准标签格式, Profile中也可以包含全部这些标签

```
	<project>
		<build>
			<defaultGoal>...</defaultGoal>
			<finalName>...</finalName>
			<resources>...</resources>
			<testResources>...</testResources>
			<plugins>...</plugins>
		</build>
		<reporting>...</reporting>
		<modules>...</modules>
		<dependencies>...</dependencies>
		<dependencyManagement>...</dependencyManagement>
		<distributionManagement>...</distributionManagement>
		<repositories>...</repositories>
		<pluginRepositories>...</pluginRepositories>
		<properties>...</properties>
	</project>
```
		
### 相关名称总汇：
	
```
	IOC容器：Plexus、Guice、Spring
	Struts、Tapesty、Wicket、JSF、Waffle、Hibernate、Velocity
```
		
### 第一章 介绍Apache Maven

### 1.1 Maven是什么？
Maven是一个构建工具，一个项目管理工具。

像Ant这样的构建工具只关注预处理，编译，打包，测试和分发。Maven不仅关注预处理，编译，打包，测试和分发，还可以生成报告， 生成Web站点，并且帮助推动工作团队成员间的交流。
	
一个更正式的 Apache Maven1 的定义: Maven是一个项目管理工具，它包含了一个项 目对象模型 (Project Object Model)，一组标准集合，一个项目生命周期(Project Lifecycle)，一个依赖管理系统(Dependency Management System)，和用来运行定义在 生命周期阶段(phase)中插件(plugin)目标(goal)的逻辑。 当你使用Maven的时候，你 用一个明确定义的项目对象模型来描述你的项目，然后 Maven 可以应用横切的逻辑， 这些逻辑来自一组共享的(或者自定义的)插件		

### 1.2 约定优于配置(Convention Over Configuration)
在没有自定义的情况下，Maven有如下约定：

1. 源代码假定是在:
```
/usr/local/hudson/hudson-home/jobs/maven-guide-zh-to-production/workspace/content-zh/src/main/java
```
1. 资源文件假定是在:
```
/usr/local/hudson/hudson-home/jobs/maven-guide-zh-to-production/workspace/content-zh/src/main/resources
```
1. 测试代码假定是在: 
```
/usr/local/hudson/hudson-home/jobs/maven-guide-zh-to-production/workspace/content-zh/src/test
```
1. 项目假定会产生一个 JAR 文件
```
/usr/local/hudson/ hudson-home/jobs/maven-guide-zh-to-production/workspace/content-zh/target
```
2. Maven 假定你想要把编译好的字节码放到:
```
/usr/local/hudson/hudson-home/jobs/maven-guide-zh-to-production/workspace/content-zh/target/classes
```
1. 当然，只要你遵循Maven的这一约定，几乎不需要做任何配置，就可以结合maven的插件来完成所有事情，但也仍然允许你自定义修改这些约定。

1. Maven的标准约定的目录结构如下：

	<font color="red">在一个Maven项目中执行 mvn help:effective-pom 可以查看到这些约定被配置在pom.xml中</font>
	```
	--projectName 						项目根目录或者module根目录
	-----src
	---------main
	-------------java					源码目录src/main/java
	-------------resources					资源目录src/main/resources
	-------------scripts					脚本文件src/main/scripts
	---------test 						测试目录src/test
	-------------java 					测试代码目录src/test/java
	-------------resources					测试资源目录src/test/resources
	---------site     					站点资源目录src/site
	-----target						结果目录
	---------classes 					编译生成class目录target/classes
	---------test-classes 					编译生成class目录target/test-classes
	---------site     					站点资源目录target/site
	-----pom.xml  						Maven配置文件
	```
1. 多module项目结构如下：

	```
	--projectName 						项目根目录
		----module1					module1根目录
			------src	 	
			---------main
			-------------java			源码目录
			-------------resources			资源目录
			-------------scripts			脚本文件
			---------test 				测试目录
			-------------java 			测试代码目录
			-------------resources			测试资源目录
			---------site     			站点资源目录
			-----target				结果目录
			---------classes 			编译生成class目录
			---------test-classes 			编译生成class目录
			---------site     			站点资源目录
			-----pom.xml  				module1的Maven配置文件
		----module2					module2根目录
			------src	 	
			---------main
			-------------java			源码目录
			-------------resources			资源目录
			-------------scripts			脚本文件
			---------test 				测试目录
			-------------java 			测试代码目录
			-------------resources			测试资源目录
			---------site     			站点资源目录
			-----target				结果目录
			---------classes 			编译生成class目录
			---------test-classes 			编译生成class目录
			---------site     			站点资源目录
			-----pom.xml  		  		module2的Maven配置文件
	   ----pom.xml   					Project的Maven配置文件
	```
1. 典型示例如：
	
	下面是我从[okhttp官方github]中fork出来准备二次封装okhttp的项目结构:
	<!--
	![Maven项目目录结构.png](https://app.yinxiang.com/shard/s45/nl/2147483647/50a6fa2c-b7ed-41f6-9702-72ae2ef0b930//res/ae2bc4cb-e0b3-47d6-bbab-fe8ac6a9f244/skitch.png?resizeSmall&width=832)
	-->
	<!--
	![maven项目目录结构.png](https://upload-images.jianshu.io/upload_images/6322932-55587e4642649287.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	-->
	![Maven项目目录结构.png](https://upload-images.jianshu.io/upload_images/6322932-3877cf45ed59a3f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
	
### 1.3 统一的构建过程

>统一的标准化构建过程约束，所有使用Maven构建的项目都遵循这一约束，不同的项目，只要都使用Maven来构建，那么，他们的构建逻辑几乎是一样的，不会再像没有Maven的年代，每个项目的构建逻辑都不一致，开发人员需要花额外的时间去理解不同项目不同的构建逻辑。例如：使用Ant构建的每一个项目几乎都拥有自己完全不同与其他项目的构建逻辑，因为，没有约定，构建逻辑完全是自定义的。

<font color="red">现在，你只要签出源码，然后运行: mvn install，你就能构建好这个项目。
</font>

### 1.4 Maven插件的全局性重用

Maven 的核心其实不做什么实际的事情，除了解析一些 XML 文档，管理生命周期与插 件之外，它什么也不懂。Maven 被设计成将主要的职责委派给一组 Maven 插件，这些 插件可以影响 Maven 生命周期，提供对目标的访问。绝大多数 Maven 的动作发生于 Maven 插件的目标，如编译源码，打包二进制代码，发布站点和其它构建任务。你从 Apache 下载的 Maven 不知道如何打包 WAR 文件，也不知道如何运行单元测试，Maven 大部分的智能是由插件实现的，而插件从 Maven 仓库获得。事实上，第一次你用全新 的 Maven 安装运行诸如 mvn install 命令的时候，它会从中央 Maven 仓库下载大部 分核心 Maven 插件。这不仅仅是一个最小化 Maven 分发包大小的技巧，这种方式更能 让你升级插件以给你项目的构建提高能力。Maven 从远程仓库获取依赖和插件的这一事 实允许了构建逻辑的全局性重用。

Surefire插件：是负责运行单元测试的插件，插件不断更新，支持JUnit3、TestNG、JUnit4；

Compiler插件：负责进行编译，通过jar插件编程jar，通过aar插件生成aar。

还有一些插件生成报告，运行 JRuby 和 Groovy 的代码，以及一 些用来向远程服务器发布站点的插件。而且插件在不断更新，我们只需要更新pom.xml中说引入的插件版本号，就能获得最新的插件功能。

Maven 将一般的构建任务抽象成插件，同时这些 插件得到了很好的维护以及全局的共享，你不需要从头开始自定义你项目的构建系统然 后提供支持。你完全可以从 Maven 插件获益，这些插件有人维护，可以从远程仓库下 载到。这就是基于 Maven 插件的全局性重用。

### 1.5 Maven、Ant、Gradle、Buildr、Nexus以及antlibs和Ivy

<font color="red">我们会很明确的说作为构建的基本技术，Maven 是 Ant 的更好选择。但使用 Maven 还是 Ant 的决定不是非此即彼的，Ant 在复杂的构建中还有它的位置。</font> 如果你目前的构建包含一些高度自定义的过程，或者你已经写了一些 Ant 脚本通过一 种明确的方法完成一个明确的过程，而这种过程不适合 Maven 标准，你仍然可以在 Maven 中用这些脚本。作为一个 Maven 的核心插件， Ant 还是可用的。自定义的插件 可以用 Ant 来实现，Maven 项目可以配置成在生命周期中运行 Ant 的脚本。

Ant 在构建过程方面十分优秀，它是一个<font color="red"><b>基于任务和依赖</b></font>的构建系统。每个任务包含一 组由 XML 编码的指令。有 copy 任务和 javac 任务，以及 jar 任务。在你使用 Ant 的时候，你为 Ant 提供特定的指令以编译和打包你的输出。
没有诸如 antlibs 和 lvy 等技术的支持(<font color="red">即使有了这些支持技术</font>)，Ant 给人感觉是自定义的程序化构建,配置过于复杂。

一个典型的Ant构建脚本buid.xml：

```
<project name="my-project" default="dist" basedir="."> 
	<description>
		simple example build file 
	</description>
	<!-- set global properties for this build --> 
	<property name="src" location="src/main/java"/> 
	<property name="build" location="target/classes"/> 
	<property name="dist" location="target"/>
	
	<target name="init">
		<!-- Create the time stamp -->
		<tstamp/>
		<!-- Create the build directory structure used by compile --> 
		<mkdir dir="org.apache.maven.model.Build@d7e661"/>
	</target>
	
	<target name="compile" depends="init" description="compile the source " >
		<!-- Compile the java code from ${src} into org.apache.maven.model.Build@d7e661 --> 
		<javac srcdir="${src}" 	destdir="org.apache.maven.model.Build@d7e661"/> 
	</target>
	
	<target name="dist" depends="compile" description="generate the distribution" >
		<!-- Create the distribution directory --> 
		<mkdir dir="${dist}/lib"/>
	<!-- Put everything in org.apache.maven.model.Build@d7e661 into the MyProject-${DSTAMP}.jar -->
		<jar jarfile="${dist}/lib/MyProject-${DSTAMP}.jar" basedir="org.apache.maven.mod 
	</target>
		
	<target name="clean" description="clean up" >
		<!-- Delete the org.apache.maven.model.Build@d7e661 and ${dist} directory trees -->
		<delete dir="org.apache.maven.model.Build@d7e661"/>
		<delete dir="${dist}"/>
	</target>
</project>
```
等价的Maven配置文件pom.xml

```
<project> 
	<modelVersion>4.0.0</modelVersion>
	<groupId>org.sonatype.mavenbook</groupId> 
	<artifactId>my-project</artifactId> 
	<version>1.0</version>
	<!-- packaging 默认就是jar类型-->
	<packaging>jar</packaging>
</project>
```

### 1.6 Maven和Ant比较

|类型|Apache Maven|Apache Ant|
|----|----|----|
|约定|Maven 拥有约定，因为你遵循了约定，它已经知道你的源代码在哪里。它把字节码放到 target/classes ，然后在 target 生成一个 JAR 文件|Ant 没有正式的约定如一个一般项目的目录结构，你必须明确的告诉 Ant 哪 里去找源代码，哪里放置输出。随着时间的推移，非正式的约定出现了，但是 它们还没有在产品中模式化|
|声明和编写|Maven 是声明式的。你需要做的只是创建一个 pom.xml 文件然后将源代码放到默认的目录。Maven 会帮你处理其它的事情|Ant 是程序化的，你必须明确的告诉 Ant 做什么，什么时候做。你必须告诉 它去编译，然后复制，然后压缩|
|生命周期|Maven 有一个生命周期，当你运行 mvn install 的时候被调用。这条命令告 诉 Maven 执行一系列的有序的步骤，直到到达你指定的生命周期。遍历生命 周期旅途中的一个影响就是，Maven 运行了许多默认的插件目标，这些目标完 成了像编译和创建一个 JAR 文件这样的工作|Ant 没有生命周期，你必须定义目标和目标之间的依赖。你必须手工为每个目 标附上一个任务序列|



### 第二章 安装和运行Maven

### 2.1 Maven安装和升级
	
		1. 执行 java -version 如果没有输出类似如下内容，则需要下载和安装JDK
			java version "1.6.0_02"
			Java(TM) SE Runtime Environment (build 1.6.0_02-b06)
			Java HotSpot(TM) Client VM (build 1.6.0_02-b06, mixed mode, sharing)
		2. JDK下载地址：
			http://www.oracle.com/technetwork/java/javase/downloads/index.html 
		3. Maven下载地址：
			https://maven.apache.org/download.cgi
		4. 安装Maven
			https://maven.apache.org/install.html
		5. 升级Maven
			只需要删除已经安装的Maven目录，并重新下载，解压缩到相同的目录既可以。
	
### 2.2 Maven全局目录结构
	
		LICENSE.txt	包含了Apache Maven的软件许可证
		NOTICE.txt 	包含了一些Maven依赖的类库所需要 的通告及权限
		README.txt 	包含了一些安装指令
		bin/   		包含了运行Maven的 mvn脚本
		boot/  		包含了一个负责创建Maven运行所需要的类装载器的JAR文件(classwords-1.1.jar)
		conf/  		包含了一个全局的settings.xml文件，该文件用 来自定义你机器上Maven的一些行为。
					但是如果你需要自定义Maven，更通常的做法是覆写 ~/.m2目录下的settings.xml文件，每个用户都有对应的这个目录。
		lib/   		有了一个包含Maven核心的JAR文件(maven-2.0.9-uber.jar)

### 2.3 用户相关配置和仓库
	
		~/.m2/settings.xml	该文件包含了用户相关的认证，仓库和其它信息的配置，用来自定义Maven的行为
		~/.m2/repository/ 	该目录是你本地的仓库。当你从远程Maven仓库下载依赖的时候，Maven在你本地仓库存储了这个依赖的一个副本。
		
### 2.4 如何获取Maven帮助

|地址|描述|
|---|----|
|<http://maven.apache.org>|你首先应该看看这里，Maven的web站点包含了丰富的信息及文档。每个插件都有 几页的文档，这里还有一系列“快速开始”的文档，它们是本书内容的十分有帮 助的补充。虽然Maven站点包含了大量信息，它同时也可能让你迷惑沮丧。那里 提供了一个自定义的Google搜索框，以用来搜索已有的Maven站点信息，它能比 通用的Google搜索提供更好的结果。|
|Maven User Mailing List|Maven用户邮件列表是用户问问题的地方。在你问问题之前，你可以先搜索一下 之前的讨论，有可能找到相关问题。问一个已经问过的问题，而不先查一下该问 题是否存在了，这种形式不太好。有很多有用的邮件列表归档浏览器，我们发现 Nabble最有用。你可以在这里浏览邮件列表:http://www.nabble.com/MavenUsers-f178.html。你也可以按照这里的指令来加入用户邮件列表:http:// maven.apache.org/mail-lists.html|
|http://www.sonatype.com|Sonatype维护了一个本书的在线副本，以及其它Maven相关的指南|
|<https://maven.apache.org/team-list.html>|Maven开发者邮件列表|

### 2.5 使用Maven help插件

Maven Help 插件有很多目标。通过help插件自己的describe目标可以查看help插件自身的描述。

mvn help:describe -Dplugin=help

|目标|描述|
|----|----|
|help:active-profiles|列出当前构建中活动的Profile(项目的，用户的，全局的)|
|help:all-profiles|Displays a list of available profiles under the current project. Note: it will list all profiles for a project. If a profile comes up with a status inactive then there might be a need to set profile activation switches/property|
|help:effective-pom|显示当前构建的实际POM，包含活动的Profile|
|help:effective-settings|打印出项目的实际settings, 包括从全局的settings和用户级别settings继承的配置|
|help:describe|描述插件的属性。它不需要在项目目录下运行。但是你必须提供你想要描述插件 的 groupId 和 artifactId|
|help:evaluate|Evaluates Maven expressions given by the user in an interactive mode.|
|help:help|Description: Display help information on maven-help-plugin. Call mvn help:help -Ddetail=true -Dgoal=<goal-name> to display parameter details|
|help:system|Displays a list of the platform details like system properties and environment variables.|


>1. 使用Maven help插件的mvn help:describe来查看其它插件、目标或者命令的描述：
>9. mvn help:describe -Dcmd=install
>10. mvn help:describe -Dcmd=help:describe
>11. mvn help:describe -Dplugin=compiler   compiler是一个插件
>12. mvn help:describe -Dcmd=compile       compile是compiler插件的一个目标
>13. mvn help:describe -Dplugin=compiler -Dmojo=compile -Dfull
>14. mvn help:describe -Dplugin=help
>15. mvn help:describe -Dplugin=org.apache.maven.plugins:maven-help-plugin
>16. mvn help:describe -DgroupId=org.apache.maven.plugins -DartifactId=maven-help-plugin
>17. 查看help插件的描述: mvn help:describe -Dplugin=help
>18. 查看更详细的细节+1: mvn help:describe -Dplugin=help -Dfull
>19. 查看更详细的细节+2: mvn help:help -Ddetail=true
>20. 查看更详细的细节+2: mvn help:describe -Dplugin=help -Ddetail=true
>1. Dmojo指定插件目标：mvn help:describe -Dplugin=compiler -Dmojo=compile -Dfull

1. 在maven-archetype-quickstart项目中执行 mvn help:effective-pom 可以查看到实际pom中内容如下：

```
		<?xml version="1.0" encoding="UTF-8"?>
		<!-- ====================================================================== -->
		<!--                                                                        -->
		<!-- Generated by Maven Help Plugin on 2018-05-16T23:17:54+08:00            -->
		<!-- See: http://maven.apache.org/plugins/maven-help-plugin/                -->
		<!--                                                                        -->
		<!-- ====================================================================== -->
		<!-- ====================================================================== -->
		<!--                                                                        -->
		<!-- Effective POM for project                                              -->
		<!-- 'com.silence:maven-archetype-quickstart:jar:0.0.1-SNAPSHOT'            -->
		<!--                                                                        -->
		<!-- ====================================================================== -->
		<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
		  <modelVersion>4.0.0</modelVersion>
		  <groupId>com.silence</groupId>
		  <artifactId>maven-archetype-quickstart</artifactId>
		  <version>0.0.1-SNAPSHOT</version>
		  <name>maven-archetype-quickstart</name>
		  <url>http://www.example.com</url>
		  <properties>
		    <maven.compiler.source>1.7</maven.compiler.source>
		    <maven.compiler.target>1.7</maven.compiler.target>
		    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		  </properties>
		  <dependencies>
		    <dependency>
		      <groupId>junit</groupId>
		      <artifactId>junit</artifactId>
		      <version>4.11</version>
		      <scope>test</scope>
		    </dependency>
		  </dependencies>
		  <repositories>
		    <repository>
		      <snapshots>
		        <enabled>false</enabled>
		      </snapshots>
		      <id>central</id>
		      <name>Central Repository</name>
		      <url>https://repo.maven.apache.org/maven2</url>
		    </repository>
		  </repositories>
		  <pluginRepositories>
		    <pluginRepository>
		      <releases>
		        <updatePolicy>never</updatePolicy>
		      </releases>
		      <snapshots>
		        <enabled>false</enabled>
		      </snapshots>
		      <id>central</id>
		      <name>Central Repository</name>
		      <url>https://repo.maven.apache.org/maven2</url>
		    </pluginRepository>
		  </pluginRepositories>
		  <build>
		    <sourceDirectory>/environment/myeclipse/maven-archetype-quickstart/src/main/java</sourceDirectory>
		    <scriptSourceDirectory>/environment/myeclipse/maven-archetype-quickstart/src/main/scripts</scriptSourceDirectory>
		    <testSourceDirectory>/environment/myeclipse/maven-archetype-quickstart/src/test/java</testSourceDirectory>
		    <outputDirectory>/environment/myeclipse/maven-archetype-quickstart/target/classes</outputDirectory>
		    <testOutputDirectory>/environment/myeclipse/maven-archetype-quickstart/target/test-classes</testOutputDirectory>
		    <resources>
		      <resource>
		        <directory>/environment/myeclipse/maven-archetype-quickstart/src/main/resources</directory>
		      </resource>
		    </resources>
		    <testResources>
		      <testResource>
		        <directory>/environment/myeclipse/maven-archetype-quickstart/src/test/resources</directory>
		      </testResource>
		    </testResources>
		    <directory>/environment/myeclipse/maven-archetype-quickstart/target</directory>
		    <finalName>maven-archetype-quickstart-0.0.1-SNAPSHOT</finalName>
		    <pluginManagement>
		      <plugins>
		        <plugin>
		          <artifactId>maven-antrun-plugin</artifactId>
		          <version>1.3</version>
		        </plugin>
		        <plugin>
		          <artifactId>maven-assembly-plugin</artifactId>
		          <version>2.2-beta-5</version>
		        </plugin>
		        <plugin>
		          <artifactId>maven-dependency-plugin</artifactId>
		          <version>2.8</version>
		        </plugin>
		        <plugin>
		          <artifactId>maven-release-plugin</artifactId>
		          <version>2.3.2</version>
		        </plugin>
		        <plugin>
		          <artifactId>maven-clean-plugin</artifactId>
		          <version>3.0.0</version>
		        </plugin>
		        <plugin>
		          <artifactId>maven-resources-plugin</artifactId>
		          <version>3.0.2</version>
		        </plugin>
		        <plugin>
		          <artifactId>maven-compiler-plugin</artifactId>
		          <version>3.7.0</version>
		        </plugin>
		        <plugin>
		          <artifactId>maven-surefire-plugin</artifactId>
		          <version>2.20.1</version>
		        </plugin>
		        <plugin>
		          <artifactId>maven-jar-plugin</artifactId>
		          <version>3.0.2</version>
		        </plugin>
		        <plugin>
		          <artifactId>maven-install-plugin</artifactId>
		          <version>2.5.2</version>
		        </plugin>
		        <plugin>
		          <artifactId>maven-deploy-plugin</artifactId>
		          <version>2.8.2</version>
		        </plugin>
		      </plugins>
		    </pluginManagement>
		    <plugins>
		      <plugin>
		        <artifactId>maven-clean-plugin</artifactId>
		        <version>3.0.0</version>
		        <executions>
		          <execution>
		            <id>default-clean</id>
		            <phase>clean</phase>
		            <goals>
		              <goal>clean</goal>
		            </goals>
		          </execution>
		        </executions>
		      </plugin>
		      <plugin>
		        <artifactId>maven-install-plugin</artifactId>
		        <version>2.5.2</version>
		        <executions>
		          <execution>
		            <id>default-install</id>
		            <phase>install</phase>
		            <goals>
		              <goal>install</goal>
		            </goals>
		          </execution>
		        </executions>
		      </plugin>
		      <plugin>
		        <artifactId>maven-resources-plugin</artifactId>
		        <version>3.0.2</version>
		        <executions>
		          <execution>
		            <id>default-resources</id>
		            <phase>process-resources</phase>
		            <goals>
		              <goal>resources</goal>
		            </goals>
		          </execution>
		          <execution>
		            <id>default-testResources</id>
		            <phase>process-test-resources</phase>
		            <goals>
		              <goal>testResources</goal>
		            </goals>
		          </execution>
		        </executions>
		      </plugin>
		      <plugin>
		        <artifactId>maven-surefire-plugin</artifactId>
		        <version>2.20.1</version>
		        <executions>
		          <execution>
		            <id>default-test</id>
		            <phase>test</phase>
		            <goals>
		              <goal>test</goal>
		            </goals>
		          </execution>
		        </executions>
		      </plugin>
		      <plugin>
		        <artifactId>maven-compiler-plugin</artifactId>
		        <version>3.7.0</version>
		        <executions>
		          <execution>
		            <id>default-testCompile</id>
		            <phase>test-compile</phase>
		            <goals>
		              <goal>testCompile</goal>
		            </goals>
		          </execution>
		          <execution>
		            <id>default-compile</id>
		            <phase>compile</phase>
		            <goals>
		              <goal>compile</goal>
		            </goals>
		          </execution>
		        </executions>
		      </plugin>
		      <plugin>
		        <artifactId>maven-jar-plugin</artifactId>
		        <version>3.0.2</version>
		        <executions>
		          <execution>
		            <id>default-jar</id>
		            <phase>package</phase>
		            <goals>
		              <goal>jar</goal>
		            </goals>
		          </execution>
		        </executions>
		      </plugin>
		      <plugin>
		        <artifactId>maven-deploy-plugin</artifactId>
		        <version>2.8.2</version>
		        <executions>
		          <execution>
		            <id>default-deploy</id>
		            <phase>deploy</phase>
		            <goals>
		              <goal>deploy</goal>
		            </goals>
		          </execution>
		        </executions>
		      </plugin>
		      <plugin>
		        <artifactId>maven-site-plugin</artifactId>
		        <version>3.3</version>
		        <executions>
		          <execution>
		            <id>default-site</id>
		            <phase>site</phase>
		            <goals>
		              <goal>site</goal>
		            </goals>
		            <configuration>
		              <outputDirectory>/environment/myeclipse/maven-archetype-quickstart/target/site</outputDirectory>
		              <reportPlugins>
		                <reportPlugin>
		                  <groupId>org.apache.maven.plugins</groupId>
		                  <artifactId>maven-project-info-reports-plugin</artifactId>
		                </reportPlugin>
		              </reportPlugins>
		            </configuration>
		          </execution>
		          <execution>
		            <id>default-deploy</id>
		            <phase>site-deploy</phase>
		            <goals>
		              <goal>deploy</goal>
		            </goals>
		            <configuration>
		              <outputDirectory>/environment/myeclipse/maven-archetype-quickstart/target/site</outputDirectory>
		              <reportPlugins>
		                <reportPlugin>
		                  <groupId>org.apache.maven.plugins</groupId>
		                  <artifactId>maven-project-info-reports-plugin</artifactId>
		                </reportPlugin>
		              </reportPlugins>
		            </configuration>
		          </execution>
		        </executions>
		        <configuration>
		          <outputDirectory>/environment/myeclipse/maven-archetype-quickstart/target/site</outputDirectory>
		          <reportPlugins>
		            <reportPlugin>
		              <groupId>org.apache.maven.plugins</groupId>
		              <artifactId>maven-project-info-reports-plugin</artifactId>
		            </reportPlugin>
		          </reportPlugins>
		        </configuration>
		      </plugin>
		    </plugins>
		  </build>
		  <reporting>
		    <outputDirectory>/environment/myeclipse/maven-archetype-quickstart/target/site</outputDirectory>
		  </reporting>
		</project>
```
2. 在maven-archetype-quickstart项目中执行 mvn help:effective-settings 可以查看到实际pom中内容如下:

```
<?xml version="1.0" encoding="UTF-8"?>
<!-- ====================================================================== -->
<!--                                                                        -->
<!-- Generated by Maven Help Plugin on 2018-05-16T23:35:31+08:00            -->
<!-- See: http://maven.apache.org/plugins/maven-help-plugin/                -->
<!--                                                                        -->
<!-- ====================================================================== -->
<!-- ====================================================================== -->
<!--                                                                        -->
<!-- Effective Settings for 'violet' on 'yexiaochaideMacBook-Pro.local'     -->
<!--                                                                        -->
<!-- ====================================================================== -->
<settings xmlns="http://maven.apache.org/SETTINGS/1.1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.1.0 http://maven.apache.org/xsd/settings-1.1.0.xsd">
  <localRepository>/Users/violet/.m2/repository</localRepository>
  <pluginGroups>
    <pluginGroup>org.apache.maven.plugins</pluginGroup>
    <pluginGroup>org.codehaus.mojo</pluginGroup>
  </pluginGroups>
</settings>
```
3. 执行mvn help:describe得到以下错误信息，显示了正确的使用方法。

```
		[ERROR] Failed to execute goal org.apache.maven.plugins:maven-help-plugin:3.0.1:describe (default-cli) on project maven-archetype-quickstart: You must specify either: both 'groupId' and 'artifactId' parameters OR a 'plugin' parameter OR a 'cmd' parameter. For instance:
		[ERROR]   # mvn help:describe -Dcmd=install
		[ERROR] or
		[ERROR]   # mvn help:describe -Dcmd=help:describe
		[ERROR] or
		[ERROR]   # mvn help:describe -Dplugin=org.apache.maven.plugins:maven-help-plugin
		[ERROR] or
		[ERROR]   # mvn help:describe -DgroupId=org.apache.maven.plugins -DartifactId=maven-help-plugin
		[ERROR] 
		[ERROR] Try 'mvn help:help -Ddetail=true' for more information.
```
				
4. 执行mvn help:describe -Dcmd=install 显示结果如下：

```		
		mvn help:describe -Dcmd=install
		
		[INFO] Scanning for projects...
		[INFO] 
		[INFO] ------------------------------------------------------------------------
		[INFO] Building maven-archetype-quickstart 0.0.1-SNAPSHOT
		[INFO] ------------------------------------------------------------------------
		[INFO] 
		[INFO] --- maven-help-plugin:3.0.1:describe (default-cli) @ maven-archetype-quickstart ---
		[INFO] 'install' is a phase corresponding to this plugin:
		org.apache.maven.plugins:maven-install-plugin:2.4:install
		
		It is a part of the lifecycle for the POM packaging 'jar'. This lifecycle includes the following phases:
		* validate: Not defined
		* initialize: Not defined
		* generate-sources: Not defined
		* process-sources: Not defined
		* generate-resources: Not defined
		* process-resources: org.apache.maven.plugins:maven-resources-plugin:2.6:resources
		* compile: org.apache.maven.plugins:maven-compiler-plugin:3.1:compile
		* process-classes: Not defined
		* generate-test-sources: Not defined
		* process-test-sources: Not defined
		* generate-test-resources: Not defined
		* process-test-resources: org.apache.maven.plugins:maven-resources-plugin:2.6:testResources
		* test-compile: org.apache.maven.plugins:maven-compiler-plugin:3.1:testCompile
		* process-test-classes: Not defined
		* test: org.apache.maven.plugins:maven-surefire-plugin:2.12.4:test
		* prepare-package: Not defined
		* package: org.apache.maven.plugins:maven-jar-plugin:2.4:jar
		* pre-integration-test: Not defined
		* integration-test: Not defined
		* post-integration-test: Not defined
		* verify: Not defined
		* install: org.apache.maven.plugins:maven-install-plugin:2.4:install
		* deploy: org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy
```




### 2.6 由上可知\<packaging\>jar\</packaging\>的生命周期如下：

		* validate: Not defined
		* initialize: Not defined
		* generate-sources: Not defined
		* process-sources: Not defined
		* generate-resources: Not defined
		* process-resources: org.apache.maven.plugins:maven-resources-plugin:2.6:resources
		* compile: org.apache.maven.plugins:maven-compiler-plugin:3.1:compile
		* process-classes: Not defined
		* generate-test-sources: Not defined
		* process-test-sources: Not defined
		* generate-test-resources: Not defined
		* process-test-resources: org.apache.maven.plugins:maven-resources-plugin:2.6:testResources
		* test-compile: org.apache.maven.plugins:maven-compiler-plugin:3.1:testCompile
		* process-test-classes: Not defined
		* test: org.apache.maven.plugins:maven-surefire-plugin:2.12.4:test
		* prepare-package: Not defined
		* package: org.apache.maven.plugins:maven-jar-plugin:2.4:jar
		* pre-integration-test: Not defined
		* integration-test: Not defined
		* post-integration-test: Not defined
		* verify: Not defined
		* install: org.apache.maven.plugins:maven-install-plugin:2.4:install
		* deploy: org.apache.maven.plugins:maven-deploy-plugin:2.7:deploy

### 第三章 一个简单的Maven项目

### 3.1 规则
>1. 当提到一个插件目标的时候，我们常常用速记符号:pluginId:goalId。 例如，当提到Archetype插件的create目标的时候，我们写 成archetype:create

### 3.2 使用archetype:generate创建Maven框架项目
		
>1. <del>mvn archetype:create</del> 方式已经过时
>1. mvn archetype:generate 现在，使用generate选择适合模板来创建项目
>1. mvn help:effective-pom 查看完成的pom.xml

### 3.3 传递性依赖(transitive dependencies)
		
让我们看一下你运行前面的样例的时候那些下载到你本地仓库的依赖。看一下这个目 录:~/.m2/repository/junit/junit/3.8.1/。如果你一直跟着本章的样例，那么这里会 有文件junit-3.8.1.jar 和junit-3.8.1.pom，还有Maven用来验证已下载构件准确性的 校验和文件。需要注意的是Maven不只是下载JUnit的JAR文件，它同时为这个JUnit依赖 下载了一个POM文件。Maven同时下载构件和POM文件的这种行为，对Maven支持传递性依 赖来说非常重要。

### 3.4 依赖范围(dependency scope)

	test、compile、provided

Simple项目的pom.xml包含了 一个依赖——junit:junit:jar:3.8.1——范围是test。当一个依赖的范围是test的 时候，说明它在Compiler插件运行compile目标的时候是不可用的。它只有在运 行compiler:testCompile和surefire:test目标的时候才会被加入到classpath中。

你也可以配置Maven，使用provided范围，让它排除WAR文件中特定的依赖。provided范 围告诉Maven一个依赖在编译的时候需要，但是它不应该被捆绑在构建的输出中。当你 开发web应用的时候provided范围变得十分有用，你需要通过Servlet API来编译你的代 码，但是你不希望Servlet API的JAR文件包含在你web应用的WEB-INF/lib目录中

### 3.5 站点生成和报告 (Site Generation and Reporting)

	mvn site

这将会运行site生命周期阶段。它不像默认生命周期那样，管理代码生成，操作资源， 编译，打包等等。Site生命周期只关心处理在src/site目录下的site内容，还有生成 报告。在这个命令运行过之后，你将会在target/site目录下看到一个项目web站点。载 入target/site/index.html你会看到项目站点的基本外貌

### 3.6 定制站点报告\<reporting\>\</reporting\>
```
报告配置在<reporting></reporting>下，而插件配置在<plugins></plugins>下
	
Clover报告：	检查单元测试覆盖率
JXR报告：		生成HTML源代码相互间引用，这在代码审查的时候非常有用
PMD报告：		告针对各种编码问题来分析源代码
JDepend报告：	分析源代码中各个包之间的依赖
```
Maven提供了很完整的可配置的报告，像Clover报告检 查单元测试覆盖率，JXR报告生成HTML源代码相互间引用，这在代码审查的时候非常有 用，PMD报告针对各种编码问题来分析源代码，JDepend报告分析源代码中各个包之间的 依赖。通过在pom.xml中配置那些报告被包含在构建中，站点报告就可以被定制了。
		
### 第四章 定制一个Maven项目

[Maven simple-weather示例项目Git地址]
```
simple-weather相关依赖：
	
Log4j
Dom4j
Jaxen
Velocity
Maven Exec 插件 ：  $ mvn help:describe -Dplugin=exec -Dfull
```
### 4.1 查看Maven项目依赖

```
1. mvn dependency:resolve
	
2. mvn dependency:tree 查看依赖关系树
	
3. mvn install -X  查看完整的依赖踪迹，包含那些因为冲突或者其它原因而被 拒绝引入的构件，打开 Maven 的调试标记运行
```
### 4.2 添加测试范围依赖

```
<project> 
 	...
	<dependencies> 
		...
		<dependency> 
			<groupId>org.apache.commons</groupId> 
			<artifactId>commons-io</artifactId> 
			<version>1.3.2</version> 
			<scope>test</scope>
		</dependency>
	... 
	</dependencies>
</project>
```	

### 4.3 添加单元测试资源

一个单元测试需要访问针对测试的一组资源。 通常你需要在测试 classpath 中 存储一些包含期望结果的文件，以及包含模拟输入的文件。 在本项目中，我们为 YahooParserTest 准备了一个名为 ny-weather.xml 的测试 XML 文档，还有一个名为 format-expected.dat 的文件，包含了 WeatherFormatter 的期望输出。format-expected.dat文件内容如下：
	
	*********************************
	 Current Weather Conditions for:
	  New York, NY, US
	  
	 Temperature: 39
	   Condition: Fair
	    Humidity: 67
	  Wind Chill: 39
	*********************************

预先设定了所有值，判断获取到的数据经过Velocity模板格式化之后是否和预期一致。

### 4.4 执行单元测试

	1. mvn test
	
	2. 忽略测试失败：<testFailureIgnore>true</testFailureIgnore> 
		<project> 
			[...]
			<build> 
				<plugins>
					<plugin> 
						<groupId>org.apache.maven.plugins</groupId> 
						<artifactId>maven-surefire-plugin</artifactId> 
						<configuration>
							<testFailureIgnore>true</testFailureIgnore> 
						</configuration>
					</plugin> 
				</plugins>
			</build>
			[...] 
		</project>
	
	3. 命令行动态设置忽略测试失败：
	mvn test -Dmaven.test.failure.ignore=true
	
	4. 当项目的单元测试十分耗时的时候，并不是每一次执行install都需要执行单元测试的，所以我们可以设置跳过单元测试来节约时间。
		<project> 
			[...]
			<build> 
				<plugins>
					<plugin> 
						<groupId>org.apache.maven.plugins</groupId> 
						<artifactId>maven-surefire-plugin</artifactId>
						<configuration>
							<skip>true</skip> 
						</configuration>
					</plugin> 
				</plugins>
			</build>
			[...] 
		</project>

	或者命令行动态设置：
	mvn install -Dmaven.test.skip=true

### 4.5 使用Maven Assembly 插件制作软件分发包
	
	配置Maven Assembly插件：
		<project> 
			[...]
			<build> 
				<plugins>
					<plugin> 
						<artifactId>maven-assembly-plugin</artifactId>
						<configuration>
							<descriptorRefs> 
									<descriptorRef>jar-with-dependencies</descriptorRef>
							</descriptorRefs> 
						</configuration>
					</plugin> 
				</plugins>
			</build>
			[...] 
		</project>
	
	如上配置好Assembly插件后，可以通过如下命令来构建：
	mvn install assembly:assembly


### 第五章 一个简单的Web应用

[Maven simple-weather示例项目Git地址]

### 5.1 创建web应用

	mvn archetype:generate
	在列出的模板中选择需要的，输入数字编号。

	pom.xml文件中最根本的区别在于：
	<packaging>war</packaging>

### 5.2 Servlet容器Jetty插件配置使用

	1. 配置Jetty插件
	<project> 
		[...]
		<build> 
			<finalName>simple-webapp</finalName> 
			<plugins>
				<plugin> 
					<groupId>org.mortbay.jetty</groupId> 
					<artifactId>maven-jetty-plugin</artifactId>
				</plugin> 
			</plugins>
		</build>
		[...] 
	</project>
	
	2. 启动Jetty服务器
	mvn jetty:run
	
### 5.3 创建和配置Servlet程序

为了把servlet 添加到你的 web 应用，并且使其与请求路径匹配， 需要添加如下的 servlet 和 servlet-mapping 元素至你项目的 web.xml 文件。 文件 web.xml 可以在目录 src/main/webapp/WEB-INF 中找到。
		
	<!DOCTYPE web-app PUBLIC
	"-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN" "http://java.sun.com/dtd/web-app_2_3.dtd" >
	<web-app>
		<display-name>Archetype Created Web Application</display-name> 
		<servlet>
			<servlet-name>simple</servlet-name>
			<servlet-class>org.sonatype.mavenbook.web.SimpleServlet</servlet-class> 	
		</servlet>
		<servlet-mapping>
			<servlet-name>simple</servlet-name>
			<url-pattern>/simple</url-pattern> 
		</servlet-mapping>
	</web-app>

### 5.4 添加Servlet API支持

	到目前为止，直接执行：mvn compile 会失败，因为我们还没有为web项目添加Servlet支持。
	
为了编写一个 servlet ，我们需要添加 Servlet API 作为项目依赖。 Servlet 规格说明是一个 JAR 文件，它能从 Sun Microsystems 的站点下载到 http:// java.sun.com/products/servlet/download.html 。JAR 文件下载好之后你需要把它 安装到位于 ~/.m2/repository 的 Maven 本地仓库。你必须为所有 Sun Microsystems 维护的 J2EE API 重复同样的过程，包括 JNDI， JDBC， Servlet， JSP， JTA， 以 及其它。 如果你不想这么做因为觉得这样太无聊了，其实不只有你这么认为。 幸 运的是，有一种更简单的方法来下载所有这些类库并安装到本地仓库 —— Apache Geronimo 的独立的开源实现。

添加像 JSP API 或者 Servlet API 这样的依赖现在很简单明了了，不再需要你从 web 站点手工下载 JAR 文件然后再安装到本地仓库。 关键是你必须知道去哪里找，使用 什么 groupId， artifactId， 和 version 来引用恰当的 Apache Geronimo 实现。给 pom.xml 添加如下的依赖元素以添加对 Servlet 规格说明 API 的依赖。.

	添加 Servlet 2.4 规格说明作为依赖：
	（你必须看一下Maven 的公共仓库来决定使用什么版本。 最好使用某个规格说明实现的最新版本）
	<project> 
		[...]
		<dependencies> 
			[...]
			<dependency> 
				<groupId>org.apache.geronimo.specs</groupId>
				<artifactId>geronimo-servlet_2.4_spec</artifactId>
				<version>1.1.1</version>
				<scope>provided</scope>
			</dependency> 
		</dependencies> 
		[...]
	</project>

如果你对在这个简单 web 应用编写自定义 JSP 标签感兴趣，你将需要添加对 JSP 2.0 规格说明的依赖。 使用以下配置来加入这个依赖。

	<project> 
		[...]
		<dependencies> 
			[...]
			<dependency> 
				<groupId>org.apache.geronimo.specs</groupId>
				<artifactId>geronimo-jsp_2.0_spec</artifactId>
				<version>1.1</version>
				<scope>provided</scope>
			</dependency>
		</dependencies>
		[...]
	</project>

### 5.6 运行Servlet

在添加好这个 Servlet 规格说明依赖之后，运行 mvn clean install ，然后运行 mvn jetty:run
	
		mvn clean install
		mvn jetty:run
		
命令行访问index.html页面，查看控制台输出：

		curl http://localhost:8080/simple-webapp/simple


### 第六章 一个多模块项目

[Maven simple-weather示例项目Git地址]    

[The Definitive Guide]

### 6.1 多模块项目结构图父POM
一个多模块项目通过一个父POM引用一个或多个子模块来定义，并且\<packaging>pom\</packaging>。如下所示：
	
	<project xmlns="http://maven.apache.org/POM/4.0.0" 
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
							http://maven.apache.org/maven-v4_0_0.xsd"> 
		<modelVersion>4.0.0</modelVersion>
		<groupId>org.sonatype.mavenbook.ch06</groupId>
		<artifactId>simple-parent</artifactId>
		<packaging>pom</packaging>
		<version>1.0</version>
		<name>Chapter 6 Simple Parent Project</name>
		
		<modules> 
			<module>simple-weather</module> 
			<module>simple-webapp</module>
		</modules>
		<build> 
			<pluginManagement>
				<plugins>
					<plugin>
						<groupId>org.apache.maven.plugins</groupId>
						<artifactId>maven-compiler-plugin</artifactId>
						<configuration>
							<source>1.5</source>
							<target>1.5</target>
						</configuration>
					</plugin>
				</plugins>
			</pluginManagement>
		</build>
		<dependencies> 
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>3.8.1</version>
				<scope>test</scope>
			</dependency>
		</dependencies>
	</project>

注意simple-parent定义了一组Maven坐 标:groupId是org.sonatype.mavenbook，artifactId是simple-parent，version是1.0。 这个父项目不像之前的项目那样创建一个JAR或者一个WAR，它仅仅是一个引用其它 Maven项目的POM。像simple-parent这样仅仅提供项目对象模型的项目，正确的的打包 类型是pom。pom.xml中下一部分列出了项目的子模块。这些模块在modules元素中定义， 每个modules元素对应了一个simple-parent/目录下的子目录。Maven知道去这些子目录 寻找pom.xml文件，并且，在构建的simp-parent的时候，它会将这些子模块包含到要构 建的项目中。

最后，我们定义了一些将会被所有子模块继承的设置。simple-parent的build部 分配置了所有Java编译的目标是Java 5 JVM。因为compiler插件默认绑定到了生 命周期，我们就可以使用pluginManagement部分来配置。我们将会在后面的章节 详细讨论pluginManagement，区分为默认的插件提供配置和真正的绑定插件是很 容易的。dependencies元素将JUnit 3.8.1添加为一个全局的依赖。build配置和 dependencies都会被所有的子模块继承。使用POM继承允许你添加一些全局的依赖如 JUnit和Log4J。

### 6.2 子POM
	
	每个子pom都需要定义\<parent>标签，值全部一样。
	
	<parent>
	    <groupId>org.sonatype.mavenbook.optimize</groupId>
	    <artifactId>simple-parent</artifactId>
	    <version>1.0-SNAPSHOT</version>
  	</parent>

### 6.3 构建多模块项目

	在parent-project中执行：
	mvn clean install

### 6.4 运行多模块项目
	
	在parent-project中执行：
	mvn jetty:run

Jetty启动之后，在浏览器载入形如：http://localhost:8080/simple-webapp/weather? zip=01201，你应该看到格式化的天气输出.
	
当Maven执行一个带有子模块的项目的时候，Maven首先载入父POM，然后定位所有的子 模块POM。Maven然后将所有这些项目的POM放入到一个称为Maven 反应堆(Reactor)的 东西中，由它负责分析模块之间的依赖关系。这个反应堆处理组件的排序，以确保相互 独立的模块能以适当的顺序被编译和安装。

	
### 第七章 多模块企业级项目

[Maven simple-weather示例项目Git地址]    

![Maven多模块项目结构图](http://book.huihoo.com/maven-the-definitive-guide/multimodule-web-spring_projects.png)

### 7.1 Hibernate项目运行
	
	1. 首先需要使用Hibernate3插件构造数据库
		mvn hibernate3:hbm2ddl
	2. 运行到web容器
		mvn jetty:run

### 7.2 运行命令行程序

	1. 运行Maven Assembly插 件的assembly目标
		mvn assembly:assembly
	2. 调用Hibernate3插件的hbm2ddl目标来创建 HSQLDB数据库
		mvn hibernate3:hbm2ddl
	3. 运行命令行
		java -cp target/simple-command-jar-with-dependencies.jar \ org.sonatype.mavenbook.weather.Main 60202
		
### 第八章 优化和重构POM

### 8.1 POM清理，清理重复的内容
### 8.2 优化依赖，被一个以上的module共同依赖的库，应该统一放入父POM的\<dependencyManagement\>中定义，并在这些module中删除依赖的版本\<version>\</version\>
	例如， 优化后的父POM如下：
		<project> 
			...
			<dependencyManagement> 
				<dependencies>
					<dependency> 
						<groupId>org.hibernate</groupId>
						<artifactId>hibernate-annotations</artifactId>
						<version>3.3.0.ga</version>
					</dependency>
					<dependency>
						<groupId>org.hibernate</groupId>
						<artifactId>hibernate</artifactId>
						<version>3.3.0.ga</version>
					</dependency>
				</dependencies>
			</dependencyManagement> 		
			...
		<project> 
	优化后的子POM：
		<project> 
			...
			<dependencies>
				<dependency> 
					<groupId> org.hibernate</groupId>
					<artifactId>hibernate-annotations</artifactId>
				</dependency>
				<dependency>
					<groupId>org.hibernate</groupId>
					<artifactId>hibernate-commons-annotations</artifactId>
				</dependency>
			</dependencies>
			...
		<project> 
				
### 8.3 可预测的共通发布的相同版本号抽取。例如hibernate和hibernate-annotations的版本号总是相同的。
			<project> 
			...
			<properties>
				<hibernate.annotations.version>3.3.0.ga<hibernate.annotations.version>
    		</properties>
			<dependencyManagement> 
				<dependencies>
					<dependency> 
						<groupId>org.hibernate</groupId>
						<artifactId>hibernate-annotations</artifactId>
						<version>${hibernate.annotations.version}</version>
					</dependency>
					<dependency>
						<groupId>org.hibernate</groupId>
						<artifactId>hibernate-commons-annotations</artifactId>
						<version>${hibernate.annotations.version}</version>
					</dependency>
				</dependencies>
			</dependencyManagement> 		
			...
		<project> 

### 8.4 解决兄弟module之间的依赖冲突
	
	1. 方案一：将依赖都抽象到父POM中
	2. 方案二：在兄弟目录中，都使用内建属性来动态获取Version。例如：${parent.version}.

为兄弟项目使用内置的项目version和groupId 使用{project.version}和org.sonatype.mavenbook来引用兄弟项目。兄弟项目 基本上一直共享同样的groupId，也基本上一直共享同样的发布版本。使用0.6- SNAPSHOT可以帮你避免前面提到的兄弟版本不一致问题。

### 8.5 优化插件
	1. 插件的版本号也可以定义成properties
	2. 使用Maven Dependency插件进行优化
		mvn dependency:analyze
		mvn dependency:tree

### 第九章 项目对象模型
![项目对象模型](https://images2015.cnblogs.com/blog/737467/201511/737467-20151101222148700-2134239729.png)
	
	项目对象模型包含了四类描述和配置：
	1. 项目总体信息 General Project Information
	2. 构建设置 Build Settings
	3. 构建环境 Build Environment
	 	构建环境包含了一些能在不同使用环境中激活的profile。例如，在开发过程 中你可能会想要将应用部署到一个而开发服务器上，
	 	而在产品环境中你会需要将 应用部署到产品服务器上。构建环境为特定的环境定制了构建设置，
	 	通常它还会 由~/.m2中的自定义settings.xml补充。这个settings文件将会在第 11 章
	 	构建 Profile中，以及第 A.1 节 “简介中”的附录 A,附录: Settings细节小节中 讨论。
	4. POM关系 POM Relationships

### 9.1 超级POM
	
	超级POM位置：
	/usr/local/maven/lib/maven-2.0.9-uber.jar中的包org.apache.maven.project目录下pom-4.0.0.xml
	
### 9.2 项目版本号
Maven中的版本包含 了以下部分:主版本，次版本，增量版本，和限定版本号

	版本号格式： <major version>.<minor version>.<incremental version>-<qualifier>

版本“1.3.5”由一个主版本1，一个次版本3，和一个增量版本5。而一个 版本“5”只有主版本5，没有次版本和增量版本。限定版本用来标识里程碑构建: alpha和beta发布，限定版本通过连字符与主版本，次版本或增量版本隔离。例如，版 本“1.3-beta-01”有一个主版本1，次版本3，和一个限定版本“beta-01”	

当你想要在你的POM中使用版本界限的时候，保持你的版本号与标准一致十分重要。 在第 9.4.3 节 “依赖版本界限”中介绍的版本界限，允许你声明一个带有版本界限的 依赖，只有你遵循标准的时候该功能才被支持。因为Maven根据本节中介绍的版本号格 式来对版本进行排序。

如果你的版本号与格式<主版本>.<次版本>.<增量版本>-<限定版本>相匹配，它就能被 正确的比较;“1.2.3”将被评价成是一个比“1.0.2”更新的构件，这种比较基于主版 本，次版本，和增量版本的数值。如果你的版本发布号没有符合本节介绍的标准，那么 你的版本号只会根据字符串被比较;“1.0.1b”和“1.2.0b”会使用字符串比较。

### 9.3 构建版本号

我们还需要对版本号的限定版本进行排序。以版本号“1.2.3-alpha-2”和“1.2.3- alpha-10”为例，这里“alpha-2”对应了第二次alpha构建，而“alpha-10”对应了第 十次alpha构建。虽然“alpha-10”应该被认为是比“alpha-2”更新的构建，但Maven 排序的结果是“alpha-10”比“alpha-2”更旧，问题的原因就是我们刚才讨论的Maven 处理版本号的方式。

Maven会将限定版本后面的数字认作一个构建版本。换句话说，这里限定版本 是“alpha”，而构建版本是2。虽然Maven被设计成将构建版本和限定版本分离，但 目前这种解析还是失效的。因此，“alpha-2”和“alpha-10”是使用字符串进行比较 的，而根据字母和数字“alpha-10”在“alpha-2”前面。要避开这种限制，你需要对 你的限定版本使用一些技巧。如果你使用“alpha-02”和“alpha-10”，这个问题就消 除了，一旦Maven能正确的解析版本构建号之后，这种工作方式也还是能用。

### 9.4 SNAPSHOT版本
	只能用于开发阶段，默认不会从远程仓库查找，但也可以通过设置来允许上传到远程仓库。

### 9.5 LATEST 和 RELEASE 版本
	不建议使用，在开发阶段方便，但也可以能因为遗忘疏忽带来意想不到的后果。

### 9.6 变量和属性引用

一、变量引用

Maven提供了三个隐式的变量，可以用来访问环境变量、POM信息和Maven Settings：

1. env 		暴露了你操作系统或者shell的环境变量
2. project	project变量暴露了POM。你可以使用点标记(.)的路径来引用POM元素的值。
			
		例如，在本节中我们使用过groupId和artifactId来设置构建配置 中的finalName元素。
		这个属性引用的语法是:org.sonatype.mavenbook- ${project.artifactId}	
3. settings	暴露了Maven settings信息。可以使用点标记(.)的路径来引用settings.xml文件中元素的值
	
		例如，${settings.offline}会引用~/.m2/ settings.xml文件中offline元素的值

二、属性引用

除了上面这三个隐式的变量，你还可以引用系统属性，以及任何在Maven POM中和构建
profile中自定义的属性组

1. 系统属性
	
		Java系统属性 
		所有可以通过java.lang.System中getProperties()方法访问的属性都被暴露成POM属性。
		一些系统属性的例子是:hudson，/home/hudson，/usr/lib/jvm/ java-1.6.0-openjdk-1.6.0.0/jre，
		和Linux。一个完整的系统属性列表可以 在java.lang.System类的Javadoc中找到。
		
		我们还可以通过pom.xml或者settings.xml中的properties元素设置自己的属 性，
		或者还可以使用外部载入的文件中属性。如果你在pom.xml中设置了一个名 为fooBar的属性，
		该属性就可以通过${fooBar}引用。当你构建一个系统，它针 对不同的部署环境过滤资源，
		那么自定义属性就变得十分有用。这里是在POM中 设置${foo}=bar的语法:
		<properties> 
			<foo>bar</foo>
		</properties>
	
2. Maven POM中自定义的属性组
3. 构建profile中自定义的属性组

### 9.7 依赖范围
依赖范围控制哪些依赖在哪些classpath中可用，哪些依赖包含在一个应用中

|依赖范围|描述|代码示例|
|----|------|----|
|compile(编译范围)|compile是默认的范围;如果没有提供一个范围，那该依赖的范围就是编译范 围。编译范围依赖在所有的classpath中可用，同时它们也会被打包。|\<scope>compile\</scope>|
|provided(已提供范围)|provided依赖只有在当JDK或者一个容器已提供该依赖之后才使用。例如，如果 你开发了一个web应用，你可能在编译classpath中需要可用的Servlet API来编 译一个servlet，但是你不会想要在打包好的WAR中包含这个Servlet API;这个 Servlet API JAR由你的应用服务器或者servlet容器提供。已提供范围的依赖在 编译classpath(不是运行时)可用。它们不是传递性的，也不会被打包|\<scope>compile\</scope>|
|runtime(运行时范围)|runtime依赖在运行和测试系统的时候需要，但在编译的时候不需要。比如，你 可能在编译的时候只需要JDBC API JAR，而只有在运行的时候才需要JDBC驱动实 现。|\<scope>compile\</scope>|
|test(测试范围)|test范围依赖 在一般的 编译和运行时都不需要，它们只有在测试编译和测试运 行阶段可用。测试范围依赖在之前介绍过|\<scope>compile\</scope>|
|system(系统范围)|system范围依赖与provided类似，但是你必须显式的提供一个对于本地系统中 JAR文件的路径。这么做是为了允许基于本地对象编译，而这些对象是系统类库 的一部分。这样的构件应该是一直可用的，Maven也不会在仓库中去寻找它。如 果你将一个依赖范围设置成系统范围，你必须同时提供一个systemPath元素。注 意该范围是不推荐使用的(你应该一直尽量去从公共或定制的Maven仓库中引用 依赖)|\<scope>compile\</scope>|
||||
|可选依赖|编译这个项目的时候你需要两个依赖类库EHCache和SwarmCache，但是你不希望在使用你类库的项 目中，这两个依赖类库同时作为传递性运行时依赖出现。<b>在理想的世界中，你不需要使用可选依赖。</b>你可以将EHCache相关的代码放到my- project-ehcache子模块中，将SwarmCache相关的代码放到my-project-swarmcache子模 块中，而非创建一个带有一系列可选依赖的大项目。这样，其它项目就可以只引用特定 实现的项目，发挥传递性依赖的功效，而不用去引用my-project项目，再自己声明特定 的依赖。|\<dependency><br>\<groupId>swarmcache\</groupId> \<artifactId>swarmcache\</artifactId><br>\<version>1.0RC2\</version><font color="red"><br>\<optional>true\</optional></font><br>\</dependency>|

### 9.8 依赖版本界限
你并不是必须为依赖声明某个特定的版本，你可以指定一个满足给定依赖的版本界限。

例如，你可以指定你的项目依赖于JUnit的3.8或以上版本，或者说依赖于JUnit 1.2.10 和1.2.14之间的某个版本。你可以使用如下的字符来围绕一个或多个版本号，来实现版 本界限。

|版本界限字符|示例|描述|
|----|----|----|
|(,) 不包含量词|(1.2.3,2.5.8)|支持从1.2.3到2.5.8之间的所有版本,开区间|
|[,] 包含量词|[1.2.3,2.5.8]|支持从1.2.3到2.5.8之间的所有版本,闭区间|
||[1.2.3,2.5.8)|支持从1.2.3到2.5.8之间的所有版本,左闭右开区间|
||(1.2.3,2.5.8]|支持从1.2.3到2.5.8之间的所有版本,左开右闭区间|
||[1.2.3,)|支持所有大于1.2.3的版本|
||(,2.5.8]|支持所有小于2.5.8的版本|
||[1.2.3]|只支持1.2.3版本|

### 9.9 传递性依赖和范围

“依赖范围”中提到的每种依赖范围不仅仅影响声明项目中的依赖范围，它也对所传递性依赖起作用。表达该信息最简单的方式是通过一张表来表述：<b>范围如何影响传递性依赖</b>

|直接范围|传递性范围||||
|----|----|----|----|----|
||<b>compile</b>|<b>provided</b>|<b>runtime</b>|<b>test</b>|
|<b>compile</b>|compile|-|runtime|-|
|<b>provided</b>| provided |provided| provided |-|
|<b>runtime</b>| runtime |-|runtime|-|
|<b>test</b>| test |-|test|-|

	A----compile---->B   B----compile---->C   结果： A---->compile---->C 
	A----compile---->B   B----provided--->C   结果： A---->不依赖于---->C
	A----compile---->B   B----runtime---->C   结果： A---->runtime---->C  
	A----compile---->B   B----test------->C   结果： A---->不依赖于---->C  

	A----provided--->B   B----compile---->C   结果： A---->provided---->C 
	A----provided--->B   B----provided--->C   结果： A---->provided---->C
	A----provided--->B   B----runtime---->C   结果： A---->provided---->C  
	A----provided--->B   B----test------->C   结果： A---->不依赖于----->C
	
	A----runtime---->B   B----compile---->C   结果： A---->runtime---->C 
	A----runtime---->B   B----provided--->C   结果： A---->不依赖于---->C
	A----runtime---->B   B----runtime---->C   结果： A---->runtime---->C  
	A----runtime---->B   B----test------->C   结果： A---->不依赖于---->C  
	
	A------test----->B   B----compile---->C   结果： A----->test----->C 
	A------test----->B   B----provided--->C   结果： A---->不依赖于---->C
	A------test----->B   B----runtime---->C   结果： A----->test----->C  
	A------test----->B   B----test------->C   结果： A---->不依赖于---->C  

### 9.10 冲突解决-排除一个传递性依赖
排除一个传递性依赖：\<exclusion>\</exclusion>

	<dependency> 
		<groupId>org.sonatype.mavenbook</groupId>
		<artifactId>project-a</artifactId> 
		<version>1.0</version>
		<exclusions>
			<exclusion> 
				<groupId>org.sonatype.mavenbook</groupId> 
				<artifactId>project-b</artifactId>
			</exclusion> 
		</exclusions>
	</dependency>

排除并替换一个传递性依赖    
其实就是排除一个传递性依赖，然后再添加一个同样的依赖来起到替换的作用

	<dependencies>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate</artifactId>
			<version>3.2.5.ga</version>
			<exclusions>
				<exclusion>
					<groupId>javax.transaction</groupId> 
					<artifactId>jta</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.apache.geronimo.specs</groupId>
			<artifactId>geronimo-jta_1.1_spec</artifactId>
			<version>1.1</version>
		</dependency> 
	</dependencies>
	

四种可能需要排除替换的情况：

1. 构建的groupId和artifactId已经更改了，而当前的项目需要一个与传递性 依赖不同名称的版本——结果是classpath中出现了同样项目的两份内容。 一般来说Maven会捕捉到这种冲突并且使用该项目的一个单独的版本，但是 当artifactId和artifactId不一样的时候，Maven就会认为它们是两种不同的类 库。
2. 某个构件没有在你的项目中被使用，而且该传递性依赖没有被标志为可选依赖。 在这种情况下，你可能想要排除这种依赖，因为它不是你的系统需要的东西，你 要尽量减少应用程序分发时的类库数目。
3. 一个构件已经在运行时的容器中提供了，因此不应该被包含在你的构件中。该情 况的一个例子是，如果一个依赖依赖于如Servlet API的东西，并且你又要确保 这样的依赖没有包含在web应用的WEB-INF/lib目录中。
4. 为了排除一个可能是多个实现的API的依赖。这种情况在例 9.8 “排除并替换一 个传递性依赖”中阐述;有一个Sun API，需要点击许可证，并且需要耗时的手 工安装到自定义仓库，对于同样的API有可免费分发版本，在中央Maven仓库中可 用(Geronimo's JTA 实现)

### 9.11 依赖管理

父POM中dependencyManagement元素中为你提供了一种方式来统一依赖版本号，使用pom.xml中的dependencyManagement元素能让你在子项目中引用一个依赖而不用显式的列出版本号。

注意：如果子项目定义了一个版本，它将覆盖顶层POM 的dependencyManagement元素中的版本。那就是:只有在子项目没有直接声明一个版本 的时候，dependencyManagement定义的版本才会被使用。

### 9.12 项目继承

POM继承关系图：

![POM继承关系图](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1527092792721&di=c572597d764f196d1dd77148ae07808b&imgtype=jpg&src=http%3A%2F%2Fimg1.imgtn.bdimg.com%2Fit%2Fu%3D2335801681%2C471830450%26fm%3D214%26gp%3D0.jpg)

有些情况你会想要一个项目从父POM中继承一些值。你可能正构建一个大型的系统，你 不想一遍又一遍的重复同样的依赖元素。如果你的项目通过parent元素使用继承，你 就可以避免这种重复。当一个项目声明一个parent的时候，它从父项目的POM中继承信 息。它也可以覆盖父POM中的值，或者添加一些新的值。

所有的Maven POM从父POM中继承值。如果一个POM没有通过parent元素指定一个直 接的父项目，那这个POM就会从超级POM继承值。

	项目继承方式如下：
	<project> 
		<parent>
			<groupId>com.training.killerapp</groupId>
			<artifactId>a-parent</artifactId>
			<version>1.0-SNAPSHOT</version>
		</parent> 
		<artifactId>project-a</artifactId> 
		...
	</project>
	
当一个项目指定一个父项目的时候，Maven在读取当前项目的POM之前，会使用这个 父POM作为起始点。它继承所有东西，包括groupId和version。你会注意到project- a没有指定groupId和version，它们从a-parent继承而来。有了parent元素，一个 POM就只需要定义一个artifactId。但这不是强制的，project-a可以有一个不同 的groupId和version，但如果不提供值，Maven就会使用在父POM中指定的值。如果你 开始使用Maven来管理和构建大型的多模块项目，你就会常常创建许多共享一组通用 的groupId和version的项目。

当你继承一个POM，你可以选择直接使用继承的POM信息，或者选择覆盖它。以下是一个 Maven POM从它父POM中继承的项目列表:

1. 定义符(groupId和artifactId中至少有一个必须被覆盖) 
2. 依赖
3. 开发者和贡献者
4. 插件列表
5. 报告列表
6. 插件执行 (id匹配的执行会被合并) 
7. 插件配置

### 9.13 POM 最佳实践

1. 创建一个集中管理依赖的pom项目 baseDeps 关键在于：<b>\<packaing>pom\</packaging></b>
	
		<project>
			<groupId>org.sonatype.mavenbook</groupId>
			<artifactId>persistence-deps</artifactId>
			<version>1.0</version>
			<packaging>pom</packaging>
			<dependencies>
				<dependency>
					<groupId>org.hibernate</groupId>
					<artifactId>hibernate</artifactId>
					<version>${hibernateVersion}</version>
				</dependency>
				<dependency>
					<groupId>org.hibernate</groupId>
					<artifactId>hibernate-annotations</artifactId>
					<version>${hibernateAnnotationsVersion}</version>
				</dependency>
				<dependency>
					<groupId>org.springframework</groupId>
					<artifactId>spring-hibernate3</artifactId>
					<version>${springVersion}</version>
				</dependency>
				<dependency>
					<groupId>mysql</groupId>
					<artifactId>mysql-connector-java</artifactId>
					<version>${mysqlVersion}</version>
				</dependency>
			</dependencies>
			<properties>
				<mysqlVersion>(5.1,)</mysqlVersion>
				<springVersion>(2.0.6,)</springVersion>
				<hibernateVersion>3.2.5.ga</hibernateVersion>
				<hibernateAnnotationsVersion>3.3.0.ga</hibernateAnnotationsVersion>
			</properties>
		</project>

2. 执行mvn install  将baseDeps pom类型项目安装到maven仓库。
3. 在其他项目中声明对baseDeps项目的依赖，关键在于\<type>pom\</type>

		<project>
			<description>This is a project requiring JDBC</description>
			...
			<dependencies>
				...
				<dependency>
					<groupId>org.sonatype.mavenbook</groupId>
					<artifactId>baseDeps</artifactId>
					<version>1.0</version>
					<type>pom</type>
				</dependency>
			</dependencies>
		</project>
4. 继承 VS 多模块

	![继承 VS 多模块](https://books.sonatype.com/mvnref-book/reference/figs/web/pom_book-example.png)

5. 多模块企业级项目

	![实际应用多模块企业级项目](https://books.sonatype.com/mvnref-book/reference/figs/web/pom_real_multi.png)

### 第十章 构建生命周期

### 第十一章 构建Profile

### 11.1 什么是构建Profile

Profile能让你为一个特殊的环境自定义一个特殊的构建;profile使得不同环境间构建 的可移植性成为可能。

Profile可以配置package信息: debug, log等
Profile可以配置生产环境信息: develop版本，product版本，release版本等
Profile可以配置渠道信息：googleplay，360，豌豆荚等
更多的时候渠道分发包，使用Assembly插件来构建是非常适合的。

Profile类似gradle中的 Flavors配置，可以配置打出不同的包。

Maven中的profile是一组可选的配置，可以用来设置或者覆盖配置默认值。有了 profile，你就可以为不同的环境定制构建。profile可以在pom.xml中配置，并给定一 个id。然后你就可以在运行Maven的时候使用的命令行标记告诉Maven运行特定profile 中的目标。以下pom.xml使用production profile覆盖了默认的Compiler插件设置。

	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
	http://maven.apache.org/maven-v4_0_0.xsd">
		<modelVersion>4.0.0</modelVersion>
		<groupId>org.sonatype.mavenbook</groupId>
		<artifactId>simple</artifactId>
		<packaging>jar</packaging>
		<version>1.0-SNAPSHOT</version>
		<name>simple</name>
		<url>http://maven.apache.org</url>
		<dependencies>
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>3.8.1</version>
				<scope>test</scope>
			</dependency>
		</dependencies>
		<profiles>
			...
			<profile>
				<id>production</id>
				...
				<build>
					...
					<plugins>
						<plugin>
							<groupId>org.apache.maven.plugins</groupId>
							<artifactId>maven-compiler-plugin</artifactId>
							<configuration>
								<debug>false</debug>
								<optimize>true</optimize>
							</configuration>
						</plugin>
					</plugins>
				</build>
			</profile>
		</profiles>
	</project>

### 11.2 profile的语法
|序号|描述|
|----|----|
|1|pom.xml中的profiles元素，它包含了一个或者多个profile元素。由于profile覆 盖了pom.xml中的默认设置，profiles通常是pom.xml中的最后一个元素。|
|2|每个profile必须要有一个id元素。这个id元素包含的名字将在命令行调用profile 时被用到。我们可以通过传给Maven一个-P<profile_id>参数来调用profile。|
|3|一个profile元素可以包含很多其它元素，只要这些元素可以出现在POM XML文档 的project元素下面。本例中，我们覆盖了Compiler插件的行为，因此必须覆盖插 件配置，该配置通常位于一个build和一个plugins元素下面|
|4|我们覆盖了Maven Compiler插件的配置。确保通过production profile产生的字 节码不会包含调试信息，并且字节码会被编译器优化|

要使用production profile来运行mvn install，你需要在命令行传入-Pproduction参 数。要验证production profile覆盖了默认的Compiler插件配置，可以像这样以开启调 试输出(-X) 的方式运行Maven。

	mvn clean install -Pproduction -X
	
Profile中允许覆盖整个pom.xml，其节点可以如下：

	<project>
		<profiles>
			<profile>
				<build>
					<defaultGoal>...</defaultGoal>
					<finalName>...</finalName>
					<resources>...</resources>
					<testResources>...</testResources>
					<plugins>...</plugins>
				</build>
				<reporting>...</reporting>
				<modules>...</modules>
				<dependencies>...</dependencies>
				<dependencyManagement>...</dependencyManagement>
				<distributionManagement>...</distributionManagement>
				<repositories>...</repositories>
				<pluginRepositories>...</pluginRepositories>
				<properties>...</properties>
			</profile>
		</profiles>
	</project>

### 11.3 激活Profile

	当jdk为1.6时，jdk16这个profile会被激活（activation）
	<profiles>
		<profile>
			<id>jdk16</id>
			<activation>
				<jdk>1.6</jdk>
			</activation>
			<modules>
				<module>simple-script</module>
			</modules>
		</profile>
	</profiles>
	
### 11.4 当所有条件满足时才会激活Profile

	<project>
		...
		<profiles>
			<profile>
				<id>dev</id>
				<activation>
					<activeByDefault>false</activeByDefault>
					<jdk>1.5</jdk>
					<os>
						<name>Windows XP</name>
						<family>Windows</family>
						<arch>x86</arch>
						<version>5.1.2600</version>
					</os>
					<property>
						<name>mavenVersion</name>
						<value>2.0.5</value>
					</property>
					<file>
						<exists>file2.properties</exists>
						<missing>file1.properties</missing>
					</file>
				</activation>
				...
			</profile>
		</profiles>
	</project>
	
### 11.5 属性缺失时激活Profile

	<project>
		...
		<profiles>
			<profile>
				<id>development</id>
				<activation>
					<property>
						<name>!environment.type</name>
					</property>
				</activation>
			</profile>
		</profiles>
	</project>
	
### 11.6 外部Profile，将pom.xml中profile抽取成单独的profile.xml

	<profiles>
	<profile>
		<id>development</id>
		<build>
			<plugins>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-compiler-plugin</artifactId>
					<configuration>
						<debug>true</debug>
						<optimize>false</optimize>
					</configuration>
				</plugin>
			</plugins>
		</build>
	</profile>
	<profile>
		<id>production</id>
		<build>
			<plugins>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-compiler-plugin</artifactId>
					<configuration>
						<debug>false</debug>
						<optimize>true</optimize>
					</configuration>
				</plugin>
			</plugins>
		</build>
	</profile>
	</profiles>

### 11.7 Settings Profile

Settings Profile可以同时应用到你的所有maven构建中，就相当于默认profile,可以在以下两个地方定义Settings Profile。
	
	1. 定义在 ~/.m2/settings.xml中
	2. 定义在 /usr/local/maven/conf/settings.xml中
配置方式如：
	
	<settings>
		<profiles>
			<profile>
				<id>dev</id>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-jar-plugin</artifactId>
					<executions>
						<execution>
							<goals>
								<goal>sign</goal>
							</goals>
						</execution>
					</executions>
					<configuration>
						<keystore>/home/tobrien/java/keystore</keystore>
						<alias>tobrien</alias>
						<storepass>s3cr3tp@ssw0rd</storepass>
						<signedjar>
							/usr/local/hudson/hudson-home/jobs/maven-guide-zh-to-production/workspace/c
						</signedjar>
						<verify>true</verify>
					</configuration>
				</plugin>
			</profile>
		</profiles>
	</settings>


### 11.8 列出活动的Profiles

	mvn help:active-profiles
	
### 11.9 可以通过default profile来设定一些属性值

	例如：下面设置了一个activeByDefault = true的profile，并且将environment.type设置成dev。以后我们可以在pom.xml中根据environment.type是否等于dev来执行一些逻辑。
	<settings>
		<profiles>
			<profile>
				<activation>
					<activeByDefault>true</activeByDefault>
				</activation>
				<properties>
					<environment.type>dev</environment.type>
				</properties>
			</profile>
		</profiles>
	</settings>


### Maven套件

Markdown文件中图片制作与超链接获取


### 1. 上传图片到印象笔记
		1. 登陆《印象笔记》和《圈点》
		2. 打开印象笔记《圈点》
		3. 在圈点中编辑图片，然后点击同步。
		4. 打开《印象笔记》，找到刚才用《圈点》编辑并同步的图片笔记，不要打开。
		5. 在笔记上点击鼠标右键-->复制笔记链接
		6. 在浏览器中打开此链接
		7. 在打开的网页中需要的图片上点击鼠标右键-->复制图片地址
		8. 制作成如下图片链接样式即可  ![定义一个图片名称](复制的图片地址)

### 2. 上传笔记到简书
		1. control + c 复制准备好的图片
		2. 登陆《简书》
		3. 点击-->写文章-->新建文章-->control + v 粘贴图片到文章编辑框，并等待上传，
 		   上传完成之后会在文章编辑框生成 markdown 图片链接。直接拷贝到markdown文档中即可。

### 3. 上传图片到其他图片服务器


[okhttp官方github]:https://github.com/square/okhttp
[Linux命令大全]:http://man.linuxde.net
[计算机书籍在线阅读]:https://books.sonatype.com/
[Maven权威指南在线阅读]:https://www.sonatype.com/
[Maven plugins列表]:https://maven.apache.org/plugins/index.html
[Maven常用插件]:http://iffiffj.iteye.com/blog/1661936
[Maven simple-weather示例项目Git地址]:https://github.com/Farubaba/simple-example
[The Definitive Guide]:http://book.huihoo.com/maven-the-definitive-guide/multimodule-web-spring.html



