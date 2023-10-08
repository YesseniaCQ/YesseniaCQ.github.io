---
title: 运用Git LFS往github上传大文件
date: 2023-08-09 00:56:33
tags: git
---

由于git不能上传>25mb的文件，一些科研数据或视频等需要借用git lfs插件下载。

此记录是在win11系统上。

## git 和 Git LFS 的安装:

### git 可以通过[git-osx-installer](https://link.zhihu.com/?target=https%3A//sourceforge.net/projects/git-osx-installer/)/下载。

![img](https://pic1.zhimg.com/80/v2-e5ac74f0e4d0ef050ef338609044a27c_720w.webp)

### Git LFS 可以通过[Git Large File Storage](https://link.zhihu.com/?target=https%3A//git-lfs.github.com)下载。

![img](https://pic1.zhimg.com/80/v2-eb7996c6e5b98e42cb36f188f397270c_720w.webp)

> 下载完成后将Git LFS的路径加不加入全局变量应该不重要，如果不能用可以试着加一下（这一步我没做）
>
> ```text
> vim ~/.zprofile
> #按i键进入编辑模式，将Git LFS的路径export
> #按Esc键退出编辑模式，输入:wq并按Enter键保存编辑
> ```
> 实际如下图所示（参考的教程中）
> ![img](https://pic1.zhimg.com/80/v2-f5891f337e28e7df46d54638a6e73584_720w.webp)
>
> 或者可以直接在环境变量-用户变量里加。

## 配置git lfs，连接git仓库，上传文件

```bash
cd upload #进入名为upload的文件夹，提前将要上传的大文件放入该文件夹下
git init #创建本地仓库环境
git lfs install #安装大文件上传应用
git lfs track * #追踪要上传的大文件，*表示路径下的所有文件
git add .gitattributes #添加先上传的属性文件(要先上传属性文件，不然有可能失败)
git commit -m "pre" #添加属性文件上传的说明
git remote add origin git仓库地址 #建立本地和Github仓库的链接
git push origin master #上传属性文件
git add * #添加要上传的大文件，*表示路径下的所有文件
git commit -m "Git LFS commit" #添加大文件上传的说明
git push origin master #上传大文件
```