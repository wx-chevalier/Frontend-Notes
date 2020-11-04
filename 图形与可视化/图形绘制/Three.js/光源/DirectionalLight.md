# DirectionalLight

我们要看的最后一个基础光源是 DirectionalLight(方向光)光源。方向光光源可以看做是距离很远的光源。这个光源发出的所有光线都是相互平行的。方向光源的一个范例是太阳。太阳是如此遥远,以至于到达地球的光线都成了平行光。方向光光源和我们之前看过的聚光灯光源之间主要的差别是:方向光不像聚焦光那样离目标越远越暗淡。被方向光光源照亮的整个区域接收到的光强是一样的。

```js
var directionalLight = new THREE.DirectionalLight(pointColor);
directionalLight.position.set(-40, 60, -10);
directionalLight.castShadow = true;
directionalLight.shadowCameraNear = 2;
directionalLight.shadowCameraFar = 200;
directionalLight.shadowCameraLeft = -50;
directionalLight.shadowCameraRight = 50;
directionalLight.shadowCameraTop = 50;
directionalLight.shadowCameraBottom = -50;

directionalLight.distance = 0;
directionalLight.intensity = 0.5;
directionalLight.shadowMapHeight = 1024;
directionalLight.shadowMapWidth = 1024;
```
