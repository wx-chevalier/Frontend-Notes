# 大前端混合开发与数据可视化

随着 Web 技术的迅猛发展，ReactNative, Weex, Weapp 为代表的混合式开发日趋成为与 Android、iOS 原生开发并肩的开发模式之一；而在可预见的未来，VR 、 AR、WebAssembly 等一系列技术粉墨登场，原本前端之间的隔阂逐渐消亡，我们慢慢进入了大前端的时代。

![default](https://i.postimg.cc/y1QXgJ6f/image.png)

# 导航

您可以通过以下任一方式阅读笔者的系列文章，涵盖了技术资料归纳、编程语言与理论、Web 与大前端、服务端开发与基础架构、云计算与大数据、数据科学与人工智能、产品设计等多个领域：

- 在 Gitbook 中在线浏览，每个系列对应各自的 Gitbook 仓库。

| [Awesome Lists](https://ngte-al.gitbook.io/i/) | [Awesome CheatSheets](https://ngte-ac.gitbook.io/i/) | [Awesome Interviews](https://github.com/wx-chevalier/Developer-Zero-To-Mastery/tree/master/Interview) | [Awesome RoadMaps](https://github.com/wx-chevalier/Developer-Zero-To-Mastery/tree/master/RoadMap) | [Awesome-CS-Books-Warehouse](https://github.com/wx-chevalier/Awesome-CS-Books-Warehouse) |
| ---------------------------------------------- | ---------------------------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |


| [编程语言理论](https://ngte-pl.gitbook.io/i/) | [Java 实战](https://ngte-pl.gitbook.io/i/go/go) | [JavaScript 实战](https://ngte-pl.gitbook.io/i/javascript/javascript) | [Go 实战](https://ngte-pl.gitbook.io/i/go/go) | [Python 实战](https://ngte-pl.gitbook.io/i/python/python) | [Rust 实战](https://ngte-pl.gitbook.io/i/rust/rust) |
| --------------------------------------------- | ----------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------- |


| [软件工程、数据结构与算法、设计模式、软件架构](https://ngte-se.gitbook.io/i/) | [现代 Web 开发基础与工程实践](https://ngte-web.gitbook.io/i/) | [大前端混合开发与数据可视化](https://ngte-fe.gitbook.io/i/) | [服务端开发实践与工程架构](https://ngte-be.gitbook.io/i/) | [分布式基础架构](https://ngte-infras.gitbook.io/i/) | [数据科学，人工智能与深度学习](https://ngte-aidl.gitbook.io/i/) | [产品设计与用户体验](https://ngte-pd.gitbook.io/i/) |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------- | ----------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------- | --------------------------------------------------------------- | --------------------------------------------------- |


- 前往 [xCompass https://wx-chevalier.github.io](https://wx-chevalier.github.io/home/#/search) 交互式地检索、查找需要的文章/链接/书籍/课程，或者关注微信公众号：某熊的技术之路。

![](https://i.postimg.cc/3RVYtbsv/image.png)

- 在下文的 [MATRIX 文章与代码矩阵 https://github.com/wx-chevalier/Developer-Zero-To-Mastery](https://github.com/wx-chevalier/Developer-Zero-To-Mastery) 中查看文章与项目的源代码。

| [iOS 实战篇](./iOS) | [Android 实战篇](./Android) | [Hybrid 混合开发篇](./Hybrid) | [数据可视化篇](./DataVisualization) | [IoT 实战篇](./IoT) |
| ------------------- | --------------------------- | ----------------------------- | ----------------------------------- | ------------------- |


# 前言

前端开发是一系列技术、实践和机构的综合，它主要由两种力量构成：其一是硬件，或者更具体地说是消费级设备；其二就是对盈利收益的不变追求，这一点与其他所有行业是一致的。

![](https://tva2.sinaimg.cn/large/007DFXDhgy1g4rc4vwxg1j30u00eagm4.jpg)

近年来，我们看到消费设备的种类经历了一场爆发式增长——智能手表、智能眼镜、智能音箱、智能电视、可折叠智能手机，等等——在物联网（IoT）时代，每天都可能有新的事物戴上名为“智能”的帽子加入智能设备大家庭，人们对此早已习以为常。

## 跨端混合开发

Hybrid 技术分为两个大的分支，一个以 Cordova 为代表的基于系统的 WebView 与本地调用。另一种早期以 Titanium、Tamarin ，如今以 React Native 这样为代表的 Cross Compilation，即跨平台编译技术。因为每个平台都有浏览器，也都有 WebView 控件，所以我们可以使用 HTML，CSS 和 JavaScript 来将 web 的内容和体验搬到本地。通过这样做我们可以将逻辑和 UI 渲染部分都统一，以减少开发和维护成本。这种方式开发的 app 一般被称为 [Hybrid app](http://blogs.telerik.com/appbuilder/posts/12-06-14/what-is-a-hybrid-mobile-app-)，像 [PhoneGap](http://phonegap.com) 或者 [Cordova](http://cordova.apache.org) 这样的解决方案就是典型的应用。除了使用前端开发的一套技巧来构建页面和交互以外，一般这类框架还会提供一些访问设备的接口，比如相机和 GPS 等。

![hybrid-app](https://onevcat.com/assets/images/2015/hybrid-app.jpg)

虽然使用全网页的开发策略和环境可以带来代码维护的便利，但是这种方式是有致命弱点的，那就是缓慢的渲染速度和难以驾驭的动画效果。这两者对于用户体验是致命而且难以接受的。随着三年前 Facebook 使用 native 代码重新构建 Facebook 的手机 app 这一[标志性事件](https://www.facebook.com/notes/facebook-engineering/under-the-hood-rebuilding-facebook-for-ios/10151036091753920)的发生，曾经一度占领半壁江山的网页套壳的 app 的发展也日渐式微。特别在现在对于用户体验的追求几近苛刻的现在，呆板的动画效果和生硬的交互体验已经完全无法满足人民群众对高质量 app 的心理预期了。

“ 一次编码，处处运行 ” 随着技术的发展，不同平台的差异会更加体现在设计，而非实现。

# 关于

## 版权

![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg) ![](https://parg.co/bDm)

笔者所有文章遵循 [知识共享 署名 - 非商业性使用 - 禁止演绎 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)，欢迎转载，尊重版权。如果觉得本系列对你有所帮助，欢迎给我家布丁买点狗粮(支付宝扫码)~

![](https://github.com/wx-chevalier/OSS/blob/master/2017/8/1/Buding.jpg?raw=true)

由于笔者平日忙于工作，几乎所有线上的文档都是我夫人帮忙整理，在此特别致谢；同时也感谢我家的布丁安静的趴在脚边，不再那么粪发涂墙。
