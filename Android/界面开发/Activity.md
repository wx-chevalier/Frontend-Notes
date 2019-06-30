# Support Library Activity

为了使在旧版手机上依然可以使用譬如 Holo、Material 这样的设计风格，Google 在 Support Library 中推出了多种兼容性的 Activity。

## AppCompatActivity

在 Android Support Library 22.1 之后，就将旧有的 ActionBarActivity 替换为了 AppCompatActivity，提供了最低到 API Level 7 的 ActionBar 的兼容。

![](https://androidresearch.files.wordpress.com/2015/04/android-support-22.png)

**1.** In order to benefit from all these things, the first thing you should do is to update the support library to 22.1.0.

```
`dependencies {``   ``// … ``   ``compile 'com.android.support:appcompat-v7:22.1.0'``}`
```

**2.** Then let your activity extend `AppCompatActivity`.

```java
public class MainActivity extends AppCompatActivity
{
	// ...
}
```

**3.** And finally, change the application theme to `AppCompat` or any descendants of it.

```
`<``application` `android:theme``=``"@style/Theme.AppCompat"``>`
```

## ActionBarActivity!deprecated

ActionBarActivity 是在 Material Design 推出之前的主流的 V7 兼容包中的基类型，目前已经被 AppCompatActivity 替代。

# Activity Theme

## Dialog

&nbsp;&nbsp;&nbsp;&nbsp;要把某个 Activity 窗口化，即直接定义其主题为 Dialog 样式即可：

```
<activity android:name=".activity02"
android:theme="@android:style/Theme.Dialog"></activity>
```

其效果如下图

![Activity 窗口示意图][1]

### Transparent

&nbsp;&nbsp;&nbsp;&nbsp;有时候需要把窗体透明化，只要为窗口定义如下的 Style 即可：

```xml
<style name="MyDialogStyle">
        <item name="android:windowBackground">@android:color/transparent</item>
        <item name="android:windowFrame">@null</item>
        <item name="android:windowNoTitle">true</item>
        <item name="android:windowIsFloating">true</item>
        <item name="android:windowIsTranslucent">true</item>
        <item name="android:windowContentOverlay">@null</item>
        <item name="android:windowAnimationStyle">@android:style/Animation.Dialog</item>
        <item name="android:backgroundDimEnabled">true</item>
</style>
```

然后在 AndroidManifest.xml 中添加如下配置：

```
<activity
     android:name="LoadingActivity"
     android:theme="@style/MyDialogStyle" />
<activity
```
