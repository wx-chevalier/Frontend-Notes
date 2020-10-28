# 二维几何变换

图形的几何变换一般是指图形的几何信息经过变换后产生新的图形，图形几何变换既可以看作是坐标系不动而图形变动，即变动后的图形在坐标系中的坐标值发生变化；又可以看作是图形不动而坐标系变动，即变动后的图形在新坐标系下具有新的坐标值。基本的变换有平移、旋转、缩放等。计算机图形学中基本的二维图形的几何变换算法：

$$
\left[x^{\prime} y^{\prime} 1\right]=\left[\begin{array}{lll}
x & y & 1
\end{array}\right]\left[\begin{array}{lll}
a & b & c \\
d & e & f \\
g & h & i
\end{array}\right]=\left[\frac{a x+d f+g}{c x+f y+i} \frac{b x+e y+h}{c x+f y+j} 1\right]
$$
