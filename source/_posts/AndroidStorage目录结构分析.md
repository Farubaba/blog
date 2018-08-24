---
title: Android Storage目录结构及API分析
date: 2018-08-24 15:56:12
tags: [interanl storage,external storage,data,system,多用户]
summery: 分析不同版本android系统中internal storage 和  external storage的区别和异同，同时分析对应的api，及使用场景。能够更好的了解android系统存储，在开发中合理使用各种存储。
---

### 概要

本文主要对比Android 4.1、4.2 和 Android 6.0三个版本中系统storage路径差异和storage API使用差异。

	4.1 版本没有加入多用户系统。
	4.2 版本加入了多用户系统。
	6.0 版本也和4.2版本相同。

Android 4.1.1 系统目录结构：

<div align="center">
	<img src="/images/0、Android4.1.1系统目录结构.png" width="90%" />
</div>

Android 6.0 目录结构：

<div align="center">
	<img src="/images/0、Android6.0系统文件目录结构.png" width="90%" />
</div>

### 一、Android系统关键目录结构


<div align="center">
	<img src="/images/1、android存储目录结构.jpeg" width="90%" />
</div>

### 二、Android4.2之前存储目录结构

<div align="center">
	<img src="/images/2、android4.2之前存储目录结构.png" width="90%" />
</div>

### 三、Android4.2之后支持多用户的存储路径图

<div align="center">
	<img src="/images/3、android4.2支持多用户的存储路径图.png" width="90%" />
</div>


### 四、Android多用户存储


<div align="center">
	<img src="/images/4、android多用户存储.png" width="90%" />
</div>


### 五、Android-data目录


<div align="center">
	<img src="/images/5、android-data目录.png" width="90%" />
</div>

### 六、Android-system目录

<div align="center">
	<img src="/images/6、android-system目录.png" width="90%" />
</div>

### 七、Android-mnt目录

<div align="center">
	<img src="/images/7、android-mnt目录.png" width="90%" />
</div>

### 八、Android不同版本sdcard软链接

<div align="center">
	<img src="/images/8、android不同版本sdcard软链接.png" width="90%" />
</div>

### 九、Android-storage映射路径及API使用

 这张图片太大，截图里字显得有些小，可以右键-->在新标签中打开，字体会大一些。
	
<div align="center">
	<img src="/images/9、android-storage映射路径图.png" width="90%" />
</div>






