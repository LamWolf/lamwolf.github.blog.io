---
layout: post
title: "高阶函数三杰"
subtitle: "filter()、map()、reduce()的运用"
author: "LamWolf"
tags: 
    - JavaScript
    - 高阶函数
date: 2017-09-16 11:20:00
catalog: true
header-img: "img/post-bg-higher-order-function.jpg"
---



## 写在前面

没啥想说的……看正文吧

## 目录

1. 什么是高阶函数？
2. filter()方法
3. map()方法
4. reduce()方法


## 以下是正文

### 1.什么是高阶函数？

> JavaScript的函数其实都指向某个变量。既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。

简单来说就是入参是函数的函数就是高阶函数。

函数A(函数B)    ===>  函数A即为高阶函数

**高阶函数的优势是什么呢？**

>高阶函数可以帮助你增强你的JavaScript，让你的代码更具有声明性。简单来说，就是简单，简练，可读。

使用高阶函数极大地提高了代码的可读性。你根本不需要了解函数内部发生了什么，它非常容易理解。

接下来就让我们学习一下几个十分常用的高阶函数

### 2.filter()方法

>filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 

简单来说filter()方法是一个**数组**过滤器，将符合条件的数组元素返回成一个新数组，原数组不发生改变。

我们举《前端早读课》中的一个例子来展示该方法的效果：

有这样一个数组，里面包含了多个对象

```
const people = [{
	name: 'John Doe',
	age: 16
}, {
	name: 'Thomas Calls',
	age: 19
}, {
	name: 'Liam Smith',
	age: 20
}, {
	name: 'Jessy Pinkman',
	age: 18
}, ]
```

我们想要挑选出年龄大于18的元素：

```
const peopleAbove18 = (collection) =>{
	return collection.filter((person) => person.age >= 18)
}

console.log(peopleAbove18(people));
```

执行上述代码后，输出结果为:

![](http://otf6ajw74.bkt.clouddn.com/HigherFilter.png)

tips: filter()方法的入参函数里，最终是需要`return`某个值的，上文的代码中通过箭头函数，默认`return`了`person.age>=18`的值，这是es6中箭头函数的特性，如果不使用该特性则代码需要写成：

```
const peopleAbove18 = (collection) =>{
	return collection.filter((person) => {
		return person.age >= 18
	})
}
```

### 3. map()方法

>map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

如果说filter()是一个过滤器，那么map()方法则应该是一个**过滤加工器**。

map()方法会对符合条件的元素进行处理然后再最终返回一个新数组。

我们依然使用《前端早读课》中的一个例子，还使用之前的那个`people`数组，这次还多了这样一个数组：`people`名单中喜欢喝咖啡的人员名单

```
const coffeeLovers = ['John Doe', 'Liam Smith', 'Jessy Pinkman'];
```

我们想根据这个名单为原先的那个名单（并不是在原数组上操作）添加一个标识：

```
const peopleLoveCoffee = (collection) => {
	return collection.map((person) => {
		person.loveCoffee = coffeeLovers.includes(person.name);
		return person;
	})
}

console.log(peopleLoveCoffee(people));
```

输出结果为：

![](http://otf6ajw74.bkt.clouddn.com/HigherMapRes.png)

### 4. reduce()方法

>reduce() 方法对累加器和数组中的每个元素 (从左到右)应用一个函数，将其减少为单个值。

reduce()方法可以说是一个万能使用工具，既可以做筛选，也可以做筛选后的处理，使用起来更加灵活一些。我们也着重学习一下这个方法。

##### reduce()的语法

`array.reduce(function(accumulator, currentValue, currentIndex, array), initialValue)`

`accumulator`上一次调用回调返回的值，或者是提供的初始值（initialValue）

`currentValue`数组中正在处理的元素

`currentIndex`数据中正在处理的元素索引，如果提供了initialValue，则初始值为0，否则为1

`array`调用reduce的数组

`initialValue`可选项，其值用于第一次调用回调函数的第一个参数（accumulator）。如果没有设置初始值，则将数组中第一个元素作为初始值。空数组调用reduce时没有设置初始值则会报错。

##### reduce()实例

还是之前的那个例子，如果我们想要求名单中人们的年龄总和，使用reduce来完成将会变得十分方便：

```
const sumAge = (collection) => {
	return collection.reduce((sum, person) => {
		return sum += person.age;
	}, 0)
}

console.log(sumAge(people));
```

执行代码输出结果如下：

![](http://otf6ajw74.bkt.clouddn.com/HigherReduceRes.png)

reduce()方法比起前两个方法，多了一个累加器，这个累加器不仅可以通过`+=`完成简单的加法，也可以通过`array.concat()`进行数组的合并，也可以完成更加复杂的逻辑，就不在这里一一尝试了，有兴趣的同学可以自己尝试一下。

##### reduce()模拟filter()和map()

因为reduce()的灵活性，所以是可以通过reduce()来写自己的filter()方法，代码如下:

```
const filter = (collection, fn) => {
	return collection.reduce((a, item) => {
		if(fn(item)){
			return a.concat(item);
		}
		return a;
	}, [])
}
```

模拟map():

```
const map = (collection, fn) => {
	return collection.reduce(a, item) => {
		return a.concat((fn(item))(item))
	}, [])
}
```

tips: 当模拟`array.map()`时需要注意的是，因为需要对itme通过fn回调函数进行“加工”，且concat()方法的入参需要是一个数组，所以需要对concat()方法中的fn(item)立即执行。

## 参考文章

* [高阶函数- 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434499355829ead974e550644e2ebd9fd8bb1b0dd721000)
* [【第1054期】高阶函数：利用Filter、Map和Reduce来编写更易维护的代码 - 前端早读课](https://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=2651226957&idx=1&sn=67c96b66ddd93656a98f67c66b185aaf&chksm=bd495ac98a3ed3dfd0e13460e586841535ecf35d9929e8c7b64abb8d2be415f570dd5a76b596&mpshare=1&scene=1&srcid=0915VD8nCIdjsY2EMSzVKtnw&key=e265549782a61304d0ba557a3af4f9d87b6627fe8ab6fb935340de1496fd18cdee248e8e8dc701fd73e1275f3518c8ff2e5f7ff484af2932c5a8c30645575c475449598495d5e7a83d73337e0d0b2abb&ascene=0&uin=Mjg3NzExNzMwMg%3D%3D&devicetype=iMac+MacBookPro14%2C1+OSX+OSX+10.12.5+build(16F2073)&version=12020810&nettype=WIFI&fontScale=100&pass_ticket=muMm3UdVz1qUrogXWkMnfiYGTn8UaexB1iPYgQfkN4HDrKrxAPPezMeF8nCgGqMT)
* [Array.prototype.filter() - JavaScript - MDN - Mozilla](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
* [Array.prototype.map() - JavaScript - MDN - Mozilla](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
* [Array.prototype.reduce() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)