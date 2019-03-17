---
title: parseInt()
date: 2017-9-29 10:10:10
tags: JS
categories: JS
---

parseInt()函数可解析一个字符串，并返回一个整数.如果第一个字符不是数字，会打印出NaN,若是字符和数字一起，并且数字在前，会打印数字忽略字母

parseInt()方法：

 首先想到的是js提供的parseInt方法，例子：

```
var str ="4500元";

var num = parseInt(str);

alert(num);//4500
```
<!-- more -->
 结果就是我们想要的， 以为就这么简单，那就错了。如果字符串前面有非数字字符，上面这种方法就不行了：

```
var str ="价格：4500元";

var num = parseInt(str);

    

alert(num);//NaN
```

