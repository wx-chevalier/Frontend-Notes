> [原文地址](https://medium.com/@sergii/best-practices-for-android-developer-productivity-cfd6ffba804c#.bp2tjpwt0)

软件工程师的工作效率不仅依赖于知识的掌握程度与经验，也会依赖于你选择的工具集合、合适的环境配置以及团队内的合作技巧。本文即是作者在[Droidcon Berlin](http://droidcon.de/en/sessions/effective-android-development)上演讲的总结。 ![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/7/4/1-e7X-rfqUmez-yI_DdsyOCw.jpeg)

# 看穿你的 AndroidManifest

众所周知，我们开发时候在文本编辑器中看到的`AndroidManifest.xml`并非最终打包到应用中的`AndroidManifest.xml`文件。在打包时，编译器会自动地将你所使用的第三方依赖库中的譬如`<uses-permission/>`这些元素从它们自己的 Manifests 文件中抽取出来然后统一在主入口的`AndroidManifest.xml`文件中声明，关于这部分的详细说明可以参考[这里](https://commonsware.com/blog/2015/06/25/hey-where-did-these-permissions-come-from.html)。 Android Studio 2.2 中提供了一个新特性，[Merged Manifest Viewer](http://android-developers.blogspot.de/2016/05/android-studio-22-preview-new-ui.html)可以用于预览在 APK 构建之后最终生成的`AndroidManifest.xml`文件，该文件中会包含项目依赖中声明的构建类型、变量等等信息。 ![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/7/4/0-wZiAP9ouYyAaY-_x.png)

# 好好使用 Support Annotations

另一个非常有用的工具就是 Support Annotations Library，可以直接在 build.gradle 文件中添加`com.android.support:support-annotations:23.4.0`依赖，然后可以在代码中添加元注释来辅助很快地 Bug 追踪与代码规则定义。最常见的使用场景就是用于标记 Nullable 与 Non-Nullable 变量，将整型标记为资源标识符以及设置某个函数的运行线程。不过因为这些注释属于 Metadata Annotations，即使某些定义的规则被破坏了项目还是会正常编译。只不过 Android Studio 与 Lint 会自动高亮地展示这些警告，并且在 CI 工具中也可以使用这些警告。

# 快速的 Code Review

Code Review 是项目开发中的常见操作，我们需要切换到某个 Feature 分支，然后重新编译运行项目，常见的工作流如下 :

* 暂存本分支的更改
* 检出待 Review 的分支
* 在 IDE 中重载 gradle 配置文件
* 在 IDE 中浏览代码
* 编译、运行与测试应用
* 切换分支并且重复以上步骤

上述流程很规范很正常，不过当你的项目拥有 1000 个以上的类与不同的配置时，估计你要好几分钟来等待项目编译完毕。我们的解决方案是为特征分支使用单独的 IDE 实例与仓库文件夹，这样会避免你来回地进行分支切换。如果你想用这套方案，那么建议你到电脑至少要 16GB 以上的 RAM，你会觉得物有所值的。

# 修改的热加载

笔者在 React 开发中使用 HOT Reload 是个很不错的体验，而在 Android 开发中，即使是一个很小的 Android 项目，也需要等待不少的时间来等待修改之后的代码重新构建与部署，更何况对于那些有成百上千个类与 XML 布局文件的项目。另一方面，你也需要在你的应用中跳转选择到合适的页面来查看你做的更改的实际效果。2015 年底，Android 社区逐步使用两个工具来加速代码更改的加载速度，首先是[JRebel](https://zeroturnaround.com/software/jrebel-for-android/)，源于 Java 在服务端开发中的长期实践。另一个是 Google 团队随着 Android Studio 2.0 一起推出的工具：[Instant Run](https://developer.android.com/studio/run/index.html#instant-run)。两个工具的目标差不多，不过 JRebel 包含更多的特性，但是它也需要付费。作者编辑了一个简单的表单来描述二者的特性对比 : ![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/7/4/0--btlhImjSPlQ37rQ.png)

* https://developer.android.com/studio/run/index.html#instant-run
* Reto Meier: “Instant Run: How Does it Work?!”
* Oleg Selajev: “Looking at JRebel for Android and Instant Run …”

两个工具都正处在不断的开发与完善中，从我们的经验来看，目前还有很多的用户场景并没有被涉及到，因此我们也相信未来这两个工具的提升空间令人期待。

# 性能分析与执行效率估测

另一个应用调试与性能分析中常需的用户特性是对于方法的输入输出与执行时间的日志记录，作者在这方面是使用了 Jake Wharton 开源的[Hugo](https://github.com/JakeWharton/hugo)，这个工具在你不希望使用复杂的譬如 Systrace 工具的时候很有帮助。譬如我们会希望监测如下的目标方法 :

```
@DebugLog
public String getName(String first, String last) {/* ... */}
```

然后在控制台中我们可以看到如下的信息 :

```
V/Example: --> getName(first="Jake", last="Wharton")
V/Example: <-- getName [16ms] = "Jake Wharton"
```

# 读取设备中的日志输出

我们在开发过程中经常会使用 Android Studio 内置的 Android Monitor 来读取日志信息，这种方式确实很简单易用，不过也有很大的缺陷 :

* 我们必须使用额外的工具来对日志记录进行格式化
* Android Studio 的调试工具是附着到应用的进程 ID 上，一旦我们重新部署应用或者杀掉了进程，之前打印出来的日志就会被覆盖掉

作者在这里推荐的是 Jake Wharton 开源的另一款工具[Pidcat](https://github.com/JakeWharton/pidcat)，其优势在于 :

* 更友好的配色与格式化
* 根据包名来获取调试信息，即使重新部署应用也不会删除之前的日志记录

# 网络调试与分析

应用开发中我们也常常需要通过查看 HTTPClient 库的日志记录来读取客户端与服务端的交互信息，不过这种方法往往会有如下的一些缺陷 :

* 如果你在开发过程中全程保持网络的日志输出，会大大影响应用的运行性能，毕竟需要一些时间来输出日志
* 如果你的应用使用的其他依赖库需要进行网络通信，譬如 Google Analytics 这样的分析工具，我们需要进行额外的配置来过滤掉这些扰乱的信息
* 在生产环境下无法监测到网络通信信息

这边作者首先建议的方法是使用类似于[Charles Proxy](https://www.charlesproxy.com/)这样的 HTTP 监测与代理工具，它可以将你的应用当成黑箱对待，并且提供了以下功能 :

* HTTP/HTTPS 的流量监控与记录
* 重定义某些特定情况下的服务端返回信息
* 设置部分网络调用的断点
* 为设备安装合适的 SSL 证书来阅读加密的流量

另一个推荐的工具就是[Facebook Stetho](http://facebook.github.io/stetho/)，它允许开发者在生产环境下获取网络交互数据，不过我们在开发中肯定也是可以使用该工具，不过作者发现好像它还不能读取基于 SSL 的加密信息。

# 多版本测试

在应用开发过程中，作者建议务必要在 Lollipop 以及更高级的 API(API 21+) 以上来进行应用测试，从而发现因为系统适配而导致的潜在问题。我们最常见的 Bug 就是因为触摸反馈与系统颜色问题导致的 Bug，譬如我们经常看到因为使用了老版本的 API 而导致应用崩溃。

# UI 自动化测试

有些用户场景下我们需要在不同的设备中进行重复的 UI 点击或者输入操作，如果你需要在两三个设备上进行重复的测试，估摸着你会爆炸的。我们首先想到的方法是通过输入`adb`命令来模拟用户点击输入，譬如*adb shell input keyevent 4*等效于在测试设备上点击了 UP 按钮。这样的话你可以在 adb 中完成系统按键点击、键盘输入与屏幕点击。而在多设备的环境下，我们建议使用 Roman Nurik 开源的 [adb-ninja](https://github.com/romannurik/env/blob/master/bin/ninja-adb) 脚本来在多个设备间同步提交指令。

# build.gradle 配置检测

即使是有经验的开发者也往往会使用些过时的配置方案，作者建议根据如下顺序来检查下你的 build.gradle 配置 :

* 使用 jcenter 来替换 mavenCentral，jcenter 的响应速度比 mavenCentral 好。
* 检测 _Android Plugin for Gradle_ 的版本号，使用最新版本的依赖有助于提高编译速度。
* 不要使用范围来定义依赖的版本号，而使用 “23.4.0” 这样固定的版本号来避免不确定的依赖中的 API 的变化。

![](http://153.3.251.190:11900/best-practices-for-android-developer-productivity)
