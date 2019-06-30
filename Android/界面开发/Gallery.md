

# ImageView

## HoverView：图片浮层

### [**AndroidViewHover**](https://github.com/daimajia/AndroidViewHover)

![](https://camo.githubusercontent.com/44affb72f0688c213500917009a1680b41492413/687474703a2f2f7777322e73696e61696d672e636e2f6d773639302f36313064633033346a7731656a356969686a746c35673230387a3066326e70642e676966)

# ImageView-Loader
Picasso是一个专门的面向图片任务的HTTP库。譬如，可以用一行代码就把网络图片加载到ImageView中：

``` 
Picasso.with(context).load("http://i.imgur.com/DvpvklR.png").into(imageView);
```

Picasso与Retrofit都是默认的使用OkHttpClient作为底层的HTTP客户端，然而，你也可以配置自己的基于HttpUrlConnection的客户端。

[Glide](https://github.com/bumptech/glide/wiki)是一个非常类似于Picasso的库，不过它提供了一些额外的功能，譬如GIF动画、简略图生成以及视频。

Facebook开源的它们自己的图片加载库[Fresco](https://code.facebook.com/posts/366199913563917/introducing-fresco-a-new-image-library-for-android/)使用了它们自定义的Android客户端。其中它的一个非常优秀的特性在于Fresco的面向Bitmaps的自定义才存储策略能够避免JVM堆顶的垃圾回收的限制。Fresco是分配了Android中被称为ashmem部分的内存，同时，它用了一些方法允许同时从Java以及C++代码访问ashmem部分，来进行NDK级别的CPU处理。为了节省数据存储空间以及CPU的消耗，Fresco有三层不同的缓存：两层在内存中，以及一层在内部存储中。

# ImageProcess：图片处理

## Label(标签)

## Sticker(贴纸)

### [StickerView](https://github.com/nimengbo/StickerView)

![](https://github.com/nimengbo/StickerView/raw/master/stickerGIF.gif)



## Crop(裁剪)

### [SimpleCropView](https://github.com/IsseiAoki/SimpleCropView)

![demo](https://camo.githubusercontent.com/7ef872746a0181356ea0b44d94b7bd939f28c5ae/68747470733a2f2f7261772e6769746875622e636f6d2f77696b692f4973736569416f6b692f53696d706c6543726f70566965772f696d616765732f6769662f64656d6f5f62617369635f75736167652e676966)

# Focus(图片缩放浏览)

### [Pull Back Layout](https://github.com/oxoooo/pull-back-layout)

![](https://github.com/oxoooo/pull-back-layout/raw/master/screenshot.gif)

Pull-Back-Layout本质上提供了一层对于ImageView或者ViewPager的封装，允许以下拉方式关闭某个放大的ImageView或者ViewPager。

- *Installation*

``` 
repositories {
    // ...
    maven { url "https://jitpack.io" }
}

dependencies {
    // ... support library ...
    // ...
    compile 'com.github.oxoooo:pull-back-layout:1.0.0'
}
```

- *Usage*


1. Make your Activity translucent by adding these two lines to your theme:
   
   ``` 
   <style name="AppTheme" ...>
       <!-- ... -->
   
       <item name="android:windowBackground">@android:color/transparent</item>
       <item name="android:windowIsTranslucent">true</item>
   
       <!-- also translucent system ui -- suggested but not required -->
       <item name="android:windowTranslucentStatus">true</item>
       <item name="android:windowTranslucentNavigation">true</item>
   </style>
   ```
   
2. Wraps `ooo.oxo.library.widget.PullBackLayout` around your `ImageView` or `ViewPager`:
   
   ``` 
   <ooo.oxo.library.widget.PullBackLayout
       android:id="@+id/puller"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
   
       <android.support.v4.view.ViewPager
           android:id="@+id/pager"
           android:layout_width="match_parent"
           android:layout_height="match_parent" />
   
   </ooo.oxo.library.widget.PullBackLayout>
   ```
   
3. Set a callback:
   
   ``` 
   public class ViewerActivity extends AppCompatActivity implements PullBackLayout.Callback {
   
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           /* ... */
           puller.setCallback(this);
       }
   
       @Override
       public void onPullStart() {
           // fade out Action Bar ...
           // show Status Bar ...
       }
   
       @Override
       public void onPull(float progress) {
           // set the opacity of the window's background
       }
   
       @Override
       public void onPullCancel() {
           // fade in Action Bar
       }
   
       @Override
       public void onPullComplete() {
           supportFinishAfterTransition();
       }
   
   }
   ```
