# 大前端混合开发

随着 Web 技术的迅猛发展，ReactNative, Weex, Weapp 为代表的混合式开发日趋成为与 Android、iOS 原生开发并肩的开发模式之一；而在可预见的未来，VR 、 AR、WebAssembly 等一系列技术粉墨登场，原本前端之间的隔阂逐渐消亡，我们慢慢进入了大前端的时代。

# Preface | 前言

![](https://tva2.sinaimg.cn/large/007DFXDhgy1g4wxluw2a6j30ku0h9jsw.jpg)

前端开发是一系列技术、实践和机构的综合，它主要由两种力量构成：其一是硬件，或者更具体地说是消费级设备；其二就是对盈利收益的不变追求，这一点与其他所有行业是一致的。

![](https://tva2.sinaimg.cn/large/007DFXDhgy1g4rc4vwxg1j30u00eagm4.jpg)

近年来，我们看到消费设备的种类经历了一场爆发式增长——智能手表、智能眼镜、智能音箱、智能电视、可折叠智能手机，等等——在物联网（IoT）时代，每天都可能有新的事物戴上名为“智能”的帽子加入智能设备大家庭，人们对此早已习以为常。

## 跨端混合开发

Hybrid 技术分为两个大的分支，一个以 Cordova 为代表的基于系统的 WebView 与本地调用。另一种早期以 Titanium、Tamarin ，如今以 React Native 这样为代表的 Cross Compilation，即跨平台编译技术。因为每个平台都有浏览器，也都有 WebView 控件，所以我们可以使用 HTML，CSS 和 JavaScript 来将 web 的内容和体验搬到本地。通过这样做我们可以将逻辑和 UI 渲染部分都统一，以减少开发和维护成本。这种方式开发的 app 一般被称为 [Hybrid app](http://blogs.telerik.com/appbuilder/posts/12-06-14/what-is-a-hybrid-mobile-app-)，像 [PhoneGap](http://phonegap.com) 或者 [Cordova](http://cordova.apache.org) 这样的解决方案就是典型的应用。除了使用前端开发的一套技巧来构建页面和交互以外，一般这类框架还会提供一些访问设备的接口，比如相机和 GPS 等。

![hybrid-app](https://onevcat.com/assets/images/2015/hybrid-app.jpg)

虽然使用全网页的开发策略和环境可以带来代码维护的便利，但是这种方式是有致命弱点的，那就是缓慢的渲染速度和难以驾驭的动画效果。这两者对于用户体验是致命而且难以接受的。随着三年前 Facebook 使用 native 代码重新构建 Facebook 的手机 app 这一[标志性事件](https://www.facebook.com/notes/facebook-engineering/under-the-hood-rebuilding-facebook-for-ios/10151036091753920)的发生，曾经一度占领半壁江山的网页套壳的 app 的发展也日渐式微。特别在现在对于用户体验的追求几近苛刻的现在，呆板的动画效果和生硬的交互体验已经完全无法满足人民群众对高质量 app 的心理预期了。

“ 一次编码，处处运行 ” 随着技术的发展，不同平台的差异会更加体现在设计，而非实现。

# About

## Copyright & More | 延伸阅读

[![技术视野](https://s2.ax1x.com/2019/12/03/QQJLvt.png)](https://github.com/wx-chevalier/Awesome-MindMaps)

您还可以前往 [NGTE Books](https://ng-tech.icu/books/) 主页浏览包含知识体系、编程语言、软件工程、模式与架构、Web 与大前端、服务端开发实践与工程架构、分布式基础架构、人工智能与深度学习、产品运营与创业等多类目的书籍列表：

[![NGTE Books](https://s2.ax1x.com/2020/01/18/19uXtI.png)](https://ng-tech.icu/books/)
