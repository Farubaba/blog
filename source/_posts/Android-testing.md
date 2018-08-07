---
title: Android测试：单元测试、集成测试、UI测试
date: 2018-07-27 23:55:20
tags:
summery: 关于Android测试，很早就有了尝试，但由于业务耦合，测试起来相当困难，几乎是弊大于利。自从android-testing之后，测试变得不那么神秘，今天就来一起了解在Android中如何正确的做各种测试。

---

### 相关资源

[网友博客android-testing]

[Mockito官网]

[BDDMockito guide]

[Use Mockito in Android with dexmaker]

[TDD]

[BDD]

[Test-Driven Development on Android with the Android Testing Support Library (Google I/O '17)]

[jetpack]



### 测试驱动开发TDD（test-driven development）

<div align="center">
	<img src="/images/TDD-testing-workflow.png" width="90%" />
</div>

1. Unit Test 				(Small Tests)
3. UI Test   				(Large Tests)

### 测试金字塔


<div align="center">
	<img src="/images/testing-pyramid-2x.png" width="90%" />
</div>

1. Unit Test 				(Small Tests)   

```
推荐多写单元测试，因为他运行在本地电脑，并且mocked或者stubed很多至关重要的核心组件，例如Android中Context对象；
该类测试旨在测试语法，异常，逻辑是否如我们所设计的一样被执行。并不能说明程序就没有错误。提前发现错误
```
	  
2. integration Test		(Medium Tests)

```
this kind of Tests told you that your function is actually works。
这种测试被运行在真机或者模拟器中，用于测试你的程序真的可以正常工作。
```
3. UI Test   				(Large Tests)


推荐各种类型的测试用例占比为： <b>70 percent small, 20 percent medium, and 10 percent large.</b>

### 多种测试框架及辅助测试工具

|名称|描述|作用|
|----|----|-----|
| Junit Test |普通单元测试||
| InstrumentationTestRunner |Android中只支持Junit3，已过时||
| AndroidJunitRunner |Android单元测试，替代InstrumentationTestRunner |支持Junit3和Junit4在Android中使用|
| AndroidJunit4 ||Android中使用@RunWith(AndroidJUnit4.class)|
| MockitoJUnitRunner|||
| Mockito |Stubed & Mocked 对象||
| Robolectric |类似Mockito||
| UI Automator |Android UI 黑盒自动化测试（又称功能测试），依赖于AndroidJunitRunner ||
| Espresso |Android UI 白盒自动化测试，依赖于AndroidJunitRunner ||
| ActivityInstrumentationTestCase2 |已过时||
| ActivityTestRule |替代ActivityInstrumentationTestCase2 ||
| ServiceTestRule |不支持IntentService||

1. org.junit.runners.JUnit4

```gradle
	apply plugin: 'com.android.application'
	
	android {
	    compileSdkVersion 26
	    buildToolsVersion rootProject.buildToolsVersion
	    defaultConfig {
	        applicationId "com.example.android.testing.unittesting.BasicSample"
	        minSdkVersion 9
	        versionCode 1
	        versionName "1.0"
	        targetSdkVersion 26
	    }
	    productFlavors {
	    }
	}
	
	dependencies {
	    // Unit testing dependencies.
	    testCompile 'junit:junit:' + rootProject.junitVersion;
	    testCompile "org.mockito:mockito-inline:+"
	    //testCompile 'org.mockito:mockito-core:' + rootProject.mockitoVersion;
	}

```
	
2. android.support.test.runner.AndroidJUnitRunner

```gradle
	apply plugin: 'com.android.application'
		
	android {
	    compileSdkVersion 26
	    buildToolsVersion rootProject.buildToolsVersion
		
	    defaultConfig {
	        applicationId "com.example.android.testing.unittesting.basicunitandroidtest"
	        minSdkVersion 9
	        targetSdkVersion 26
	        versionCode 1
	        versionName "1.0"
	        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
	    }
	}
		
	dependencies {
	    // Testing-only dependencies
	    // Force usage of support annotations in the test app, since it is internally used by the runner module.
	    androidTestCompile 'com.android.support:support-annotations:' + rootProject.supportLibVersion;
	    androidTestCompile 'com.android.support.test:runner:' + rootProject.runnerVersion;
	    androidTestCompile 'org.hamcrest:hamcrest-library:' + rootProject.hamcrestVersion;
	}

```

3. 水电费

```gradle

```

4. sfd
	

### 参考文档

[Mockito guide]

[Mocktio 语法简介]

### Mock Android Issue Track

[mockito android issues]

[mockito android issues]:https://github.com/mockito/mockito/issues/new

### Android项目中引入Mockito支持

1. 使用官方提供的androidTestCompile支持

	```gradle
	 repositories {
	   jcenter()
	 }
	 dependencies {
	   //对普通java mock测试支持
	   testCompile "org.mockito:mockito-core:+"
	   //mockito官方于mockito 2.6.1开始对android依赖库提供mock支持
	   androidTestCompile "org.mockito:mockito-android:+"
	 }
	 
	```
   
2. 使用LinkedIn公司dexmaker团队基于mockito提供的androidTestCompile支持

	```gradle
	 repositories {
	   jcenter()
	 }
	 dependencies {
	   //对普通java mock测试支持
	   testCompile "org.mockito:mockito-core:+"
	   //使用LinkedIn公司dexmaker团队基于mockito提供的androidTestCompile支持
	   androidTestCompile 'com.linkedin.dexmaker:dexmaker-mockito:2.19.0'
	 }
	 
	```
	
3. Mocking final types, enums and final methods (Since 2.1.0)

	```gradle
	repositories {
	   jcenter()
	 }
	 dependencies {
	   //想要mock final types，enums，final methods
	   //使用mockito-inline替换mockito-core
	   testCompile "org.mockito:mockito-inline:+"
	   //androidTestCompile "org.mockito:mockito-android:+"
	   //androidTestCompile 'com.linkedin.dexmaker:dexmaker-mockito:2.19.0'
	 }
	```

4. sdfsf

### 初始化mockito及MockitoAnnotations

1. MockitoAnnotations
1. MockitoJUnitRunner
2. MockitoRule

[Mockito官网]:http://site.mockito.org/#training
[Mockito guide]:http://static.javadoc.io/org.mockito/mockito-core/2.20.0/org/mockito/Mockito.html
[Mocktio 语法简介]:https://blog.csdn.net/qq_17766199/article/details/78450007
[BDDMockito guide]:http://static.javadoc.io/org.mockito/mockito-core/2.20.0/org/mockito/BDDMockito.html
[Use Mockito in Android with dexmaker]:https://github.com/linkedin/dexmaker
[TDD]:https://baike.baidu.com/item/TDD/9064369
[Test-Driven Development on Android with the Android Testing Support Library (Google I/O '17)]:https://www.youtube.com/watch?v=pK7W5npkhho&start=111
[jetpack]:https://developer.android.com/jetpack/
[网友博客android-testing]:https://blog.csdn.net/u012496940/article/details/49081151
[BDD]: