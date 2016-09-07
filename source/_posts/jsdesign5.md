---
title: Javascript 设计模式（ 四 ）适配器模式
date: 2016-09-04 20:59:00
categories:
    - Javascript 设计模式
tags:
    - 设计模式
---

## 适配器模式

### 定义：
在程序开发中有许多这样的场景：当我们试图调用模块或者对象的某个接口时，却发现这个接口的格式并不符合目前的需求。我们可以修改原来的接口实现，单如果原来的模块很复杂，或者我们拿到的模块时一段别人编写后压缩过的代码，修改接口就不太现实了。第二种方法就是创建一个适配器，将原有接口转换为客户希望的另一个接口，客户只需要和适配器打交道。<br>
适配器模式(Adapter) 是将一个类（对象）的接口（方法或属性）转化成客户希望的另外一个接口（方法或属性），使得原本由于接口不兼容而不能一起工作的那些类（对象）可以一起工作。
<!-- more -->

### 例子：地图渲染
#### 接口相同
当我们向googleMap和baiduMap都发出“显示”请求时，分别以各自的方式在页面中展现了地图：
```
var googleMap={
  show:function(){
    console.log('开始渲染谷歌地图');
  }
};

var baiduMap={
  show:function(){
    console.log('开始渲染百度地图');
  }
};

var renderMap = function(map){
  if(map.show instanceof Function){
    map.show();
  }
};

renderMap(googleMap);
renderMap(baiduMap);

```

#### 接口不相同
由于第三方的接口方法并不在我们的控制范围之内，假如baiduMap提供显示地图的方法不叫show而叫display的话。baiduMap这个对象源于第三方，正常情况下我们都不应该去改动它。此时我们可以通过增加baiduMapAdapter来解决问题：
```
var googleMap={
  show:function(){
    console.log('开始渲染谷歌地图');
  }
};

var baiduMap={
  display:function(){
    console.log('开始渲染百度地图');
  }
};

var baiduMapAdapter = {
  show:function(){
    return baiduMap.display();
  }
}

var renderMap = function(map){
  if(map.show instanceof Function){
    map.show();
  }
};

renderMap(googleMap);
renderMap(baiduMap);
```

### 例子2：地图数据
#### 创建原始的结构类型
假如我们正在编写一个渲染广东省地图的页面。目前从第三方资源里获得广东省的所有城市以及它们所对应的ID，并且成功渲染到页面。
```
var getGuangdongCity = function(){
  var guangdongCity = [
    {
      name:'shenzhen',
      id:11,
    },
    {
      name:'guangzhou',
      id:12,
    }
  ];

  return guangdongCity;
};

var render = function(fn){
  console.log('开始渲染广东省地图');
  document.write(JSON.stringify(fn()));
}

render(getGuangdongCity);

```
然后我们发现这些数据不太可靠，里面还缺少很多城市。于是我们又在网上找了另外一些数据资源，这次的数据更加全面，单遗憾的是，数据结构和正在运行的项目并不一致。

#### 遇到新的数据类型
```
var guangdongCity = {
  shenzhen:11,
  guangzhou:12,
  zhunhai:13
};
```
除了大动干戈地改写渲染页面的前端代码之外，另一种更轻便的解决方式就是新增一个数据格式转换的适配器。

#### 新增数据转换适配器
```
var getGuangdongCity = function(){
  var guangdongCity = [
    {
      name:'shenzhen',
      id:'11',
    },{
      name:'guangzhou',
      id:'12,'
    }
  ];
  return guangdongCity;
};

var render = function(fn){
  console.log('开始渲染广东省地图');
  document.write(JSON.stringify(fn()));
};

var addressAdapter = function(oldAddressfn){

  var address = {},
      oldAddress = oldAddressfn();

  for(var i = 0,c; c = oldAddress[i++];){
    address[c.name]=c.id;
  }

  return function(){
    return address;
  }
}

render(addressAdapter(getGuangdongCity));

```

### 小结
适配器模式主要用于解决两个已有接口之间不匹配的问题，它不考虑这些接口是怎么实现的，也不考虑如何演化。适配器模式不需要改变已有的接口，就能过使他们协调作用。