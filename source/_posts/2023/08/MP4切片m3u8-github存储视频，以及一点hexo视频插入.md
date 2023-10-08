---
title: MP4切片m3u8+github存储视频，以及一点hexo视频插入
date: 2023-08-11 22:48:56
tags:
---
由于我想给博客的背景应用视频，但出现了难题：需要视频床（且视频太大不行）。于是我跟着教程使用ffmeg将MP4切成m3u8，并传上github使用cdn加速。这时，新的难题出现，如何导入网页，特别是hexo博客。于是我下载了模块hexo-tag-videojs（主要目的是导入必要的css和js，另外这个插件在md中插入m3u8也极为方便，干脆下了插件而不是自己导入js）

## 使用ffmeg切片m3u8

1. （windows 11）在cmd中下载ffmeg

    ```powershell
    winget isntall ffmeg
    ```

2. 继续在cmd中操作（记得转到目标视频所在文件夹下）

   ```powershell
   ffmpeg -i '视频文件名称.mp4' -c:v h264 -flags +cgop -g 30 -hls_time 5 -hls_list_size 0 -hls_segment_filename 文件夹/名称%3d.ts '文件夹/名称.m3u8' 
   #文件夹可不加
   ```

   > 注意这里的视频要h.264编码，然后-hls_time 5意为5秒作为一个切片，当视频文件比较小的时候可以设置10s或20s等作为一个切片，注意切片的大小不能超过20m，要不然没法使用jsd加速。

3. 接下来将文件夹推送到github，使用cdn加速，这个链接就可以直接用了。

   ```powershell
   https://cdn.jsdelivr.net/gh/用户名/仓库名/文件夹名/名称.m3u8
   ```

## 在网页中使用m3u8

1. 引用相关css和js

   1. hexo直接安装模块hexo-tag-videojs

      ```bash
      npm install hexo-tag-videojs --save
      ```

   2. 其他网页直接引用

      ```html
      <link href="https://unpkg.com/video.js/dist/video-js.css" rel="stylesheet">
      <script src="https://unpkg.com/video.js/dist/video.js"></script>
      <script src="https://unpkg.com/videojs-contrib-hls/dist/videojs-contrib-hls.js"></script>
      ```

2. 插入视频

   ```html
   <video id="myvideo" class="video-js vjs-default-skin" controls preload="auto" width="640" height="268" 
     data-setup='{}'> #data-setup里可以调控大小，自动等，具体看说明文档
       <source src="http://d2zihajmogu5jn.cloudfront.net/bipbop-advanced/bipbop_16x9_variant.m3u8" type="application/x-mpegURL">
    </video>
   
   #这个是我用来做背景的ejs代码，视频也可以直接style定义样式，更推荐
   <video id="welcomeVideo" loop muted playsinline webkit-playinginline data-scroll data-scroll-direction="vertical" data-setup='{}' preload="true" style='width: 100%;height: auto' >
               <source src="<%= url_for(bgurl) %>" type='application/x-mpegURL'>
               Your browser does not support HTML5 video.
   </video>
   ```

   > 可以插入多个source

   后面应该需要跟个js，虽然官方文档说video或data-setup里设置了就不用，但我尝试的结果是不可缺少
   
   ```html
   <script>
       var myVideo = videojs('welcomeVideo', {
         loop: true,
         controls: false,
         preload: 'auto',
         autoplay: true,
       });
</script>
   ```
   
   最后附上[官方文档](https://gitcode.gitcode.host/docs-cn/video.js-docs-cn/docs/guides/setup.html)

