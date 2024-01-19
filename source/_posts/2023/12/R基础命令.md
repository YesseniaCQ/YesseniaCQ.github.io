---
title: R基础命令
banner: https://cdn.jsdelivr.net/gh/YesseniaCQ/img/video/心壑/心壑.m3u8

date: 2023-12-28 10:16:27
tags:

---
```r
setwd(dir = "D:/R/R_Workspace") #设置需要的工作路径,注意斜杠的方向和其他编程语言不同  
getwd()  #查看当前工作目录  
[1] "D:/R/R_Workspace"

list.files()  #查看当前工作目录下的文件  
[1] "R_工作目录修改.R"  
dir()         #查看当前工作目录下的文件  
[1] "R_工作目录修改.R"
```
## 工作路径
```r
getwd()
setwd("/.../...") #用绝对路径
dir.create() #创建新目录
```
## 数据读入
```r
load("data.Rdata") #可以相对路径
read.csv(file, 
		 header, 
		 sep, #分隔符
		 )
```
## 变量查看
```r
ls() #查看有哪些变量
ls.str() #对每个变量做简短描述
ls.str(name) #对特定变量做详细描述
name #展示变量内容
```