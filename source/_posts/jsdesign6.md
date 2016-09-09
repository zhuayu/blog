---
title: Javascript 设计模式（ 六 ）单例模式
date: 2016-09-04 21:59:00
categories:
    - Javascript 设计模式
tags:
    - 单例模式
---

## 单例模式
###  定义：
保证一个类仅有一个实例，并提供一个访问它的全局访问点。
<!-- more -->

### 全局变量不是单例模式，但在javascript开发中，我们经常会把全局变量当成单例模式来使用。例如：
```
var a = {};
```
对象 a 确实是独一无二的，如果 a 变量声明在全局的作用域下， 这样就满足了单例的两个条件。但是全局变量存在很多问题，它很容易造成命名空间污染。以下两种方式可以相对降低全局变量带来的命名污染。

#### 使用命名空间
```
var namespace1 = {
  a:function(){
    alert(1);
  },
  b:function(){
    alert(2);
  }
};
```
把a和b都定义为namespace1的属性，这样可以减少变量和全局作用域打交道的机会。

#### 使用闭包封装私有变量
```
var user = (function(){

    var _name = 'sven',
        _age = 29;

    return {
      getUserInfo:function(){
        return _name + '_' + _age;
      }
    }

  })();

```
这种方法吧一些变量封装在闭包的内部，只暴露一些接口跟外界通讯。我们用下划线来约定私有变量_name和_age,它们呗封装在闭包产生的作用域中，外部是访问不到这两个变量的，这就避免了对全局的命名污染。

### 惰性单例
在需要的时候才创建对象实例。后加载，先写了，在用的时候再加载。

#### 例子：弹窗
当我们点击登录按钮时候，会出现一个登录浮窗。
##### 页面加载完成的时候创建
```
<button id="loginBtn">登录</button>

<script>

  var loginLayer = (function(){
      var div = document.createElement('div');
      div.innerHTML = '我是登录浮窗';
      div.style.display = 'none';
      document.body.appendChild(div);
      return div;
    })();

    document.getElementById('loginBtn').onclick = function(){
      loginLayer.style.display = 'block';
    };

</script>

```
或许，用户根本不需要登录操作，这样登录浮窗总是一开始就被创建好，那么很有可能将白白浪费一些DOM节点。

##### 触发事件的时候创建
```
<button id="loginBtn">登录</button>

<script>

  var createLoginLayer = function(){
      var div = document.createElement('div');
      div.innerHTML = '我是登录浮窗';
      div.style.display = 'none';
      document.body.appendChild(div);
      return div;
    };

    document.getElementById('loginBtn').onclick = function(){
      var loginLayer = createLoginLayer();
      loginLayer.style.display = 'block';
    };

</script>

```
虽然达到了惰性的目的，但是失去了单例的效果。当我们每次点击登录按钮时候，都会创建一个新的登录浮窗。虽然我们可以在点击浮窗上添加关闭按钮把这个浮窗从页面删除掉，但这样频繁的创建和删除节点明显是不合理的，也是不必要的。

##### 判断是否已经创建过
```
<button id="loginBtn">登录</button>

<script>

  var createLoginLayer = (function(){
      var div;
      return function(){
        if(!div){
          div = document.createElement('div');
          div.innerHTML = '我是登录浮窗';
          div.style.display = 'none';
          document.body.appendChild(div);
        }

        return div;

      }
  })();

  document.getElementById('loginBtn').onclick = function(){
      var loginLayer = createLoginLayer();
      loginLayer.style.display = 'block';
  };

</script>

```
