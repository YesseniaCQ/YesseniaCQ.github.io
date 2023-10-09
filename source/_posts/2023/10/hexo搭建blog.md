---
title: hexo搭建blog
date: 2023-10-07 18:55:46
tags:
---

## 准备工作

### 1. node.js安装

   版本必须>10.13，安装地址：https://nodejs.org/en/download/ ，一般装最新版应该没问题。除了安装位置自己选外，其他可以一直默认（环境变量会自动加上），但新电脑在最后一步 []Automatically...可以勾选。

   > 自带npm，如果后续npm装东西慢或者无效，可以从淘宝源装cnpm（用来替代npm）
    ```powershell
   npm install -g cnpm --registry=https://registry.npm.taobao.org #-g是安装到全局环境，不加则安装到当前文件夹下的node_modules
    ```
	记得检查下版本
	```powershell
	node.js -v
	npm -v
	```
#### 全局配置

是很重要的一点，关系你的C盘红不红，当然C盘巨佬随意啦~

1. 查看当前全局包存储路径

    ```powershell
    npm root -g #查看当前执行npm install -g XXXX时下载的全局包，默认存放路径
    npm config ls #查看npm基本配置
    ```

    > 该路径通常为C:\Users\Administrator\AppData\Roaming\npm\node_modules，很多应用的数据都放在C:\Users\Administrator\AppData\Roaming下，所以如果应用数据太多导致C盘爆炸，可以搜搜怎么移动AppData\Roaming\（我的这个文件夹放在了D盘，感谢之前的我）

2. 在安装node.js的文件夹内，新建`node_global`和`node_ache`两个文件夹，并且进行配置

   ```powershell
   # 修改npm全局安装目录
   npm config set prefix D:\tools\nodejs\node_global
   # 修改npm cache目录（node缓存文件夹)
   npm config set cache D:\tools\nodejs\node_cache
   ```

   >  在安装包的时候，会在node_global（全局）或当前文件夹（本地）下自动创建node_modules，不过由于下一步要进行环境变量配置，所以先手动创。

3. 右键我的电脑-->属性-->高级系统设置-->环境变量

   3.1 用户变量中
   

​		修改path，把C:\Users\Administrator\AppData\Roaming\npm\node_modules改为`D:\tools\nodejs\node_global\node_modules`

   	3.2  系统变量中

   ​		新建NODE_PATH，值为`D:\tools\nodejs\node_modules`

4. 可以看下此时的`npm root -g`和`npm config ls`
5. 之后安装估计会报无权限错误，右键文件夹-->属性-->安全，把“完全控制”的权限加上去。

### 2. hexo安装

   ```powershell
   npm install -g hexo-cli
   或
   cnpm install -g hexo-cli
   ```

## hexo搭建

1. 初始化博客所在文件夹

   ```powershell
   cd 博客文件夹外的文件夹
   hexo init 博客文件夹
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
   
   如果端口冲突
   
   ```powershell
   hexo server -p 5000
   ```
   
   