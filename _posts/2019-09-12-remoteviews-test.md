---
title: RemoteViews初体验
date:  2019-09-12 14:59:13 +0800
category: develop
tags: android
excerpt: 通知栏和桌面小部件开发。
---

最近在二刷《Android开发艺术探索》，看到RemoteViews，以前没接触过，遂即体验一下。

>RemoteViews，从名字可以看出，RemoteViews应该是一种远程View，那么什么是远程View呢？如果说远程服务可能比较好理解，但是远程View的确没听说过，其实它和远程Service是一样的，RemoteViews表示的是一个View结构，它可以在其他进程中显示，由于它在其他进程中显示，为了能够更新它的界面，RemoteViews提供了一组基础的操作用于跨进程更新它的界面。这听起来有点神奇，竟然能跨进程更新界面！但是RemoteViews的确能够实现这个效果。RemoteViews在Android中的使用场景有两种：通知栏和桌面小部件。

因为我的手机是安卓8.0版本，targetSDK是26，在写Demo过程中也发现了书中一些API已经不起作用了，例如Notification发不了、桌面小部件响应不了点击事件，下面简单写一下能起作用的Demo。

### 1、RemoteViews在通知栏上的应用
由于Google在8.0上新增了notification channel这个东西，书上原本的代码在新的sdk及手机上已经是不起作用了= =（我一度怀疑是魅族搞的鬼），翻阅了Google官方文档后整理出如下写法：
```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(MainActivity.this, "2", Toast.LENGTH_SHORT).show();
                final String CHANNEL_ID = "channel_id_1";
                final String CHANNEL_NAME = "channel_name_1";
                NotificationManager mNotificationManager = (NotificationManager)
                        getSystemService(Context.NOTIFICATION_SERVICE);

                if (android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.O) {
                    //只在Android O之上需要渠道
                    NotificationChannel notificationChannel = new NotificationChannel(CHANNEL_ID,
                            CHANNEL_NAME, NotificationManager.IMPORTANCE_HIGH);
                    //如果这里用IMPORTANCE_NOENE就需要在系统的设置里面开启渠道，
                    //通知才能正常弹出
                    mNotificationManager.createNotificationChannel(notificationChannel);
                }
                NotificationCompat.Builder builder= new NotificationCompat.Builder(MainActivity.this,CHANNEL_ID);
                Intent intent = new Intent(MainActivity.this, MainActivity.class);
                PendingIntent pendingIntent = PendingIntent.getActivity(MainActivity.this, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT);


                RemoteViews remoteViews = new RemoteViews(getPackageName(), R.layout.remoteview);
                builder.setSmallIcon(R.mipmap.ic_launcher)
                        .setContentTitle("通知标题")
                        .setContentText("通知内容")
                        .setAutoCancel(false)
                        .setContentIntent(pendingIntent)
                        .setCustomContentView(remoteViews);

                mNotificationManager.notify(1, builder.build());
            }
        });
    }
}
```
这段代码写入了Android O需要的通知渠道，下面是效果：
![image.png](https://upload-images.jianshu.io/upload_images/13517457-27929857ceacef41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2、RemoteViews在桌面小部件上的应用
>AppWidgetProvider是Android中提供的用于实现桌面小部件的类，其本质是一个广播，即BroadcastReceiver。在实际使用中，把其当成BroadcastReceiver就可以了，这样许多功能就很好理解了。

下面是桌面小部件的开发步骤：
#### 2.1、定义小部件界面
在res/layout/下新建一个XML文件，命名为remoteview.xml，名称和内容可以自定义，这里简单写一个TextView。
需要注意的是：RemoteViews并不支持所有的View，它所支持的类型如下：
> #####Layout
> FrameLayout、LinearLayout、RelativeLayout、GridLayout
>##### View
>AnalogClock、Button、Chronometer、ImageButton、ImageView、ProgressBar、TextVIew、ViewFlipper、ListView、GridView、StackView、AdapterViewFlipper、ViewStub

除了上述的View之外，RemoteViews也不支持上述view的之类以及其他类型，更不支持自定义View，观望后期Android会不会改吧。

下面是remoteview.xml:
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/colorPrimary">

    <TextView
        android:id="@+id/text"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="Hello World" />

</LinearLayout>
```
#### 2.2、定义小部件配置信息
在res/xml/下新建appwidget_provider_info.xml，名称随意，添加如下内容：
```xml
<?xml version="1.0" encoding="utf-8"?>
<appwidget-provider xmlns:android="http://schemas.android.com/apk/res/android"
    android:initialLayout="@layout/remoteview"
    android:minHeight="84dp"
    android:minWidth="84dp"
    android:updatePeriodMillis="86400000">
</appwidget-provider>
```
上面几个参数的意义很明确，initialLayout就是指小工具所使用的初始化布局，minHeight和minWidth定义小工具的最小尺寸，updatePeriodMillis定义小工具的自动更新周期，毫秒为单位，每隔一个周期，小工具的自动更新就会触发。

#### 2.3、定义小部件的实现类
这个类需要继承AppWidgetProvider，代码如下：
```java
public class MyAppWidgetProvider extends AppWidgetProvider {

    public static final String TAG = "MyAppWidgetProvider";
    public static final String CLICK_ACTION = "io.varenyzc.remoteviewtest.action.CLICK";
    

    public MyAppWidgetProvider(){
        super();
    }

    @Override
    public void onReceive(final Context context, Intent intent) {
        super.onReceive(context, intent);
        Log.d(TAG, "onReceive: action = " + intent.getAction());
        //这里判断自己的Action，做自己的点击事件，这里是显示一个Toast
        if (intent.getAction().equals(CLICK_ACTION)) {
            Toast.makeText(context, "clicked it", Toast.LENGTH_SHORT).show();
        }
    }   
}
```

#### 2.4、在AndroidManifest.xml中声明小部件
因为桌面小部件本质上是一个广播组件，因此必须要注册，如下所示
```xml
      <receiver android:name=".MyAppWidgetProvider">
            <meta-data
                android:name="android.appwidget.provider"
                android:resource="@xml/appweidget_provider_info">
            </meta-data>

            <intent-filter>
                <action android:name="io.varenyzc.remoteviewtest.action.CLICK"/>
                <action android:name="android.appwidget.action.APPWIDGET_UPDATE" />
            </intent-filter>
     </receiver>
```
上面的代码有两个Action，其中第一个是用于识别小部件的单击行为，而第二个则是小部件的标志而必须存在，这个安卓系统的规范，如果不加，那么这个receiver就不是一个桌面小部件并且也无法出现在手机的小部件列表里。

跟着书上的如上步骤敲完后，我以为小部件就成功了，但是还是too young too simple，当我去点击小部件时，在Android Studio始终没有出现我自定义的action事件，仅出现了android.appwidget.action.APPWIDGET_UPDATE
![image.png](https://upload-images.jianshu.io/upload_images/13517457-9ed0c1745469b4b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这就傻眼了，我以为是自定义的Action没对上，反复看了几遍都没问题啊，后面根据Android Studio上的log，发现了这个receiver需要动态注册！接下来是最后一步！

#### 2.5、新建一个Application类，命名为MyApplication，代码如下
```java
public class MyApplication extends Application {

    private IntentFilter mIntentFilter;
    private MyAppWidgetProvider mMyAppWidgetProvider;

    @Override
    public void onCreate() {
        super.onCreate();
        mIntentFilter = new IntentFilter();
        mIntentFilter.addAction("io.varenyzc.remoteviewtest.action.CLICK");
        mMyAppWidgetProvider = new MyAppWidgetProvider();
        registerReceiver(mMyAppWidgetProvider,mIntentFilter);
    }
}
```

以上代码作用是在Application中动态注册receiver，还记得上面说的把AppWidgetProvider当做BrocastReceiver用吗，这里就是把其进行动态注册，将自定义Action注册进去。

然后在AndroidManifest.xml中把这个Application引入，然后重新下到手机上，点击小部件，yes！成功了！

以下是效果图:
![image.png](https://upload-images.jianshu.io/upload_images/13517457-339cdbcf58f78358.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

至此，RemoteViews初体验结束，enjoy yourself~