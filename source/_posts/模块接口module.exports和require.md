---
title: 模块接口module.exports和require
date: 2017-10-29 10:10:10
tags: ES6
categories: ES6
---

### Node.js 中的模块接口 module.exports 的使用方法

在 nodejs 中，提供了 exports 和 require 两个对象，其中 exports 是模块公开的接口，你可以用它来创建你的模块 ; require 用于从外部获取一个模块的接口(也就是获取模块的 exports 对象).

require 函数使用一个参数, 参数值可以带有完整路径的模块的文件名,也可以为模块名.当使用 node 中提供的模块时,在 require 函数中只需要指定模块名即可.

#### 一 . 返回一个普通函数

我们创建了一个 app.js 文件,需要在 main.js 中引用它,于是在 app.js 同级目录新建了一个 main.js 文件
<!-- more -->
**app.js**

```
function add(a,b){
    return a+b;
}
module.exports=add;
```

**main.js**

```
let addSum=require('./app');

function main(){

    let sum = addSum(1,2);
    console.log(sum);

}

main();//3
```

#### 二. 中的这种方式还可以用于返回几个 require 的其他模块，可以实现一次 require 多个模块

**module_collection.js**

```
var module_collection = {

    "module1": require("./module1"),
    "module2": require("./module2"),

};

module.exports = module_collection;
```

**main.js**

```
let module_collection = require("./module_collection");

let module1 = module_collection.module1;

let module2 = module_collection.module2;

 // Do something with module1 and module2
```

#### 三 . 用这种方式返回几个函数

**functions.js**

```
let func1 = function() {

    console.log("func1");

};

let func2 = function() {

    console.log("func2");

};

exports.function1 = func1;

exports.function2 = func2;
```

**main.js**

```
let functions = require("./functions");

functions.function1();

functions.function2();
```