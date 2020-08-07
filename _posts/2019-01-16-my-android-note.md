---
title: Android个人开发笔记
date:  2019-01-16 14:00:33 +0800
category: develop
tags: develop
excerpt: 在Android开发中的点点滴滴。
---

记录学习Android开发过程中的点点滴滴。

1.生成系统默认的返回按钮:
---

![image](https://upload-images.jianshu.io/upload_images/13517457-33923c5e17521262.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

onCreate方法中键入上图的两行代码
```
 ActionBar actionBar = getSupportBar();
 actionBar.setDisplayHomeAsUpEnabled(true);
```
ActionBar导入的包应该是android.support.v7.app.ActionBar而不是android.app.ActionBar。

生成按钮后需要生成按钮的点击事件

![image](https://upload-images.jianshu.io/upload_images/13517457-f3bb52714cd9b343.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

重写onOptionsItemSelected方法，在方法中判断item的id，该按钮的id是android.R.id.home。

2.将Activity设为全屏且状态栏为透明
---

代码如下：

![image](https://upload-images.jianshu.io/upload_images/13517457-e28e17495c2893de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先判断SDK的版本，如果版本号大于21即Android 5.0才能执行目的操作。

核心代码：
```
 if (Build.VERSION.SDK_INT >=21) { 
    View decorView = getWindow().getDecorView(); 
    decorView.setSystemUiVisibility(View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN 
             | View.SYSTEM_UI_FLAG_LAYOUT_STABLE);
    getWindow().setStatusBarColor(Color.TRANSPARENT); 
 }
```
3.设置屏幕常亮
---


有时app需要手机屏幕常亮，这时需要用到以下代码：
```
Window window=this.getWindow();  
window.addFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON);
```


4.播放网络音频
---

如来自金山词霸api的网络音频：http://res.iciba.com/hanyu/zi/19a73d32bf61c88bc4ed86c40f26bc9c.mp3

从api获取到音频地址后，执行以下代码，voiceurl为上面的地址。
![image.png](https://upload-images.jianshu.io/upload_images/13517457-da98bf7040c5731d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里我是用一个按钮来控制发声，用到onClick方法。

核心代码为：
```
MediaPlayer mediaPlayer = new MediaPlayer();
mediaPlayer.setDataSource(voiceUrl);
mediaPlayer.prepareAsync();
mediaPlayer.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
              @Override
              public void onPrepared(MediaPlayer mediaPlayer) {
                        mediaPlayer.start();
                 }
};
```

5.复制文字到剪切板
---

通过一个按钮来将输入框中的文字全部复制到剪切板，操作代码如下：
![image.png](https://upload-images.jianshu.io/upload_images/13517457-2380c1672456ab0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

核心代码为：
```
ClipboardManager clipboardManager = (ClipboardManager)
                getActivity().getSystemService(Context.CLIPBOARD_SERVICE);
ClipData clipData = ClipData.newPlainText("Label", output.getText().toString());
clipboardManager.setPrimaryClip(clipData);
```
其中label为任意标签。




