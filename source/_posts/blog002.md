---
title: 响应式图片srcset,picture
date: 2016-08-30
tags:
- html5
- css3
---

## srcset,sizes

通过设置srcset，seze，w描述符等来提供图片资源和断点
至于选择加载哪个最佳图片，完全交给浏览器去智能判断

```html
<img src="example.jpg" alt="example"
     srcset="example.jpg 128w, example2.jpg 256w, example3.jpg 512w">
```

看上述这段代码，我们给浏览器提供了3张不同大小的图片
其中w是一个描述符，w用来描述文件的宽度
这样浏览器会自动匹配最佳图片

再看下面这段代码

```html
<img src="example.jpg" alt="example"
     srcset="example.jpg 128w, example2.jpg 256w, example3.jpg 512w"
     sizes="(max-width: 360px) calc(100vw - 20px), 128px">
```

通过sizes来指定断点，上述这段代码表示
当视区宽度不大于360px的时候，制定图片宽度为视区宽度减去20px，否则其他情况图片宽度为128px
有一个设备宽度360px的手机，设备像素比是1(去除干扰)，那么加载哪张图片呢？

**分析：**

通过calc的计算，图片宽度为340px，提供的三个图片资源只有512w能满足，所以会加载最后一张图片

## 通过picture标签来匹配不同查询

### 尺寸匹配

```html
<picture>
  <source media="(max-width: 800px)"
          srcset="example.jpg 400w, example2.jpg 800w, example3.jpg 1200w"
          sizes="(min-width: 530px) calc(100vw - 96px), 100vw">
  <img src="example.jpg" alt="">
</picture>
```

上述代码会首先让窗体去匹配source的查询，匹配到了则继续在该source里面的srcset进行选择
没有匹配到的则使用img里面的srcset
