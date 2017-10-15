---
layout: post
title: "你还喜欢小甜饼吗"
subtitle: "cookie,LocalStorage,SessionStorage"
author: "LamWolf"
tags: 
    - LocalStorage
    - cookie
    - HTML5
date: 2017-09-18 15:20:00
catalog: true
header-img: "img/post-bg-cookie-storage.jpg"
---


## 写在前面

这篇文章也是搁置在更新计划里许久，今天才翻出来总结一下。关于cookie的是非对错大概在好几年前就有所耳闻，如今也算是第一次正经地来讨论这个问题。这篇文章除了总结cookie，localStorage和sessionStorage之间的异同点，也会总结一下localStorage和sessionStorage的Web API，尽可能得做到全面。

## 目录

1. 基本概念
2. 三者异同点
3. Web API
4. 安全性问题

## 以下是正文

### 1.基本概念

#### Cookie

>Cookie（复数形态Cookies），中文名称为“小型文本文件”或“小甜饼”，指某些网站为了辨别用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）。定义于RFC2109。是网景公司的前雇员卢·蒙特利在1993年3月的发明。

cookie的体积非常小，它的最大体积限制为4KB左右。最常见的一个应用场景就是各种登录网站中的“记住密码”，就是通过cookie来保存用户数据信息的。

#### localStorage

localStorage是HTML5中提出的新技术，不过并不是一个划时代的新技术。在IE6时代，便存在一个userData的东西用于本地数据存储，但那个时候考虑到兼容性，更通用的方案是使用Flash。

#### sessionStorage

>sessionStorage 属性允许你访问一个 session Storage 对象。它与 localStorage 相似，不同之处在于 localStorage 里面存储的数据没有过期时间设置，而存储在 sessionStorage 里面的数据在页面会话结束时会被清除。页面会话在浏览器打开期间一直保持，并且重新加载或恢复页面仍会保持原来的页面会话。在新标签或窗口打开一个页面会初始化一个新的会话，这点和 session cookies 的运行方式不同。

sessionStorage与localStorage十分相似，但是保存数据的生命周期不同， sessionStorage 是一个前端的概念，它只是可以将一部分数据在当前会话中保存下来，刷新页面数据依旧存在。但当页面关闭后，sessionStorage 中的数据就会被清空。

### 2.三者异同点

#### 特性

<table>
	<tr>
		<td><b>特性</b></td>
		<td><b>cookie</b></td>
		<td><b>localStorage</b></td>
		<td><b>sessiongStorage</b></td>
	</tr>
	<tr>
		<td>数据的生命周期</td>
		<td>一般由服务器生成，可设置失效时间。如果在浏览器端生成Cookie，默认是关闭浏览器后失效</td>
		<td>除非被清除，否则永久保存</td>
		<td>仅在当前会话下有效，关闭页面或浏览器后被清除</td>
	</tr>
	<tr>
		<td>存放数据的大小</td>
		<td>4K左右</td>
		<td colspan="2">一般为5MB</td>
	</tr>
	<tr>
		<td>与服务器端通信</td>
		<td>每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题</td>
		<td colspan="2">仅在客户端（即浏览器）中保存，不参与和服务器的通信</td>
	</tr>
	<tr>
		<td>易用性</td>
		<td>需要程序员自己封装，源生的Cookie接口不友好</td>
		<td colspan="2">源生接口可以接受，亦可再次封装来对Object和Array有更好的支持</td>
	</tr>
</table>


#### 应用场景

因为他们有不同特性，所以三者有着不同的适用场景，根据他们的特性来简单讨论一下他们的应用场景。

考虑到每次HTTP请求都会带有cookie的信息，所以cookie能小就小，能少就少，这样可以保证HTTP的性能。比较常用的一个地方是用户登录状态的判断，对于登录过的用户，服务端会在他登录时往cookie中插入一段具有唯一性的标识码，下次再登录时就可以根据这个码来判断他是否是已经登录了（即自动登录）。

在HTML5之前，购物车功能是由cookie来完成数据保存的，现在已由localStorage接棒，除此之外还有一些HTML5的游戏，也是通过localStorage来存储部分数据。

通过sessionStorage所存储数据的生命周期，和Session类似，关闭浏览器（或标签页）后数据就不存在了。但刷新页面或使用“前进”、“后退按钮”后sessionStorage仍然存在。所以sessionStorage比较常用的一个场景是多页填写表单数据时，页面间的数据传递。

### 3.Web API

#### cookie

cookie的Web API对于开发者来说，十分的不友好，通常开发者们都会自行封装cookie接口，便于后期的开发。不过为了了解cookie的一些原理，我们还是来讨论总结一下cookie的原生API。

创建一个新的cookie：

```js
document.cookie = newCookie;
```

`newCookie`是一个键值对形式的字符串。需要注意的是，用这个方法一次只能对一个cookie进行设置或更新。

如：`document.cookie = "name=oeschger";`

* 以下可选的cookie属性值可以跟在键值对后，用来具体化对cookie的设定/更新，使用分号以作分隔：
		
	*  `;path=path`（例如 '/', '/mydir'）如果没有定义，则默认当前文档位置的路径。
	*  `;domain=domain`(例如 'example.com'， '.example.com' (包括所有子域名), 'subdomain.example.com') 如果没有定义，则默认当前文档位置的路径的域名部分。
	*  `;max-age=max-age-in-seconds`(例如一年为60\*60\*24*365)
	*  `;expires=date-in-GMTString-format`如果没有定义，则cookie会在会话结束时过期
		* 这个值的格式参见[Date.toUTCString()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/toUTCString)
	* `;secure`(cookie只通过https协议传输)
* cookie的值字符串可以用encodeURIComponent()来保证它不包含任何逗号、分号或空格(cookie值中禁止使用这些值)。

NOTE：对于永久cookie我们用了"Fri, 31 Dec 9999 23:59:59 GMT"作为过期日。即，将cookie的`expires`属性设置为这个，那么这个cookie即为永久cookie；如果将cookie的`expires`属性设为"Thu, 01 Jan 1970 00:00:00 GMT"，即初始时间，就可以将该cookie清除掉。

#### localStorage & sessionStorage

`localStorage`和`sessionStorage`是伴随着HTML5诞生的，两者的API比起cookie来说友好太多。

* **属性**
	
	`Storage.length` *只读*
		
	返回一个整数，表示存储在 Storage 对象中的数据项数量。
		
* **方法**

	* `Storage.key()`
		
		该方法接受一个数值 n 作为参数，并返回存储中的第 n 个键名。
	
	* `Storage.setItem()`
	
		该方法接受一个键名作为参数，返回键名对应的值。
		
	* `Storage.getItem()`

		该方法接受一个键名和值作为参数，将会把键值对添加到存储中，如果键名存在，则更新其对应的值。
		
	* `Storage.removeItem()`

		该方法接受一个键名作为参数，并把该键名从存储中删除。
		
	* `Storage.clear()`

		调用该方法会清空存储中的所有键名。
		
* **实例**

	`localStorage`与`sessionStorage`的API相同，此处以`localStorage`为例：
	
	```js
	//设置一个新数据项
	localStorage.setItem("author", "LamWolf");
	
	//获取某个数据项
	localStorage.getItem("author");  // "LamWolf"
	
	//删除某个数据项
	localStorage.removeItem("author");
	localStorage.getItem("author");  // null
	
	//清除所有数据项
	localStorage.setItem("author", "LamWolf");
	localStorage.setItem("age", "22");
	
	localStorage.clear();
	
	localStorage.getItem("author"); //null
	localStorage.getItem("age");  //null
	
	//根据索引返回键名
	localStorage.setItem("author", "LamWolf");
	localStorage.key(0);  //"author"
	
	//求数据项数量
	localStorage.length;  // 1
	```


`localStorage`和`sessionStorage`在开发中虽然API十分友好，但开发者依然会对`Storage`进行封装，用于对Object和Array有更好的支持。

### 4.安全性问题

关于本地存储的安全性，最常涉及的就是XSS注入攻击，如果对数据存储攻击疏加防范，那么攻击者通过脚本注入，便可以肆意查看、窃取和更改你的localStorage。cookie经过如此长时间的发展，对于安全性问题要远比Storage采取的措施更多，但依然不能保证安全无恙。

>本着一切输入输出都是有害的原则，要对数据进行严格的输入输出过滤。

严格控制数据的输入输出，在一定程度上便可以降低数据存储的安全性。

更多的安全性问题，就不在这展开了，不然又是另一篇文章了。


## 参考文章
* [详说Cookie, LocalStorage 与SessionStorage \| 咀嚼之味](http://jerryzou.com/posts/cookie-and-web-storage/)
* [Cookie - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/Cookie)
* [Document.cookie - Web APIs \| MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie)
* [Window.localStorage - Web API 接口\| MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)
* [Storage - Web API 接口\| MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage)
* [cookie、sessionStorage、localStorage 详解及应用场景- 爱前端的数学宝宝](https://segmentfault.com/a/1190000010400892)
* [HTML5 本地存储- Rain Man - 博客园](http://www.cnblogs.com/rainman/archive/2011/06/22/2086069.html)