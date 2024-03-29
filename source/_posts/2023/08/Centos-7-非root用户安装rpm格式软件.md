---
title: Centos 7 非root用户安装rpm格式软件
date: 2023-08-06 19:18:54

---



以安装fontconfig为例

### 查找包名

yum search fontconfig

我要使用的包名叫：fontconfig.x86_64

![img](https://pic2.zhimg.com/80/v2-d8b5a3d9ae8feb7cc443e1bf819bdce1_720w.webp)

### 下载包

包的保存位置在：~/rpm

mkdir -p ~/rpm

使用yumdownloader下载
yumdownloader --destdir ~/rpm --resolve fontconfig.x86_64

--destdir： 保存位置。

--resolve：表示下载依赖

下载完成后，~/rpm 目录的内容为：

![img](https://pic1.zhimg.com/80/v2-0b3b5347475936a7c96b2dd66cea12c8_720w.webp)



### 开始安装

安装位置：~/centos

安装语法：rpm2cpio ~/rpm/xxxxxx.rpm | cpio -id

```text
mkdir ~/centos
rpm2cpio ~/rpm/dejavu-fonts-common-2.33-6.el7.noarch.rpm | cpio -id
rpm2cpio ~/rpm/dejavu-sans-fonts-2.33-6.el7.noarch.rpm | cpio -id
rpm2cpio ~/rpm/fontconfig-2.13.0-4.3.el7.x86_64.rpm | cpio -id
rpm2cpio ~/rpm/fontpackages-filesystem-1.44-8.el7.noarch.rpm | cpio -id
```

### 配置环境变量

在~/.bashrc中增加以下内容

```text
export PATH="$HOME/centos/usr/sbin:$HOME/centos/usr/bin:$HOME/centos/bin:$PATH"
export MANPATH="$HOME/centos/usr/share/man:$MANPATH"
L='/lib:/lib64:/usr/lib:/usr/lib64'
export LD_LIBRARY_PATH="$HOME/centos/usr/lib:$HOME/centos/usr/lib64:$L"
```

再执行：source ~/.bashrc
