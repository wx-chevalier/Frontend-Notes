# SVG

SVG 指可伸缩矢量图形 (Scalable Vector Graphics)，用来定义用于网络的基于矢量的图形，它在放大或改变尺寸的情况下其图形质量不会有所损失。Svg 使用 XML 格式定义图形，与诸如 DOM 和 XSL 之类的 W3C 标准是一个整体。

# 语法定义

SVG 基础语法如下：

```xml
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
"http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <circle cx="100" cy="50" r="40" stroke="black"
  stroke-width="2" fill="red" />
</svg>
```

第一行包含了 XML 声明。请注意 standalone 属性，该属性规定此 SVG 文件是否是独立的，或含有对外部文件的引用。standalone=”no” 意味着 SVG 文档会引用一个外部文件，这里即是 DTD 文件。第二和第三行引用了这个外部的 SVG DTD。该 DTD 位于 “http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd"。该 DTD 位于 W3C，含有所有允许的 SVG 元素。

SVG 代码以 `<svg>` 元素开始，包括开启标签 `<svg>` 和关闭标签。这是根元素。width 和 height 属性可设置此 SVG 文档的宽度和高度。version 属性可定义所使用的 SVG 版本，xmlns 属性可定义 SVG 命名空间。SVG 的 `<circle>` 用来创建一个圆。cx 和 cy 属性定义圆中心的 x 和 y 坐标。如果忽略这两个属性，那么圆点会被设置为 (0, 0)。r 属性定义圆的半径。stroke 和 stroke-width 属性控制如何显示形状的轮廓，fill 属性设置形状内的颜色。

## 可见区域与缩放

viewport 表示 SVG 可见区域的大小，或者可以想象成舞台大小，画布大小。

```html
<svg width="500" height="300"></svg>
```

上面的 SVG 代码定义了一个视区，宽 500 单位，高 300 单位。viewBox 顾名思意是“视区盒子”的意思，`viewBox="x, y, width, height" // x:左上角横坐标，y:左上角纵坐标，width:宽度，height:高度`：

```html
<svg
  width="400"
  height="300"
  viewBox="0,0,40,30"
  style="border:1px solid #cd0000;"
>
  <rect x="10" y="5" width="20" height="15" fill="#cd0000" />
</svg>
```

SVG 就像是我们的显示器屏幕，viewBox 就是截屏工具选中的那个框框，最终的呈现就是把框框中的截屏内容再次在显示器中全屏显示。

![viewBox 缩放实例](https://image.zhangxinxu.com/image/blog/201408/2014-08-27_105046-viewbox.gif)

## HTML 中使用

- 使用 `<embed>` 标签，所有主要浏览器都支持，并允许使用脚本；不推荐在 HTML4 和 XHTML 中使用。

```html
<embed src="circle1.svg" type="image/svg+xml" />
```

- 使用 `<object>` 标签，所有主要浏览器都支持，并支持 HTML4，XHTML 和 HTML5 标准；不过不允许使用脚本。

```html
<object data="circle1.svg" type="image/svg+xml"></object>
```

- 直接在 HTML 嵌入 SVG 代码。

```html
<html>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
      <circle
        cx="100"
        cy="50"
        r="40"
        stroke="black"
        stroke-width="2"
        fill="red"
      />
    </svg>
  </body>
</html>
```
