---
title: Flex
date: 2016-09-04 10:54:01
tags:
    - Flex
---

## Flex

### 一、简介

#### 1.是什么？
Flexible Box , 弹性盒子 ，用于弹性布局

#### 2.兼容性
IE10 + ／Chrome 21+ ／Opera 12.1+ ／Firefox 22+ ／Safari 6.1+
参考：[caniuse.com](http://caniuse.com/#search=flex)

#### 3.影响
设置 flex 布局以后，子元素的 float 、clear 、vertical-align 属性将失效。
<!-- more -->

#### 4.基本概念
- 采用flex布局的元素，成为flex容器（flex container）。他的子元素自动变成容器成员，成为项目（flex item）。
- 容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。
- 主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；
- 交叉轴的开始位置叫做cross start，结束位置叫做cross end。
- 项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。

### 二、使用Flex

#### 1.定义flex容器，任何容器都可以定义为flex布局，包括行内元素。需要添加内核前缀。
```
.box-container{
    display: flex;
    display: -ms-flex;          /* IE10 */
    display: -webkit-flex;      /* Safari chrome opera */
}

.box-inline-container{
    display: inline-flex;
}
```

#### 2.设置容器属性
```
flex-direction: row | row-reverse | column | column-reverse;
flex-wrap: nowrap | wrap | wrap-reverse;
flex-flow: <flex-direction> || <flex-wrap>;
justify-content: flex-start | flex-end | center | space-between | space-around;
align-items: flex-start | flex-end | center | baseline | stretch;
align-content: flex-start | flex-end | center | space-between | space-around | stretch;
```

- flex-direction    :  排列方向
    - row（默认值）：水平方向 ，起点在左端。
    - row-reverse：水平方向，起点在右端。
    - column：垂直方向，起点在上沿。
    - column-reverse：垂直方向，起点在下沿。
- flex-wrap    :    如何换行
    - nowrap（默认值） ：不换行
    - wrap ：换行，第一行在上方
    - wrap-reverse：换行，第一行在下方
- flex-flow ：flex-direction + flex-wrap 的简写
    - row nowrap（ 默认值）
- justify-content  : 主轴对齐方式
    - flex-start（默认值）：左对齐
    - flex-end：右对齐
    - center：居中
    - space-between：两端对齐，项目之间的间隔都相等。
    - space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
- align-items  ：叉轴对齐方式
    - stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
    - flex-start：交叉轴的起点对齐。
    - flex-end：交叉轴的终点对齐。
    - center：交叉轴的中点对齐。
    - baseline: 项目的第一行文字的基线对齐。
- align-content ：多轴线的对齐方式，一个轴线不起作用
    - stretch（默认值）：轴线占满整个交叉轴。
    - flex-start：与交叉轴的起点对齐。
    - flex-end：与交叉轴的终点对齐。
    - center：与交叉轴的中点对齐。
    - space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
    - space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。

#### 3.设置项目属性
```
order: <integer>;
flex-grow: <number>; /* default 0 */
flex-shrink: <number>; /* default 1 */
flex-basis: <length> | auto; /* default auto */
flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
align-self: auto | flex-start | flex-end | center | baseline | stretch;
```
- order  :  排列属性
    - 0 （默认值）：越小排列越靠前，可以为负数。
- flex-grow  :  放大比例
    - 0（默认值）：如果存在剩余空间，也不放大。
    - 1 ：按 1 比例等分空间。
- flex-shrink  :  缩小比例
    - 1（默认值）：当空间不足时，都按比例缩小。
    - 0    ：  如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
- flex-basis  :  分配多余空间前，占据主轴的空间
    - auto（默认值）：项目本来大小。
- flex  ： flex-grow, flex-shrink 和 flex-basis的简写，后两个属性可选。
    - 0 1 auto （默认值）
    - auto ：1 1 auto
    - none：0 0 auto
- align-self  :  允许单个项目与其它项目不一样的对齐方式
    - auto（默认值）： 表示继承父元素的align-items属性

### 三、注意事项

- flexbox有多个版本，在低版本安卓下实行的是09年的旧版标准Flexible Box Layout Module，这个标准需要添加-webkit-前缀。其余的实行的是新的flexbox标准CSS Flexible Box Layout Module Level 1，其中ios9以下Safari需要添加-webkit-前缀。
- 旧版flexbox标准各浏览器支持属性有限，比如说不支持flex-wrap等。所以如果考虑兼容性的话需要只使用旧版标准中浏览器可以支持的属性。
- 还是旧版的问题，旧版的使用比例伸缩布局时会导致盒子内容大小不等会导致无法‘等分’等布局。这个时候需要设置width:0%;等把原始大小设置成0。
- 依旧是旧版的问题，旧版的box item要求属性是块级结构，所以很多inline元素需要设置display：block等才能显示正常。
- 新发现 text-overflow: ellipsis;在display：flex元素上无效