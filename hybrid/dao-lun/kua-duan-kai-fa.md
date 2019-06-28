# 跨端开发

终端开发离不开三大要素——界面表现（结构、外观）层、逻辑处理层与系统接口层（网络、存储与媒体等）。

![](https://tva1.sinaimg.cn/large/007rAy9hgy1g2jxq1pqs5j30u00ba3zn.jpg)

开发者编写代码时在初始化阶段（生命周期）调用“界面表现层”界面模型的接口绘制界面，当用户触摸界面时，“界面表现层”将事件发送给用户“逻辑处理层”，后者经过条件判断再处理并反馈到用户界面，处理过程可能需要调用“系统接口层”，反馈过程需要调用“界面表现层”的接口。常规的终端开发架构模式下，无论是 Web 端、Android 端还是 iOS 端的项目开发，都强依赖各端的环境接口，特别是依赖界面相关模型设计。iOS 系统下绘制界面基于 Objective-C 语言环境下的 UIKit 框架；Android 系统下用户绘制界面基于 Java 语言环境，由 LayoutInflater 处理 XML 结构层次树；Web 端使用 DOM 模型和 CSS 来描述绘制界面。

MVVM 中的关键是它通过 ViewModel 这一层将界面和逻辑层彻底隔离开来，负责关联界面表现和逻辑处理层的响应事件（update/notify）关系，这一“隔离层”上下通信足够规范、足够纯净单一。Model 进行逻辑处理是纯业务响应逻辑，任何一种语言都可以实现，你可以用 Android 的 Java，也可以用 iOS 的 Objective-C。

React Native、Weex 与快应用的 MVVM，开发者编写的代码在虚拟机（V8、JavaScriptCore）里面运行，虚拟机容器里面包含扩展的系统基础接口。运行时，将描述界面的数据（主要是 CSS+DSL 所描述内容）通过通信层传递给 Android、iOS 端的渲染引擎，用户触摸界面时，通过通信层传递给虚拟机里面的业务处理代码，业务处理代码可能调用网络、储存与媒体等接口，最后再次反馈到界面。

![](https://i.postimg.cc/Z5md1zJd/image.png)

Flutter 和 RN 的最大区别在于将“JavascriptCore/V8+JS”替换成“C++ 实现的 engine+Dart 实现的 Framework+静态类型 Dart+编译成机器码”。

![](https://i.postimg.cc/q7MCvchB/image.png)

小程序本质上和 Weex、React Native 的设计思路基本一样，最大区别在于前者还是用浏览器 WebView 做渲染引擎，而后者是单独实现了渲染引擎（所以大量的 CSS 布局模型不支持）。

![](https://i.postimg.cc/hGyQzJTL/image.png)

