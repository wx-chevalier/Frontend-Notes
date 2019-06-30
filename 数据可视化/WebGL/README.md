# WebGL

```js
const canvas = document.getElementById('container');
const gl =
  canvas.getContext('webgl') || canvas.getContext('experimental-webgl');
gl.clearColor(0.0, 0.0, 0.0, 1.0); // 指定清空canvas的颜色
gl.clear(gl.COLOR_BUFFER_BIT); // 清空canvas
```

使用 WebGL 绘制，依赖于着色器（shader）；

- 顶点着色器（Vertex shader）: 绘制每个定点都会调用一次；
- 片段着色器（Fragment shader）: 每个片源（可以简单的理解为像素）都会调用一次；

```js
/**
 * 使用WebGL画点
 * xu.lidong@qq.com
 * */

// 顶点着色器源码
var vertexShaderSrc = `
void main(){
    gl_Position = vec4(0.0, 0.0, 0.0, 1.0);// gl_Position 内置变量，表示点的位置，必须赋值
    gl_PointSize = 10.0;// gl_PointSize 内置变量，表示点的大小（单位像素），可以不赋值，默认为1.0，，绘制单个点时才生效
}`;

// 片段着色器源码
var fragmentShaderSrc = `
void main(){
    gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0);// 内存变量，表示片元颜色RGBA
}`;

// 初始化使用的shader
function initShader(gl) {
  var vertexShader = gl.createShader(gl.VERTEX_SHADER); // 创建顶点着色器
  gl.shaderSource(vertexShader, vertexShaderSrc); // 绑定顶点着色器源码
  gl.compileShader(vertexShader); // 编译定点着色器

  var fragmentShader = gl.createShader(gl.FRAGMENT_SHADER); // 创建片段着色器
  gl.shaderSource(fragmentShader, fragmentShaderSrc); // 绑定片段着色器源码
  gl.compileShader(fragmentShader); // 编译片段着色器

  var shaderProgram = gl.createProgram(); // 创建着色器程序
  gl.attachShader(shaderProgram, vertexShader); // 指定顶点着色器
  gl.attachShader(shaderProgram, fragmentShader); // 指定片段着色色器
  gl.linkProgram(shaderProgram); // 链接程序
  gl.useProgram(shaderProgram); //使用着色器
}

function main() {
  var canvas = document.getElementById('container');
  var gl =
    canvas.getContext('webgl') || canvas.getContext('experimental-webgl');
  initShader(gl); // 初始化着色器
  gl.clearColor(0.0, 0.0, 0.0, 1.0); // 指定清空canvas的颜色
  gl.clear(gl.COLOR_BUFFER_BIT); // 清空canvas
  gl.drawArrays(gl.POINTS, 0, 1); // 画点
}
```

向 shader 中传值有两种方式：

- attribute 变量，传递与顶点相关的数组，只能在顶点着色器中使用；
- uniform 变量，传递与顶点无关的数据；

前面的代码将点的位置和大小都直接写在了顶点着色器中，现在将其改为由外面的程序传入。首先修改顶点着色器：

```js
const vertexShaderSrc = `
    attribute vec4 a_Position;// 接收传入位置坐标，必须声明为全局
    attribute float a_PointSize;// 接收传入位置坐标，必须声明为全局
    void main(){
        gl_Position = a_Position;// gl_Position 内置变量，表示点的位置，必须赋值
        gl_PointSize = a_PointSize;// gl_PointSize 内置变量，表示点的大小（单位像素），可以不赋值，默认为1.0
    }
`;
```

然后在 initShader 的最后给这两个变量赋值：

```js
// 获取shader中的a_Position变量
vaconstr a_Position = gl.getAttribLocation(shaderProgram, 'a_Position');
// 给变量a_Position赋值
gl.vertexAttrib4f(a_Position, 0.0, 0.0, 0.0, 1.0);

// 获取shader中的a_PointSize变量
vaconstr a_PointSize = gl.getAttribLocation(shaderProgram, 'a_PointSize');
// a_PointSize
gl.vertexAttrib1f(a_PointSize, 10.0);
```
