---
title: hexo部署自定义live2d(moc3版)
date: 2023-08-11 23:23:04
tags:
---

说真的，hexo自带的live2d需要更新一下了，又少又不怎么好看，其中只有两个小动物我还可以接受。总之，由于现在很多新的live2d都是用的moc3，hexo的live2d模块如<code>hexo-helper-live2d</code>、<code>live2d-widget</code>并不适用（它们适合moc），所以终于找到有大佬写出[live2d-moc3](https://github.com/LitStronger/live2d-moc3)，但使用起来略微有些繁杂，又有大佬打包成了[Live2dLoader](https://github.com/Weidows-projects/Live2dLoader)，所以能愉快地导入了（由于我那主题的锅，以及打包大佬的表述问题，其实并不怎么愉快）

## 正片

在你的主题或需要live2d的页面里插入。主题的话看看说明文档，哪个文件是用来进行整体页面搭建的，改就完事了（最好是提供了自定义功能的主题）

我这个Async主题是插入main.ejs（需要自己建立，为什么？详见[主题配置 | Hexo-Theme-Async](https://hexo-theme-async.imalun.com/guide/config.html#自定义模板-layout)）

```html
<!--live2d moc3导入-->
<script src="https://cubism.live2d.com/sdk-web/cubismcore/live2dcubismcore.min.js"></script>
<script src="https://cdn.jsdelivr.net/combine/gh/dylanNew/live2d/webgl/Live2D/lib/live2d.min.js,npm/pixi.js@6.5.2/dist/browser/pixi.min.js,npm/pixi-live2d-display/dist/index.min.js,gh/Weidows-projects/Live2dLoader/dist/Live2dLoader.min.js"></script>
<script>
addEventListener("DOMContentLoaded", function () {
  let models = [
    {
      width: 1980,
      height: 200,
      left: "10px",
      bottom: "0px",
      role: "https://cdn.jsdelivr.net/gh/用户名/仓库名/文件夹名/模型名.model3.json",
      background: "",
      opacity: 1,
      mobile: false,
      draggable: false,
    }
  ];
  new Live2dLoader(models);
});
</script>
</body>
```

> 模型名要与文件夹名一致，文件夹外面有多少文件夹不要紧

可以导入多个，在[]里加入就行，最后要再new一个models，不过我没试过，大概是这样的。

```html
<!--live2d moc3导入-->
<script src="https://cubism.live2d.com/sdk-web/cubismcore/live2dcubismcore.min.js"></script>
<script src="https://cdn.jsdelivr.net/combine/gh/dylanNew/live2d/webgl/Live2D/lib/live2d.min.js,npm/pixi.js@6.5.2/dist/browser/pixi.min.js,npm/pixi-live2d-display/dist/index.min.js,gh/Weidows-projects/Live2dLoader/dist/Live2dLoader.min.js"></script>
<script>
addEventListener("DOMContentLoaded", function () {
  let models = [
    {
      width: 1980,
      height: 200,
      left: "10px",
      bottom: "0px",
      role: "https://cdn.jsdelivr.net/gh/用户名/仓库名/文件夹名/模型名.model3.json",
      background: "",
      opacity: 1,
      mobile: false,
      draggable: false,
    },
     {
      width: 1980,
      height: 200,
      left: "10px",
      bottom: "0px",
      role: "https://cdn.jsdelivr.net/gh/用户名/仓库名/文件夹名/模型名.model3.json",
      background: "",
      opacity: 1,
      mobile: false,
      draggable: false,
    }
  ];
  new Live2dLoader(models);
  new Live2dLoader(models);
});
</script>
</body>
```



