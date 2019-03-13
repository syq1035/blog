---
title: JS取整
date: 2018-10-29 10:10:10
tags: JS
categories: web
---

1.丢弃小数部分,保留整数部分
parseInt(9/2)

2.向上取整,有小数就整数部分加1
Math.ceil(9/2)

3,四舍五入
Math.round(9/2)
<!-- more -->
4,向下取整
Math.floor(9/2)

5、保留小数位数
11.989.toFixed(2) // 11.99
11.984.toFixed(2) // 11.98

6、javascript多位数四舍五入的通用方法

```
//num表示要四舍五入的数,v表示要保留的小数位数。

function decimal(num,v) {

var vv = Math.pow(10,v);

return Math.round(num*vv)/vv;

}
```