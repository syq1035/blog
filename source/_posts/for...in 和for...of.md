---
title: for...in和for...of
date: 2018-11-29 20:10:10
tags: JS
categories: JS
---

### for-in是一种精准的迭代语句，可以用来枚举对象的属性。
```
var myObj = { "name":"runoob", "alexa":10000, "site":null };
for (var x in myObj) {
    document.getElementById("demo").innerHTML += x + "<br>";
}
```
或
<!-- more -->
```
var myObj = { "name":"runoob", "alexa":10000, "site":null };
for (var x in myObj) {
    document.getElementById("demo").innerHTML += myObj[x] + "<br>";
}
```
### for...in遍历数组，返回数组的下标
```
var sites = [ "Google", "Runoob", "Taobao" ]
for (var i in sites) {
    x += sites[i] + "<br>";
} //"Google", "Runoob", "Taobao"
```
### for...of 遍历数组，返回数组项值
```
sites:[ "Google", "Runoob", "Taobao" ]
for (var item of sites) {
    x += item + "<br>";
} //"Google", "Runoob", "Taobao"
```

