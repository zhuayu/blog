---
title: Javascript 设计模式（ 三 ）建造者模式
date: 2016-09-04 17:59:00
categories:
    - Javascript 设计模式
tags:
    - 设计模式
---

## 建造者模式

### 定义：
将一个复杂的对象构建与他的表象分离，使得同样的构建过程可以创建不同的表示。
<!-- more -->

建造者模式通常包括下面几个角色：
* Builder：给出一个抽象借口，以规范产品对象的各个组成成分的构建。这个借口规定要实现复杂对象的哪些部分的建造。这个结构规定要实现复杂对象的哪些部分的创建，并不设计具体对象部件的建造。
* ConcreteBuilder：实现builder接口，针对不同的商业逻辑，具体化复杂对象的各部分的创建。在创建过程中完成后，提供产品实例。
* Director：调用具体建造者来创建复杂对象的各个部分，不涉及具体产品的信息，只负责保证对象各部分完整创建或按某种顺序来创建。
* Product：要创建的复杂对象。

### 例子：
例如一个客户要做一个好大上的网页，网页就是要创建的对象。然后老板说没问题抱在我身上，老板就是director。老板就啪一声扔给了程序员，程序员就是builder。程序员做的网站中，要弄html框架，要弄css样式，要弄js行为，最后实现用户提出的高大上需求，完成了打成一个包，好让老板提着走，这个包就是concretebuilder。
#### 客户说要一个高大上的网页
提出要创建复杂对象的一个需求
```
var Web = function(){
    this.html ='高大上',
    this.css='高大上',
    this.js='高大上';
}
```

#### 老板说没问题。翻滚吧！程序员！
程序员首先搭建html，搭建css，搭建js，然后把html，css，js写得高大上。
```
var Builder = function(){
    this.makehtml = function(){
        console.log('构建html文件');
    }
    this.makecss = function(){
        console.log('构建css文件');
    }
    this.makejs = function(){
        console.log('构建js文件');
    }
    this.getResult = function(){
        var web = new Web();
        web.html = '这个是高大上的html';
        web.css = '这个是高大上的css';
        web.js = '这个是高大上的js';
        return web;
    }
}
```

#### 老板就指指点点说，先弄这个，再弄这个
```
var Director = function(){
    this.getWeb = function(builder){
        builder.makehtml();
        builder.makecss();
        builder.makejs();
    }
}
```

#### 客户呼唤老板交稿
```
var builder = new Builder();
var director = new Director();
director.getWeb(builder);
var hightWeb = builder.getResult();
console.log(hightWeb); 
```


