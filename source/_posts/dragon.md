---
title: 用 canvas 实现泡泡龙
date: 2021-05-19
tags: react
---
今天在掘金上看到一个很有意思的，用一张图片生成泡泡龙，[传送门](https://juejin.cn/post/6963476650356916254?utm_source=gold_browser_extension#heading-8)

效果图是这样的

![image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4d686084e16d4dc88b5b17a1d7cd0e1a~tplv-k3u1fbpfcp-watermark.image)

# let's start!
<font size=1>**如有转载，请注明出处。**</font>

> 先找到一张龙的剪映，要是明暗差比较大的那种，将图片绘制到canvas中

```javascript
  const canvas = document.getElementById('canvas');
  const ctx = canvas.getContext('2d');

  const image = new Image();
  image.src = jg;

  image.onload = () => {
    canvas.width = image.width;
    canvas.height = image.height;

    ctx.drawImage(image,0,0);
  }

```
> 获取并裁剪画布的点阵信息 (放在img的onload时间内，确保图片已经加载完成)

```javascript
  const imageData = ctx.getImageData(0, 0, image.width, image.height).data;
  ctx.fillStyle = '#ffffff';
  ctx.fillRect(0, 0, image.width, image.height);

  const gap = 7; // 这个是气泡的密度（数值越大越密）

  for (let h = 0; h < image.height; h += gap) {
    for (let w = 0; w < image.width; w += gap) {
      const position = (image.width * h + w) * 4;
      const r = imageData[position];
      const g = imageData[position + 1];
      const b = imageData[position + 2];

      if (r + g + b == 0) {
        ctx.fillStyle = '#000';
        ctx.fillRect(w, h, 4, 4);
      }
    }
  }

```

这样我们就得到了 ![dragon](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ab8c4d11d4e54e0980f0eb02b5426131~tplv-k3u1fbpfcp-watermark.image)

> 通过点阵信息生成气泡dom
```javascript
  const dragonContainer = document.getElementById('container');
  const dragonScale = 2; // 这个是龙的大小

  for (let h = 0; h < image.height; h += gap) {
    for (let w = 0; w < image.width; w += gap) {
      const position = (image.width * h + w) * 4;
      const r = imageData[position];
      const g = imageData[position + 1];
      const b = imageData[position + 2];

      if (r + g + b == 0) {
        const bubble = document.createElement('img');
        bubble.src = bubblej;
        bubble.setAttribute('class', 'bubble');

        const bubbleSize = Math.random() * 10 + 20;
        bubble.style.left = `${w * dragonScale - bubbleSize / 2}px`;
        bubble.style.top = `${h * dragonScale - bubbleSize / 2}px`;
        bubble.style.width = bubble.style.height = `${bubbleSize}px`;
        bubble.style.animationDuration = `${Math.random() * 6 + 4}s`;

        dragonContainer.appendChild(bubble);
      }
    }
  }

  // html的结构是这样的
  <div
    id="container"
    style={{
      width: '100%',
      height: '100%',
      position: 'absolute',
      zIndex: 2,
      background: 'black',
    }}
  />
  <canvas id="canvas" />

  // css代码为
  .bubble {
		position: absolute;
		animation-timing-function: linear;
		animation-name: floating;
		animation-iteration-count: infinite;
	}

	@keyframes floating {
		0%{
			transform: translateY(0px);
		}
		50%{
			transform: translateY(-5px);
		}
		100%{
			transform: translateY(0px);
		}
	}
```

> 这里的重点在于取到画布的`像素数据`，也就是getImageData
> getImageData() 方法返回 ImageData 对象，该对象拷贝了画布指定矩形的像素数据。

> 对于 ImageData 对象中的每个像素，都存在着四方面的信息，即 RGBA 值：

> R - 红色 (0-255)
> G - 绿色 (0-255)
> B - 蓝色 (0-255)
> A - alpha 通道 (0-255; 0 是透明的，255 是完全可见的)

color/alpha 以数组形式存在，并存储于 ImageData 对象的 data 属性中。

提示：在操作完成数组中的 color/alpha 信息之后，您可以使用 putImageData() 方法将图像数据拷贝回画布上

```javascript
  red = imgData.data[0];
  green = imgData.data[1];
  blue = imgData.data[2];
  alpha = imgData.data[3];
```

> 您也可以使用 getImageData() 方法来反转画布上某个图像的每个像素的颜色。

```javascript
  red = 255-old_red;
  green = 255-old_green;
  blue = 255-old_blue;

  const c=document.getElementById("myCanvas");
  const ctx=c.getContext("2d");
  const img=document.getElementById("tulip");
  ctx.drawImage(img,0,0);
  const imgData=ctx.getImageData(0,0,c.width,c.height);

  // 反转颜色
  for (let i=0;i<imgData.data.length;i+=4)
    {
    imgData.data[i]=255-imgData.data[i];
    imgData.data[i+1]=255-imgData.data[i+1];
    imgData.data[i+2]=255-imgData.data[i+2];
    imgData.data[i+3]=255;
    }
  ctx.putImageData(imgData,0,0);

  // 改上面的部分代码，就得到一个被气泡围绕的空心龙
  const r = 255 - imageData[position];
  const g = 255 - imageData[position + 1];
  const b = 255 - imageData[position + 2];
```



More info: [麦麦](https://github.com/maimai123)
