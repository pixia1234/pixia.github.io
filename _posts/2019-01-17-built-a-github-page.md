---
title: 使用github pages搭建个人博客
date:  2019-01-17 14:00:33 +0800
category: Minimalism
tags: test
excerpt: 省去服务器搭建自己的博客网站。
---

偶然一次知道使用github repo可以存放静态网页代码，因此可以以此来搭建自己的博客。以前在服务器中搭过wordpress，但是一旦服务器挂了，博客文件也都没了。因此在github中搭建博客也不失为一个不错的解决方案。

[github pages官方网站](https://pages.github.com/)

官方推荐jeklly主题框架，因此在github中找到了这个主题：[https://github.com/showzeng/Minimalism](https://github.com/showzeng/Minimalism)，感觉不错，因此把它clone下来上传到自己的github。
![image.png](https://upload-images.jianshu.io/upload_images/13517457-8356b2486b64335e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**1.准备步骤：新建一个repo，仓库名字为xxx.github.io，其中xxx为你github的username。**
![image.png](https://upload-images.jianshu.io/upload_images/13517457-007ee0ed31a81de2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**2.把想要的主题clone下来：**
示例代码：
```
git clone https://github.com/showzeng/Minimalism.git
```
![image.png](https://upload-images.jianshu.io/upload_images/13517457-7c5a58b7b3f7e3fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**3.再把刚才自己新建的repo clone下来：**
```
git clone http://github.com/varenyzc/varenyzc.github.io.git
```

**4.接着把主题文件夹中除了.git文件夹全部复制到自己仓库所在的文件夹**
![image.png](https://upload-images.jianshu.io/upload_images/13517457-d6ba7e562c1296d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/13517457-6a929026a9b00a23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**5.在自己仓库文件夹中右键git bash here**
代码如下：
```
git add .
git commit -m "first commit"
git push -u origin master
```
![image.png](https://upload-images.jianshu.io/upload_images/13517457-1e344201a21c1579.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

至此完成博客的搭建。过点时间访问xxx.github.io就可以访问到你的博客啦~Enjoy yourself！

欢迎访问[我的博客](https://varenyzc.github.io)
---


