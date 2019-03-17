---
title: Canvas
date: 2017-10-28 20:10:10
tags: Canvas
categories: Canvas
---

### 创建canvas

首先创建一个canvas元素，我们只需要在html文件中加入这么一句代码：

```
<canvas id="canvas"></canvas>
```

同时我们也可以通过canvas的标签属性width和height设置canvas画布的大小：

```
<canvas id="canvas" width="800" height="800"></canvas>
```
<!-- more -->
当然，我们也可以通过js来设置canvas的宽高。

接下来我们就在js中获取到该canvas元素，然后设置它的宽高，并获取到上下文的环境：

```
var canvas = document.getElementById("canvas");//获取到canvas元素
//设置宽高
canvas.width = 800;
canvas.height = 800;
var context = canvas.getContext("2d");//获取上下文的环境
```

接下来的所有操作都是基于这个context的上下文环境。

### 画线

```
context.moveTo(100,100);
context.lineTo(500,500);
```

moveTo()方法表示一次绘制的起点坐标，lineTo()表示基于上一个坐标点到这个坐标点之间的直线连接。

值得注意的是，canvas是基于状态的绘制，而不是基于对象的绘制。所以，上面代码都是状态的设置，我们还需要使用stroke()方法进行绘制：

```
context.stroke();//绘制
```

线条的其它基本属性：

```
context.lineWidth = 8;//线条的宽度
context.strokeStyle = "#333";//线条的颜色
```

一个简单的绘制一条直线的完整例子：

```
<canvas id="canvas"></canvas>                                                     **.html
```

```
var canvas = document.getElementById("canvas");//获取到canvas元素                    **.js
//设置宽高
canvas.width = 800;
canvas.height = 800;
var context = canvas.getContext("2d");//获取上下文的环境
//canvas 是基于状态的绘制，而不是对象
context.moveTo(100,100);
context.lineTo(500,500);
context.lineWidth = 8;//线条的宽度
context.strokeStyle = "#333";//线条的颜色
context.stroke();//绘制
```

### 画矩形

```
//绘制实心矩形:
context.fillStyle = 'blue';
context.fillRect(100,125,50,50); //(x,y,w,h)

//绘制空心矩形
context.strokeStyle = 'yellow';
context.strokeRect(100,180,50,50) //(x,y,w,h)
```

### 画网格

网格是通过先画矩形，然后在矩形内画线形成的

```
//描绘边框
context.beginPath();
context.linewidth = 1; 
context.rect(0,0,width,height);
context.stroke();
//准备画横线
for(let row_i = 1; row_i<row; row_i++){
var y = row_i * 30;             
context.moveTo(0,y);  
context.lineTo(width,y);
}
//准备画竖线
for(let col_i = 1; col_i<col; col_i++){
var x = col_i * 30;                          
context.moveTo(x, 0);  
context.lineTo(x, height);
}
//完成描绘  
context.stroke();
```

绘制动态大小的网格例子:

```
//单个网格宽高
let GAID_WIDTH = 30;
let GAID_HEIGHT = 30;

let canvas = document.getElementById('canvas');
context = canvas.getContext("2d");
 
function createGrid(row,col){
    //计算画布宽高
    let width = GAID_WIDTH*col;
    let height = GAID_HEIGHT*row;
    //设置画布宽高
    canvas.setAttribute("width",width);
    canvas.setAttribute("height",height);
    
    //描绘边框
    context.beginPath();
    context.linewidth = 1; 
    context.rect(0,0,width,height);
    context.stroke();

    //准备画横线
    for(let row_i = 1; row_i<row; row_i++){
    var y = row_i*GAID_HEIGHT;          
    context.moveTo(0,y);  
    context.lineTo(width,y);
    }
    //准备画竖线
    for(let col_i = 1; col_i<col; col_i++){
    var x = col_i*GAID_WIDTH;                        
    context.moveTo(x,0);  
    context.lineTo(x,height);
    }
    //完成描绘  
    context.stroke();
 }
```

### 方法

1 . `moveTo(x, y)` 将笔触移动到指定的坐标x以及y上。

当canvas初始化或者`beginPath()`调用后，你通常会使用`moveTo()`函数设置起点。我们也能够使用`moveTo()`绘制一些不连续的路径。

2 . `lineTo(x, y)`

该方法有两个参数：x以及y ，代表坐标系中直线结束的点。开始点和之前的绘制路径有关，之前路径的结束点就是接下来的开始点，等等。。。开始点也可以通过`moveTo()`函数改变。

3 . `beginPath()` 开始一条路径，或重置当前的路径

4 .`closePath()` 创建从当前点到开始点的路径

5 .`fillStyle = color` 设置图形的填充颜色。

`strokeStyle = color` 设置图形轮廓的颜色。

```
context.fillStyle = "black";      //fillStyle 属性来填充另一个颜色/渐变
context.fill();      //fill()方法填充图像（默认是黑色）
```

```
context.font="15px Georgia";     //设置字体
context.fillText( text,x,y,maxWidth )    //在画布上绘制填色的文本。文本的默认颜色是黑色
```

6 .`clearRect( x,y,width,height )` 清空给定矩形内的指定像素 , 可用于在重绘时清空画布

7 .`isPointInPath( x,y )` 方法返回 true，如果指定的点位于当前路径中；否则返回 false。

仅能识别当前上下文环境里的图形路径，而之前绘制的路径，无法回溯判断。这种问题的解决方法是：当点击事件发生时，重绘所有图形，每绘制一个就使用isPointInPath方法，判断事件坐标是否在该图形覆盖范围内。

8 .`getImageData(x, y, width, height)` 获取 canvas 指定范围的像素数据，然后遍历判断 RGBA 色值。

### 使用jquery控制canvas的时候出现的问题

```
//DOM操作，没有问题
let canvas = document.getElementById('canvas');
context = canvas.getContext("2d");
```

换成jQuery

```
context = $("#canvas").getContext("2d");
//出现错误，对象获取不到getContext()方法
```

jQuery操控的正确方法：

```
context = $("#canvas")[0].getContext("2d");
```

原因：jQuery对象就是类数组，用数组下标可以取得Dom对象