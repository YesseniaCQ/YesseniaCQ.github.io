---
title: linux常用命令总结
banner: https://cdn.jsdelivr.net/gh/YesseniaCQ/img/video/心壑/心壑.m3u8
date: 2023-12-26 16:23:49
tags:

---

```shell
#删除全部screen
screen -ls | wc -l #10
screen -ls | awk 'NR>=2&&NR<=8{print $1}' | awk '{print "screen -S "$1" -X quit"}'| sh #10-2(最后一行是空行)
```

```shell
conda config --add channels conda-forge
conda config --add channels defaults
conda config --add channels r
conda config --add channels bioconda
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
conda config --set show_channel_urls yes
```