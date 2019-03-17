---
title: node+express实现个性化聊天室
date: 2018-11-29 10:10:10
tags: NodeJS
categories: NodeJS
---

## 实现功能

- 登录检测
- 系统自动提示用户状态（进入/离开）
- 显示在线用户
- 支持发送和接收消息
- 自定义字体颜色
- 支持发送表情
- 支持发送图片
<!-- more -->
## 前期准备

[node及npm环境](https://link.juejin.im/?target=https%3A%2F%2Fwww.liaoxuefeng.com%2Fwiki%2F001434446689867b27157e896e74d51a89c25cc8b43bdb3000%2F001434501245426ad4b91f2b880464ba876a8e3043fc8ef000)、[express](https://link.juejin.im/?target=http%3A%2F%2Fwww.expressjs.com.cn%2F4x%2Fapi.html)、[socket.io](https://link.juejin.im/?target=https%3A%2F%2Fsocket.io%2F)

## 具体实现

**1、将聊天室部署到服务器**

先用node搭建一个服务器，部署在localhost:3000端口，先尝试向浏览器发送一个“Hello”，新建server.js文件。

```
let express = require('express');//引入express模块
let app = express();
let http = require('http').Server(app);

//路由为localhost:3000时向客户端响应"Hello"
app.get('/',function(req,res){
    res.send('<h1>Hello</h1>');//发送数据
});

//监听3000端口
http.listen(3000,function(){
    console.log("3000端口启动");
});
```

安装express模块：

```
#安装express模块
npm install --save express
```

一个node服务器搭建成功。

在chatRoom目录下启动服务器

```
node server.js
```

打开浏览器输入网址：localhost:3000是这样的

[3000](https://github.com/syq1035/syq1035.github.io/blob/master/img/3000-chatRoom.jpg)

新建文件夹client (css, js, image)，

在server.js里添加代码：

```
// 路由为/默认client静态文件夹
app.use('/', express.static(__dirname + '/client'));
```

修改:

```
app.get('/',function(req,res){
    res.send(index);//发送数据
});
```

express.static(__dirname + ‘/client’)是将www文件夹托管为静态资源，意味着这个文件夹里的文件（html、css、js）彼此可以用相对路径。

在client下新建index.html以及相应的css文件(此处不粘贴)，该页面用了font-awesome小图标

```
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>chatRoom</title>
    <link rel="stylesheet" href="css/index.css">
    <link rel="stylesheet" href="css/font-awesome-4.7.0/css/font-awesome.min.css">
</head>
<body>
    <div class="all">
        <div class="name">
            <input type="text" id="name" placeholder="请输入昵称···">
            <button id="nameBtn">确定</button>
        </div>
        <div class="main">
            <div class="header">
                <img src="image/logo.jpeg">
                秘密聊天室
            </div>
            <div class="container">
                <div class="conversation">
                    <ul id="messages"></ul>
                    <form action="">
                        <div class="edit">
                            <input type="color" id="color" value="#000000">
                            <i title="双击取消选择" class="fa fa-smile-o" id="smile"></i>
                            <i title="双击取消选择" class="fa fa-picture-o" id="img"></i>
                            <div class="selectBox">
                                <div class="smile"></div>
                                <div class="img"></div>
                            </div>
                        </div>
                        <textarea id="m"></textarea>
                        <button class="btn rBtn" id="sub">发送</button>
                        <button class="btn" id="clear">关闭</button>
                    </form>
                </div>
                <div class="contacts">
                    <h1>在线成员<span id="num">0</span></h1>
                    <ul id="users"></ul>
                    <p>当前无好友在线！</p>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```

打开localhost:3000，看到如下页面：

[page](https://github.com/syq1035/syq1035.github.io/blob/master/img/page-chatRoom.png)

聊天室部署成功。

**2、检测登录**

在客户端和服务器之间传送消息需要用到socket.io

```
//安装socket.io模块
npm install --save socket.io
```

server.js 做如下修改：

```
let app = express();
let http = require('http').Server(app);
let io = require('socket.io')(http);

// 路由为/默认client静态文件夹
app.use('/', express.static(__dirname + '/client'));

io.on('connection',function (socket) {
    console.log('a user connected');
});
```

当打开localhost:3000的时候会触发服务器端io的connection事件，会在服务器打印“a user connected”，但是我们想统计一下连接该服务器的用户人数，如果有用户连接就打印“n users connected”，n为用户人数，怎么办呢？

在server.js设置一个全局数组为user，每当一个用户连接成功就在连接事件中将用户的昵称push进user，打印user.length即可知道已成功连接用户的人数。

在用户连接的时输入昵称登录，我们应该检测一下用户的昵称是否已存在，避免昵称相同的情况发生，在服务器监听一个登录事件来判断该情况，由于一切都发生在用户连接之后，所以触发事件应该写在connection事件的回调函数中。

```
io.on('connection',function (socket) {
    //渲染在线人员
    io.emit('disUser',usersInfo);

    //登录，检测用户名
    socket.on('login',(user)=>{
        if(user.indexOf(user.name)>-1){    //判断昵称是否存在
            socket.emit('loginError');    //触发客户端的登录失败事件
        }else{
            users.push(user.name);    //存储用户昵称
            usersInfo.push(user);    //存储用户的昵称和头像
            socket.emit('loginSuc');   //触发客户端的登录成功事件
            socket.nickname = user.name;
            io.emit('system',{        //向所有用户广播该用户进入房间
                name:user.name,
                status:'进入'
            });
            io.emit('disUser',usersInfo);   //渲染右侧在线人员信息
            console.log(users.length + 'user connect');   //打印连接人数
        }
    })
});
```

这里区分一下 io.emit(foo)、socket.emit(foo)、socket.broadcast.emit(foo)

```
io.emit(foo);   //会触发所有客户端用户的foo事件
socket.emit(foo);   //只触发当前客户端用户的foo事件
socket.broadcast.emit(foo);   //触发除了当前客户端用户的其他用户的foo事件
```

客户端代码client.js

```
$(function () {
    // io-client
    // 连接成功会触发服务器端的connection事件
    var socket = io();

    // 点击输入昵称，回车登录
    $('#name').keyup((ev)=> {
        if(ev.which == 13) {
            inputName();
        }
    });
    $('#nameBtn').click(inputName);
    // 登录成功，隐藏登录层
    socket.on('loginSuc', ()=> {
        $('.name').hide();
    })
    socket.on('loginError', ()=> {
        alert('用户名已存在，请重新输入！');
        $('#name').val('');
    });

    function inputName() {
        var imgN = Math.floor(Math.random()*4)+1; // 随机分配头像
        if($('#name').val().trim()!=='')
            socket.emit('login', {
                name: $('#name').val(),
                img: 'image/user' + imgN + '.jpg'
            });  // 触发登录事件
        return false;
    }
})
```

**3、系统自动提示用户状态（进入/离开）**

该功能是为了实现上图所示的系统提示“XXX进入聊天室”，在登录成功时触发system事件，向所有用户广播信息，注意此时用的是io.emit而不是socket.emit，客户端client.js代码如下：

```
// 系统提示消息
socket.on('system', (user)=> { 
  var data = new Date().toTimeString().substr(0, 8);
  $('#messages').append(`<p class='system'><span>${data}</span><br /><span>${user.name}  ${user.status}了聊天室<span></p>`);
  // 滚动条总是在最底部
  $('#messages').scrollTop($('#messages')[0].scrollHeight);
});
```

**4、显示在线用户**

客户端监听一个显示在线用户的事件disUser，在以下三个时间段服务器端就触发一次该事件重新渲染一次

- 程序开始启动时

- 每当用户进入房间

- 每当用户离开房间

  客户端client.js代码如下：

```
//显示在线人员
   socket.on('disUser',(usersInfo)=>{
       displayUser(usersInfo);
   });
   function displayUser(users) {
       $('#users').text('');   // 每次都要重新渲染
       if(!users.length){
           $('.contacts p').show();
       }else {
           $('.contacts p').hide();
       }
       $('#num').text(users.length);
       for(var i = 0; i < users.length; i++) {
           var $html = `<li>
               <img src="${users[i].img}">
               <span>${users[i].name}</span>
           </li>`;
           $('#users').append($html);
       }
   }
```

登录成功后，可看到如下页面

[login](https://github.com/syq1035/syq1035.github.io/blob/master/img/login-chatRoom.png)

**5、支持发送和接收消息**

用户发送消息时触发服务器端的sendMsg事件，并将消息内容作为参数，服务器端监听到sendMsg事件之后向其他所有用户广播该消息，用的socket.broadcast.emit(foo) , 以下为server.js代码

```
// 发送消息事件
   socket.on('sendMsg', (data)=> {
       var img = '';
       for(var i = 0; i < usersInfo.length; i++) {
           if(usersInfo[i].name == socket.nickname) {
               img = usersInfo[i].img;
           }
       }
       socket.broadcast.emit('receiveMsg', {
           name: socket.nickname,
           img: img,
           msg: data.msg,
           color: data.color,
           type: data.type,
           side: 'left'
       });
       socket.emit('receiveMsg', {
           name: socket.nickname,
           img: img,
           msg: data.msg,
           color: data.color,
           type: data.type,
           side: 'right'
       });
   });
```

服务器端接受到来自用户的消息后会触发客户端的receiveMsg事件，并将用户发送的消息作为参数传递，该事件会向聊天面板添加聊天内容 , 以下为client.js代码

```
//点击按钮或回车键发送消息
    $('#sub').click(sendMsg);
    $('#m').keyup((ev)=>{
        if(ev.which ==13){
            sendMsg();
        }
    })
    //发送消息
    function sendMsg() {
        if($('#m').val()==''){
            alert('请输入内容！');
            return false;
        }
        socket.emit('sendMsg',{
            msg:$('#m').val()
        });
        $('#m').val('');
        return false;
    }

    //接收消息
    socket.on('receiveMsg',(obj)=>{
        $('#messages').append(`
            <li class='${obj.side}'>
          <img src="${obj.img}">
          <div>
            <span>${obj.name}</span>
            <p>${obj.msg}</p>
          </div>
        </li>
        `);
        //滚动条总是在最底部
        $('#messages').scrollTop($('#messages')[0].scrollHeight);
    })
```