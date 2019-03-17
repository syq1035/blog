---
title: clientX、pageX、offsetX、screenX的区别
date: 2018-11-25 20:10:10
tags: JS
categories: JS
---

### **event.clientX、event.clientY**
鼠标相对于[浏览器](https://www.2cto.com/os/liulanqi/)窗口可视区域的X，Y坐标（窗口坐标），可视区域不包括工具栏和滚动条。IE事件和标准事件都定义了这2个属性
### **event.pageX、event.pageY**
类似于event.clientX、event.clientY，但它们使用的是文档坐标而非窗口坐标。这2个属性不是标准属性，但得到了广泛支持。IE事件中没有这2个属性。
<!-- more -->
### **event.offsetX、event.offsetY**
鼠标相对于事件源元素（srcElement）的X,Y坐标，只有IE事件有这2个属性，标准事件没有对应的属性。
### **event.screenX、event.screenY**
鼠标相对于用户显示器屏幕左上角的X,Y坐标。标准事件和IE事件都定义了这2个属性
![图片](https://images-cdn.shimo.im/PjqoamUz6UAyHkae/image.png!thumbnail)
