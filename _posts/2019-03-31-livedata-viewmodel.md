---
title: LiveData与ViewModel实现数据共享
date:  2019-03-31 13:27:13 +0800
category: develop
tags: android
excerpt: 解决Fragment之间的通信问题。
---

## 1、优点
> LiveData 是一个可以感知 Activity 、Fragment生命周期的数据容器。当 LiveData 所持有的数据改变时，它会通知相应的界面代码进行更新。同时，LiveData 持有界面代码 Lifecycle 的引用，这意味着它会在界面代码（LifecycleOwner）的生命周期处于 started 或 resumed 时作出相应更新，而在 LifecycleOwner 被销毁时停止更新。

上面的描述介绍了LiveData的优点：不用手动控制生命周期，不用担心内存泄露，数据变化时会收到通知。

>ViewModel 将视图的数据和逻辑从具有生命周期特性的实体（如 Activity 和 Fragment）中剥离开来。直到关联的 Activity 或 Fragment 完全销毁时，ViewModel 才会随之消失，也就是说，即使在旋转屏幕导致 Fragment 被重新创建等事件中，视图数据依旧会被保留。ViewModels 不仅消除了常见的生命周期问题，而且可以帮助构建更为模块化、更方便测试的用户界面。

**ViewModel**的优点也很明显，为**Activity **、**Fragment**存储数据，直到完全销毁。尤其是屏幕旋转的场景，常用的方法都是通过**onSaveInstanceState()**保存数据，再在**onCreate()**中恢复，真的是很麻烦。

其次因为**ViewModel**存储了数据，所以**ViewModel**可以在当前**Activity**的**Fragment**中实现数据共享。

那么**LiveData**与**ViewModel**的组合使用可以说是双剑合璧，而**Lifecycles**贯穿其中。

## 2、这里介绍Fragment之间的用法

##### 1）首先要引入liffcycle的拓展包，在app目录下的build.gradle中的dependencies引入以下依赖：

```
api "android.arch.lifecycle:extensions:1.1.1"
```

##### 2）随后新建一个类SeekBarViewModel继承于ViewModel

```
public class SeekBarViewModel extends ViewModel {
    public MutableLiveData<Integer> seekbarValue =new MutableLiveData<>();
}
```

##### 3）在Fragment中实现如下代码

```
public class SeekBarFragment extends Fragment {

    private SeekBar mSeekBar;

    private SeekBarViewModel mSeekBarViewModel;

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View root = inflater.inflate(R.layout.fragment_seek_bar, container, false);
        mSeekBar = root.findViewById(R.id.seekBar);

        mSeekBarViewModel = ViewModelProviders.of(getActivity()).get(SeekBarViewModel.class);
        subscribeSeekBar();
        return root;
    }

    private void subscribeSeekBar() {
        mSeekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
            @Override
            public void onProgressChanged(SeekBar seekBar, int i, boolean b) {
                if(b){
                    mSeekBarViewModel.seekbarValue.setValue(i);
                }
            }

            @Override
            public void onStartTrackingTouch(SeekBar seekBar) {

            }

            @Override
            public void onStopTrackingTouch(SeekBar seekBar) {

            }
        });
        mSeekBarViewModel.seekbarValue.observe(this, new Observer<Integer>() {
            @Override
            public void onChanged(@Nullable Integer integer) {
                if (integer != null) {
                    mSeekBar.setProgress(integer);
                }
            }
        });
    }
}
```

##### 4）随后在MainActivity中引入两个SeekBarFragment
![image.png](https://upload-images.jianshu.io/upload_images/13517457-0b3c2d66d92f10b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

调试后可以看到在其中一个Fragment中拖动seekbar，另一个Fragment中的seekBar也跟着拖动，至此，两个Fragment实现了数据共享。
