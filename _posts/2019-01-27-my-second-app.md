---
title: 第二个个人App，轻翻译，轻量级的翻译软件
date:  2019-01-27 13:27:13 +0800
category: develop
tags: android
excerpt: 我的第二个app。
---

[官网地址](https://varenyzc.github.io/easytranslate)
[开源地址](https://github.com/varenyzc/easytranslate)

## 简介

  轻翻译，轻量级的翻译软件。使用金山API提供翻译服务，具有打开即搜、划词翻译、OCR识别图片翻译等功能。界面遵循谷歌Material Design设计，无广告，无其他冗余功能，实现真正的轻量级。

## 预览图片  
![image.png](https://upload-images.jianshu.io/upload_images/13517457-9530b2b1e8186a9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/13517457-0e6ebf16c8dbe402.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/13517457-e5e4285dae0bc0b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/13517457-da5ca0abe5e8ebef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/13517457-5135c500a556a80d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 创作初衷

在六级考试复习时，经常要用翻译软件进行单词的翻译查询（英语渣），我使用的是金山词霸，每次打开时都要弹几秒的广告，很不爽，加上一向用金山词霸习惯了，一开始用它也是因为它的秒开，无广告，到后来慢慢加了广告，启动也要等一会才启动，这时萌生了自己写一个的念头。等到期末考试完，我就开始挖坑，在参考了其他翻译软件的源代码后，我用金山的翻译api做了第一版，再加上了划词翻译的功能，能够实现在浏览网页的过程中随时翻译，这对于经常浏览github且英语渣的我很有帮助。经过几天的填坑、测试、增加和完善功能，这个软件终于可以面世啦！1.0版本，多多少少会有一些bug，欢迎大家给我提[issues](https://github.com/varenyzc/easytranslate/issues)



## 下载

- [最新版本下载](https://github.com/varenyzc/easytranslate/releases)
- [酷安网下载](https://www.coolapk.com/apk/216403)

[支持我](http://github.com/varenyzc/easytranslate)


## 更新日志

### Version 1.0
* 支持划词翻译功能
* 支持OCR识别图片功能
* 支持单词本功能


## 待完成功能
- [ ] 支持多翻译源
- [ ] 支持全局划词翻译（包括微信等修改系统文字操作栏的软件）
- [ ] 支持分词功能（类似于锤子Bigbang)



## 鸣谢


- [咕咚翻译](https://github.com/maoruibin/TranslateApp): 一个实现『划词翻译』功能的 Android 应用 ，可能是目前 Android 市场上翻译效率最高的一款应用。
- [Materialdrawer](https://github.com/mikepenz/MaterialDrawer): The flexible, easy to use, all in one drawer library for your Android project. Now brand new with material 2 design.
- [Gson](https://github.com/google/gson): A Java serialization/deserialization library to convert Java Objects into JSON and back.
- [Okhttp](https://github.com/square/okhttp):  An HTTP+HTTP/2 client for Android and Java applications.
- [Litepal](https://github.com/LitePalFramework/LitePal): An Android library that makes developers use SQLite database extremely easy.
- [BaseRecyclerViewAdapterHelper](https://github.com/CymChad/BaseRecyclerViewAdapterHelper): Powerful and flexible RecyclerAdapter.
- [金山api](http://open.iciba.com/?c=api): 翻译来源。
- [百度OCR](http://ai.baidu.com/): OCR来源。
