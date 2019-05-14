[![返回目录](https://parg.co/UY3)](https://github.com/wx-chevalier/Web-Series)

# React Native

目前我们常用的移动端应用开发方式主要为原生方式、混合开发这两种，其中原生开发运行效率高,流畅,用户体验好,可以做各种复杂的动画效果。不过我们需要去掌握不同开发平台上特定的开发语言与内建的组件框架，譬如在 Android 开发中开发者需要掌握 Java，而 iOS 开发中开发者需要掌握 Objective-C 或者 Swift；并且由于平台之间的独立性，代码无法在其他平台上运行,无法做到跨平台。而传统混合开发方式则以 Cordova 与 Ionic 为代表，定义好原生功能与 Web 界面之间的协议,拦截特定的 URL Schema 进行原生功能的调用，应用则调用 Web 提供的 JavaScript 方法，将数据回传给 Web 界面。这种方式可以满足一套代码到处运行的目标，不过受限于 UIWebView 等容器本身的限制，其性能体验与原生应用不可同日而语。实际上无论哪一种开发方式都致力于解决如下几个问题：找到一种能达到或者接近原生体验的开发方式、找到一种一套代码能在各个平台上运行,达到代码复用的目的、能够以热更新或者类似的方式进行快速问题修复。

随着 React 在 Web 领域取得的巨大成功，Facebook 继续推出 React Native 以创建接近原生性能的跨平台移动应用，其倡导的 Learn Once，Write Anywhere 的概念同时兼顾了性能与快速迭代的需求。React 的核心设计理念其提供了抽象的、平台无关的组件定义范式，然后通过 react-dom 等库将其渲染到不同的承载体上；这些承载可以是服务端渲染中的字符串，或者客户端渲染中的 DOM 节点。在 React Native 中，我们只需要了解 React 组件定义规范与语法，然后利用 React Native 这个新的渲染库将界面渲染到原生界面组件中。在未来的客户端开发中，负责与用户交互以及存储这一部分建议采用原生的代码，而对于逻辑控制这边，建议是采用 JavaScript 方式实现。

React Native 本质上是用 JSX 的语法风格编写原生的应用，它本质上还是跨平台编译性质的，并没有提供完整的类似于 WebView 那样的上下文，并且大量的 HTML 元素也是不可以直接应用的。React Native 只是借用了 HTML 的语法风格，并且提供了 JavaScript 与原生的桥接。React Native 使用了所谓的 Native Widget APIs 来调用底层的操作系统相关代码，并且处于性能的考虑它会异步批量地调用原生平台接口，其整体架构如下所示：

![](https://www.safaribooksonline.com/library/view/react-and-react/9781786465658/graphics/image_12_001.jpg)
