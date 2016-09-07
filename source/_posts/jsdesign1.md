---
title: Javascript 设计模式（ 一 ）模块方法模式
date: 2016-09-04 15:59:00
categories:
    - Javascript 设计模式
tags:
    - 设计模式
---

## 模块方法模式

###  定义：
模块方法模式是一种需要使用继承就可以实现的模式。
由2部分组成，第一部分是抽象父类，第二部分是具体的实现子类。
通常在抽象父类中封装了子类的算法框架，包括实现一些公共方法以及封装子类中所有的方法的执行顺序。子类通过继承这个抽象的类，也继承了整个算法结构，并且可以选择重写父类的方法。
<!-- more -->

###  例子：Coffer or Tea
####  泡一杯咖啡
首先我们泡一杯咖啡，基本步骤是：
* 把开水煮沸
* 用沸水冲泡咖啡
* 把咖啡倒入杯子
* 加糖和牛奶
```
var Coffee = function(){};

Coffee.prototype.boilWater = function(){
    console.log('把水煮沸');
}
Coffee.prototype.brewCoffee = function(){
    console.log('冲泡咖啡');
}
Coffee.prototype.pourInCup = function(){
    console.log('把咖啡倒入杯子');
}
Coffee.prototype.addSomething = function(){
    console.log('加糖和牛奶');
}

Coffee.prototype.init = function(){
    this.boilWater();
    this.brewCoffee();
    this.pourInCup();
    this.addSomething();
}

var coffee = new Coffee();
coffee.init();
```

####  泡一杯茶
和泡咖啡的步骤相差不大
* 把水煮沸
* 用沸水冲泡茶叶
* 把茶倒入杯子
* 加柠檬
```
var Tea = function(){};

Tea.prototype.boilWater = function(){
    console.log('把水煮沸');
}
Tea.prototype.brewTea = function(){
    console.log('冲泡茶叶');
}
Tea.prototype.pourInCup = function(){
    console.log('把茶倒入杯子');
}
Tea.prototype.addSomething = function(){
    console.log('加柠檬');
}

Tea.prototype.init = function(){
    this.boilWater();
    this.brewTea();
    this.pourInCup();
    this.addSomething();
}

var tea = new Tea();
tea.init();                                                      
```

####  分离出共同点
不管是泡咖啡还是泡茶都要经历以下几个步骤：
* 把水煮沸
* 用沸水冲泡原料
* 把成品倒入杯子
* 加配料

<p>所以，不管是泡什么原料，我们都可以给它一个新的方法名称，例如：brew( ) 。<br>
同理，不管是加什么配料，我们也可以给他一个新的方法名称，例如：add( ) 。<br>
因此，不管是泡什么东西，我们就可以给他一个新的父类名称，例如：Beverage( )。</p>

```
var Drunk = function(){};

Drunk.prototype.boil = function(){
    console.log('把水煮沸');
}
Drunk.prototype.brew = function(){
    throw new Error('子类必须重写brew方法');
}
Drunk.prototype.pour = function(){
    throw new Error('子类必须重写pour方法');
}
Drunk.prototype.add = function(){
    throw new Error('子类必须重写add方法');
}

Drunk.prototype.init = function(){
    this.boil();
    this.brew();
    this.pour();
    this.add();
}                                                    
```

####  创建coffee和tea子类
现在创建一个Drunk类的对象对我们来说没有意义，因为饮品有非常多种，在这里Drunk只是一个抽血的存在。接下来我们要创建咖啡喝茶类，让他们继承饮品Drunk类。
```
var Coffee = function(){};
Coffee.prototype = new Drunk();
//接下来需要重写抽象父类的一些方法，只有把水煮沸boil这个行为可以直接使用父类中boil的方法，其他方法都需要在Coffee子类中重写。
Coffee.prototype.brew = function(){
    console.log('用沸水冲泡咖啡');
}
Coffee.prototype.pour = function(){
    console.log('把咖啡倒入杯子');
}
Coffee.prototype.add = function(){
    console.log('加糖和牛奶');
}

var coffee = new Coffee();
coffee.init();
//我们的coffee已经完成，当调用coffee对象的init方法时，由于coffee对象和Coffee构造函数的原型prototype上都没有对应的init方法，所以该请求会顺着原型链，被委托给Coffee的父类Drunk原型上的init方法。
```
接下来照葫芦画瓢，来创建一下tea类
```
var Tea= function(){};
Tea.prototype = new Drunk();

Tea.prototype.brew = function(){
    console.log('用沸水冲泡茶叶');
}
Tea.prototype.pour = function(){
    console.log('把茶倒入杯子');
}
Tea.prototype.add = function(){
    console.log('加柠檬');
}

var tea = new Tea();
tea.init();
```

#### 总结
在上面栗子中，Drunk.prototype.init，就是所谓的模版方法。
因为方法中封装了子类的算法框架，它作为一个算法的模版，指导子类以何种顺序去执行哪些方法。在Drunk.prototype.init方法中，算法内的每一个步骤都是清楚展现在我们眼前的。
