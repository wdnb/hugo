---
title: Hugo使用本地图片
date: 2023-12-05
tags:
  - hugo
categories:
  - Blog
---
## 问题
图片上传到图床比较麻烦，搜了一下网上hugo如何使用图片很多人都说在hugo根目录的 static 目录下存放图片 感觉这样太麻烦 还是想和文章放在一个目录
稍微研究了一下
发现文章内嵌图片的访问的uri就是你当前访问链接后加上图片名 而hugo生成静态页面的时候会将同名的**文件夹**和**文件**放在同一个目录下 那这样就很简单了`http://localhost:1313/posts/blog/hugo%E4%BD%BF%E7%94%A8%E6%9C%AC%E5%9C%B0%E5%9B%BE%E7%89%87/image.png`
## 解决方法
直接在文章目录下新建一个文件夹文件名要和你的文章名一样，图片丢进去，然后就是在markdown里直接输入图片名就行 不需要加上目录`![](image.png)`




![](image.png)
![](image2.png)