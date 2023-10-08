---
title: hexo搭建blog
date: 2023-10-07 18:55:46
tags:
---

## 准备工作

1. node.js安装

   版本必须>10.13，安装地址：https://nodejs.org/en/download/ ，一般装最新版应该没问题

   > 自带npm，如果后续npm装东西慢或者无效，可以从淘宝源装cnpm（用来替代npm）
    ```powershell
   npm install -g cnpm --registry=https://registry.npm.taobao.org
    ```
	记得检查下版本
	
	```powershell
	node.js -v
	npm -v
	```
	
2. hexo安装

   ```powershell
   npm install -g hexo-cli
   或
   cnpm install -g hexo-cli
   ```

## hexo搭建

1. 初始化博客所在文件夹

   ```powershell
   cd 博客文件夹所在的文件夹
   hexo init 文件夹名
   或
   cd 博客文件夹（空文件夹）
   hexo init
   ```

2. hexo默认博客内容生成+开启本地服务器（默认4000）

   ```powershell
   hexo g
   hexo s #开启本地服务器
   ```

   > 如果hexo g没有生成index，可以cnpm install，再不行看看另一篇博文“hexo生成博客报错”