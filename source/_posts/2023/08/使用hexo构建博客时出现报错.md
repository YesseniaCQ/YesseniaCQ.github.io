---
title: 使用hexo构建博客时出现报错
date: 2023-08-06 21:52:08
tags:
---

在Hexo博客中，出现Cannot GET/xxx错误便意味着xxx文件未被找到。Cannot GET/xxx错误本质是hexo server返回的一个404错误。

因此，排查方式如下：

1. 判断public目录下xxx文件是否存在。
（我的错误是 Cannot GET /，因此在public目录下寻找index.html是否存在。）

2. 如果说index.html不存在，那么执行hexo c，hexo g重新生成一次，回到步骤1。

3. 步骤2执行完后index.html仍不存在，执行`npm audit fix`

  ```bat
  $ npm audit fix
  npm ERR! code ELOCKVERIFY
  npm ERR! Errors were found in your package-lock.json, run  npm install  to fix them. #如果有直接告诉可能的修法，先尝试。
  npm ERR!     Missing: hexo-renderer-marked@^6.0.0 #缺少的组件
  ```

npm ERR! A complete log of this run can be found in:
npm ERR!     D:\tools\blog\node.js\node_cache\_logs\2023-08-06T08_58_55_270Z-debug.log
  ```

> 有几点需要看：
	1. 如果有直接告诉可能的修法，先尝试。
 	2. 查看是否少了什么组件，通过npm install hexo-xxx-xxx 安装试试。（如上述报错，hexo缺少了hexo-renderer-marked组件，因此执行npm install hexo-renderer-marke）
 	3. 组件安装失败可以试试用cnpm安装

​```bat
cnpm install hexo-renderer-marked --save #虽然还是有报错，但index.html生成了
  ```

4.  步骤3完成之后，执行hexo c，hexo g重新生成静态文件。



如果有啥模块没被找到，`npm install` 一下，如果说生成package.json的npm版本低，需要把当前版本降级

```powershell
npm install -g npm@6
```

再报错就`cnpm install` 或`cnpm install pkgname`  或  `cnpm install pkgname -g`（不报错就停）（我估计多电脑操作时两台电脑的npm版本不同，导致根据package.json进行cnpm时装不上一些基础包）



### 总结下来的第一次构建方式

```bat
hexo int
hexo g
hexo s
```

> 如果hexo g没有生成index，可以cnpm install

或

```bat
hexo init
npm install dependencies
npm audit fix#查看缺少什么模块
cnpm install hexo-renderer-marked@^6.0.0
hexo g
```

