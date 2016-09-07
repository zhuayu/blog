---
title: Javascript 设计模式（ 五 ）策略模式
date: 2016-09-04 18:59:00
categories:
    - Javascript 设计模式
tags:
    - 设计模式
---

## 策略模式

### 定义：
定义了一些列的算法，把它门一个个封装起来，并使他们可以相互互换，算法的变化不会影响使用算法的客户。
<!-- more -->

### 例子：
用策略模式创建一个表单验证的功能。
以下是表单结构：
```
<form  id="registerForm" action="http://xxx.com/register" style="padding:50px;">
    <label for="userName" >userName</label>
    <div>
      <input type="text" id="userName" placeholder="userName" name="userName">
    </div>

    <label for="password" name="password" >Password</label>
    <div>
      <input type="password" id="password" name="password" placeholder="Password">
    </div>

    <label for="phoneNumber" name="phoneNumber">phoneNumber</label>
    <div>
     <input type="text" id="phoneNumber" name="phoneNumber" placeholder="phoneNumber">
    </div>

    <div>
      <button type="submit">提交</button>
    </div>
</form>
```

#### 封装策略类
我们创建一个策略对象strategies，并把算法类型都在里面分别封装起来。
```
var strategies = {
    isNonEmpty:function(value,errorMsg){                  //不为空
        if (value==='') { 
            return errorMsg;
        }
    },
    minLength:function(value,length,errorMsg){            //限制最小长度
        if (value.length<length) {
            return errorMsg;
        }
    },
    isMobile:function(value,errorMsg){                   //手机号码格式
        console.log(this);
        if (!/(^1[3|5|8][0-9]{9}$)/.test(value)) {
            return errorMsg;
        }
    }
};
```

#### 封装验证类
* add方法接收到registerForm.userName,[{strategy:'minLength:6'},{errorMsg:'用户名长度不能少于6位'}];
* strategyAry就把'minLength:6'通过冒号区分了策略和参数
* strategy通过shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。
* 这样stragety就等于'minLength'
* strategyAry就等于'6'
* strategyAry通过unshift() 方法可向数组的开头添加了'dom.value',
* strategyAry就等于｛'value','6'}
* strategyAry通过push()方法添加上了'errorMsg',
* strategyAry就等于了{'value','6','errorMsg'}
```
var Validator = function(){
    this.cache = [];                                     //保存验证规则
};

Validator.prototype.add = function(dom,rules){

    var self = this;

    for(var i = 0, rule; rule = rules[i++];){
        (function(rule){
            var strategyAry = rule.strategy.split(':');  //把strategy和参数分开
            var errorMsg = rule.errorMsg;

            self.cache.push(function(){                 //把校验的步骤用空函数包装起来，并放入cache
                var strategy = strategyAry.shift();     //取出用户挑选的strategy
                strategyAry.unshift(dom.value);         //把input的value 添加进参数列表
                strategyAry.push(errorMsg);             //把errorMsg     添加进参数列表
                return strategies[strategy].apply(dom,strategyAry);
            });
        })(rule)
    }
};

Validator.prototype.start=function(){                   
    for(var i=0, validatorFunc;validatorFunc=this.cache[i++];){
        var errorMsg = validatorFunc();                  //开始校验，并取得校验后的返回信息
        if (errorMsg) {                                  //如果有确切的返回值，说明没有验证通过
            return errorMsg;
        };
    }
}
```

#### 客户调用代码
先创建一个validator对象，然后通过validator.add方法，往validator对象添加一些校验规则。validator.add方法接受3个参数。
registerForm.password 为参与验证的输入框。minLength:6 是一个以冒号隔开的字符串。冒号前的minlength代表客户挑选的strategy对象，冒号后面的6表示校验需要的必要参数。第三个参数就是返回的错误信息。
往vlidator对象添加完一系列的校验规则之后，会调用validator。start返回一个确切的errormsg字符串当作返回值，说明这次校验没有通过。
然后呢，让registerform.onsubmit方法返回false来阻止表单的提交。
```
var registerForm = document.getElementById('registerForm');

var validataFunc = function(){
    var validator = new Validator();                   

    validator.add(registerForm.userName,[{     //添加校验规则
        strategy:'isNonEmpty',
        errorMsg:'用户名不能为空'
    },{
        strategy:'minLength:6',
        errorMsg:'用户名长度不能小于6位'
    }]);

    validator.add(registerForm.password,[{
        strategy:'minLength:6',
        errorMsg:'密码长度不得能少于6位'
    }]);

    validator.add(registerForm.phoneNumber,[{
        strategy:'isMobile',
        errorMsg:'手机号码格式不正确'
    }]);

    var errorMsg = validator.start();           //获得校验结果
    return errorMsg;                            //返回校验结果
}

registerForm.onsubmit = function(){             //如果errormsg有确切返回值，说明验证没有通过
    var errorMsg = validataFunc();

    if (errorMsg) {
        alert(errorMsg);
        return false;
    };
};

```
使用策略模式重构代码之后，我们仅仅通过配置的方法就可以完成一个表单的校验，
这些校验规则也可以服用在程序的任何地方，还能作为插件的形式，方便地被移植到其它项目中。
在修改校验规则的时候，只需要编写或改动少量的代码。

#### 总结
策略模式是一种可常用且有效的设计模式，从案例中我们可以总结出策略模式的一些优点：
* 策略模式利用组合，委托和多态，有效地避免了多重条件语句。
* 策略模式提供了对开放－封闭原则的支持，将算法封装在独立的strategy中，使得它们易与切换，易于理解，易于扩展。
* 策略模式中的算法也可以复用在系统的其它地方，从而避免许多复制粘贴
* 策略模式中利用组合和委托来让context拥有算法的能力，这也是继承的的一种更轻便的替代方案。
