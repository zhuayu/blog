---
title: ES6实用小记 (一)
date: 2016-09-22 15:20:30
categories:
    - ES6实用小记
tags:
    - let
    - const
    - stringTemplet
    - funDefult
---

## 定义
在ES6中挑选出比较常用、实用的部分。
<!-- more -->

## var、let、const
let和const的出现，应该就是为了替代var吧，能使代码结构更加之严谨。
### let用于声明变量，const用于声明常量。
意思就是let可以重新赋值，const则不能，并且const声明就必须赋值。
```
let a =1;
a = 2;
// 2
const b = 1;
b = 2;
// Identifier 'a' has already been declared

let a ;
// undefind
const b ;
// Missing initializer in const declaration
```

### 块级作用域
```
if(true){
    var a = 3;
}
console.log(a)
//  3

if(true){
    let b = 3;
}
console.log(b)
// Uncaught ReferenceError: b is not defined
```

### 不允许重复声明
在一定程度上防止了变量污染
```
let a = 1;
var a = 2;  // or const a = 3;let a = 4;
// Identifier 'a' has already been declared
```

### 不存在变量提升
```
console.log(a)
var a =1;
// undefind

console.log(b)
let b =1;
// Uncaught ReferenceError: a is not defined
```

### 暂存性死区
在当前的块级作用域中声明，即代表绑定了该环境，在当前环境中声明前赋值都会报错。因为在这个区域不支持变量提升。
```
var  a = 3;
if(true){
    a =4;
    let a;
}
console.log(a)
// Uncaught ReferenceError: a is not defined

### 最外层不再是window
var a = 1;
let b = 1;
const c = 1;

console.log(window.a) // 1
console.log(window.b) // undefined
console.log(window.c) // undefined
```

## 模版字符串
在拼接HTML插入DOM的时候，功效尤其显著。
```
// ES5
var name = 'jax';
var say = 'hello,'+name+'.'+'Nice to meet you!'

// ES6
var name = 'jax';
var say = `hello,${name}.Nice to meet you!`
```

## 函数默认值
在ES6之前，不能直接为函数的参数指定默认值，只能采用变通的方法。
```
//ES5
(function log(x) {
  x = x || 'Hello';
  console.log(x);
})();
// ES6
(function log( x = 'Hello') {
  console.log(x);
})();
```


