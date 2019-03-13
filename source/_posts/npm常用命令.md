---
title: npm常用命令
date: 2018-10-29 10:10:10
tags: NodeJS
categories: web
---

**npm init**        初始化创建package.js文件

**npm install packagename --save 或 -S**  // --save、-S参数意思是把模块的版本信息保存到dependencies（生产环境依赖）中，即你的package.json文件的dependencies字段中；

**npm install packagename --save-dev 或 -D**  //--save-dev 、 -D参数意思是吧模块版本信息保存到devDependencies（开发环境依赖）中，即你的package.json文件的devDependencies字段中；
<!-- more -->
**npm install packagename --save-optional 或 -O**   //--save-optional 、 -O参数意思是把模块安装到optionalDependencies（可选环境依赖）中，即你的package.json文件的optionalDependencies字段中。

**npm install  （包名）-g**    //安装全局的模块（不加参数的时候默认安装本地模块）

**npm publish**   发布新版本

**npm update [-g]**   更新已经安装的模块(或全局的模块)

npm install 安装模块

npm uninstall 卸载模块

npm outdated 检查模块是否已经过时

npm ls 查看安装的模块

npm init 在项目中引导创建一个package.json文件

npm help 查看某条命令的详细帮助

npm root 查看包的安装路径

npm config 管理

npm的配置路径

npm cache 管理模块的缓存

npm start 启动模块

npm stop 停止模块

npm restart 重新启动模块

npm test 测试模块

npm version 查看模块版本

npm view 查看模块的注册信息

npm adduser  用户登录

npm publish 发布模块

npm access 在发布的包上设置访问级别