---
title: 碎片化知识点总结-to be continued
date: 2018-07-18 03:07:20
tags:
summery: 记录工作学习过程中遇到的碎片化的知识点，不断更新，便于查对。未完待续……

---

### 碎片化知识 

#### CSS 相关

|css 预处理器|描述|
|----|----|
|[less]||
|[sass]||
|[stylus]||
|||

#### javascript相关

|javascript template|描述|
|----|----|
|[ejs]||
|[EJS]||
|[Handlebars]||
|[Jade]||
|[Pug]|由Jade更名而来|

#### 博客框架

|名称|描述|
|----|----|
|[hexo]||
|[jekyll]||
|[wordpress]||

#### Web应用开发框架

|名称|描述|
|----|----|
|[Node.js 之 Express]||
|||

#### 规则记忆

|1|2|3|4|5|
|----|----|----|----|----|
|[css 优先级顺序]|[css 速查手册]||||
|[fontawesome-icons]|||||
 
#### 杂项

|1|2|3|4|5|
|----|----|----|----|----|
|[为什么Url需要转码]|[Junit如何执行异步方法测试]|[面试干货]|||

[css 速查手册]:http://www.css88.com/book/css/
 
[less]:http://lesscss.org/
[sass]:https://www.sass.hk/
[stylus]:http://stylus-lang.com/docs/bifs.html

[EJS]:https://ejs.bootcss.com/
[ejs]:https://www.npmjs.com/package/ejs
[Handlebars]:http://handlebarsjs.com/
[Jade]:http://jade-lang.com/
[Pug]:https://pug.bootcss.com/api/getting-started.html

[fontawesome-icons]:https://fontawesome.com/icons?d=gallery&s=brands

[css 优先级顺序]:https://www.cnblogs.com/lonelyxmas/p/7807017.html

[hexo]:https://hexo.io/zh-cn/docs/index.html
[jekyll]:https://jekyllrb.com/
[wordpress]:https://wordpress.org/

[Node.js 之 Express]:http://www.expressjs.com.cn/

[为什么Url需要转码]:https://www.cnblogs.com/jerrysion/p/5522673.html

[Junit如何执行异步方法测试]:https://www.jianshu.com/p/dfbc15d919be

[面试干货]:https://mp.weixin.qq.com/s/WVBJqqF6HGGJyzI1hc4fPg
#### 碎片化记忆

1. 输出javascript对象的方法
		
		function showObjProperty(obj) { 
			var propertys=''; 
			var propertyCounts=0; 
			for(i in obj){ 
				if(obj.i !=null) 
					propertys = propertys +i+'属性：'+obj.i+'\r\n'; 
				else 
					propertys = propertys +i+'方法\r\n'; 
			} 
			alert(propertys); 
		} 
		
		ejs方式输出：<%- propertys %>
		
2. Hexo blog中的theme.post对象中常用属性
		
		title  		标题
		date   		日期
		categories	分类
		excerpt		摘要
		tags   		标签
		path   		路径
		content		内容
		_content   	原始内容
		
3. 是否