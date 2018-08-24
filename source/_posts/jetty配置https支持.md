---
title: Jetty配置https支持
date: 2018-08-14 01:04:38
tags: [Jetty,HTTPS, SSL, TLS, Http2]
summery: 使用jetty xml ioc 为Jetty服务器配置https支持。了解jetty各个层级的配置文件，如.ini, .xml 等
---
### 一、jetty配置https支持

[https握手链接过程详解]

[jetty javadoc for 配置]

[ERR\_SSL\_VERSION\_OR\_CIPHER\_MISMATCH 错误]

/start.ini  > /etc/jetty-http.xml > /demo-base/start.d/http.ini(只对当前应用有效)

|配置文件|配置对象|存放位置|作用|相关文件|
|----|----|----|----|----|
|jetty.xml|org.eclipse.jetty.server.Server||||
|jetty-web.xml|org.eclipse.jetty.webapp.WebAppContext|/WEB-INF/jetty-web.xml|配置单一项目||
|jetty-env.xml|org.eclipse.jetty.webapp.WebAppContext|WEB-INF/jetty-env.xml|配置单一项目的JNDI|[jetty-env.xml配置]|
|webdefault.xml|org.eclipse.jetty.webapp.WebAppContext|jetty.home/etc/webdefault.xml|before web.xml defaultsDescriptor ||
|jetty-deploy.xml||JETTY-HOME/etc/jetty-deploy.xml|||
|override-web.xml|org.eclipse.jetty.webapp.WebAppContext||after web.xml overrideDescriptor ||


[jetty javadoc for 配置]

[see in Jetty 8 SSL_Connectors]


[maven jetty9 plugin 配置https]

[jetty-documentation]

[jetty-config-connectors]

[jetty-configuring-ssl]

[configuring-jetty-maven-plugin]



#### 1. TLS and SSL versions

Which browser/OS supports which protocols can be found on Wikipedia.[Transport\_Layer\_Security in Web\_browsers]

```
TLS v1.0 -> TLS v1.1 -> TLS v1.2
SSL v1   -> SSL v2   -> SSL v3
```

Older Protocols

```
TLS v1.0, v1.1 and SSL v3 are no longer supported by default.
```

New Protocol

```
TLS v1.2
The protocol which should be used wherever possible. All CBC based ciphers are supported since Java 7, the new GCM modes are supported since Java 8
```

名词解释：

CA:

CSR:

key:

certificate:

keystore:(mac中存储在 /Users/violet/.keystore)

<http://www.eclipse.org/jetty/documentation/current/configuring-ssl.html#generating-key-pairs-and-certificates-JDK-keytool>

Certificates:

<http://en.tldp.org/HOWTO/SSL-Certificates-HOWTO/index.html>

<http://mindprod.com/jgloss/certificate.html>

Keytool:

<https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html>

<https://docs.oracle.com/javase/8/docs/technotes/tools/windows/keytool.html>

Other tools:

<https://www.ibm.com/developerworks/community/groups/service/html/communityview?communityUuid=6fb00498-f6ea-4f65-bf0c-adc5bd0c5fcc>

OpenSSL:

<https://www.openssl.org/docs/faq.html>

#### 2. 使用keytool生成minimal requirements key to run an SSL

The following command generates a key pair and certificate directly into file keystore

```
keytool -keystore keystore -alias jetty -genkey -keyalg RSA

```

完整一点的key,需要提供更多的信息，正式环境还不能使用它，但在测试环境中足够了。

```
keytool -keystore keystore -alias jetty -genkey -keyalg RSA -sigalg SHA256withRSA
```

#### 4. 创建适合商用SSL 的key
<http://www.eclipse.org/jetty/documentation/current/configuring-ssl.html#generating-csr-from-keytool>

#### 3. 独立安装的Jetty配置https支持

1). 相关命令

	```
		java -jar $JETTY_HOME/start.jar --list-modules=connector
		
		java -jar $JETTY_HOME/start.jar --create-startd
		
		java -jar $JETTY_HOME/start.jar --add-to-start=http,https
		
		tree
			.
			├── etc
			│   └── keystore
			└── start.d
			    ├── http.ini
			    └── https.ini
		
		java -jar $JETTY_HOME/start.jar
		
	```
2). 修改https.ini


### 二、常见链接中间人：反向代理（Reverse Proxy）和负载均衡（Load Banlancer）

Often a Connector needs to be configured to accept connections from an intermediary such as a Reverse Proxy and/or Load Balancer deployed in front of the server.

[ERR\_SSL\_VERSION\_OR\_CIPHER\_MISMATCH 错误]:https://blog.csdn.net/u013332124/article/details/79480665
[WireShark抓包for Mac]:https://www.jianshu.com/p/c67baf5fce6d
[https握手链接过程详解]:https://www.jianshu.com/p/7158568e4867
[see in Jetty 8 SSL_Connectors]:https://wiki.eclipse.org/Jetty/Reference/SSL_Connectors
[jetty-env.xml配置]:http://www.eclipse.org/jetty/documentation/current/jetty-env-xml.html
[jetty javadoc for 配置]:http://www.eclipse.org/jetty/javadoc/9.4.11.v20180605/overview-summary.html

[maven jetty9 plugin 配置https]:http://zhangwei8607.iteye.com/blog/2205127
[jetty-config-connectors]:http://www.eclipse.org/jetty/documentation/current/configuring-connectors.html
[configuring-jetty-maven-plugin]:http://www.eclipse.org/jetty/documentation/current/jetty-maven-plugin.html
[jetty-documentation]:http://www.eclipse.org/jetty/documentation/current/

[jetty-configuring-ssl]:http://www.eclipse.org/jetty/documentation/current/configuring-ssl.html
[Transport\_Layer\_Security in Web\_browsers]:https://en.wikipedia.org/wiki/Transport_Layer_Security#Web_browsers



