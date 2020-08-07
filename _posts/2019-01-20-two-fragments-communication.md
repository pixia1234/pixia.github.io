---
title: 在同一个Activity下的两个Fragment之间的通信
date:  2019-01-20 0:20:13 +0800
category: develop
tags: android
excerpt: 使用接口方法实现。
---

最近在做一个app，界面的架构为：一个Activity里面嵌套若干个Fragment，通过侧滑导航栏切换Activity中的Fragment。

为什么要把这篇文章单独拿出来而不放在开发笔记中呢，因为感觉这个比较难实现，其实也不是难实现，而是我在写的过程中尝试了几种方法，最终实现了我想要的功能，故在此记下，以免忘记。

那么我想实现的功能是什么？

我做的是一个翻译app（即将上线），其中想实现通信的是TranslateFragment和WordbookFragment，即翻译Fragment和单词本Fragment，实现的功能是在单词本中长按某个单词然后查看该单词的具体释义，然后跳转到翻译界面实现翻译然后获取具体释义。
![单词本](https://upload-images.jianshu.io/upload_images/13517457-3d819f6bb7c552ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![长按查看具体释义](https://upload-images.jianshu.io/upload_images/13517457-9e59d7df3ec00d81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![在翻译界面查看该词具体含义](https://upload-images.jianshu.io/upload_images/13517457-0214d5a0570b095a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
## 实现方法其实挺简单，分三个步骤：

### 1、在WordbookFragment中定义一个接口:
```java
public interface sendWord{
        void sendWord(String word);
    }
```


### 2、在MainActivity中实现这个接口：
```java
public class MainActivity extends AppCompatActivity implements BookFragment.sendWord{
    public static TranslateFragment translateFragment;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        translateFragment = new TranslateFragment();
    }  
        .....
        .....
        
    @Override
    public void sendWord(String word) {
        translateFragment.setWord(word);
    }
  
```
在这里我把一开始create的TranslateFragment设为static，即确保只有一个这个实例。然后在实现接口的方法里调用translateFragment里面的set方法把想要的String变量传到translateFragment里面去。

## 3、在translateFragment里面接收这个变量：
从单词本回到翻译中，translateFragment会触发onResume这个生命周期，即可以重写onResume方法来实现接收变量。
首先在TranslateFragment中定义一个变量，接着定义getter方法，然后在onResume中接收这个变量。
```java
public class TranslateFragment extends Fragment{
     private static EditText input;
     private ImageButton IB_tran;
     public static String word = "";
     
     ......
     ......
     public static void setWord(String word) {
        TranslateFragment.word = word;
     }
    @Override
    public void onResume() {
        super.onResume();
        if (word != "") {
            //Toast.makeText(getContext(),word,Toast.LENGTH_SHORT).show();
            input.setText(word);
            IB_tran.performClick();
            word = "";
        } 
    }
}
```

随后在WordbookFragment中调用这个方法，就可以实现两个Fragment之间的通信。

核心代码：
```java
MainActivity mainActivity = (MainActivity)getActivity();
mainActivity.getResult().setSelection(1);  //该行代码是在MaterialDrawer中选中第一个Item，使之跳转到TranslateFragment
mainActivity.sendWord(wordbook.getQuereText());
```

至此可以实现从单词本到翻译的跳转。

不知道这个是不是最好的方法，如果有大神有更好的方法可以在评论区中评论，thanks!

