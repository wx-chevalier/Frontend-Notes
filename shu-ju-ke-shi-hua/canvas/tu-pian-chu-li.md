# 图片处理

```javascript
async function getCanvasFromImageUrl(imgUrl) {
  const img = new Image();
  img.crossOrigin = 'Anonymous';
  img.src = imgUrl;

  return new Promise((resolve, reject) => {
    img.onload = () => {
      const canvas = document.createElement('canvas');
      canvas.width = img.width;
      canvas.height = img.height;
      const ctx = canvas.getContext('2d');
      ctx.drawImage(img, 0, 0);
      resolve(canvas);
    };

    img.onerror = err => {
      reject(err);
    };
  });
}
```

