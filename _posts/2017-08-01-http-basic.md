---
layout: post
title: "http那些事儿"
tags:
    - http
    - 前端
date: 2017-08-01 12:00:00
author: "LamWolf"
catalog: true
header-img: "img/post-bg-http.jpg"
---



## 写在前面

作为一名前端开发人员，少不了跟网络活动打交道，而网络最基本的活动之一就是一次次的http请求，那http是什么，http请求又是什么呢？本篇文章，就对http进行一次基础的系统总结。

##目录

1. 什么是http协议？
2. 一次http请求是由哪几部分组成的？
3. http的不同请求方法
4. http的状态码


## 正文从这里开始

### 1.什么是http协议？

>超文本传输协议（英文：HyperText Transfer Protocol，缩写：HTTP）是一种用于分布式、协作式和超媒体信息系统的应用层协议。HTTP是万维网的数据通信的基础。

上文是维基百科中对于超文本传输协议，即HTTP的定义。HTTP最开始被设计出来的目的是为了提供一种发布和接收HTML页面的方法。通过HTTP或者HTTPS协议请求的资源由统一资源标识符（Uniform Resource Identifiers，URI）来标识。

HTTP共经历过0.9,1.0,1.1和2.0（目前最新的版本）共四个版本，HTTP/0.9已被弃用，后文中可能会涉及到部分各HTTP版本的特性，如果想要了解更多关于不同HTTP版本的特性，请自行搜索查阅相关文档。

简单说来，HTTP协议就是双方为了能够完成正常的网络通信而提前做好的一个约定。

### 2.一次http请求是由哪几部分组成的？

##### URL

URL(Uniform/Universal Resource Locator 的缩写，统一资源定位符)属于URL(Uniform Resource Identifier 的缩写，统一资源标识符)更低层次的抽象，是一种字符串文本标准。
<br>
也就是说，URI属于父类，而URL属于URI的子类。URL是URI的一个子集。
<br>
二者的区别在于，**URI表示服务器的路径，定义这么一个资源**。而**URL同时说明要如何访问这个资源（http://）**。

1. URL的标准格式

	**scheme://host[:port#]/path/.../[;url-params][?query-string][#anchor]**

	|名称|意义|
	|----|---|
	|scheme|协议，例如：http,https,ftp,ed2k以及迅雷的thunder等|
	|host|http服务器的ip地址或者域名，例如：192.168.1.1或者www.google.com|
	|port#|http服务器的默认端口是80，这种情况下端口号可以省略。如果使用了别的端口号就需要指明，比如：localhost:8080/|
	|path|访问资源的路径|
	|url-params|所带参数|
	|query-string|发送给服务器的数据|
	|anchor|锚点定位|

2. URL中的特殊字符

	在URL中除了字母还会出现特殊的字符，他们的意义如下：

	|字符|含义|十六进制|
	|---|---|-------|
	|+|表示空格|%2B|
	|/|分隔目录和子目录|%2F|
	|?|分隔实际的URL和参数|%3F|
	|#|表示书签或者锚点|%23|
	|&|URL中指定的参数间的分隔符|%26|
	|=|URL中指定的参数的值|%3D|

	**"#"**的特殊作用（**Hash**）
	
	* **井号在URL中指定的是页面中的一个位置**
		
		井号作为页面定位符出现在URL中，比如：www.google.com/123.html#print(该地址并不存在，乱写的)，此URL表示在123.html页面中的#print位置。浏览器读取到此URL后会自动将print位置滚动到可视区域。
	
	* **井号后面的内容不会发送到http请求中**
	
		原因是井号后面的参数是针对浏览器起作用而不是服务器端。
	
	* **位于井号后面的字符都是位置标识符**

		比如一个地址链接中含有`.../?color=#ffffff`，此参数为颜色，然而服务器解析的参数结果为`.../?color=`
	
	* **改变井号后面的参数不会触发页面的重新加载，但是会留下一个历史记录**

		仅改变井号后面的内容，只会使浏览器滚动到相应的位置，并不会重新加载页面
		
	* **可以通过JavaScript使用window.location.hash来改变井号后面的值**

		window.location.hash这个属性对URL中的井号参数进行修改，基于这个原理，我们可以在不重载页面的前提下创造一天新的访问记录。<br>
		除此之外，H5新增的onhashchange事件，当#值发生变化时，就会触发这个事件。
	
	* **Googlebot对井号的过滤机制**

		默认情况下Google在索引页面的时候会忽略井号后面的参数，同时也不会去执行页面中的javascript。然而谷歌为了支持对Ajax生成内容的索引，定义了如果在URL中使用“#!”，则Google会自动将其后面的内容转成查询字符串_escaped_fragment_的值。
		
	井号除了简单的“定位”之外，作用还有很多，甚至可以结合？等元素标记流量来源，更深层次的用法就不在这里深究了，有兴趣可以自行搜索查找相关文档。

3. URL编码

	一个东西需要进行编码，那就说明这个东西是不适合直接传输的。
	
	* **会引起歧义**

		

## 参考文章
* [超文本传输协议](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)（维基百科，传送需要翻墙）
* [【基础进阶】URL详解与URL编码](http://www.cnblogs.com/coco1s/p/5038412.html)
* [URL中“#” “？” &“”号的作用](http://www.cnblogs.com/julin-peng/p/4237791.html)