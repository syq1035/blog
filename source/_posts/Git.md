---
title: Git
date: 2017-10-29 10:10:10
tags: Git
categories: Git
---

### 配置

```
git config --global user.email "you@example.com"  //获取当前登录用户的邮箱

git config --global user.name "your name"  //获取当前登录的用户
```

注：global 为全局配置
<!-- more -->
### GitHub的ssh key

1. 生成一对shh key（id_rsa私钥，id_rsa.pub公钥）

```
ssh-keygen
```

注：在主目录下生成的密钥在 /c/Users/用户名/.ssh/id_rsa 里

1. 查看公钥内容（需在.ssh目录下执行）

```
cat id_rsa.pub
```

1. 复制密钥内容添加到 github 上
2. 使用ssh协议上传文件到仓库

```
git init

git add .

git commit -m " "

git remote add origin git@github.com:...

git push -u origin master
```

注：若在创建新仓库时有readme.md 文件会上传失败

### **直接上传本地文件**

1. 初始化git仓库

   ```
   git init   //在文件夹下初始化一个仓库，此时文件里会得到一个.git的隐藏文件夹
   ```

2. 添加文件到暂存区

   ```
   git add .   //全部添加到缓存区

   git add index.html     //添加index.html到缓存区

   git add —A        //所有修改内容添加到缓存区
   ```

3. 增加到版本库中

   ```
   git commit -m "版本留言描述"
   ```

4. 连接远程仓库

   ```
   git remote add origin 仓库地址
   ```

5. 将本地仓库推送到GitHub远程仓库上

   ```
   git push 

   git push -u origin master    //首次执行，说明上传到仓库的master分支上
   ```

### 从自己的远程仓库中克隆下来修改后上传

(相当于已经和远程仓库链接了，克隆下来也有属于自己仓库中的.git）

1. 从远程库克隆到本地

   ```
   git clone 仓库地址
   ```

2. 添加文件到暂存区

   ```
   git add .
   ```

3. 增加到版本库中

   ```
   git commit -m "备注信息"
   ```

4. 推送到GitHub仓库里

   ```
   git push
   ```

### 分支

1. 查看分支

```
git branch
```

1. 添加并切换分支

```
git checkkout -b 分支名    //默认与master分支内容相同
```

1. 切换分支

```
git checkout 分支名
```

1. 删除分支

```
git branch -D 分支名
```

1. 删除GitHub上的分支

```
git push origin :分支名
```

1. 合并分支

```
git merge 分支名
```

### 其它

1. 查看状态

```
git status
```

1. 查看远程仓库

```
git remote -v
```

1. 删除远程仓库

```
git remote rm origin
```

注：仓库地址可以选择HTTPS协议（http://github.com...）、ssh协议（git@github.com...）如果选择ssh协议，必须将公钥添加到GitHub上。