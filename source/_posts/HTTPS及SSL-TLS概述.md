---
title: HTTPS及SSL/TLS概述
date: 2018-09-03 17:30:28
tags: [SSL/TLS,Cipher Suites,对称加密,非对称加密,随机数,证书,Certificate,pre Master Secret,Master Secret,会话秘钥,CA,Public key,Private key]
summery: 理解SSL/TLS加密套件如何配置HTTP协议实现完全的加密传输协议HTTPS。并了解整个HTTP协议握手和传输阶段的各个流程，以及流程中涉及到的各种专业术语。例如：SSL/TLS、Cipher Suites、Cipher Suite、对称加密、非对称加密、随机数、证书Certificate、pre Master Secret、主秘钥Master Secret、会话秘钥、CA、Public key、Private key等等。
---

### 参考信息

1. 快速理解Http及Https相关基础信息在Java中的相关术语：[Apache Http Client Tutorial]
2. [为什么Android中证书不能使用JKS，而只能使用BKS] 和 [android-trusting-ssl-certificates]
3. [Android使用BKS证书详细说明]
2. [Android下Https使用自定义证书]
3. <b>[Android Developer: android-security-ssl Best Practice]</b>
4. [Android Webview SSL 自签名安全校验解决方案]
2. JDK中相关的package
	
	>java.net
	javax.net
	javax.net.ssl
	javax.security
	javax.security.cert
	
### 一、SSL/TLS通信模型概要


<div align="center">
	<img src="/images/10、SSL_TSL通信模型概要.png" width="90%" />
</div>

### 二、SSL/TLS通信模型时序图简要说明


<div align="center">
	<img src="/images/11、SSL_TSL通信模型时序图.png" width="90%" />
</div>

### 三、javax.net.ssl.HttpsURLConnection分析

<div align="center">
	<img src="/images/12、javax.net.ssl.HttpsUrlConnection方法分析.png" width="90%" />
</div>

构成HttpsURLConnection的关键对象有：

|名称|作用|示例|
|----|----|----|
|SSLSocketFactory|创建SSLSocket对象，用于建立SSLSocket连接||
|CipherSuite|密码套件，用于https协议握手，传输过程中的加解密套件。“密码套件”是给定的 SSL 连接所使用的加密算法组合，在协商过程中，两个端点必须对可用于双方环境的密码套件达成一致。如果不存在这种公共的套件，就不能建立 SSL 连接，也不能交换数据。||
|HostnameVerifier|javax.net.ssl.HostnameVerifier <br>此类是用于主机名验证的基接口。在握手期间，如果 URL 的主机名和服务器的标识主机名不匹配，则验证机制可以回调此接口的实现程序来确定是否应该允许此连接。<br><b>策略可以是基于证书的或依赖于其他验证方案。</b><br>当验证 URL 主机名使用的默认规则失败时使用这些回调||
|Certificate|类java.security.cert.Certificate<br>管理各种身份证书的抽象类，身份证书是一个主体与由另一个主体所担保的公钥之间的绑定关系。（一个主体表示一种实体，如个体用户、一个用户组或一家公司）。此类是具有不同格式但是很常用的证书的抽象。例如，不同的证书类型（如 X.509 和 PGP）共享通用的证书功能（如编码和验证）和某些信息类型（如公钥）。<br>虽然 X.509、PGP 和 SDSI 证书包含不同的信息集，并且它们以不同的方式存储和获取信息，但都可以通过继承 Certificate 类来实现它们||
|Certificate[]|证书链，包括服务器证书链和客户端证书链||
|Principal|证书实体，及证书拥有者和签发者。能代表多个实体:<br>1）证书拥有者 getSubjectX500Principal()<br>2）证书签发者 getIssuerX500Principal()<br>参考：java.security.cert.X509Certificate中上述方法描述 | 无|


### 四、javax.net.ssl.SSLSocket分析

<div align="center">
	<img src="/images/13、javax.net.ssl.SSLSocket分析.png" width="90%" />
</div>

构建SSLSocket类的关键对象如下：

|名称|作用|示例|
|----|----|----|
|SSLContext|此类的实例表示安全套接字协议的实现，它充当用于安全套接字工厂(SSLSocketFactory)或 SSLEngine 的工厂。用可选的一组密钥和信任管理器及安全随机字节源初始化此类。||
|SSLSocket||无|
| SSLSession|||
|SSLParameters|||

### 五、相关知识

|术语|描述|示例|
|----|----|----|
|SSL/TLS|||
|Cipher Suite|||
|对称加密|||
|非对称加密|||
|BouncyCastle|||
|Certificate|||
|CA|||
|crt|||
|cer|||
|bks证书|android 平台支持bks格式证书文件|生成bks文件命令：<br> keytool -importcert -v -trustcacerts -alias certfarubaba -file server.crt -keystore farubaba.bks -storetype BKS -providerclass org.bouncycastle.jce.provider.BouncyCastleProvider -providerpath ./bcprov-jdk15on-146.jar -storepass 123456 其中server.crt是原格式证书 farubaba.bks是目标生成证书 123456是bks格式文件证书密码|
|Keystore|||
|TrustStore|||
|keytool|[Java 安全套接字编程以及 keytool 使用最佳实践]||
|openssl|||
|主密钥|||
|会话秘钥|||
|HostnameVerify|||
|SSLSocketFactory|||
|X.509|||
|TrustManger|||


[Apache Http Client Tutorial]:http://hc.apache.org/httpcomponents-client-4.5.x/tutorial/html/index.html
[Java 安全套接字编程以及 keytool 使用最佳实践]:https://www.ibm.com/developerworks/cn/java/j-lo-socketkeytool/index.html?ca=drs
[Android下Https使用自定义证书]:https://blog.csdn.net/raptor/article/details/18898937
[为什么Android中证书不能使用JKS，而只能使用BKS]:https://stackoverflow.com/questions/11117486/wrong-version-of-keystore-on-android-call
[android-trusting-ssl-certificates]:http://blog.crazybob.org/2010/02/android-trusting-ssl-certificates.html
[Android使用BKS证书详细说明]:http://nelenkov.blogspot.com/2011/12/using-custom-certificate-trust-store-on.html
[Android Developer: android-security-ssl Best Practice]:https://developer.android.com/training/articles/security-ssl
[Android Webview SSL 自签名安全校验解决方案]:https://www.cnblogs.com/liyiran/p/7011317.html