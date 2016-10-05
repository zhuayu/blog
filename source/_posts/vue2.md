---
title: VUE （二）数据绑定 与 计算属性
date: 2016-10-05 20:16:55
categories:
    - vue
---

## 实例
每个 Vue.js 应用的起步都是通过构造函数 Vue 创建一个 Vue 的根实例：
```
var vm = new Vue({
  // 构造选项
  el: '#app',
  data: {
    message: 'Hello Vue.js'
  },
  methods: {
    greeting: function () {
      alert('Hi, there.')
    }
  }
})
```
构造选项包含属性和方法，其中 data 对象属性包含了 Vue 实例内部可直接使用的数据属性，除非使用 Vue.js 扩充的 $set() 方法（我们会在“列表渲染及其注意事项”中详细介绍）或声明为计算属性，所有需要在 Vue 实例中使用的数据均需在 data 对象中提前声明。
在上面的示例中，data 对象包含 message 数据属性，在 Vue 实例方法中，可以使用 this.message 访问，message 可使用数据绑定语法绑定到 Vue.js 模板中。

<!-- more -->

## 模版
Vue.js 模板是包括特殊格式文本、 Vue.js 指令以及 Vue.js 特殊属性的 HTML 片段，这里的特殊格式文本指用于插值的文本内容，其中的文本内容称为绑定表达式。
```
<div id=“app">
  <div>{{ message }}</div>
  <button v-on:click=“greeting">Say Hi</button>
</div>
```
Vue.js 的模板是基于 DOM 实现的，编译完成之后就是真实的 DOM 元素结构。
我们通常使用的 handlebar、ejs 和 arttemplate 等模板，编译完成之后生成的是字符串，然后需要通过 innerHTML 属性或者 jQuery.html() 方法插入到 DOM 结构中。
Vue.js 的另一个特点是，除了 
```
{{ message }}
{{{ htmlSegment }}} 
```
这样的插值方式之外，其他的数据绑定和指令都是直接写在 HTML 标签内，这样做的好处有以下几点：
- 没有额外的标签，不必像其他模板那样纠结于额外标签是否需要缩进这样的问题；
- 指令作用于哪个 DOM 元素一目了然；
- 判断和循环这样的流程控制结构通过 DOM 元素天然“关闭”，不必担心额外标签的错配和漏配，结构检查和排错比较容易；
- 不用担心使用编辑器的代码格式化功能时出现代码混乱。


## 数据绑定
Vue.js 通过插值和指令这两种方式来实现数据绑定，绑定的数据发生变化时，DOM 元素中的内容、属性会响应式地发生变化。
```
<!-- 插值 -->
{{ message }}
{{{ htmlSegment }}}
<img src="{{ photoURL }}" />

<!-- 指令 -->
<div v-if=“upgradable">升级指南：...</div>
<span v-text=“anotherMessage"></span>
<div v-html=“anotherHTMLSegment"></div>
<li v-bind:class="{'current': thisIsTheCurrentItem}”>Item</li>
<a v-bind:href.literal="javascript:void(0)" v-on:click.stop=“doSomething">Click Me</a>
```

### 插值
使用Mustache语法:
```
- {{msg}}
- {{{HTMLFragment}}}
```
插值类型：

- 文本
- 原始HTML
- HTML属性

Vue.js 的双大括号插值和三大括号插值其实是语法糖，双大括号会被编译成一个 textNode，然后使用 v-text 指令插入插值内容，而三大括号插值则被编译成一个锚节点，然后使用 v-html 指令替换为插值内容，上述过程比直接在 DOM 元素中使用 v-text 或 v-html 在性能上略有降低。
```
<link rel="stylesheet" href="http://cdn.bootcss.com/bootstrap/3.3.6/css/bootstrap.min.css">
<div class="container">
  <h2>Vue.js 数据绑定</h2>
  <div id="app">
    <img src="{{{user.photo}}}" />
    <div>姓名：{{user.name}}</div>
    <div>简介：{{{user.profile}}}</div>
  </div>
</div>
```

```
var vm = new Vue({
  el: '#app',
  data: {
    user: {
      photo: 'http://tva1.sinaimg.cn/crop.0.0.400.400.180/6d2828b2jw8f193hei9uij20b40b4mxd.jpg',
      name: 'Bill Gates',
      profile: '<div><strong>技术专家</strong>、<strong>商业领袖</strong>与<strong>慈善家</strong>，比尔及梅琳达·盖茨基金会联席主席。</div><div><a href="http://www.weibo.com/gates" target="_blank">访问微博</a></div>'
    }
  }
})
```

### 指令
带有v- 的特殊HTML属性：v-if 、v-text 、v-html 、v-bind 、v-on …...因为是HTML属性，所以在模版中是不能单独存在的，必需写在HTML标签中。
把某些特殊行为应用道DOM上，例如：
```
<img src=“{{user.photo}}”/>
// 改写为
<img v-bind:src=“user.photo”/>
```
指令的缩写
```
<a v-bind:href.literal=“javascript:void(0)” v-on:click.stop=“doSomething”>Click Me</a>
// 改写为
<a :href.literal=“javascript:void(0)” @click.stop=“doSomething”>Click Me</a>
```

### 绑定表达式

- 支持全功能JavaScript表达式
- 表达式将在其所在Vue实例的作用域内进行计算
- 不支持多于一个JavaScript表达式
- 不支持语句及流程控制
- 要避免循环更新

### 可选过滤器
Vue.js 2.0 版中将移除所有内置过滤器，过滤器的使用方式也将发生改变，在新版本中，过滤器后使用括号而非空格来添加参数，并只可应用于插值方式的数据绑定，其他使用过滤器的场景使用计算属性替代，并根据需要选择使用针对不同专业领域的第三方独立库，如针对日期时间处理使用 Moment.js，针对金融货币处理使用 Accounting.js，针对数组和对象处理使用 lodash。

## 计算属性
为什么要使用计算属性？

- 模版不宜过于复杂
- 绑定限定为一个表达式
- 如果获取绑定数据需要用刀多于一个表达式的逻辑，应使用计算属性

### 应用场景

- 电商网站中，购物车商品总重量及运费
- 电商网站中，购物车的总价
- 旅游网站中，飞机航班的航程时间、中转时间
- 旅游网站中，根据身份证号码获取乘客类型

### 声明方法：

- 创建Vue实例时，使用构造选项中的computed属性声明计算属性
- computed属性包含
    - 用户获取计算属性的方法
    - 带用get和set方法

### 栗子：
虽然Vue.js 提供了一个 $watch 方法用于观察 Vue 实例上的数据变动，这与 AngularJS 的做法类似。不过，通常更好的办法是使用计算属性而不是一个命令式的 $watch 回调。
以计算长方形面积为例，如果我们要观察长方形的长和宽的数据变动并计算其面积，需要分别设置长和宽的 $watch 回调。

模版
```
<link rel="stylesheet" href="http://cdn.bootcss.com/bootstrap/3.3.6/css/bootstrap.min.css">
<div class="container">
  <h2>Vue.js 计算属性</h2>
<div id="app">
  <div>
    长：{{ a }}m、宽：{{ b }}m
  </div>
  <input type="range" v-model="a">
  <input type="range" v-model="b">
  <div>
    长方形面积：{{ s }}m<sup>2</sup>
  </div>
</div>
```
watch
```
var vm = new Vue({
  el: '#app',
  data: {
    a: 6,
    b: 5,
    s: 30
  }
})

vm.$watch('a', function (val) {
  this.s = val * this.b
})

vm.$watch('b', function (val) {
  this.s = this.a * val
})
```
computed
```
var vm = new Vue({
  el: '#app',
  data: {
    a: 6,
    b: 5
  },
  computed: {
    s: function () {
      return this.a * this.b
    }
  }
})
```
如果使用计算属性，我们只需要编写一次计算逻辑，Vue.js 会自动建立该计算属性与 Vue 实例数据 a、b 的依赖关系并追踪变化。
如果我们将长方形面积公式改为椭圆面积公式，则也只需要修改一次，维护工作量降低，不易出现遗漏的情况。
一般情况下，使用计算属性比 $watch 实现简洁、维护方便，对于需要观察 Vue 实例数据变动的需求，如无特殊情况均应优先使用计算属性。
根据 Vue.js 的 $watch API 文档，当调用 Vue 实例的 $watch 方法创建 $watch 时，返回一个 unwatch 函数，用于取消观察，因此在需要取消观察的场合，使用 $watch 为佳。

## 注意事项
Vue.js 应用从 Vue 实例起步，通过 Vue 构造函数构造 Vue 实例过程中对其模板进行编译，模板中含有插值标记或数据绑定指令，在编译过程中，Vue.js 将 DOM 元素的内容或属性与数据进行绑定，当数据发生变化时，DOM 元素的内容或属性响应式地发生改变。
在编写模板进行数据绑定时，需要注意如下几点：

- 插值语法可用于 DOM 元素内容和 HTML 普通属性，但插值语法和指令语法不可混用，注意不要在 Vue.js 指令中使用双大括号或三大括号插值语法；
- 不要不加限制地对用户提交的内容进行三大括号插值，防止 XSS 攻击；
- 绑定表达式可以是一个 Javascript 表达式，但不能是 Javascript 语句，也不能包含判断、循环之类的复杂的流程结构；
- 要注意避免循环更新；
- 绑定表达式支持过滤器，过滤器不是 Javascript 语法，不能用在表达式内部；
- 模板中的绑定表达式主要用于描述视图的结构，因此不建议使用复杂的逻辑，如果需要多于一个表达式的逻辑，应当使用计算属性；
- 使用 v-bind 和 v-on 指令进行数据绑定时，使用饰符表示指令应当以特殊方式绑定，修饰符的说明参见 Vue.js 的 v-bind 和 v-on 的 API 文档。