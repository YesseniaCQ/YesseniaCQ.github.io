---
title: 将hexo本地博客部署到github
date: 2023-08-13 15:30:59
tags:
---

在**桌面**右键-在powershell中运行

```bash
 git config --global user.name "github用户名"
 git config --global user.email "创建github的邮箱"
  # 生成 ssh 密钥
 ssh-keygen -t rsa -C "创建github的邮箱"
```

一般执行上述命令之后，会生成 `id_rsa` 和 `id_rsa.pub` 两个文件，前者是我们私有的，而后者则是对外开放的。接着找到生成的 `.ssh` 的文件夹中的 id_rsa.pub 密钥，将内容复制；

```bash
cat id_rsa.pub #生成在哪个文件夹输出中有写，去那个文件夹下cat
```

去github账户-->settings-SSH and GPG keys-->New SSH key，粘贴就行，title可以命名成“hexo”

然后去配置文件`_config.yml`中设置：

```yaml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: https://github.com/YesseniaCQ/YesseniaCQ.github.io.git #仓库链接
  branch: main
```

最后在博客文件夹的git bash中输入

```bash
hexo clean  #删除生成的文件和缓存
hexo g      #生成静态文件，也就是public文件夹
hexo d      #部署网站
```

### hexo d后出现报错？

“# ERROR Deployer not found: git” 原因：这是因为没安装`hexo-deployer-git`插件，在项目目录下运行以下命令安装即可：

```bash
npm install hexo-deployer-git --save
```