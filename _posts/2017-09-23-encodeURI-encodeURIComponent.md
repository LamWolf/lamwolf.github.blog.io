---
layout: post
title: "url的“摩斯电码”"
subtitle: "encodeURI && encodeURIComponent"
author: "LamWolf"
tags: 
    - http
    - JavaScript
date: 2017-09-23 15:00:00
catalog: true
header-img: "img/post-bg-encodeURI-encodeURIComponent.jpg"
---



## 写在前面

这篇文章算是填之前的一个坑，涉及到的知识点也很少，只有`encodeURI`和`encodeURIComponent`的基本介绍和基础用法，更加深入的知识目前还没有打算深入研究，可能在之后需要的时候会再深挖一下，然后再来补充到这篇文章中。

## 目录

1. encodeURI
2. encodeURIComponent
3. 使用场景

## 以下是正文

### 1. encodeURI

>encodeURI() 是对统一资源标识符（URI）进行编码的方法。它使用1到4个转义序列来表示每个字符的UTF-8编码（只有由两个代理字符区组成的字符才用四个转义字符编码）。

#### 语法

```js
encodeURI(URI)
```

* URI:一个完整的URI

`encodeURI()`**不会****不会****不会**对以下几种字符进行编码：

ASCII字母、数字、~!@#$&*()=:/,;?+'

#### 实例

举一个简单的例子：

```js
//对一个网址进行编码
encodeURI('http://lamwolfwang.cn/2017/09/18/你还喜欢小甜饼吗/')
```

返回的结果为：

```js
"http://lamwolfwang.cn/2017/09/18/%E4%BD%A0%E8%BF%98%E5%96%9C%E6%AC%A2%E5%B0%8F%E7%94%9C%E9%A5%BC%E5%90%97/"
```

如果链接中出现与参数无关的`?``&`字符呢？

```js
encodeURI('http://lamwolfwang.cn/2017/09/18/你喜欢吗?&/')
```

返回值为：

```js
"http://lamwolfwang.cn/2017/09/18/%E4%BD%A0%E5%96%9C%E6%AC%A2%E5%90%97?&/"
```

我们可以看到，最后的`?`和`&`没有进行编码。

对于`encodeURI()`的具体使用场景将放在后文中总结。

### 2. encodeURIComponent

>encodeURIComponent()是对统一资源标识符（URI）的组成部分进行编码的方法。它使用一到四个转义序列来表示字符串中的每个字符的UTF-8编码（只有由两个Unicode代理区字符组成的字符才用四个转义字符编码）。

#### 语法

```js
encodeURIComponent(str)
```

* str: String. URI 的组成部分。

`encodeURIComponent()`**不会****不会****不会**对以下几种字符进行编码：

ASCII字母、数字、~!*()'

与`encodeURI()`相比，少了： **@#$&=:/,;?+**

#### 实例

如果给`encodeURIComponent()`传入一个完整的URI，会是怎样？

```js
encodeURIComponent('http://lamwolfwang.cn/2017/09/18/你还喜欢小甜饼吗/');
```

返回的结果为：

```js
"http%3A%2F%2Flamwolfwang.cn%2F2017%2F09%2F18%2F%E4%BD%A0%E8%BF%98%E5%96%9C%E6%AC%A2%E5%B0%8F%E7%94%9C%E9%A5%BC%E5%90%97%2F"
```

可以看到，如果将这一串返回值输入到浏览器中，是不能直接跳转到相关链接的。

具体的使用场景将在后文中总结。

### 3. 使用场景

在了解`encodeURI`和`encodeURIComponent`的适用场景之前，需要大家先了解**“URI”**和**“URL”**的概念。

简单说来，URL是URI的一个子集，URL比URI多的东西是传输协议，即:`http://`这一部分，关于URL详见——[http那些事儿](http://lamwolfwang.cn/2017/08/01/http-basic/)

#### encodeURI

如果需要对整个URL进行编码，并且在编码后需要使用这个URL，那么就需要使用`encodeURI()`方法。

通过该方法，可以将URL路径中的一些非法字符，比如空格、汉字等进行编译。

对应的解码方法是`decodeURI()`。

#### encodeURIComponent

`encodeURIComponent()`与`encodeURI()`一个非常明显的区别就是，`encodeURIComponent()`是对URL中的某个部分进行编码，而不是对整个URL进行编码，通常被使用在对URL的参数的编码操作中。

`encodeURIComponent()`的编码更加彻底，对于URL参数中会影响参数判断的字符，比如会对 ? 和 & (当然不只是比`encodeURI`多了这两个字符的编译)进行编译，这样属于参数值的字符就不会影响到参数的划分判断。


## 参考文章

* [escape,encodeURI,encodeURIComponent有什么区别? - 知乎](https://www.zhihu.com/question/21861899)
* [简单明了区分escape、encodeURI和encodeURIComponent - 哎呦大黄](http://www.cnblogs.com/season-huang/p/3439277.html)
* [encodeURIComponent() - JavaScript - MDN - Mozilla](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent)
* [encodeURI() - JavaScript - MDN - Mozilla](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI)
* [url的三个js编码函数escape(),encodeURI(),encodeURIComponent()简介](http://www.haorooms.com/post/js_escape_encodeURIComponent)