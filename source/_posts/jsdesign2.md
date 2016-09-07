---
title: Javascript 设计模式（ 二 ）职责链模式
date: 2016-09-04 16:59:00
categories:
    - Javascript 设计模式
tags:
    - 设计模式
---

## 职责链模式

### 定义：
使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系，将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理为止。
链中受到请求的对象要么亲自处理它，要么转发给下一个候选者。提交方并不明确有多少个对象会处理它，任一候选者都可以响应相对应的请求，可以再运行时刻决定哪些候选者参与到链中。
<!-- more -->

### 例子：
一个售卖手机的电商网站，之前有用户分别交纳500元订金和200元定金预订，现在已经可以正式购买了。
支付过定金的用户肯定有优先购买权还有优惠券补贴，支付500定金的发放100元优惠券，支付200定金的发放50优惠券。没有支付过定金的用户按正常购买。
假设订单页面是PHP吐出的模版，在页面加载之处，PHP会传递给页面几个字段：
* orderType：表示订单类型。code值为1的时候表示支付500元定金的用户，2表示支付200元定金，3表示普通用户。
* pay：表示用户是否已支付定金，code值为true的时候表示已经支付，false的时候虽然下了订单，但没有支付，降级为普通用户。
* stock：表示当前产品的存库数量，已经支付过定金的用户将不受限制。

#### 一般般的写法
```
var order = function(orderType,pay,stock){
    if(orderType == 1 ){
        if( pay === true ){
            console.log('500元定金预购，得到100元优惠券');
        }else{
            if( stock > 0 ){
                console.log('普通用户购买，无优惠券');
            }else{
                console.log('手机库存不足');
            }
        }
    }
    else if(orderType == 2 ){
        if( pay === true ){
            console.log('200元定金预购，得到50元优惠券');
        }else{
            if( stock > 0 ){
                console.log('普通用户购买，无优惠券');
            }else{
                console.log('手机库存不足');
            }
        }
    }
    else if(orderType == 3 ){
        if( stock > 0 ){
            console.log('普通用户购买，无优惠券');
        }else{
            console.log('手机库存不足');
        }
    }
}

order(1,true,500);
order(1,false,500);
order(2,true,500);
order(3,true,500);
order(3,true,0);
```
以上代码中order函数非常巨大，而且需要经常进行修改。虽然能正常运行，但不利于后期维护。

#### 用职责链模式重构代码
先把500元订单、200元订单以及普通购买分成3个函数。让500元订单函数处理，如果该函数不符合处理条件，则把请求传递到200元订单函数。如果200元订单函数不能处理，则把请求传递给普通购买函数处理。
```
var order500 = function(orderType,pay,stock){
    if(orderType===1 && pay==true){
        console.log('500元定金预购，得100元优惠券');
    }else{
        order200(orderType,pay,stock);
    }
}

var order200 = function(orderType,pay,stock){
    if(orderType===2 && pay==true){
        console.log('200元定金预购，得50元优惠券');
    }else{
        orderNormal(orderType,pay,stock);
    }
}

var orderNormal = function(orderType,pay,stock){
    if(stock>0){
        console.log('普通用户购买，没有优惠');
    }else{
        console.log('手机存库不足');
    }
}

order500(1,true,500);
order500(1,false,500);
order500(2,true,500);
order500(3,true,500);
order500(3,true,0);
｀｀｀
