---
title: Best Practices of HTTPS&TLS in Android
date: 2018-09-11 16:11:48
tags: [SSL/TLS,Cipher Suites,对称加密,非对称加密,随机数,证书,Certificate,pre Master Secret,Master Secret,会话秘钥,CA,Public key,Private key]
summery: 理解SSL/TLS加密套件如何配置HTTP协议实现完全的加密传输协议HTTPS。并了解整个HTTP协议握手和传输阶段的各个流程，以及流程中涉及到的各种专业术语。例如：SSL/TLS、Cipher Suites、Cipher Suite、对称加密、非对称加密、随机数、证书Certificate、pre Master Secret、主秘钥Master Secret、会话秘钥、CA、Public key、Private key等等。
---

### 参考信息

[app security best prectices]

[https-locally-without-browser-privacy-errors]
	
[app security best prectices]:https://developer.android.com/topic/security/best-practices#java

[app signing]:https://developer.android.com/studio/publish/app-signing
[network security configuration]:https://developer.android.com/topic/security/best-practices#network-security-config
[unknown cerificate authority]:https://developer.android.com/training/articles/security-ssl.html#UnknownCa
[TrustManager]:https://developer.android.com/reference/javax/net/ssl/TrustManager
[TrustManagerFactory]:https://developer.android.com/reference/javax/net/ssl/TrustManagerFactory
[CertificateFactory]:https://developer.android.com/reference/java/security/cert/CertificateFactory
[https-locally-without-browser-privacy-errors]:https://deliciousbrains.com/https-locally-without-browser-privacy-errors/
### 相关参考

|名称|描述|相关|
|----|----|----|
|[app security best prectices]|||
|[app signing]|||
|[Certificate Authority (CA)]|CA 是数字证书授权机构||
|[certificate]|数字证书||
|[CertificateFactory]|此类定义了用于从相关的编码中生成证书、证书路径 (CertPath) 和证书撤消列表 (CRL) 对象的 CertificateFactory 功能。Android只支持X.509类型的CertificateFactory <p>which is used to generate certificate, certification path (CertPath : a certificate chain) and certificate revocation list (CRL) objects from their encodings.|A certificate factory for X.509 must return certificates that are an instance of java.security.cert.X509Certificate<br>and <br>CRLs that are an instance of java.security.cert.X509CRL|
|[TrustManager]|不应该信任所有证书，应该处理好以下三种情况：<p> 1. You're communicating with a web server that has a certificate signed by a new or custom CA.<br> 2. That CA isn't trusted by the device you're using.<br> 3. You cannot use a [network security configuration].<p>To learn more about how to complete these steps, see the discussion about handling an [unknown cerificate authority]|[unknown cerificate authority]|
|[TrustManagerFactory]|用于生成[TrustManager]||
|[Keystore]|包含了证书（Certificate）和公钥（public key）信息，是[TrustManager]验证的对象||

### Eclipse Tomcat 配置https支持

1. 在conf目录下存放keystore,开发环境中keystore应该放在这里。
	
	```java
	/environment/eclipse/.metadata/.plugins/org.eclipse.wst.server.core/tmp0/conf/
	```
		
1. 在server.xml中增加https connector。
	
	```xml
	 <Connector SSLEnabled="true" 
		 clientAuth="false" 
		 keystoreFile="conf/farubaba.keystore" 
		 keystorePass="123456" maxThreads="200" port="8443"
		 protocol="org.apache.coyote.http11.Http11NioProtocol" 
		 scheme="https" 
		 secure="true" 
		 sslProtocol="TLS">
    </Connector>
	```
3. 如果keystore位置配置错误，tomcat找不到则会报错，需要在错误信息中的提示位置防止服务器的keystore。
	
	```java
	严重: Failed to initialize connector [Connector[HTTP/1.1-8443]]
org.apache.catalina.LifecycleException: Failed to initialize component [Connector[HTTP/1.1-8443]]
	at org.apache.catalina.util.LifecycleBase.init(LifecycleBase.java:112)
	at org.apache.catalina.core.StandardService.initInternal(StandardService.java:549)
	at org.apache.catalina.util.LifecycleBase.init(LifecycleBase.java:107)
	at org.apache.catalina.core.StandardServer.initInternal(StandardServer.java:875)
	at org.apache.catalina.util.LifecycleBase.init(LifecycleBase.java:107)
	at org.apache.catalina.startup.Catalina.load(Catalina.java:632)
	at org.apache.catalina.startup.Catalina.load(Catalina.java:655)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:497)
	at org.apache.catalina.startup.Bootstrap.load(Bootstrap.java:309)
	at org.apache.catalina.startup.Bootstrap.main(Bootstrap.java:492)
Caused by: org.apache.catalina.LifecycleException: Protocol handler initialization failed
	at org.apache.catalina.connector.Connector.initInternal(Connector.java:995)
	at org.apache.catalina.util.LifecycleBase.init(LifecycleBase.java:107)
	... 12 more
Caused by: java.lang.IllegalArgumentException: /environment/eclipse/.metadata/.plugins/org.eclipse.wst.server.core/tmp0/conf/farubaba.keystore (No such file or directory)
	at org.apache.tomcat.util.net.AbstractJsseEndpoint.createSSLContext(AbstractJsseEndpoint.java:116)
	at org.apache.tomcat.util.net.AbstractJsseEndpoint.initialiseSsl(AbstractJsseEndpoint.java:87)
	at org.apache.tomcat.util.net.NioEndpoint.bind(NioEndpoint.java:225)
	at org.apache.tomcat.util.net.AbstractEndpoint.init(AbstractEndpoint.java:1086)
	at org.apache.tomcat.util.net.AbstractJsseEndpoint.init(AbstractJsseEndpoint.java:268)
	at org.apache.coyote.AbstractProtocol.init(AbstractProtocol.java:581)
	at org.apache.coyote.http11.AbstractHttp11Protocol.init(AbstractHttp11Protocol.java:68)
	at org.apache.catalina.connector.Connector.initInternal(Connector.java:993)
	... 13 more
Caused by: java.io.FileNotFoundException: /environment/eclipse/.metadata/.plugins/org.eclipse.wst.server.core/tmp0/conf/farubaba.keystore (No such file or directory)
	at java.io.FileInputStream.open0(Native Method)
	at java.io.FileInputStream.open(FileInputStream.java:195)
	at java.io.FileInputStream.<init>(FileInputStream.java:138)
	at java.io.FileInputStream.<init>(FileInputStream.java:93)
	at sun.net.www.protocol.file.FileURLConnection.connect(FileURLConnection.java:90)
	at sun.net.www.protocol.file.FileURLConnection.getInputStream(FileURLConnection.java:188)
	at org.apache.tomcat.util.file.ConfigFileLoader.getInputStream(ConfigFileLoader.java:96)
	at org.apache.tomcat.util.net.SSLUtilBase.getStore(SSLUtilBase.java:132)
	at org.apache.tomcat.util.net.SSLHostConfigCertificate.getCertificateKeystore(SSLHostConfigCertificate.java:204)
	at org.apache.tomcat.util.net.jsse.JSSEUtil.getKeyManagers(JSSEUtil.java:184)
	at org.apache.tomcat.util.net.AbstractJsseEndpoint.createSSLContext(AbstractJsseEndpoint.java:114)
	... 20 more
	```
