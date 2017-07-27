---
layout: post
title: "行内元素（内联元素）的简单罗列和布局"
tags:
    - HTML
    - css
date: 2017-07-19 12:00:00
author: "LamWolf"
catalog: true
---




### 写在前面（此条可略读）

在日常写项目以及实习过程中，时常碰到的一个问题就是当使用行内元素作为父元素时，其子元素的垂直对齐（包括顶部对齐、居中、底部对齐等等）经常不能做到“随心所欲”地控制，经常是在同一个父元素内，某一个元素“垮掉”或者“跌落”，尤其是图和文字并排时更加难以控制。为此我决心系统地复习并整理一遍行内元素的相关知识，以填补此处的坑。**所以本文偏重于介绍行内元素的相关属性知识，不偏重于行内元素和块级元素的对比。**




## 目录

1. 什么是行内元素？
2. 列举行内元素，对常用元素进行简单说明
3. 行内元素重要样式

## 以下是正文

#### 1.什么是行内元素？

> HTML (超文本标记语言) 元素大多数都是行内元素或块级元素。一个行内元素只占据它对应标签的边框所包含的空间。

上文是MDN中关于[行内元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Inline_elemente)的摘要。

这段话的描述是很抽象的，对于读者来说其实并不是十分容易理解。所以在此我借用 @张鑫旭 大佬的一个比喻（在写本文的时候没有找到此比喻所在的原文，如有记错请指正），然后转化成自己的话表达出来。↓↓↓

>与元素生成的框的类型属性为`display`，`display:inline`将元素设为行内元素，行内元素不可设置宽高，其大小取决于内部元素的大小，由内部元素“撑开”，所以`inline`属性的元素可以视为“液体”；`display:block`将元素设为块级元素，块级元素独占一行，可设置宽高，所以可将其视为“固体”；`display:inline-block`则既不会独占一行，同时也可以设置宽高，所以可以将其视为“胶体”。

借用以上比喻，使初学者能对行内元素和块级元素有一个更形象的理解，如果想更加清晰的了解他们之间的区别，还是需要大家多在实际操作中去体会。

也因为`inline-block`的以上特性，在实际中本人也越来越多地使用`inline-block`来代替`inline`。


#### 2.列举行内元素，对常用元素进行简单说明

行内元素数量庞大，尤其是在h5之后，对于标签语义化要求的提高，行内元素已经不再局限`span`和`h1`-`h6`等这些简单标签，在被大佬问起我有记住哪些行内元素标签时，我也是一脸懵逼，因此我在此尽量将所有使用频率为*可能使用*及其以上的标签列举出来，并分别简单介绍一下他们的功能。

#####  列表

* \<span>
* \<a>
* \<b>
* \<strong>
* \<em>
* \<mark>
* \<i>
* \<code>
* \<dfn>
* \<var>
* \<img>
* \<button>
* \<input>
* \<sub>
* \<sup>

<br>

#####  span标签

```
<span>这是一个span标签</span>
```
>HTML <span> 元素是短语内容的通用行内容器，并没有任何特殊语义。

是行内元素中最常用的几个标签之一，没有特殊效果,应该在没有其他合适的语义元素时才使用它。

`<span>`标签详情请转[\<span> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/span)

<br>

##### a标签

```
<a href="#">这是一个超链接</a>
```
>HTML \<a> 元素  (或锚元素) 创建一个到其他网页，文件，同一页面内的位置，电子邮件地址或任何其他URL的超链接。

是行内元素中最常用的几个标签之一，用于建立锚点和超链接，一般情况下都带有`href`属性，其值为所指向的锚点位置或页面地址。

在H5中还加入以下几个新属性：

| 属性      | 值         | 描述                 |
| -------- | ---------- | --------------------|
| download | filename   | 规定被下载的超链接目标  |
| media    | media_query| 规定被链接文档是为何种媒介/设备优化的|
| type     | MIME type  | 规定被链接文档的MIME类型|

`<a>`标签详情请转[\<a> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a)

<br>

##### b标签

```
<b>这是一个字体加粗的区域</b>
```
>HTML \<b> 元素表示相对于普通文本字体上的区别，但不表示任何特殊的强调或者关联。它通常用在摘要中的关键字、审查中的产品名称或者其他需要显示为加粗的文字区域。它的另一个使用例子是用来标记一篇文章中每一段的引言。

需要注意区分的是`<b>`、`<strong>`、`<em>`以及`<mark>`。`<b>`标签**不包含特殊的语义信息**，应当仅仅当其他元素都不合适时使用它。其他三种元素的详细信息也会在后文中进行介绍。

`<b>`标签详情请转[\<b> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/b)

<br>

##### strong标签

```
<strong>这是一个确定十分重要的区域</strong>
```
>Strong 元素 (\<strong>)表示文本十分重要，一般用粗体显示。

`<strong>`元素明确表示文本很重要，是语义很明显的一个元素。

`<strong>`标签详情请转转[\<strong> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/strong)

<br>

##### em标签

```
<em>这是一块需要着重阅读的区域</em>
```
>HTML 着重元素 (\<em>) 标记出需要用户着重阅读的内容， \<em> 元素是可以嵌套的，嵌套层次越深，则其包含的内容被认定为越需要着重阅读。

`<em>`元素与`<i>`元素达到的视觉效果是相同的，即文本均为斜体，但是两者的语义并不相同，`<em>`标签标示着重强调其内容，`<i>`标签表示从正常的散文中区分出的文本，如电影或书籍的名字等。

`<em>`标签详情请转[\<em> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/em)

<br>

##### i标签

```
<i>这是一块需要区分于普通文本的文本区域</i>
```
>HTML元素 \<i> 用于表现因某些原因需要区分普通文本的一系列文本。例如技术术语、外文短语或是小说中人物的思想活动等，它的内容通常以斜体显示。

`<i>`标签范围内的文本是默认斜体的，他与`<em>`标签的区别在`<em>`标签的介绍中有提到，`<i>`标签的详情请转[\<i> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/i)

<br>

##### code标签

```
<code>this is a piece of code</code>
```
>HTML \<code> 元素呈现一段计算机代码. 默认情况下, 它以浏览器的默认等宽字体显示。

`<code>`标签内的文字会有所不同。

代码如下：

```
<div>
	<code>this is a fragment of computer code
</div>
<div>
	<span>this is a fragment of computer code
</div>
<div>
	<code>这是一段代码</code>
</div>
<div>
	<span>这是一段代码</span>
</div>
```

效果如下：

![](http://otf6ajw74.bkt.clouddn.com/code%E6%A0%87%E7%AD%BE.png)

`<code>`详情请转[\<code> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/code)

<br>

##### dfn标签

```
<dfn>这是一段术语</dfn>
```
>HTML 定义元素 (\<dfn>) 表示术语的一个定义。

`<dfn>`标签内的文字默认为斜体样式。

`<dfn>`标签详情请转[\<dfn> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/dfn)

<br>

##### var标签

```
<var>这是一个变量</var>
```
>\<var> 标签表示变量的名称，或者由用户提供的值。

`<var>`标签内的文字默认为斜体样式。

`<var>`标签详情请转[\<var> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/var)

<br>

##### img标签

```
<img src="[图片路径]" alt=""></img>
```
>HTML Image 元素（ \<img> ）代表文档中的一个图像。

`<img>`标签在在 HTML 中，<img> 标签没有结束标签，在 XHTML 中，<img> 标签必须被正确地关闭。

`<img>`在H5中加入了很多新属性，具体的就不在这里罗列了，有兴趣可以自己搜索一下。

`<img>`标签详情请转[\<img> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img)

<br>

##### button标签

```
<button>这是一个按钮</button>
```
>HTML \<button>元素 表示一个可点击的按钮。

`<button>`元素也是在页面中一个非常常用的标签，不过实际中，在扁平化的趋势下往往更多的人选择通过给`<span>`或者`<input>`添加伪类等方法来代替`<button>`元素，所以对于`<button>`元素来说，也要根据实际情况进行取舍。

`<button>`标签详情请转[\<button> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button)

<br>

##### input标签

```
<input>这是一块文本输入区域</input>
```
>HTML \<input> 元素用于为基于Web的表单创建交互式控件，以便接受来自用户的数据。

`<input>`元素的关键点在于拥有许多控件类型，尤其是在H5之后又加入了更多type的值，具体的我就不在本文中介绍了，有兴趣可以自行搜索一下，以后如果有这方面的需要，我会另外整理一下关于`<input>`标签的type属性。

`<input>`标签详情请转[\<input> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input)

<br>

##### sub标签

```
<sub>这是下标</sub>
```
>HTML \<sub> 元素定义了一个文本区域，出于排版的原因，与主要的文本相比，应该展示得更低并且更小。

`<sub>`这个元素应该只用于排版目的，也就是改变文本的位置会改变含义，例如在数学中（t<sub>2</sub>，也可以考虑使用 [MathML](https://developer.mozilla.org/en-US/docs/Web/MathML) 公式）或者化学符号（ H<sub>2</sub>O）。如果只是用于样式上的目的，请使用css进行调整。

`<sub>`标签详情请转[\<sub> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/sub)

<br>

##### sup标签

```
<sup>这是上标</sup>
```
>HTML \<sup> 元素定义了一个文本区域，出于排版的原因，与主要的文本相比，应该展示得更高并且更小。

`<sup>`标签的用法与`<sub>`标签基本类似，可以参照上文的介绍进行类比。

`<sup>`标签详情请见[\<sup> - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/sup)

<br>

通过以上总结可以看出，许多拥有不同语义和不同所属适用环境的标签在视觉效果上是相同的，尤其是在H5推行之后，DOM文档语义化的要求下，为了[更好地书写HTML标签](https://segmentfault.com/a/1190000002695791)，在不同的使用环境下使用最合适的HTML标签显得尤为重要，所以希望大家不要从样式的角度来思考使用哪一种标签，做到HTML与CSS分离，这对DOM文档的编写和后期维护都是很有利的。

行内元素远不止我列举的这几个标签，还有更多具有语义的行内元素标签我没有进行介绍，作为一个小白，我也只是根据我目前的一个使用经验来进行选择，选出了以上几个个人觉得可能会用到的标签，肯定还会有很多疏漏，不完整的地方希望大家能够指正，也希望大家在我的介绍下能够对HTML基础知识进行一次系统的梳理，将一些HTML的基础坑填好。

#### 3.行内元素的重要样式

关于行内元素的布局，为了达到某一目标，方法是多样性的，每个人有每个人的习惯和方法，不同的方法也有不同的优点和缺点。不管使用哪种方法，理论上来说总会有一种情况会出现布局上的瑕疵，所以我们只能在实际中考虑尽可能多的情况，找一个最优解。在本文中，我也不会去分析哪种情况下使用哪种方法，排列组合下的可能情况太多了，所以我也只能对应某个case提出一些解决方法，限于个人的经验和能力，可能我提出的方法也不是正解（比如是误打误撞）或者不是最优解，对于浏览器的兼容性也不在本文的计划内，大家如果有兴趣可以查找相关资料，希望大家能够谅解和指正。

在进行下文中的布局讨论之前，大家需要先搞清楚几个概念：**行高**和**基线**。参考文章：[深入理解CSS中的行高与基线](http://blog.csdn.net/lulujiajiawenwen/article/details/8245201)，这篇文章中，既有对于**行高**和**基线**等定义的介绍，也做了关于行内元素对齐的各种介绍，十分值得参考。

##### 列表

* display
* font-size
* margin & padding
* float
* vertical-align
* line-height* text-align（只有块级元素可以使用）
* 图片底部留白
* 元素之间的空白

<br>

##### display

`display`属性，在本文中对于`display`属性，将主要讨论`display:inline-block`为布局带来的一些便利。因为使用了`display:inline-block`属性的元素能够同时拥有*行内元素*和*块级元素*的一些特性，所以能够更加方便的实现一些布局，具体的将会在下文中与其它属性组合使用来完成一些效果。

对于`inline`元素和`inline-block`元素有一个很大的不同，就是它们是如何被内部元素“撑开”的。

例子如下：

设置一个`<span>`，内部包入一个`<img>`元素和一个`<span>`元素，给`<span>`设置一个边框，如图：

![](http://otf6ajw74.bkt.clouddn.com/inlineBorderCode.png)
![](http://otf6ajw74.bkt.clouddn.com/inlineBorder.png)

可以看到父元素`<span>`的高并没有随着图片“撑开”。

如果我们给父元素添加一个`display:inline-block`属性，如图：

![](http://otf6ajw74.bkt.clouddn.com/inlineBlockBorderCode.png)
![](http://otf6ajw74.bkt.clouddn.com/inlineBlockBorder.png)

可以看到，在给父元素设置了`display:inline-block`之后，高度就会被子元素撑开，能够随子元素的改变而改变。

<br>

##### font-size

那么，在设置`display:inline-block`之前，父元素的高度是如何改变的呢？这里就会涉及到一个与行内元素布局关系十分密切的属性——`font-size`。

我们删掉为父元素添加上的`display:inline-block`，改为添加一个`font-size:30px`的属性，如图：

![](http://otf6ajw74.bkt.clouddn.com/inlineFontBorderCode.png)
![](http://otf6ajw74.bkt.clouddn.com/inlineFontBorder.png)

我们可以看到父元素的高度发生了改变。

这是因为，`inline`元素的高度其实就是其`font-size`属性的值，也就是说父元素的高度是30px。

这个知识点在之前对我来说是个隐藏的坑，在平时写代码时，最终虽然能够排出想要的布局效果，但往往是在经历过多次尝试之后“试”出来的，在这次的系统梳理之后，这个坑也算是终于被填上。

<br>

##### margin & padding

`margin`和`padding`属性对于行内元素是无效的，所以如果想在行内元素上使用内外边距属性，就必须为行内元素添加`display:inline-block`属性，使其变成一个“胶体”，这样就可以在保持行内元素的特性下，设置内外边距。

这个属性主要的坑在于该属性在何时才会生效，当清楚其适用范围之后，该属性就会变得十分容易操作，所以在这里就不再多讲了。

<br>

##### float

想要对父元素内部的元素进行布局，有一个很便利的方法是采用浮动布局模型（[css几种布局模型](http://www.jianshu.com/p/5f11d3e82d28)）。

当对元素设置了浮动之后，该元素会脱离文档流。这时，该元素会变为弹性元素，即会被内部元素“撑”开，同时，对于`inline`元素来说，可以设置`margin-top`和`margin-bottom`。