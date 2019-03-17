---
title: Flexbox
date: 2018-11-26 20:10:10
tags: CSS
categories: CSS
---

### Flexbox 让复制布局变得简单
传统上我们用 float 来设置布局，但是会带来恼人的副作用，需要用 clearfix 这样的复杂技巧去弥补。Flexbox 来了，一切变得简单。
[css-tricks 上的 flexbox 指南](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
<!-- more -->
### 浏览器兼容
比较新的各个移动版浏览器，包括 Chrome 等比较新的浏览器对 flexbox 都有很好的支持。如果不是太老的微信中，flexbox 也没有问题了。到 [http://caniuse.com](http://caniuse.com) 上看一下，唯一有问题的就是 IE ，但是 IE 是一个即将推出历史舞台的东西了，不用担心。
注意：根据 [css-tricks 的文章](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) ，IE10 是支持 flexbox 的，但是用的语法是 display: flexbox 而不是主流通用的 display: flex ，这个不用我们自己操心，想 autoprefixer 这样的工具会自动帮我们处理的。
### 使用flex布局
```
.container {
  display: flex;
}
```


![图片](https://images-cdn.shimo.im/ETVr3xJ4pyYx565R/image.png!thumbnail)
### flex-direction :*对 flex item 的排列方向做控制。*

![图片](https://images-cdn.shimo.im/kkp6bl0OqGgcsaDS/image.png!thumbnail)
row                                row-reverse                 column             column-reverse

```
.container {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

### flex-wrap ：设置可以确定当全部的 flex item 的宽度之和大于 flex container 的时候，排列是否要换行。
![图片](https://images-cdn.shimo.im/3ta1dRT0fuQQ4NrC/image.png!thumbnail)
```
.container{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```
默认为nowrap不换行。

### justify-content ：控制元素的**对齐**方式
![图片](https://images-cdn.shimo.im/uteF7flkV4ge3iw4/image.png!thumbnail)

```
.container {
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
}
```

### order：控制元素的排列顺序
![图片](https://images-cdn.shimo.im/qNaAXJxOhBQUrIKG/image.png!thumbnail)

```
.item {
  order: <integer>; /* default is 0 */
}
```
### flex 设置尺寸比例
```
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```
### flex-basis 设置基本宽度
```
.item {
  flex-basis: <length> | auto; /* default auto */
}
```
### flex-grow 设置延展
![图片](https://images-cdn.shimo.im/pGhQZNldG3EfjStp/image.png!thumbnail)

```
.item {
  flex-grow: <number>; /* default 0 */
}
```
如果其他 item 不设置，而
```
.second {
  flex-grow: 1;
}
```
那么它就会在 container 内占据所有剩余空间。因为 flex-grow 的默认值是 0 。

### flex-shrink 设置压缩
```
.item {
  flex-shrink: <number>; /* default 1 */
}
```
flex-shrink 默认值是 1 。当 container 的宽度装不下所有的 flex item 的时候，flex-shrink 就会发挥作用了。
比如有下面的代码
```
.first {
  flex-shrink: 1;
}
.second {
  flex-shrink: 2;
}
```
这样, first 元素没被压缩1个像素，second 元素就会被压缩2个。
也可以通过设置 flex-shrink: 0 来表示“我是一块砖，不能压缩”。
### align-items
```
.container {
  align-items: flex-start | flex-end | center | baseline | stretch;
}

```
![图片](https://uploader.shimo.im/f/yWIiAgstEkANdlr3.png!thumbnail)

### align-content
```
.container {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```
![图片](https://uploader.shimo.im/f/8iv5xhWj3HYSQBz0.png!thumbnail)
# 响应式布局
```
<meta name="viewport" content="width=device-width, initial-scale=1" />
```
# 三栏布局
用 flexbox 来制作响应式的三栏布局，主要涉及 flex-grow ，order 等属性的使用。

main.css 内容如下
```
body {
  margin: 0;
}
.parent {
  height: 100vh;
  display: flex;
  flex-direction: column;
}



.header, .footer {
  color: white;
  background: #212121;
  padding: 20px;
}

.main {
  flex-grow: 1;
  background-color: #B6B6B6;
  display: flex;
}

.col-1, .col-2, .col-3 {
  padding: 20px;
  color: white;
}
.col-1 {
  background-color: #9C27B0;
  flex-grow: 1;
}
.col-2 {
  background-color: #FF5722;
  order: -1;
  width: 120px;
}
.col-3 {
  background-color: #388E3C;
  width: 120px;
}

@media (max-width: 600px) {
  .main {
    flex-direction: column;
  }
  .col-2, .col-3 {
    width: auto;
  }
}
```
对应 index.html 内容
```
<head>
  ...
  <meta name="viewport" content="width=device-width, initial-scale=1" />
</head>
<body>
  <div class="parent">
    <div class="header">header</div>
    <div class="main">
      <div class="col-1">主体内容</div>
      <div class="col-2">导航栏</div>
      <div class="col-3">侧边栏</div>
    </div>
    <div class="footer">footer</div>
  </div>
</body>
```


# 流体网格
主要涉及到 flex-basis 和 flex-wrap 的深入理解。

全部 CSS 代码就是这些了
```
body {
  margin: 0;
  display: flex;
  padding-top: 30px;
  flex-wrap: wrap;
}
.box {
  background-color: goldenrod;
  height: 200px;
  flex-basis: 100%;
  margin: 0 20px 40px;
}

@media (min-width: 600px) {
  .box {
    flex-basis: calc(50% - 40px);
  }
}
@media (min-width: 900px) {
  .box {
    flex-basis: calc(33.3333% - 40px);
  }
}
```
对应 index.html
```
<body>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
</body>
```

