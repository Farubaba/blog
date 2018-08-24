---
title: OkHttp快速入门
date: 2018-08-05 19:13:46
tags: [OkHttp,http2,https,cache,IPv4,IPv6]
summery: 从OkHttp官方文档开始，一步步了解OkHttp，分析其源码，二次封装，提供Demo并发布到github，最终以组件的方式发布到仓库，提供gradle支持。
---

### 概述

>1. 学习OkHttp
2. OkHttp源码分析
3. 二次封装OkHttp
4. 参考github中其他二次封装库
5. 相关推荐

### Request结构

```
url

method

headers

body 

```

### Response结构

```
code

headers

body

```

### OkHttp 框架基本特性

1. Rewriting Requests: add absent headers like below

>	Content-Type
	Content-Length
	Transfer-Encoding
	User-Agent
	Host
	Connection
	Accept-Encoding : 便于response知道客户端能接收什么压缩类型的数据
	Cookie
	If-Modified-Since 和 If-None-Match ：for cache response

2. Rewriting Responses:
	
	If transparent compression was used, OkHttp will drop the corresponding response headers Content-Encoding and Content-Length because they don’t apply to the decompressed response body
	
>	Content-Encoding
	Content-Length

3. Follow-up Requests
	
>	302 redirect
	Authenticator

	
4. Retrying Requests
	
### OkHttp功能组件

|名称|说明|
|---|----|
| Calls||
| Dispatcher ||
| URL |url说明了是明文(http)传输还是密文传输(https);<br>url并没有说明具有用什么加密算法（cryptographic algorithms）来传输;<br>url也没有说明如何验证服务器端证书（certificates）[干这事儿得： HostnameVerifier];<br>url也没有说明哪一个证书应该被信任. [干这事儿得找：SSLSocketFactory ];<br>url也没有说明是否需要代理服务器（proxy server）;<br>url也没有说明如何验证（authenticate ）代理服务器|
| Address |Addresses specify a webserver (like github.com) and all of the static configuration necessary to connect to that server。In OkHttp some fields of the address come from the URL (scheme, hostname, port) and the rest come from the OkHttpClient.<br>|
| Route |Routes supply the dynamic information necessary to actually connect to a webserver. This is the specific IP address to attempt (as discovered by a DNS query), the exact proxy server to use (if a ProxySelector is in use), and which version of TLS to negotiate (for HTTPS connections).There may be many routes for a single address. For example, a webserver that is hosted in multiple datacenters may yield multiple IP addresses in its DNS response.|
|Connections|When you request a URL with OkHttp, here's what it does:<br><br><b>1</b>.It uses the URL and configured OkHttpClient to create an address. This address specifies how we'll connect to the webserver.<br><br><b>2</b>.It attempts to retrieve a connection with that address from the connection pool.<br><br><b>3</b>.If it doesn't find a connection in the pool, it selects a route to attempt. This usually means making a DNS request to get the server's IP addresses. It then selects a TLS version and proxy server if necessary.<br><br><b>4</b>.If it's a new route, it connects by building either a direct socket connection, a TLS tunnel (for HTTPS over an HTTP proxy), or a direct TLS connection. It does TLS handshakes as necessary.<br><br><b>5</b>.it sends the HTTP request and reads the response.<br><br>If there's a problem with the connection, OkHttp will select another route and try again. This allows OkHttp to recover when a subset of a server's addresses are unreachable. It's also useful when a pooled connection is stale or if the attempted TLS version is unsupported.Once the response has been received, the connection will be returned to the pool so it can be reused for a future request. Connections are evicted from the pool after a period of inactivity.|

### OkHttp基本用法步骤

1. 创建OkHttpClient对象
 
	```
	OkHttpClient okHttpClient = new OkHttpClient.Builder().build();
	```
2. 创建Request对象
	
	```
	Request request = new Request.Builder()
	        .url(host +"mobile-server/api/sys/config/v1")
	        .build();
	```
3. 创建Call
	
	```
	Call call = okHttpClient.newCall(request);
	```
4. 同步执行request
	
	```
	try {
        Response response = call.execute();
        String result = response.body().string();
    } catch (IOException e) {
        e.printStackTrace();
    }
	```
5. 异步执行request
	
	```
	call.enqueue(new Callback() {
        @Override
        public void onFailure(Call call, IOException e) {
                e.printStackTrace();
        }

        @Override
        public void onResponse(Call call, Response response) throws IOException {
        	  if(response.isSuccessful()){
				ResponseBody body = response.body();
				json = body.string();
				System.out.println(json);
        	  }
        }
    });
	```
	
### OkHttp UseCase

1. Asynchronous Get

```java
private final OkHttpClient client = new OkHttpClient();

  public void run() throws Exception {
    Request request = new Request.Builder()
        .url("http://publicobject.com/helloworld.txt")
        .build();

    client.newCall(request).enqueue(new Callback() {
      @Override public void onFailure(Call call, IOException e) {
        e.printStackTrace();
      }

      @Override public void onResponse(Call call, Response response) throws IOException {
        try (ResponseBody responseBody = response.body()) {
          if (!response.isSuccessful()) throw new IOException("Unexpected code " + response);

          Headers responseHeaders = response.headers();
          for (int i = 0, size = responseHeaders.size(); i < size; i++) {
            System.out.println(responseHeaders.name(i) + ": " + responseHeaders.value(i));
          }

          System.out.println(responseBody.string());
        }
      }
    });
  }
```
2. Accessing Headers

	Typically HTTP headers work like a Map<String, String>: each field has one value or none. But some headers permit multiple values, like Guava's Multimap. For example, it's legal and common for an HTTP response to supply multiple Vary headers. OkHttp's APIs attempt to make both cases comfortable
	
	headers通常是key-value组合，类似一个Map<String,String>，但也允许一个key对应多个value。类似Guava's [Multimap].Okhttp同时支持两种类型。
	
```java
private final OkHttpClient client = new OkHttpClient();

  public void run() throws Exception {
    Request request = new Request.Builder()
        .url("https://api.github.com/repos/square/okhttp/issues")
        .header("User-Agent", "OkHttp Headers.java")
        .addHeader("Accept", "application/json; q=0.5")
        .addHeader("Accept", "application/vnd.github.v3+json")
        .build();

    try (Response response = client.newCall(request).execute()) {
      if (!response.isSuccessful()) throw new IOException("Unexpected code " + response);

      System.out.println("Server: " + response.header("Server"));
      System.out.println("Date: " + response.header("Date"));
      System.out.println("Vary: " + response.headers("Vary"));
    }
  }
```

3. Posting a String

```java
public static final MediaType MEDIA_TYPE_MARKDOWN
      = MediaType.parse("text/x-markdown; charset=utf-8");

  private final OkHttpClient client = new OkHttpClient();

  public void run() throws Exception {
    String postBody = ""
        + "Releases\n"
        + "--------\n"
        + "\n"
        + " * _1.0_ May 6, 2013\n"
        + " * _1.1_ June 15, 2013\n"
        + " * _1.2_ August 11, 2013\n";

    Request request = new Request.Builder()
        .url("https://api.github.com/markdown/raw")
        .post(RequestBody.create(MEDIA_TYPE_MARKDOWN, postBody))
        .build();

    try (Response response = client.newCall(request).execute()) {
      if (!response.isSuccessful()) throw new IOException("Unexpected code " + response);

      System.out.println(response.body().string());
    }
  }
```
4. 
	

### Recipes
|名称|描述|
|----|----|
|string()|The string() method on response body is convenient and efficient for small documents. But if the response body is large (greater than 1 MiB), avoid string() because it will load the entire document into memory. In that case, prefer to process the body as a stream.|
|header(name,value)|use header(name, value) to set the only occurrence of name to value. If there are existing values, they will be removed before the new value is added|
|addHeader(name,value)|Use addHeader(name, value) to add a header without removing the headers already present|
|header(name)|从response中读取name对应的最后一个值，通常他就只有一个值，如果没有值，则返回null|
|headers(name)|从response中读取name对应的所有值|

### 参考链接

[okhttp square]

[okhttp github]

[okhttp github wiki]

[okhttp github]:https://github.com/square/okhttp
[okhttp square]:http://square.github.io/okhttp/
[okhttp github wiki]:https://github.com/square/okhttp/wiki
[Multimap]:http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/collect/Multimap.html

