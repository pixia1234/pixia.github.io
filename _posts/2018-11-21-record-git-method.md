---
title: 记录一下git的使用方法
date:  2018-11-21 00:30:33 +0800
category: git
tags: git
excerpt: 使用git的方法，记录一下备忘。
---

研究了一下git的使用方法记录一下 免得忘了。大神勿喷，给需要的人看。


首先在github上新建一个远程开源库
![image](https://upload-images.jianshu.io/upload_images/13517457-8251c119329ef8ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


然后在你需要git的文件夹目录下右键Git Bash Here
![image](https://upload-images.jianshu.io/upload_images/13517457-dc6c0c1d332c9720.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


命令行窗口
然后在命令行窗口下输入代码。
```
git init
git add .
git commit -m "说明文字"
```
后面的步骤需要看在github上新建的仓库的HTTPS地址是多少。



![HTTPS地址](https://upload-images.jianshu.io/upload_images/13517457-9627a32d247749c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后在git命令行窗口下输入以下代码：
```
git remote add origin https://github.com/varenyzc/DGUTAirMouse.git

git push -u origin master
```


![git成功](https://upload-images.jianshu.io/upload_images/13517457-c21f8a21b388f43e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

推送完成后会让你输入github的用户名和密码。这里因为我已经输入过用户名密码，所以不用重复输入。



![github界面](https://upload-images.jianshu.io/upload_images/13517457-ed53f2d99ba414a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入完就推送到github上啦。大功告成！在其他平台如gitlab、gogs等开源网站操作类似，只需修改HTTPS地址即可。

也可以follow一下我的github，电子专业转行cs：[https://github.com/varenyzc](https://github.com/varenyzc)

enjoy yourself！


