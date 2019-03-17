---
title: map,filter,reduce的区别
date: 2017-11-29 10:10:10
tags: ES6
categories: ES6
---

###### 一张图及其形象的说明：

![知乎用户提供](http://ow47touqj.bkt.clouddn.com/filter_map_reduce.png)

## 一.map

**map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。**
用一张图来说明：
![来自廖雪峰的](https://www.liaoxuefeng.com/files/attachments/0013879622109990efbf9d781704b02994ba96765595f56000/0)
代码实例：（初学者简单的使用就好了）
<!-- more -->
```
let numbers = [1, 5, 10, 15];
let doubles = numbers.map((x) => {
   return x * 2;
});

// doubles is now [2, 10, 20, 30]
// numbers is still [1, 5, 10, 15]


let numbers = [1, 4, 9];
let roots = numbers.map(Math.sqrt);

// roots is now [1, 2, 3]
// numbers is still [1, 4, 9]
```

##### map()的真正的语法：

```
let array = arr.map(function callback(currentValue, index, array) { 
    // Return element for new_array 
}[, thisArg])
```

参数说明：
callback 生成新数组元素的函数，使用三个参数：
**currentValue**
callback 的第一个参数，数组中正在处理的当前元素。
**index**
callback 的第二个参数，数组中正在处理的当前元素的索引。
**array**
callback 的第三个参数，map 方法被调用的数组。

thisArg
可选的。执行 callback 函数时 使用的this 值。
返回值
一个新数组，每个元素都是回调函数的结果。
运行说明：

```
let a = [1,2,3,4,5,2,6,10] ;
let b =a.map(function(value,index,array){
    console.log(value,index,array);
})
//1 0 [1,2,3,4,5,2,6,10]
//2 1 [1,2,3,4,5,2,6,10]
//3 2 [1,2,3,4,5,2,6,10]...
```

暂时还不知道thisArg的用法（^-^）,所有最多用两个参数好了
来自微软的map说明：

```
// Define an object that contains a divisor property and
// a remainder function.
var obj = {
    divisor: 10,
    remainder: function (value) {
        return value % this.divisor;
    }
}

// Create an array.
var numbers = [6, 12, 25, 30];

// Get the remainders.
// The obj argument specifies the this value in the callback function.
var result = numbers.map(obj.remainder, obj);
document.write(result);

// Output:
// 6,2,5,0
```

暂时就先这样吧，以后再改
碰到的一个有趣的地方

```
["1","2","3"].map(parseInt)//会得到什么?
// 你可能觉的会是[1, 2, 3]
//但是现实总是残酷的
// 但实际的结果是 [1, NaN, NaN]
// 通常使用parseInt时,只需要传递一个参数.
// 但实际上,parseInt可以有两个参数.第二个参数是进制数.
// 可以通过语句"alert(parseInt.length)===2"来验证.
// map方法在调用callback函数时,会给它传递三个参数:当前正在遍历的元素, 
// 元素索引, 原数组本身.
// 第三个参数parseInt会忽视, 但第二个参数不会,也就是说,
// parseInt把传过来的索引值当成进制数来使用.从而返回了NaN.

function returnInt(element) {
  return parseInt(element, 10);
}

['1', '2', '3'].map(returnInt); // [1, 2, 3]
// 意料之中的结果

// 也可以使用简单的箭头函数，结果同上
['1', '2', '3'].map( str => parseInt(str) );

// 一个更简单的方式:
['1', '2', '3'].map(Number); // [1, 2, 3]
// 与`parseInt` 不同，下面的结果会返回浮点数或指数:
['1.1', '2.2e2', '3e300'].map(Number); // [1.1, 220, 3e+300]
```

## 二.filter

**filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。**
和上面一样，直接拿代码

```
function isBigEnough(value) {
  return value >= 10;
}

var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);

// filtered is [12, 130, 44]

// ES6 way

const isBigEnough = value => value >= 10;//很骚的函数

let [...spraed]= [12, 5, 8, 130, 44];//解构赋值

let filtered = spraed.filter(isBigEnough);

// filtered is [12, 130, 44]
```

语法可以参考map的，这里就简单说明

```
var new_array = arr.filter(callback[, thisArg])
```

**callback**
用来测试数组的每个元素的函数。调用时使用参数 (*element*, *index*, *array*)。
返回true表示保留该元素（通过测试），false则不保留。
**thisArg**
可选。执行 callback 时的用于 this 的值。
返回值
一个新的通过测试的元素的集合的数组

## 三.reduce

**reduce() 方法对累加器和数组中的每个元素 (从左到右)应用一个函数，将其减少为单个值。**

```
var total = [0, 1, 2, 3].reduce(function(sum, value) {
  return sum + value;
}, 0);
// total is 6
//居然可以用reduce降维
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(function(a, b) {
  return a.concat(b);
}, []);
// flattened is [0, 1, 2, 3, 4, 5]
```

语法

```
array.reduce(function(accumulator, currentValue, currentIndex, array), initialValue)
```

说明
callback
执行数组中每个值的函数，包含四个参数
**accumulator**
上一次调用回调返回的值，或者是提供的初始值（initialValue）
**currentValue**
数组中正在处理的元素
**currentIndex**
数据中正在处理的元素索引，如果提供了 initialValue ，从0开始；否则从1开始
**array**
调用 reduce 的数组
**initialValue**
可选项，其值用于第一次调用 callback 的第一个参数。如果没有设置初始值，则将数组中的第一个元素作为初始值。空数组调用reduce时没有设置初始值将会报错。
返回值
函数累计处理的结果
**注意**:
不提供 initialValue ，reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引。提供 initialValue ，从索引0开始。

如果数组为空并且没有提供initialValue， 会抛出TypeError 。如果数组仅有一个元素（无论位置如何）并且没有提供initialValue， 或者有提供initialValue但是数组为空，那么此唯一值将被返回并且callback不会被执行。

提供 initialValue 通常更安全，正如下面的例子，如果没有提供initialValue，则可能有三种输出：

```
var maxCallback = ( pre, cur ) => Math.max( pre.x, cur.x );
var maxCallback2 = ( max, cur ) => Math.max( max, cur );

// reduce() without initialValue
[ { x: 22 }, { x: 42 } ].reduce( maxCallback ); // 42
[ { x: 22 }            ].reduce( maxCallback ); // { x: 22 }
[                      ].reduce( maxCallback ); // TypeError

// map/reduce; better solution, also works for empty arrays
[ { x: 22 }, { x: 42 } ].map( el => el.x )
                        .reduce( maxCallback2, -Infinity );
```

**reduce的运行**

```
[0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array){
  return accumulator + currentValue;
//箭头操作
[0, 1, 2, 3, 4].reduce( (prev, curr) => prev + curr );
//同样的结果
});
```

懒得弄表格了，直接上图
![来自MDN截图](http://ow47touqj.bkt.clouddn.com/QQ%E5%9B%BE%E7%89%8720170922162803.png)
[相关reduce文档(来自MDN)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
相信你看了这么多，应该可以简单的使用这三个函数了

### 再讲一下foreach吧

**forEach() 方法对数组的每个元素执行一次提供的函数。**

```
let a = ['a', 'b', 'c'];

a.forEach(function(element) {
    console.log(element);
});

// a
// b
// c
```

语法：

```
array.forEach(callback(currentValue, index, array){
    //do something
}, this)

array.forEach(callback[, thisArg])
```

现在，你会发现foreach和map,reduce好像可以通用,的确，在某些方面你随便用哪个都可以。
但是，你需要知道foreach的返回值是undefined
**参数**
*callback*
为数组中每个元素执行的函数，该函数接收三个参数：
*currentValue*(当前值)
数组中正在处理的当前元素。
*index*(索引)
数组中正在处理的当前元素的索引。
*array*
forEach()方法正在操作的数组。
thisArg可选
可选参数。当执行回调 函数时用作this的值(参考对象)。
**返回值**
undefined.
个人不建议使用这个东西，不为什么，就任性 ;
[详情请参考MND文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)