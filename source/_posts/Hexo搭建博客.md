---
title: hexo搭建博客
date: 2018-10-29 10:10:10
tags: hexo
categories: web
---

###什么是 Hexo？

[hexo](https://hexo.io/)是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

### 安装前提

安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：

- [Node.js](http://nodejs.org/)
- [Git](http://git-scm.com/)

如果您的电脑中已经安装上述必备程序，那么恭喜您！接下来只需要使用 npm 即可完成 Hexo 的安装。

```
$ npm install -g hexo-cli
```
<!-- more -->
如果您的电脑中尚未安装所需要的程序，请根据以下安装指示完成安装。

### 初始化 hexo 博客项目

1.新建一个文件夹 

2.在Hexo文件下，输入命令：

```
$ hexo init
$ npm install
```

会生成如下图所示的文件结构 :



###文件目录介绍 

**1. _config.yml**

全局配置文件，网站的很多信息都在这里配置，诸如网站名称，副标题，描述，作者，语言，主题，部署等等参数, 这个文件下面会做较为详细的介绍 。

**2. scaffolds**

`scaffolds` 是“ 脚手架、骨架 ”的意思，当你新建一篇文章（`hexo new 'title'`）的时候，`hexo`是根据这个目录下的文件进行构建的 。

**3. package.json**

`hexo` 框架的参数和所依赖插件，如下：

```
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "hexo": {
    "version": ""
  },
  "dependencies": {
    "hexo": "^3.2.0",
    "hexo-generator-archive": "^0.1.4",
    "hexo-generator-category": "^0.1.3",
    "hexo-generator-index": "^0.2.0",
    "hexo-generator-tag": "^0.2.0",
    "hexo-renderer-ejs": "^0.3.0",
    "hexo-renderer-stylus": "^0.3.1",
    "hexo-renderer-marked": "^0.3.0",
    "hexo-server": "^0.2.0"
  }
}

```

如果后期我们想安装一些插件, 也会写入 `package.json` 当中 .

**4. source**

这个目录很重要，新建的文章都是在保存在这个目录下的.

- `_posts` 。需要新建的博文都放在 `_posts` 目录下。
- `_posts` 目录下是一个个 `markdown` 文件。你应该可以看到一个 `hello-world.md` 的文件，文章就在这个文件中编辑 。
- `_posts` 目录下的`md`文件，会被编译成 `html` 文件，放到 `public` （此文件现在应该没有，因为你还没有编译过）文件夹下。

**5. themes**

网站主题目录，hexo有非常好的主题拓展，支持的主题也很丰富。该目录下，每一个子目录就是一个主题

**6. 我们打开 theme 文件夹下的主题文件夹, 会发现也有一个 _config.yml 文件**

`_config.yml` 文件中的内容，是主题的一个配置信息

`_config.yml` 采用YAML语法格式，具体配置可以参考[官方文档](https://hexo.io/zh-cn/docs/configuration.html)

### 本地浏览博客

分别输入下面两条命令 :

```
$ hexo g
$ hexo s

```

在浏览器中输入 : `http://localhost:4000/`

### 部署到 `Github` 上

1. 申请Github账号 
2. 新建一个 `Repository` , 注意仓库的名字前缀必须和自己 `github` 的用户名保持严格一致，如 `xxx.github.io`
3. 在 `_config.yml` 进行配置

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo:
    github: https://github.com/syq1035/syq1035.github.io
  branch: master
```

在 `deploy` 中配置自己的仓库信息 ( 注意冒号后面有空格 )

1. 安装[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)自动部署发布工具

```
$  npm install hexo-deployer-git -–save
```

1. 发布到Github

输入如下命令 :

```
$ hexo clean && hexo g && hexo d
```

然后输入自己的 `Github` 用户名和密码即可

在浏览器中输入 https://xxx.github.io/ 便可以打开自己的博客啦 !

### 修改主题

[进入主题官网](https://hexo.io/themes/) 选择喜欢的主题克隆；

修改主程序的`_config.yml`文件

```
theme: XXX
```