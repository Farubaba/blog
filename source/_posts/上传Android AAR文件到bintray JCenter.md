---
title: 使用gradle-bintray-plugin插件，上传Android AAR文件到bintray JCenter
date: 2018-09-29 16:45:01
tags:
summery: 使用gradle-bintray-plugin插件，上传Android AAR文件到bintray JCenter。上传可以有多种方式，其一是publishing，其二是install，要使用它们都需要引入相关的插件依赖。
---
### 一、相关参考

Gradle DSL描述： <http://google.github.io/android-gradle-dsl/> 

[android-gradle-configuration]

[gradle-bintray-plugin examples]

[gradle-bintray-plugin 文档]

[上传android arr到jcenter网友博文参考]

[maven-left-cycle]

[https://docs.gradle.org]


<https://github.com/bintray/gradle-bintray-plugin#readme>

[一步一步教你将开源项目上传到jcenter]

[一步一步教你将开源项目上传到jcenter]:https://www.cnblogs.com/AbrahamCaiJin/p/7058147.html

### 二、概述
 
1.注册jcenter账号 <https://bintray.com/signup/oss>

	注册gmail邮箱（因为163，qq邮箱等都不能在jcenter中注册成功）
	本地Maven client 配置目录 ： /usr/local/opt/maven/conf
	
```
	用户名：farubaba
	API Key: xxxxx
```

2.创建jcenter私有仓库(repository)
	
jcenter中的repository和github中的repository不同，jcenter中的package等同于github中的repository

```
	例如：
	仓库名：android
	类型选择：maven，generic等
```
	
	
3.进入刚建立的仓库:android

	 1. 点击settting按钮
	 2. 选择uploading
	 3. deploying withe gradle 
	 4. 查看[gradle-bintray-plugin]插件文档：https://github.com/bintray/gradle-bintray-plugin#readme
	 5. 查看[gradle-bintray-plugin]插件的示例：https://github.com/bintray/bintray-examples/tree/master/gradle-bintray-plugin-examples
	 6. 以上文档看个大概，仍然不能很清楚的搭建号项目，需要进一步查看bintray在github中的资源：https://github.com/bintray
	 7. 更多更完整的aar相关示例： https://github.com/bintray/bintray-examples
	 8. 打开：https://github.com/bintray/bintray-examples/tree/master/gradle-aar-example，查看build.gradle文件
	 10.在bintray中搜索最新版本的android-maven-gradle-plugin：https://bintray.com/dcendents/gradle-plugins/com.github.dcendents%3Aandroid-maven-gradle-plugin
	 11.点击package描述中的WebSite： https://github.com/dcendents/android-maven-gradle-plugin

### 三、使用clean、build、bintrayUpload 和-x test 命令来上传和跳过test

1. 在项目根目录下执行如下命令，会将项目中的所有module都upload到jcenter，如果已经存在，则会上传失败。

```
	./gradlew clean build bintrayUpload --stacktrace --info --debug -x test
```
2. 如果需要单独上传某一个module到jcenter，则应使用如下命令，假设某个module的名称为：basefeature

```
	./graadlew :rootfeature:clean :rootfeature:build :rootfeature:bintrayUpload --stacktrace --info --debug -x test
	
	./gradlew :basefeature:clean :basefeature:build :basefeature:bintrayUpload --stacktrace --info --debug -x test
	
	./gradlew :uikitfeature:clean :uikitfeature:build :uikitfeature:bintrayUpload --stacktrace --info --debug -x test
		
```

###

### 四、使用publishing方式上传

1. 项目根目录build.gradle文件dependencies下添加如下依赖：
	
	```
	classpath 'com.github.dcendents:android-maven-gradle-plugin:2.0'
	classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.8.0'
	```
2. module目录下增加gradle.properties文件，并添加如下key，value根据自己的项目来修改
	
	```
	bintrayUser=farubaba
	bintrayApiKey=7b2db169d93937cdbe36482d9793d75069e4b7b7
	bintrayRepo=android
	bintrayPackage=newaarmodule
	pkgGroupId=com.farubaba.android
	pkgArtifactId=newaarmodule
	pkgVersion=3.0.3
	siteUrl=https://github.com/Farubaba/Mobile
	vcsUrl=https://github.com/Farubaba/Mobile.git
	issueUrl=https://github.com/Farubaba/Mobile/issues
	```
3. module目录下增加publish.gradle文件
	
	```
	//参考 ： https://github.com/bintray/bintray-examples/blob/master/gradle-bintray-plugin-examples/android-gradle-3.0.0-maven-example/app/publish.gradle
	
	/**
	 * 【使用Gradle上传aar到jcenter】
	 *
	 *  主要文件：
	 *  1）project根目录build.gradle
	 *  2) module目录下build.gradle
	 *  3) module目录下publish.gradle
	 *  4) module目录下gradle.properties
	 *
	 * 使用本文件时，在module的build.gradle文件末尾添加一行：
	 *
	 *      apply from: 'publish.gradle'
	 *
	 * 1、[gradle-bintray-plugin插件文档]:
	 *
	 *      https://github.com/bintray/gradle-bintray-plugin#readme
	 *
	 * 2、[android-maven-gradle-plugin，在gradle中使用maven来publish上传文件到jcenter私有仓库] :
	 *
	 *      https://github.com/dcendents/android-maven-gradle-plugin
	 *
	 * 3、[编写gradle.properties文件,定义属性]:
	 *
	 *      bintrayUser=farubaba
	 *      bintrayApiKey=7b2db169d93937cdbe362035739593d75069e4b9f
	 *      bintrayRepo=android
	 *      bintrayPackage=newaarmodule
	 *
	 * 4、[编写独立的publish.gradle文档]:
	 *
	 *      https://github.com/bintray/bintray-examples/blob/master/gradle-bintray-plugin-examples/android-gradle-3.0.0-maven-example/app/publish.gradle
	 *
	 * 5、[引入源码jar包]：
	 *
	 * 6、[执行命令，上传到jcenter私有仓库]:
	 *
	 *      执行所有module：./gradlew clean build bintrayUpload --stacktrace --info
	 *      执行指定module：./gradlew :newaarmodule:clean :newaarmodule:build :newaarmodule:bintrayUpload --stacktrace --info
	 *
	 *
	 *
	 ***********************************************************/
	
	//放入项目根目录下的build.gradle文件中，如果不设置，则需要在module的build.gradle文件中设置plugins{}来替代
	//classpath 'com.github.dcendents:android-maven-gradle-plugin:2.0'
	//classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.8.0'
	
	//plugins {
	//    id "com.github.dcendents.android-maven" version "2.1"
	//}
	
	//apply plugin: 'com.github.dcendents.android-maven' //该插件提供了install方法来上传
	apply plugin: 'maven-publish' //该插件中提供了publishing方法来上传
	apply plugin: 'com.jfrog.bintray'
	
	/**为plugin中预定义的变量赋值**/
	group this.pkgGroupId
	version this.pkgVersion
	
	/*******************************************publishing 方式上传start*********************************************/
	/***打包javadoc 和sourcesJar**/
	task sourcesJar(type: Jar) {
	    from android.sourceSets.main.java.srcDirs
	    classifier = 'sources'
	}
	task javadoc(type: Javadoc) {
	    options {
	        encoding "UTF-8"
	        charSet 'UTF-8'
	        author true
	        version true
	        links "http://docs.oracle.com/javase/7/docs/api"
	    }
	    source = android.sourceSets.main.java.srcDirs
	    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
	}
	task javadocJar(type: Jar, dependsOn: javadoc) {
	    classifier = 'javadoc'
	    from javadoc.destinationDir
	}
	
	/***会上传所有的jar，aar等archives****/
	artifacts {
	    archives javadocJar
	    archives sourcesJar
	}
	
	/**
	 * 要使用publishing，则需要引入如下插件，该插件中提供了publishing方法来上传
	 * apply plugin: 'maven-publish'
	 * 同时，记得反注释bintray中publications行
	 * bintray{
	 *   //FIXME 此行只有使用publishing来上传时才需要，如果使用install方法来上传，则需要注释掉此行。
	 *   publications = ['Production']
	 * }
	 *
	 */
	
	publishing {
	    publications {
	        Production(MavenPublication) {
	//            artifact("$buildDir/outputs/aar/${this.pkgArtifactId}-release.aar")
	//            artifacts {
	//                archives javadocJar
	//                archives sourcesJar
	//            }
	            groupId this.group
	            artifactId this.pkgArtifactId
	            version this.version
	
	            pom.withXml {
	                def dependenciesNode = asNode().appendNode('dependencies')
	
	                // Iterate over the implementation dependencies (we don't want the test ones), adding a <dependency> node for each
	                configurations.implementation.allDependencies.each {
	                    // Ensure dependencies such as fileTree are not included in the pom.
	                    if (it.name != 'unspecified') {
	                        def dependencyNode = dependenciesNode.appendNode('dependency')
	                        dependencyNode.appendNode('groupId', it.group)
	                        dependencyNode.appendNode('artifactId', it.name)
	                        dependencyNode.appendNode('version', it.version)
	                    }
	                }
	            }
	        }
	    }
	}
	
	/*******************************************publishing 方式上传end*********************************************/
	
	bintray {
	//    user = project.hasProperty('bintrayUser') ?: System.getenv('BINTRAY_USER')
	//    key = project.hasProperty('bintrayApiKey') ?: System.getenv('BINTRAY_API_KEY')
	    user = this.bintrayUser ?: System.getenv('BINTRAY_USER')
	    key = this.bintrayApiKey ?: System.getenv('BINTRAY_API_KEY')
	    publications = ['Production'] //FIXME 此行只有使用publishing来上传时才需要，如果使用install方法来上传，则需要注释掉此行。
	    configurations = ['archives']
	    override = true
	    pkg {
	        repo = this.bintrayRepo //bintray 仓库名称
	        name = this.bintrayPackage // bintray 仓库中 packge name
	        description = "An example of using the bintray plugin with gradle plugin 3.0.0"
	        publish = true
	        dryRun = false //设置成true，则不能上传，why？
	        publicDownloadNumbers = true
	        licenses = ['Apache-2.0']
	        vcsUrl = this.vcsUrl
	        issueTrackerUrl = this.issueUrl
	        websiteUrl = this.siteUrl
	        version {
	            name = this.version
	            desc = "${this.pkgArtifactId} ${this.version}"
	            released = new Date()
	            vcsTag = this.version
	        }
	
	    }
	}
	/***************************************************************************/
	/** Plugin DSL : https://github.com/bintray/gradle-bintray-plugin#readme  **/
	/***************************************************************************/
	/**
	 * bintray {
	 *     user = 'bintray_user'
	 *     key = 'bintray_api_key'
	 *
	 *     configurations = ['deployables'] //When uploading configuration files
	 *     // - OR -
	 *     publications = ['mavenStuff'] //When uploading Maven-based publication files
	 *     // - AND/OR -
	 *     filesSpec { //When uploading any arbitrary files ('filesSpec' is a standard Gradle CopySpec)
	 *         from 'arbitrary-files'
	 *         into 'standalone_files/level1'
	 *         rename '(.+)\\.(.+)', '$1-suffix.$2'
	 *     }
	 *     dryRun = false //[Default: false] Whether to run this as dry-run, without deploying
	 *     publish = true //[Default: false] Whether version should be auto published after an upload
	 *     override = false //[Default: false] Whether to override version artifacts already published
	 *     //Package configuration. The plugin will use the repo and name properties to check if the package already exists. In that case, there's no need to configure the other package properties (like userOrg, desc, etc).
	 *     pkg {
	 *         repo = 'myrepo'
	 *         name = 'mypkg'
	 *         userOrg = 'myorg' //An optional organization name when the repo belongs to one of the user's orgs
	 *         desc = 'what a fantastic package indeed!'
	 *         websiteUrl = 'https://github.com/bintray/gradle-bintray-plugin'
	 *         issueTrackerUrl = 'https://github.com/bintray/gradle-bintray-plugin/issues'
	 *         vcsUrl = 'https://github.com/bintray/gradle-bintray-plugin.git'
	 *         licenses = ['Apache-2.0']
	 *         labels = ['gear', 'gore', 'gorilla']
	 *         publicDownloadNumbers = true
	 *         attributes= ['a': ['ay1', 'ay2'], 'b': ['bee'], c: 'cee'] //Optional package-level attributes
	 *
	 *         githubRepo = 'bintray/gradle-bintray-plugin' //Optional Github repository
	 *         githubReleaseNotesFile = 'README.md' //Optional Github readme file
	 *
	 *         //Optional Debian details
	 *         debn {
	 *             distribution = 'squeeze'
	 *             component = 'main'
	 *             architecture = 'i386,noarch,amd64'
	 *         }
	 *         //Optional version descriptor
	 *         version {
	 *             name = '1.3-Final' //Bintray logical version name
	 *             desc = //Optional - Version-specific description'
	 *             released  = //Optional - Date of the version release. 2 possible values: date in the format of 'yyyy-MM-dd'T'HH:mm:ss.SSSZZ' OR a java.util.Date instance
	 *             vcsTag = '1.3.0'
	 *             attributes = ['gradle-plugin': 'com.use.less:com.use.less.gradle:gradle-useless-plugin'] //Optional version-level attributes
	 *             //Optional configuration for GPG signing
	 *           gpg {
	 *                 sign = true //Determines whether to GPG sign the files. The default is false
	 *                 passphrase = 'passphrase' //Optional. The passphrase for GPsigning'*             }
	 *             //Optional configuration for Maven Central sync of the version
	 *           mavenCentralSync {
	 *                 sync = true //[Default: true] Determines whether to sync the version to Maven Central.
	 *                 user = 'userToken' //OSS user token: mandatory
	 *                 password = 'paasword' //OSS user password: mandatory
	 *                 close = '1' //Optional property. By default the staging repository is closed and artifacts are released to Maven Central. You can optionally turn this behaviour off (by puting 0 as value) and releasemuay.*             }
	 *         }
	 *     }
	 * }
	 */

	```

4. 执行命令,假设module为：newaarmodule

	```
	./gradlew :newaarmodule:clean :newaarmodule:build :newaarmodule:bintrayUpload --stacktrace --info --debug -x test
	```

### 五、使用install方式上传

1. 项目根目录build.gradle文件dependencies下添加如下依赖：
	
	```
	classpath 'com.github.dcendents:android-maven-gradle-plugin:2.0'
	classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.8.0'
	```
2. module目录下增加gradle.properties文件，并添加如下key，value根据自己的项目来修改
	
	```
	bintrayUser=farubaba
	bintrayApiKey=7b2db169d93937cdbe36482d9793d75069e4b7b7
	bintrayRepo=android
	bintrayPackage=newaarmodule
	pkgGroupId=com.farubaba.android
	pkgArtifactId=newaarmodule
	pkgVersion=3.0.3
	siteUrl=https://github.com/Farubaba/Mobile
	vcsUrl=https://github.com/Farubaba/Mobile.git
	issueUrl=https://github.com/Farubaba/Mobile/issues
	```
3. module目录下增加install.gradle文件

	```
	//参考 ： https://github.com/bintray/bintray-examples/blob/master/gradle-bintray-plugin-examples/android-gradle-3.0.0-maven-example/app/publish.gradle
	
	/**
	 * 【使用Gradle上传aar到jcenter】
	 *
	 *  主要文件：
	 *  1）project根目录build.gradle
	 *  2) module目录下build.gradle
	 *  3) module目录下install.gradle
	 *  4) module目录下gradle.properties
	 *
	 * 使用本文件时，在module的build.gradle文件末尾添加一行：
	 *
	 *      apply from: 'install.gradle'
	 *
	 * 1、[gradle-bintray-plugin插件文档]:
	 *
	 *      https://github.com/bintray/gradle-bintray-plugin#readme
	 *
	 * 2、[android-maven-gradle-plugin，在gradle中使用maven来publish上传文件到jcenter私有仓库] :
	 *
	 *      https://github.com/dcendents/android-maven-gradle-plugin
	 *
	 * 3、[编写gradle.properties文件,定义属性]:
	 *
	 *      bintrayUser=farubaba
	 *      bintrayApiKey=7b2db169d93937cdbe362035739593d75069e4b9f
	 *      bintrayRepo=android
	 *      bintrayPackage=newaarmodule
	 *
	 * 4、[编写独立的publish.gradle文档]:
	 *
	 *      https://github.com/bintray/bintray-examples/blob/master/gradle-bintray-plugin-examples/android-gradle-3.0.0-maven-example/app/publish.gradle
	 *
	 * 5、[引入源码jar包]：
	 *
	 * 6、[执行命令，上传到jcenter私有仓库]:
	 *
	 *      执行所有module：./gradlew clean build bintrayUpload --stacktrace --info
	 *      执行指定module：./gradlew :newaarmodule:clean :newaarmodule:build :newaarmodule:bintrayUpload --stacktrace --info
	 *
	 *
	 *
	 ***********************************************************/
	
	//放入项目根目录下的build.gradle文件中，如果不设置，则需要在module的build.gradle文件中设置plugins{}来替代
	//classpath 'com.github.dcendents:android-maven-gradle-plugin:2.0'
	//classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.8.0'
	
	//plugins {
	//    id "com.github.dcendents.android-maven" version "2.1"
	//}
	
	apply plugin: 'com.github.dcendents.android-maven' //该插件提供了install方法来上传
	//apply plugin: 'maven-publish' //该插件中提供了publishing方法来上传
	apply plugin: 'com.jfrog.bintray'
	
	/**为plugin中预定义的变量赋值**/
	group this.pkgGroupId
	version this.pkgVersion
	
	bintray {
	//    user = project.hasProperty('bintrayUser') ?: System.getenv('BINTRAY_USER')
	//    key = project.hasProperty('bintrayApiKey') ?: System.getenv('BINTRAY_API_KEY')
	    user = this.bintrayUser ?: System.getenv('BINTRAY_USER')
	    key = this.bintrayApiKey ?: System.getenv('BINTRAY_API_KEY')
	    //publications = ['Production'] //FIXME 此行只有使用publishing来上传时才需要，如果使用install方法来上传，则需要注释掉此行。
	    configurations = ['archives']
	    override = true
	    pkg {
	        repo = this.bintrayRepo //bintray 仓库名称
	        name = this.bintrayPackage // bintray 仓库中 packge name
	        description = "An example of using the bintray plugin with gradle plugin 3.0.0"
	        publish = true
	        dryRun = false //设置成true，则不能上传，why？
	        publicDownloadNumbers = true
	        licenses = ['Apache-2.0']
	        vcsUrl = this.vcsUrl
	        issueTrackerUrl = this.issueUrl
	        websiteUrl = this.siteUrl
	        version {
	            name = this.version
	            desc = "${this.pkgArtifactId} ${this.version}"
	            released = new Date()
	            vcsTag = this.version
	        }
	
	    }
	}
	
	/*******************************************Install 方式上传begin************************************************/
	/**
	 * 使用install，要应用如下插件，该插件提供了install方法来上传
	 *
	 * apply plugin: 'com.github.dcendents.android-maven'
	 *
	 * 同时，记得注释掉bintray中publications行
	 * bintray{
	 *   //FIXME 此行只有使用publishing来上传时才需要，如果使用install方法来上传，则需要注释掉此行。
	 *   //publications = ['Production']
	 * }
	 *
	 */
	install {
	    repositories.mavenInstaller {
	        pom {
	            project {
	                packaging 'aar'
	                name 'Bintray publish Gradle aar example'
	                url siteUrl
	                licenses {
	                    license {
	                        name 'The Apache Software License, Version 2.0'
	                        url 'http://www.apache.org/licenses/LICENSE-2.0.txt'
	                    }
	                }
	                developers {
	                    developer {
	                        id 'farubaba'
	                        name 'Baruch Sadogursky'
	                        email 'farubaba@qq.com'
	                    }
	                }
	                scm {
	                    connection 'https://github.com/Farubaba/Mobile.git'
	                    developerConnection 'https://github.com/Farubaba/Mobile.git'
	                    url this.siteUrl
	
	                }
	            }
	        }
	    }
	}
	
	/***打包javadoc 和sourcesJar**/
	task sourcesJar(type: Jar) {
	    from android.sourceSets.main.java.srcDirs
	    classifier = 'sources'
	}
	task javadoc(type: Javadoc) {
	    options {
	        encoding "UTF-8"
	        charSet 'UTF-8'
	        author true
	        version true
	        links "http://docs.oracle.com/javase/7/docs/api"
	    }
	    source = android.sourceSets.main.java.srcDirs
	    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
	}
	task javadocJar(type: Jar, dependsOn: javadoc) {
	    classifier = 'javadoc'
	    from javadoc.destinationDir
	}
	
	/***会上传所有的jar，aar等archives****/
	artifacts {
	    archives javadocJar
	    archives sourcesJar
	}
	
	task findConventions << {
	    println project.getConvention()
	}
	
	/*******************************************Install 方式上传end************************************************/
	
	/***************************************************************************/
	/** Plugin DSL : https://github.com/bintray/gradle-bintray-plugin#readme  **/
	/***************************************************************************/
	/**
	 * bintray {
	 *     user = 'bintray_user'
	 *     key = 'bintray_api_key'
	 *
	 *     configurations = ['deployables'] //When uploading configuration files
	 *     // - OR -
	 *     publications = ['mavenStuff'] //When uploading Maven-based publication files
	 *     // - AND/OR -
	 *     filesSpec { //When uploading any arbitrary files ('filesSpec' is a standard Gradle CopySpec)
	 *         from 'arbitrary-files'
	 *         into 'standalone_files/level1'
	 *         rename '(.+)\\.(.+)', '$1-suffix.$2'
	 *     }
	 *     dryRun = false //[Default: false] Whether to run this as dry-run, without deploying
	 *     publish = true //[Default: false] Whether version should be auto published after an upload
	 *     override = false //[Default: false] Whether to override version artifacts already published
	 *     //Package configuration. The plugin will use the repo and name properties to check if the package already exists. In that case, there's no need to configure the other package properties (like userOrg, desc, etc).
	 *     pkg {
	 *         repo = 'myrepo'
	 *         name = 'mypkg'
	 *         userOrg = 'myorg' //An optional organization name when the repo belongs to one of the user's orgs
	 *         desc = 'what a fantastic package indeed!'
	 *         websiteUrl = 'https://github.com/bintray/gradle-bintray-plugin'
	 *         issueTrackerUrl = 'https://github.com/bintray/gradle-bintray-plugin/issues'
	 *         vcsUrl = 'https://github.com/bintray/gradle-bintray-plugin.git'
	 *         licenses = ['Apache-2.0']
	 *         labels = ['gear', 'gore', 'gorilla']
	 *         publicDownloadNumbers = true
	 *         attributes= ['a': ['ay1', 'ay2'], 'b': ['bee'], c: 'cee'] //Optional package-level attributes
	 *
	 *         githubRepo = 'bintray/gradle-bintray-plugin' //Optional Github repository
	 *         githubReleaseNotesFile = 'README.md' //Optional Github readme file
	 *
	 *         //Optional Debian details
	 *         debn {
	 *             distribution = 'squeeze'
	 *             component = 'main'
	 *             architecture = 'i386,noarch,amd64'
	 *         }
	 *         //Optional version descriptor
	 *         version {
	 *             name = '1.3-Final' //Bintray logical version name
	 *             desc = //Optional - Version-specific description'
	 *             released  = //Optional - Date of the version release. 2 possible values: date in the format of 'yyyy-MM-dd'T'HH:mm:ss.SSSZZ' OR a java.util.Date instance
	 *             vcsTag = '1.3.0'
	 *             attributes = ['gradle-plugin': 'com.use.less:com.use.less.gradle:gradle-useless-plugin'] //Optional version-level attributes
	 *             //Optional configuration for GPG signing
	 *           gpg {
	 *                 sign = true //Determines whether to GPG sign the files. The default is false
	 *                 passphrase = 'passphrase' //Optional. The passphrase for GPsigning'*             }
	 *             //Optional configuration for Maven Central sync of the version
	 *           mavenCentralSync {
	 *                 sync = true //[Default: true] Determines whether to sync the version to Maven Central.
	 *                 user = 'userToken' //OSS user token: mandatory
	 *                 password = 'paasword' //OSS user password: mandatory
	 *                 close = '1' //Optional property. By default the staging repository is closed and artifacts are released to Maven Central. You can optionally turn this behaviour off (by puting 0 as value) and releasemuay.*             }
	 *         }
	 *     }
	 * }
	 */

	```
4. 执行命令,假设module为：newaarmodule

	```
	./gradlew :newaarmodule:clean :newaarmodule:build :newaarmodule:bintrayUpload --stacktrace --info --debug -x test
	```
	
[OAuth 2.0 阮一峰]:http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html
[maven-left-cycle]:http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html
[上传android arr到jcenter网友博文参考]:https://www.cnblogs.com/AbrahamCaiJin/p/7058147.html
[android-gradle-configuration]:https://developer.android.com/studio/build/build-variants
[gradle-bintray-plugin examples]:https://github.com/bintray/bintray-examples/tree/master/gradle-bintray-plugin-examples
[gradle-bintray-plugin 文档]:https://github.com/bintray/gradle-bintray-plugin/blob/master/README.md





[https://docs.gradle.org]:https://docs.gradle.org





