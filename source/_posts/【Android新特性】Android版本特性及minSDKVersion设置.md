### Android版本特性及minSDKVersion设置


		推荐：minSDKVersion=17 
		推荐：提供一套 xhdpi （UI设计按：720x1080设计）图片来做大部分图片缩放适配。
		
		下面内容是详细分析过程。
		
		
#### 一、参考资料
[Android dashboards]  

		minSDKVersion理解:
		An integer designating the minimum API Level required for the application to run. 
		The Android system will prevent the user from installing the application if the 
		system's API Level is lower than the value specified in this attribute. You 
		should always declare this attribute.

		应用程序运行所需要的最小api版本，如果手机的系统版本低于app设置的minSDKVersion，
		那么android系统会阻止app安装。始终应该设置minSDKVersion。


#### 二、Android系统版本特性
|系统版本号|版本名称sdkVersion|Revision|版本特性|关键特性|
|-----|-----|-----|----|----|
||27|1|||
|8.0|26 Oreo |1|[android-8.0-changes]||
|7.1|25 Nougat|2|||
|7.0|24 Nougat|2|[android-7.0-changes]||
|6.0|23 Marshmallow |3|[android-6.0-changes]|Runtime Permission|
|5.1|22 Lollipop |2|[android-5.1]||
|5.0|21 Lollipop |2|[android-5.0-changes]|1.ART、自动Multidex  2.Instant Run <br>3.支持 vector drawables|
|4.4W|20 KitKat Wear|2|||
|4.4|19 KitKat |4|[android-4.4]|Android 4.4 (API level 20) and lower doesn't support vector drawables, but can use Support Library.If you want to use vector drawables only, you can use <font color="red">Android Support Library 23.2 or higher,Android Plugin for Gradle 2.0 or higher</font> The <code>VectorDrawableCompat</code> class in the Support Library allows you to support VectorDrawable in <font color="red">Android 2.1 (API level 7) and higher.</font> [android-support-library-232]|
|4.3|18 Jelly Bean|3|[android-4.3]|1.webp lossless compress & transparent alpha channel support|
|4.2|17 Jelly Bean |3|[android-4.2]|强制@JavascriptInterface|
|4.1|16 Jelly Bean |5|[android-4.1]||
|4.0.3|15 IcecreamSandwich|5|[android-4.0.3]||
|4.0|14 IcecreamSandwich|4|[android-4.0]|Webp lossy compress [convert images to webp]|
|3.2|13 HoneyComb|1|[android-3.2]| WebP file format for your images, when targeting Android 3.2 (API level 13) and higher|
|3.1|12 HoneyComb|3|[android-3.1]||
|3.0|11 HoneyComb|2|[android-3.0]||
|2.3.3|10 Gingerbread|2|[android-2.3.3]||
|2.3|9 Gingerbread|2|[android-2.3]||
|2.2|8 Froyo |3|[android-2.2]||
|2.1|7 Eclair |3|[android-2.1]||



#### 三、Android各系统版本占有率分布

##### 1、实时数据参考：[Android dashboards]    


![Android Platform Version Percent](http://upload-images.jianshu.io/upload_images/6322932-b814708f69f28120.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 2、Android Studio中创建项目时提供参考数据    

![Android系统版本分布](http://upload-images.jianshu.io/upload_images/6322932-6a2811470cd8a4ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 四、Android现有设备屏幕密度分布
##### 1、实时数据参考：[Android dashboards]  

![Android现有设备屏幕密度分布](http://upload-images.jianshu.io/upload_images/6322932-3c5ac8a7f5a19fc0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 五、Android屏幕密度及代表分辨率

|密度类型|代表分辨率（px）|屏幕密度（dpi）|换算(px/dp)|比例|
|----|----|----|----|----|
| 低密度（ldpi） | 240x320 | 120 |1dp=0.75px|3|
| 中密度（mdpi） | 320x480 | 160 |1dp=1px|4|
| 高密度（hdpi） | 480x800 | 240|1dp=1.5px|6|
| 超高密度<font color="red">（xhdpi） </font>| <font color="red">720x1280</font> | 320|1dp=2px|8|
| 超超高密度（xxhdpi） | 1080x1920 | 480 |1dp=3px|12|

#### 六、OpenGL 版本支持

![OpenGL版本支持](http://upload-images.jianshu.io/upload_images/6322932-fb082a2bb4321371.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 七、主流App支持minSdkVersion
|应用|Andriod最低版本|minSdkVersion|统计日期|
|----|----|----|----|
|携程|4.4.4|<font color="red">19</font>|2018-02-11|
|支付宝|4.3|<font color="red">18</font>|2018-02-11|
|微信|4.2.x|<font color="red">17</font>|2018-02-11|
|ofo|4.1.x|16|2018-02-11|
|QQ|4.0.3|15|2018-02-11|
|爱奇艺|4.0.3|15|2018-02-11|
|美团外卖|4.1.x|16|2018-02-11|
|滴滴出行|4.0.3|15|2018-02-11|
|优酷|4.1.x|16|2018-02-11|
|美图秀秀|4.0.3|15|2018-02-11|
|快手|4.0.3|15|2018-02-11|
|抖音|4.1.x|16|2018-02-11|
|今日头条|4.0.3|15|2018-02-11|
|作业帮|4.0.3|15|2018-02-11|
|火山小视频|4.0.3|15|2018-02-11|
|天猫|4.0.3|15|2018-02-11|
|淘宝|4.0.3|15|2018-02-11|

#### 八、结论

##### 1、设置minSDKVersion=17
>1. Android 4.2及以上版本已经覆盖96%的机型。
>2. 微信采用minSDKVersion=17，最小支持到android 4.2版本，我们有理由设想，使用我们应用的用户，一定会使用微信，所以和微信采用相同minSDKVersion即可。
>3. 支付宝minSDKVersion=18，携程minSDKVersion=19，我们采用17不会有问题。

#### 2、图片适配屏幕密度xhdpi
> 1. 大部分的设备属于xhdpi屏幕密度范围,xxhdpi设备也有相当保有量。
> 2. 分辨率太低的图片，在高密度设备下会被放大儿变模糊，hdpi中的图片会在xhdpi和xxhdpi设备中变得模糊，所以不采用hdpi来适配。
> 3. 分辨率太高的图片，android系统缩放时消耗过多内存，例如xxhdpi中的图片再xhdpi和hdpi设备上，会被缩小，分辨率越高，占用内存越多，所以也不适合使用xxhdpi来适配。
> 4. 大部分的设备属于xhdpi屏幕密度范围，我们只提供xhdpi一套图片即可完成图片适配。这样在面对hdpi和xxhdpi的设备时候，性能和显示清晰度都可以做到不错。

<font color="red">推荐：minSDKVersion = 17</font></br>
<font color="red">推荐：想要缩小apk 体积，则提供一套 xhdpi （UI设计按：720x1080设计）图片来做大部分图片缩放适配。

官方推荐：提供一套 xxhdpi （UI设计按：1080x1920设计）图片来做大部分图片缩放适配。如下图
</font>
![图片适配xxhdpi](http://upload-images.jianshu.io/upload_images/6322932-89d993c158f0125b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[Vector Drawable 中文]

[convert images to webp]:https://developer.android.com/studio/write/convert-webp.html#convert_images_to_webp
[Android dashboards]:https://developer.android.com/about/dashboards/index.html
[android-8.0-changes]:https://developer.android.com/about/versions/oreo/android-8.0-changes.html
[android-7.0-changes]:https://developer.android.com/about/versions/nougat/android-7.0-changes.html
[android-6.0-changes]:https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html
[android-5.1]:https://developer.android.com/about/versions/android-5.1.html
[android-5.0-changes]:https://developer.android.com/about/versions/android-5.0-changes.html
[android-4.4]:https://developer.android.com/about/versions/android-4.4.html
[android-4.3]:https://developer.android.com/about/versions/android-4.3.html
[android-4.2]:https://developer.android.com/about/versions/android-4.2.html
[android-4.1]:https://developer.android.com/about/versions/android-4.1.html
[android-4.0.3]:https://developer.android.com/about/versions/android-4.0.3.html
[android-4.0]:https://developer.android.com/about/versions/android-4.0.html
[android-3.2]:https://developer.android.com/about/versions/android-3.2.html
[android-3.1]:https://developer.android.com/about/versions/android-3.1.html
[android-3.0]:https://developer.android.com/about/versions/android-3.0.html
[android-2.3.3]:https://developer.android.com/about/versions/android-2.3.3.html
[android-2.3]:https://developer.android.com/about/versions/android-2.3.html
[android-2.2]:https://developer.android.com/about/versions/android-2.2.html
[android-2.1]:https://developer.android.com/about/versions/android-2.1.html
[android-support-library-232]:https://android-developers.googleblog.com/2016/02/android-support-library-232.html
[Vector Drawable 中文]:http://www.jb51.net/article/84613.htm