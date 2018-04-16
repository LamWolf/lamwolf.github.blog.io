---
layout: post
title: "pacage.json中的scripts脚本是什么？"
subtitle: "scripts字段的原理和语法"
author: "LamWolf"
tags: 
    - npm
date: 2018-04-13 11:00:00
catalog: true
header-img: "img/post-bg-script.jpg"
---

## 写在前面

昨天刚把毕设做完，其实也不算完全做完，只是把逻辑调通，流程调通，还有数据需要补充，整体项目也还没有完全测试，不过不管怎样毕设的事要先告一段落了。

趁公司的需求暂时还没有跟上，就先来充充电。公司一个前辈问我，package.json中的scripts字段你知道里面的内容都是什么吗？知道“&&”的作用吗？那“&”又代表什么意思呢？前两个问题我还能答的上来，最后一个问题就不了解了。

今天就借这个机会，研究一下package.json中scripts的相关知识，npm run到底做了些什么。

PS：回顾了一下自己写过的文章，觉得目录结构有一些臃肿了，从这篇开始尝试一下更简单的目录结构。

## 一、什么是npm脚本

package.json文件中有一个scritps对象，该对象的每个属性，都对应一个段脚本，比如：

```js
"scripts":{
	"build": "node build.js"
}
```

那么对应的

```
$ npm run build
#等同于
$ node build.js
```

这样，执行你预定义的一个指令名，npm会帮你执行你定义好的对应的那段脚本。

## 二、原理

每当执行`npm run`的时候，就会自动建立一个Shell，在这个Shell里面执行指定的脚本命令。因此，只要是Shell（一般是 Bash）可以运行的命令，就可以写在 npm 脚本里面。

由于 npm 脚本的唯一要求就是可以在 Shell 执行，因此它不一定是 Node 脚本，任何可执行文件都可以写在里面。

## 三、通配符

由于 npm 脚本就是 Shell 脚本，因此可以使用 Shell 通配符。

```json
"lint": "jshint *.js"
"lint": "jshint **/*.js"
```

上面代码中，`*`表示任意文件名，`**`表示任意一层子目录。

如果要将通配符传入原始命令，防止被 Shell 转义，要将星号转义。

```json
"test": "tap test/\*.js"
```

## 四、传参

向 npm 脚本传入参数，要使用`--`标明。

```json
"lint": "jshint **.js"
```
向上面的`npm run lint`命令传入参数，必须写成下面这样。

```
$ npm run lint --  --reporter checkstyle > checkstyle.xml
```

也可以在package.json里面再封装一个命令。

```json
"lint": "jshint **.js",
"lint:checkstyle": "npm run lint -- --reporter checkstyle > checkstyle.xml"
```

不过根据实际应用经验和网上资料搜索，一般来说只使用一个`--`，即：

```json
"lint:checkstyle": "npm run lint --reporter checkstyle > checkstyle.xml"
```

## 五、执行顺序

npm脚本里可以一个脚本执行多个任务，那这个时候就需要明确它们的执行顺序。

如果是并行执行（即同时的平行执行），可以使用`&`符号

```
$ npm run script1.js & npm run script2.js
#这两个脚本是同时执行的
"demo": "node script1.js & node script2.js"
```

如果是继发执行（即只有前一个任务成功，才有下一个任务），可以使用`&&`符号

```json
$ webpack --watch && node server.js
#只有webpack指令执行成功后才能执行node server.js的脚本
"build": "webpack --watch && node server.js"
```

## 六、>、<、| 操作符简介

`>`表示将输出结果写入进某个文件中

```
"write": "node math.js > result.json"
```

`<`表示将某个文件的内容作为输入参数传入某个指令中

```
"read": "node math.js < param.txt"
```

`|`表示管道，与Shell的`|`用法相同，将一个指令的执行结果传入到下一个指令中

```
"past": "node read.js | node math.js"
```

## 七、钩子

npm 脚本有pre和post两个钩子。举例来说，`build`脚本命令的钩子就是`prebuild`和`postbuild`。

```
"prebuild": "xxxxxxxxxxxxxxx",
"build": "xxxxxxxxxxxxxx",
"postbuild": "xxxxxxxxxxxxxxxx"
```

当执行`npm run build`时，其实是执行如下的指令

```
npm run prebuild && npm run build && npm run postbuild
```

因此我们常把一些准备工作和清理工作放在钩子里，如下

```
"clean": "rimraf ./dist && mkdir dist",
"prebuild": "npm run clean",
"build": "cross-env NODE_ENV=production webpack"
```

npm中有提供一些默认的钩子，如下：

* prepublish，postpublish
* preinstall，postinstall
* preuninstall，postuninstall
* preversion，postversion
* pretest，posttest
* prestop，poststop
* prestart，poststart
* prerestart，postrestart

当然，除了这些默认钩子，我们同样也可以给自定义脚本上加钩子。比如，`myscript`这个脚本命令，也有`premyscript`和`postmyscript`这两个钩子。

不过，当一个脚本名中有两个`pre`或者`post`时，钩子就会失效。比如`preprescript`和`postpostscript`就不会有钩子效果。

## 参考文章

* [npm scripts 使用指南- 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)
* [How to Use npm as a Build Tool](https://www.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/)
* [Awesome npm scripts](https://github.com/RyanZim/awesome-npm-scripts#articles)