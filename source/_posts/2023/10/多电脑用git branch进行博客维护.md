---
title: 多电脑用git branch进行博客维护
date: 2023-10-07 22:48:56
tags:
description: 
---

因为电脑有台式和笔记本，在不同的场景下进行博客记录的需求产生了，而且有些博文写一半也不好展示，幸好有git的多分支功能。

## 已有博客源文件的电脑上操作

### 1. 对远程仓库新建分支，并克隆该分支到本地

在Github的username.github.io仓库上新建一个main->developing分支，并切换到该分支，并在该仓库->Settings->Branches->Default branch中将默认分支设为developing，保存；

然后copy ssh，在博客文件夹外的文件夹内git bash，将该仓库克隆到本地，进入该username.github.io文件目录。

```bash
git clone ...
git branch #确认当前分支
```

### 2.将博客源文件复制到username.github.io文件夹内，上传到分支备份

```bash
git add .
git commit -m "源文件备份"
git push
```

> - 将themes目录以内中的主题的.git目录删除（如果有），因为一个git仓库中不能包含另一个git仓库，提交主题文件夹会失败。
> - 可能有人会问，删除了themes目录中的.git不就不能`git pull`更新主题了吗，很简单，需要更新主题时在另一个地方`git clone`下来该主题的最新版本，然后将内容拷到当前主题目录即可
> - 顺便主题能作为npm模块直接装最好，就没上述烦恼了

master分支和developing分支各自保存着一个版本，master分支用于保存博客静态资源，提供博客页面供人访问（hexo d直接推送就行，不用合并）；developing分支用于备份博客部署文件，供自己维护更新，两者在一个GitHub仓库内互不冲突

## 另一台电脑

### 1. 基础node.js和hexo安装

- 最好装和老电脑一样的版本，特别是npm

  npm换版本

```powershell
npm install npm@6 -g
```

### 2. 配置远程仓库连接

- 将新电脑的生成的ssh key添加到GitHub账户上

  在**桌面**右键-在powershell中运行

```bash
 git config --global user.name "github用户名"
 git config --global user.email "创建github的邮箱"
  # 生成 ssh 密钥
 ssh-keygen -t rsa -C "创建github的邮箱"
```

- 一般执行上述命令之后，会生成 `id_rsa` 和 `id_rsa.pub` 两个文件，前者是我们私有的，而后者则是对外开放的。接着找到生成的 `.ssh` 的文件夹中的 id_rsa.pub 密钥，将内容复制；

```bash
cat id_rsa.pub #生成在哪个文件夹输出中有写，去那个文件夹下cat
```

- 去github账户-->settings-SSH and GPG keys-->New SSH key，粘贴就行，title可以命名成“hexo”

### 3. 在新电脑上克隆分支到本地

- 切换到username.github.io目录，执行`npm install`，不行就`cnpm install`

  > 由于仓库有一个.gitignore文件，用来忽略不用备份的文件，里面默认是忽略掉 node_modules文件夹的，也就是说仓库的hexo分支并没有存储该目录（也不需要，可以自己根据package.json安装），所以需要install下

- hexo s 看看有无报错，本地看看效果

## 协同时

```bash
git add .
git commit -m "xxx"
git pull
git push
```