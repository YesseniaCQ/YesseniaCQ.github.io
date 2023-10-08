---
title: 多电脑用git branch进行博客维护
date: 2023-10-07 22:48:56
tags:
description: sssssss
---

因为电脑有台式和笔记本，在不同的场景下进行博客记录的需求产生了，而且有些博文写一半也不好展示，幸好有git的多分支功能。

#### 对username.github.io仓库新建hexo分支，并克隆

在Github的username.github.io仓库上新建一个xxx分支，并切换到该分支，并在该仓库->Settings->Branches->Default branch中将默认分支设为xxx，save保存；然后将该仓库克隆到本地，进入该username.github.io文件目录。