---
title: How To Create Github Pages with Hexo 未完待续
date: 2018-07-11 01:13:40
tags:
summery: 使用github pages  +  Hexo themes快速搭建简洁漂亮的个人blog，绑定域名，以及自定义模板。
---

#### Guide

#### 全面官方文档

[github-help-all]

[github-pages-guide]

[jekyll-guide]

[customizing-github-pages]

##### Hexo命令 Quick Start

##### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

##### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

##### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

##### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)

##### Add Theme "next"

	git clone --branch v5.1.2 https://github.com/iissnan/hexo-theme-next themes/next

more info: [hexo-theme-next]

##### Add Theme "even"

	git clone https://github.com/ahonn/hexo-theme-even themes/even
	
##### Add Comments

[add comments]

##### Highlight 高亮显示code代码
使用[highlightjs]来替换掉hexo自带的highlight功能
[highlightjs]

##### 设置Markdown文件中图片的位置和大小

[Resize-Image]

##### 设置markdown表格的宽度

[markdown 表格宽度设置]

[markdown 单元格宽度]

##### 在Markdown中使用css style

```css
<style>

table th{
	background: #ddd
}

table th:nth-child(1) {
    width: 100px;
}

table th:nth-child(2) {
    width: 150px;
}

table th:nth-child(3) {
    width: 50px;
}

table th:nth-child(4) {
    width: 160px;
}
</style>
```

##### 在markdown中使用javascript [需要将javascript代码放在文档末尾]

```javascript
<script>
   var ths = document.getElementsByTagName("th");
   for(var i=0;i<ths.length;i++){
       var myth = ths[i];
       if(i==0){//设置文档中的第一个th的宽度
           myth.style.width = "80px";
       }
       if(i==1){
           myth.style.width = "150px";
       }
       if(i==2){
           myth.style.width = "70px";
       }
       if(i==3){
           myth.style.width = "150px";
       }       
   }
   //alert(document.getElementsByTagName("table").length);
   	
</script>
```
#### 超链接

[github-help-all]:https://help.github.com/
[github-pages-guide]:https://help.github.com/categories/github-pages-basics/
[jekyll-guide]:https://jekyllrb.com/docs/home/
[customizing-github-pages]:https://help.github.com/categories/customizing-github-pages/
[hexo-theme-next]:https://github.com/iissnan/hexo-theme-next
[add comments]:https://widgetpack.com/
[highlightjs]:https://highlightjs.org/
[Resize-Image]:https://support.typora.io/Resize-Image/
[markdown 表格宽度设置]:https://blog.csdn.net/maxsky/article/details/54666998
[markdown 单元格宽度]:https://blog.csdn.net/liuyan19891230/article/details/52839788


<script>
   
   var ths = document.getElementsByTagName("th");
   for(var i=0;i<ths.length;i++){
       var myth = ths[i];
       if(i==0){//文档中的第一个th的宽度
           myth.style.width = "80px";
       }
       if(i==1){
           myth.style.width = "150px";
       }
       if(i==2){
           myth.style.width = "70px";
       }
       if(i==3){
           myth.style.width = "150px";
       }       
   }
   //alert(document.getElementsByTagName("table").length);
   	
</script>