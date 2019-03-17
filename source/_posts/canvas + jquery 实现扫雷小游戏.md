---
title: canvas + jquery 实现扫雷小游戏
date: 2017-10-26 10:10:10
tags: 小游戏
categories: 小游戏
---

[游戏试玩](https://syq1035.github.io/clearMine/)

### 前言

这个小游戏主要思路是canvas与数组的映射，每一个小网格对应二维数组的一个元素，利用数组来存储网格的信息，如 某网格是否有雷，周围九宫格雷的个数，是否已经被打开

<!-- more -->

这里定义了三个数组

```
let gridStatus_arr = [];          //存储格子身份状态，状态1为雷
let mineNum_arr = [];            //存储格子周围八个格子雷的数量
let click_arr = [];           //存储格子点击状态，点击过的为1
```

### 1. 布局

先添加三个难度的按钮

```
<button class="type" data-row="9" data-col="9" data-mine="10">简单</button>
<button class="type" data-row="14" data-col="14" data-mine="36">中级</button>
<button class="type" data-row="14" data-col="22" data-mine="56">高级</button>
```

点击相应按钮通过 js 获取对应的 `data` 属性, 然后动态的生成相应的方格, 然后动态计算包裹所有小方格的宽度和高度

然后创建一个canvas元素

```
<canvas id="canvas"></canvas>
```

canvas画布的大小属性width和height先不设置，后面通过js动态设置

### 2. 按钮点击事件，并初始化画布

点击难度按钮，动态初始化画布

```
//获取canvas元素
context = $("#canvas")[0].getContext("2d");

$(".type").click(function(){
    row = $(this).data('row');
    col = $(this).data('col');
    mine = $(this).data('mine');
    init(mine);
})

//初始化
function init(mine){
    createGrid();      //创建网格
    createMine(mine);     //随机生成雷
    countArountMine();      //计算每个格子周围八个格子雷的数量
    createClickEvent();    //添加点击事件
}
```

### 3.canvas创建网格

利用canvas画网格，先画一个相应大小的矩形，然后在矩形内画线形成网格

```
function createGrid(){
        //计算画布宽高
        let width = GAID_WIDTH*col;
        let height = GAID_HEIGHT*row;

        //设置画布宽高
        canvas.setAttribute("width",width)
        canvas.setAttribute("height",height)
        //描绘边框
        context.beginPath();
        context.linewidth = 1; 
        context.rect(0,0,width,height);
        context.stroke();
        context.fillStyle = '#909090';
        context.fill();

        //准备画横线
        for(let row_i = 1; row_i<row; row_i++){
            var y = row_i*GAID_HEIGHT; 
            context.strokeStyle = 'white';            
            context.moveTo(0,y);  
            context.lineTo(width,y);
        }
        //准备画竖线
        for(let col_i = 1; col_i<col; col_i++){
            var x = col_i*GAID_WIDTH;  
            context.strokeStyle = 'white';                        
            context.moveTo(x,0);  
            context.lineTo(x,height);
        }
        //完成描绘  
        context.stroke();
    }
```

### 4. 随机生成雷

添加雷，改变格子身份状态 ，1代表雷区

这里是利用二维数组和网格的映射来实现的

```
function createMine(mine){
        $('#mine').text(mine);
        //将数组变为二维
        for(let i=0; i<row; i++){
            gridStatus_arr[i] = [];
            mineNum_arr[i] = [];
            click_arr[i] = [];
        }
        //初始化二维数组
        for(let i=0; i<row; i++){
            for(let j=0; j<col; j++){
                gridStatus_arr[i][j] = 0;
                mineNum_arr[i][j] = 0;
                click_arr[i][j] = 0;
            }
        }
        //添加雷，改变格子身份状态
        while(mine!=0){
            let mine_row = Math.floor(Math.random() * row);
            let mine_col = Math.floor(Math.random() * col);
            mine--;
            //预防生成位置相同
            if(gridStatus_arr[mine_row][mine_col] == 1){
                mine++;
                continue;
            }
            gridStatus_arr[mine_row][mine_col] = 1;
        }
    }
```

### 5.计算每个格子周围八个格子雷的数量

利用对象来取当前格子周围九宫格的范围，遍历查看对应数组的身份状态

```
function countArountMine(){
    for(let i=0; i<row; i++){
        for(let j=0; j<col; j++){
            let around = aroundGrid(i,j);
            let nx = around.nx,
                sx = around.sx,
                wy = around.wy,
                ey = around.ey;
            
            let mineNum = 0; 

            for(let x=nx; x<=sx; x++){
                for(let y=wy; y<ey; y++){
                    if(gridStatus_arr[x][y]==1){
                        mineNum++;
                    }
                }
            }
            mineNum_arr[i][j] = mineNum;                            
        }
    }
}
```

### 6. 添加点击事件

要先去掉默认的contextmenu事件，否则会和默认右键事件同时出现。

```
document.oncontextmenu = function (e) {
        e.preventDefault();
};
```

再添加鼠标点击事件，e.which = 1为鼠标左键 ，e.which = 3为鼠标右键

```
function createClickEvent(){     
        $("#canvas").off();
        $("#canvas").mousedown(function(e){
            //因为canvas的X，Y坐标和数组的行列相反，这里直接转换了一下
            let x = Math.floor(e.offsetY / GAID_HEIGHT);
            let y = Math.floor(e.offsetX / GAID_WIDTH);

            if(e.which == 1){
                judgeGridStatus(x,y);
            }else if(e.which == 3){
                signMine(x,y);
            }
        })
    }
```

### 7. 判断输赢

对比身份状态是否为1 来判断是否点击到雷。

通过 已打开的格子个数 和 row*col - mine 对比判断是否扫雷成功

这里就不贴代码了，详细可参考我的GitHub仓库

### 最后

在canvas的用法上可能有一些不规范，因为花的时间比较短, 用的方法可能不太好, 还请大佬们多多包涵。

[项目GitHub地址](https://github.com/syq1035/clearMine)