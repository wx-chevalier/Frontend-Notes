[![](https://i.postimg.cc/DfL8fsVd/image.png)](https://github.com/wx-chevalier/Frontend-Series)

# 混合开发/跨端开发实践

Cordova 的基础是 html 和 js 运行在 webview 容器里面,通过 Cordova 提供的接口与硬件通讯;所以它的效率天生收到限制,而且也受到了各个厂商对 webkit 内核的好坏;比如之前基于国产某 Cloud 的程序,在华为手机上显示就不正常,花费了不少精力修改;
RN 的效率由于是将 View 编译成了原生 View,所以效率上要比基于 Cordova 的 HTML5 高很多,但是它也有效率问题,RN 的渲染机制是基于前端框架的考虑,复杂的 UI 渲染是需要依赖多个 view 叠加.比如我们渲染一个复杂的 ListView,每一个小的控件,都是一个 native 的 view,然后相互组合叠加.想想此时如果我们的 list 再需要滑动刷新,会有多少个对象需要渲染.所以也就有了前面所说的 RN 的列表方案不友好;
Flutter  吸收了前两者的教训之后,在渲染技术上,选择了自己实现(GDI),由于有更好的可控性,使用了新的语言 Dart,避免了 RN 的那种通过桥接器与 Javascript 通讯导致效率低下的问题,所以在性能方面比 RN 更高一筹;有经验的开发者可以打开 Android 手机开发者选项里面的显示边界布局,发现 Flutter 的布局是一个整体.说明 Flutter 的渲染没用使用原生控件进行渲染

# HTML5

HTML5 于 2007 年在 W3C 立项，与 iPhone 发布同年。乔布斯曾期待 HTML5 能帮助 iPhone 打造起应用生态系统。但 HTML5 的发展速度并不如预期，它虽然成功地实现了打破 IE+Flash 垄断局面的目标，却没有达到承载优秀的移动互联网体验的地步。

于是在 iPhone 站稳脚跟后，发布了自己的 App Store，开启了移动互联网的原生应用时代。随后的 Android，本来是基于 Linux 的 OS，与之同期的 MeeGo 等竞争对手采用 C + HTML5 的双模应用生态策略，然而 C 的开发难度太大，HTML5 体验又不行。Android 依靠 Java 技术生态，在竞争中脱颖而出。于是在移动互联网初期，应用生态被定了基调 —— 原生开发。

# RN & Flutter

依赖于像 APICloud 这样的第三方工具，开发者是可以较好地屏蔽底层开发细节；而在 React Native 与 Flutter 开发中，我们仍需要去改造或实现许多的原生代码。

![](https://i.postimg.cc/V6SC1t89/image.png)

# 小程序开发

# 链接

- https://mp.weixin.qq.com/s/Q3Dfrcf5FTmWUrsIkPWncA
