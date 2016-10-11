---
title: CSS 揭秘（一）背景与边框
date: 2016-10-09 09:21:02
categories:
    - CSS 揭秘
tags:
    - 背景与边框
---

## 半透明边框
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: #fff;
        border:10px solid hsla(0,0%,100%,.5);
        background-clip: padding-box;
    "></div>
</div>

```
display: inline-block;
height: 200px;
width: 200px;
background: #fff;
border:10px solid hsla(0,0%,100%,.5);
background-clip: padding-box;
```

<!-- more -->

## 多重边框
<div style="text-align: center;background: #ccc;padding: 40px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: #ccc;
        box-shadow: 0 0 0 10px hsla(0,0%,100%,.7),
                    0 0 0 20px hsla(0,0%,100%,.6),
                    0 0 0 30px hsla(0,0%,100%,.5);
    "></div>
</div>
box-shadow方案：
```
display: inline-block;
height: 200px;
width: 200px;
background: #ccc;
box-shadow: 0 0 0 10px hsla(0,0%,100%,.7),
            0 0 0 20px hsla(0,0%,100%,.6),
            0 0 0 30px hsla(0,0%,100%,.5);
```
outline方案：
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: #fff;
        border:10px solid #35b558;
        outline: 10px solid hsla(0,0%,100%,.4);
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background: #fff;
border:10px solid #35b558;
outline: 10px solid hsla(0,0%,100%,.4);
```

## 背景定位
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background:#fff url(/images/touxiang.jpg) no-repeat right 10px bottom 10px/ 100px 100px;
    "></div>
</div>
background-position 扩展语法:
```
display: inline-block;
height: 200px;
width: 200px;
background:#fff url(/images/touxiang.jpg) no-repeat right 10px bottom 10px / 100px 100px;
```
background-origin 方案：
```
display: inline-block;
height: 200px;
width: 200px;
padding: 10px;
background:#fff url(/images/touxiang.jpg) no-repeat right  bottom  / 100px 100px;
background-origin: content-box;
```
background-positon-calc()方案：
```
display: inline-block;
height: 200px;
width: 200px;
background:#fff url(/images/touxiang.jpg) no-repeat 0 0/ 100px 100px;
background-position: calc(100% - 20px) calc(100% - 20px);
```

## 边框内圆角
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background:tan;
        border-radius: 10px;
        box-shadow:0 0 0 10px #655;
        outline: 10px solid #655;
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background:tan;
border-radius: 10px;
box-shadow:0 0 0 10px #655;
outline: 10px solid #655;
```

## 条纹背景
双色：
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: linear-gradient(#fb3 50%,#58a 50%);
        background-size: 100% 20px;
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background: linear-gradient(#fb3 50%,#58a 50%);
background-size: 100% 20px;
```
三色：

<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: linear-gradient(#fb3 33.33%, #58a 0,#58a 66.66%,#35b558 0);
        background-size: 100% 30px;
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background: linear-gradient(#fb3 33.33%, #58a 0,#58a 66.66%,#35b558 0);
background-size: 100% 20px;
```

垂直：
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: linear-gradient(to right,#fb3 50%,#58a 50%);
        background-size:20px 100% ;
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background: linear-gradient(to right,#fb3 50%,#58a 50%);
background-size:20px 100% ;
```

斜向：
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: linear-gradient(45deg,#fb3 25%, #58a 0,#58a 50%,#fb3 0,#fb3 75%,#58a 0);
        background-size:30px 30px ;
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background: linear-gradient(
    45deg,#fb3 25%, #58a 0,#58a 50%,#fb3 0,#fb3 75%,#58a 0);
background-size:30px 30px ;
```

同色系：
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: #fb3;
        background-image: repeating-linear-gradient(30deg,
            hsla(0,0%,100%,.2),
            hsla(0,0%,100%,.2) 15px,
            transparent 0,
            transparent 30px)
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background: #fb3;
background-image: repeating-linear-gradient(30deg,
    hsla(0,0%,100%,.2),
    hsla(0,0%,100%,.2) 15px,
    transparent 0,
    transparent 30px)
```

## 背景图案
网格：
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: white;
        background-image: 
            linear-gradient(90deg,
                rgba(200,0,0,.5) 50%,transparent 0),
            linear-gradient(
                rgba(200,0,0,.5) 50%,transparent 0);
        background-size: 20px 20px;
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background: white;
background-image: 
    linear-gradient(90deg,
        rgba(200,0,0,.5) 50%,transparent 0),
    linear-gradient(
        rgba(200,0,0,.5) 50%,transparent 0);
background-size: 20px 20px;
```

细网格：
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: #58a;
        background-image: 
            linear-gradient(
                white 1px,transparent 0),
            linear-gradient(90deg,
                white 1px,transparent 0);
        background-size: 20px 20px;
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background: white;
background-image: 
    linear-gradient(90deg,
        rgba(200,0,0,.5) 50%,transparent 0),
    linear-gradient(
        rgba(200,0,0,.5) 50%,transparent 0);
background-size: 20px 20px;
```

波点：
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: #655;
        background-image:
            radial-gradient(tan 30%,transparent 0),
            radial-gradient(tan 30%,transparent 0);
        background-size: 20px 20px;
        background-position:0 0 , 30px 30px;
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background: #655;
background-image:
    radial-gradient(tan 30%,transparent 0),
    radial-gradient(tan 30%,transparent 0);
background-size: 20px 20px;
background-position:0 0 , 30px 30px;
```

棋盘：
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background: #eee;
        background-image:
            linear-gradient(45deg,#bbb 25%,transparent 0),
            linear-gradient(45deg,transparent 75%,#bbb 0),
            linear-gradient(45deg,#bbb 25%,transparent 0),
            linear-gradient(45deg,transparent 75%,#bbb 0);
        background-size: 20px 20px;
        background-position: 0 0 ,10px 10px,10px 10px,20px 20px;
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background: #eee;
background-image:
    linear-gradient(45deg,#bbb 25%,transparent 0),
    linear-gradient(45deg,transparent 75%,#bbb 0),
    linear-gradient(45deg,#bbb 25%,transparent 0),
    linear-gradient(45deg,transparent 75%,#bbb 0);
background-size: 20px 20px;
background-position: 0 0 ,10px 10px,10px 10px,20px 20px;
```

伪随机背景：
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        background-image:
            linear-gradient(90deg,#fb3 11px,transparent 0),
            linear-gradient(90deg,#ab4 23px,transparent 0),
            linear-gradient(90deg,#655 41px,transparent 0),
            linear-gradient(90deg,transparent 75%,#bbb 0);
        background-size: 41px 100%,61px 100%,83px 100%;
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
background-image:
    linear-gradient(90deg,#fb3 11px,transparent 0),
    linear-gradient(90deg,#ab4 23px,transparent 0),
    linear-gradient(90deg,#655 41px,transparent 0),
    linear-gradient(90deg,transparent 75%,#bbb 0);
background-size: 41px 100%,61px 100%,83px 100%;
```

## 连续图像边框
<div style="text-align: center;background: #ccc;padding: 20px;">
    <div style="
        display: inline-block;
        height: 200px;
        width: 200px;
        border:20px solid transparent;
        background: 
            linear-gradient(white,white),
            url(/images/touxiang.jpg);
        background-size: cover;
        background-clip: padding-box,border-box;
        background-origin: border-box;
    "></div>
</div>
```
display: inline-block;
height: 200px;
width: 200px;
border:20px solid transparent;
background: 
    linear-gradient(white,white),
    url(/images/touxiang.jpg);
background-size: cover;
background-clip: padding-box,border-box;
background-origin: border-box;
```


















