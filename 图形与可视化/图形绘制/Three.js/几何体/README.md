# Three.js Primitive

Three.js 有大量的基本元素。基本元素通常是在运行时通过一堆参数生成的 3D 形状。通常使用基本元素来绘制地球仪的球体或绘制 3D 图形的一堆方框。使用基本元素来实验和开始使用 3D 尤其常见。对于大多数 3D 应用来说，让艺术家在 Blender 或 Maya 或 Cinema 4D 等 3D 建模程序中制作 3D 模型更为常见。在本系列的后面，我们将介绍从几个 3D 建模程序中制作和加载数据。现在，让我们来介绍一些可用的基本元素。

以下许多基本元素的部分或全部参数都有默认值，因此您可以根据自己的需要多用或少用。

# BoxBufferGeometry, BoxGeometry

```js
const width = 8;
const height = 8;
const depth = 8;
const geometry = new THREE.BoxBufferGeometry(width, height, depth);

const width = 8;
const height = 8;
const depth = 8;
const widthSegments = 4;
const heightSegments = 4;
const depthSegments = 4;
const geometry = new THREE.BoxBufferGeometry(
  width,
  height,
  depth,
  widthSegments,
  heightSegments,
  depthSegments
);
```

![Box 示意图](https://s1.ax1x.com/2020/10/30/BtihrQ.png)

# CircleBufferGeometry, CircleGeometry

```js
const radius = 7;
const segments = 17;
const geometry = new THREE.CircleBufferGeometry(radius, segments);
```
