---
title: jasmine的简单使用
date: 2018-5-23 20:10:10
tags: 测试
categories: 测试
---

### 一·介绍
Jasmine就是一种JavaScript单元测试框架，它不依赖任何其他JS框架，也不需要对DOM操作，具有灵巧而明确的语法可以让你轻松的编写测试代码。它是一套Javascript行为驱动开发框架（BDD），干净简洁，表达力强且易于组织，不依赖于其他任何框架和DOM，可运行于Node.js，浏览器端或移动端。
**这里有官方的文档，github的源码下载**
现在是2017-11-21，jasmine是2.8版本;
* [jsamine官方文档](https://jasmine.github.io/)
* [github下载源码](https://github.com/jasmine/jasmine/releases)
<!-- more -->
### 二. 安装
先执行npm init来生成package.json文件
![图片](https://images-cdn.shimo.im/i98cWNvGmQMR3G2X/1.png!thumbnail)
然后全局安装jasmine
```
npm install -g jasmine
```
给这个项目添加依赖包
```
npm install --save jasmine
```
![图片](https://images-cdn.shimo.im/BHM5pfBcOTAGRhy3/2.png!thumbnail)
再要初始化jasmine
```
jasmine init
```
![图片](https://images-cdn.shimo.im/YUX4xzT9GegRlnx8/3.png!thumbnail)
最后需要修改一下package.json
![图片](https://images-cdn.shimo.im/bbhRxjsVssgggBqR/4.png!thumbnail)
现在就可以spec里面新建文件写测试了，也可以新建一个文件写函数
  
                      ![图片](https://images-cdn.shimo.im/jiPpoduj3m853UjZ/5.png!thumbnail)
