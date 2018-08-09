---
title: Android项目架构概述
date: 2018-07-24 10:55:39
tags: [android,架构,faruAndroid]
summery: Android项目架构概述
---

### 目标
		整合时下优秀的开源框架，实现一个高度解耦，可快速开发，易复用，易扩展的Android基础库。涉及到常规应用开发的各方面。
		
### 关注点

1. code规范
1. 通用基础库(Base*、Util)
2. 业务组件封装
7. 架构（MVP、MVVM）
8. 抽象中间层
9. 解耦，易替换，易删除
10. 快速易开发
11. 易测试（自动化测试，单元测试等）
11. 持续集成CI
12. 渠道打包
13. 灰度发布
14. 强制更新
15. 异步任务调度
16. View & Animation
4. 组件化
5. 插件化
6. 热更新
	
### 第三方库

|名称|功能描述（见WIKI）|同类项目|相关优化方向|相关产考|
|---|---|---|---|---|
|[Android Jetpack]|||||
|[阿里 sophix]|||||
|[阿里 Atlas]|||||
|[阿里 Small]|||||
|[阿里 AndFix]|||||
|[Kotlin]|||||
|[React Native]|||||
|[Android Testing]|||||
|[MVP]|||||
|[MVVM]|||||
|[RxJava]|异步编程|||[给 Android 开发者的 RxJava 详解]|
|[RxAndroid]|异步编程||||
|[RxPermissions]|RxJava支持6.0权限||||
|[Retrofit2]|网络访问封装||||
|[Dagger2]|对象注入||||
| [Logger] |日志记录|[Timer]|无埋点日志、无埋点统计||
|[RxAndroid Tutorial]|指南||||
|[RxBinding]|||||
|[retrofit-adapters for rxjava2]|基础封装||||
|[OKHTTP3]|网络访问||||
| [Fresco] |图片处理||||
| [Butterknife] |View注入||||
|[JsBridge]|hybird||||
|[material-dialogs]|Dialog||||
|[MPAndroidChart]|图表||||
|[airbnb-lottie-android]|AirBnB 动画框架，渲染高级动效|||[Material-Animations]、[recyclerview-animators]|
|[AVLoadingIndicatorView]|Loading动画||||
|[uCrop]|图片剪切||||
||推送服务||||
||推送后台管理页面开发-开源项目-daquan||||
||社会化||||
||数据统计||||
||单元测试||||
||接入SDK||||
||数据加解密||||
||业务流程动态配置化管理||||
||应用模块权限配置管理||||
||应用模块动态配置||||
||配置文件动态下发||||
||页面跳转动态配置管理||||
||渠道配置||||
||代码混淆||||
||应用加壳||||
||自动化测试||||
||灰度发布||||
|[AppUpdate]|应用版本更新模块||||
||插件化，版本更新||||
||热修复，bug修复||||
||相机||||
||图片||||
||视频||||
||音频||||
||即时通信IM||||
||PDF文件在线查看，下载查看||||
||状态栏||||
||标题栏||||
||Back键||||
||手势识别||||
||主菜单||||


### 开发、测试、集成、发布、灰度工具

|名称|功能描述（见WIKI）|相关产考|
|---|---|---|
|[接口文档管理工具 Postman、Swagger、RAP]|接口测试|无|

### 新项目建议

1. 新项目建议采用 minSdkVersion="14"（覆盖目前99%设备） 或者更高版本，采用minSdkVersion="17"（覆盖目前96%设备） 可直接越过webview js漏洞。



### 相关参考
[接口文档管理工具 Postman、Swagger、RAP]:http://qa.blog.163.com/blog/static/190147002201611155418810/

[Logger]:https://github.com/orhanobut/logger
[Timer]:https://github.com/JakeWharton/timber
[RxAndroid Tutorial]:https://www.raywenderlich.com/170233/reactive-programming-rxandroid-kotlin-introduction
[RxJava]:https://github.com/ReactiveX/RxJava
[RxAndroid]:https://github.com/ReactiveX/RxAndroid
[RxPermissions]:https://github.com/tbruyelle/RxPermissions
[retrofit-adapters for rxjava2]:https://github.com/square/retrofit/tree/master/retrofit-adapters/rxjava2
[给 Android 开发者的 RxJava 详解]:http://gank.io/post/560e15be2dca930e00da1083
[RxBinding]:https://github.com/JakeWharton/RxBinding

[Retrofit2]:https://github.com/square/retrofit


[Butterknife]:https://github.com/JakeWharton/butterknife
[OKHTTP3]:https://github.com/square/okhttp
[Dagger2]:https://github.com/JakeWharton/dagger

[Fresco]:https://github.com/facebook/fresco
[AppUpdate]:https://github.com/WVector/AppUpdate
[JsBridge]:https://github.com/lzyzsd/JsBridge
[material-dialogs]:https://github.com/afollestad/material-dialogs
[MPAndroidChart]:https://github.com/PhilJay/MPAndroidChart
[airbnb-lottie-android]:https://github.com/airbnb/lottie-android
[Material-Animations]:https://github.com/lgvalle/Material-Animations
[recyclerview-animators]:https://github.com/wasabeef/recyclerview-animators
[AVLoadingIndicatorView]:https://github.com/81813780/AVLoadingIndicatorView
[uCrop]:https://github.com/Yalantis/uCrop

### 和当前项目类似项目  

[AndroidUtilCode]   :  [AndroidUtilCode功能列表]  借鉴其通用基础模块封装

[android-common] : 借鉴其通用基础模块封装
   
[EasyAndroid] 

[XSnow]   

[EasyAndroid]:https://github.com/wu928320442/EasyAndroid
[AndroidUtilCode]:https://github.com/Blankj/AndroidUtilCode
[AndroidUtilCode功能列表]:http://blog.csdn.net/qq_33445600/article/details/78487857
[XSnow]:https://github.com/xiaoyaoyou1212/XSnow
[android-common]:https://github.com/litesuits/android-common