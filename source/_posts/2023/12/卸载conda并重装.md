---
title: 卸载conda并重装
banner: https://cdn.jsdelivr.net/gh/YesseniaCQ/img/video/心壑/心壑.m3u8
date: 2023-12-26 16:30:23
tags:

---
1. 确保没有进程正在使用conda
	特别是screen下，不确定可以都杀掉
	```shell
	#删除全部screen
	screen -ls | wc -l #10
	screen -ls | awk 'NR>=2&&NR<=8{print $1}' | awk '{print "screen -S "$1" -X quit"}'| sh #8=10-2(最后一行是空行，倒数第二行是统计行)
	```
2. 安装卸载包并卸载
	```shell
	conda install anaconda-clean
	anaconda-clean
	rm -rf ~/anaconda3
	```
3. 去[官网]([Free Download | Anaconda](https://www.anaconda.com/download#downloads))对应平台对应版本的安装包，并运行安装
	```shell
	wget -c https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh
	bash Anaconda3-2023.09-0-Linux-x86_64.sh
	```


## 重装后可能会有的错误
1. 不能init、激活环境失败
	```shell
	Traceback (most recent call last):
	      ...
	An unexpected error has occurred. Conda has prepared the above report.
	
	If submitted, this report will be used by core maintainers to improve
	future releases of conda.
	Would you like conda to send this report to the core maintainers? [y/N]: 
	Timeout reached. No report sent.
	```
	解决方法2种：
	1. 用source activate代替conda activate激活，但退出还是conda deactivate
	2. 手动在~/.bashrc内加入（推荐这个）
		```shell
		#added by Anaconda3 installer
		export PATH="/public/home/username/anaconda3/bin:$PATH"
		. /public/home/username/anaconda3/etc/profile.d/conda.sh #关键是这步
		conda activate
		```