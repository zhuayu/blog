---
title: CSS 揭秘（二）形状
date: 2016-10-10 09:21:02
categories:
    - CSS 揭秘
tags:
    - 形状
---

## 自适应椭圆
圆
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: #fff;
        border-radius: 50%;
    "></div>
</div>

```
display: inline-block;
height: 200px;
width: 200px;
background: #fff;
border-radius: 50%;
```

<!-- more -->
椭圆
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 150px;
        width: 200px;
        background: #fff;
        border-radius: 50%;
    "></div>
</div>

```
display: inline-block;
height: 150px;
width: 200px;
background: #fb3;
border-radius: 50%;
```

二分一椭圆
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 150px;
        width: 200px;
        background: #fff;
        border-radius:100% 0 0 100% / 50%;
    "></div>
</div>

```
display: inline-block;
height: 150px;
width: 200px;
background: #fff;
border-radius:100% 0 0 100% / 50%;
```


四分一椭圆
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 150px;
        width: 200px;
        background: #fff;
        border-radius:100% 0 0 0;
    "></div>
</div>


```
display: inline-block;
height: 150px;
width: 200px;
background: #fff;
border-radius:100% 0 0 0;
```

## 平行四边形
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        position: relative;
        height: 100px;
        width: 200px;
        background: #fff;
        transform: skewX(-45deg);
    "></div>
</div>

```
display: inline-block;
position: relative;
height: 100px;
width: 200px;
background: #fff;
transform: skewX(-45deg);
```

伪元素方法：
```
display: inline-block;
position: relative;
height: 100px;
width: 200px;
background: #fff;
position:relative

//thisEle::before
content:'';
position:absolute;
top:0;right:0;bottom:0;left:0;
background:#58a;
z-index:-1;
transform:skew(45deg);
```

## 菱形图片
<div style="text-align: center;background: #ccc;padding: 20px;">
    <img src="/images/touxiang.jpg" style="
        display: inline-block;
        height: 200px;
        width: 200px;
        -webkit-clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
    "/>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
-webkit-clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
```
兼容IE
```
display: inline-block;
height: 200px;
width: 200px;
background: #fff;
transform: rotate(-45deg);
overflow: hidden;

transform: rotate(45deg) scale(1.5);
```


