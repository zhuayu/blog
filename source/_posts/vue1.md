---
title: VUE (一) 入门
date: 2016-10-02 14:53:12
categories:
    - vue
---

# 概览

## 核心
Reactive Components for Modern web interfaces.
数据驱动的组件，为现代化的web界面而生。
<!-- more -->
- 数据驱动
- 组件化

### 特点：
- 入侵性底
- 鼓励模块化
- 轻量，高性能

## 架构

### MVC架构
<img src="/images/mvc.png"  />
- Model：模型，用来封装业务程序的逻辑相关的数据以及数据处理方法，可以直接访问业务数据。
- View：视图，实现数据的显示，为了相应数据的变化必需注册到他监视的数据模式上。视图具有一定的业务逻辑，当业务逻辑发生变化时，视图相应的发生变化。
- Controller：控制器，起到不同层次的组织作用。控制业务程序的流程，相应用户行为，创造用户模型。
MVC架构的缺点时由于三个部分耦合性比较高，使得单独拆出一个部分或接口拿出来测试会比较困难。

### MVP
<img src="/images/mvp.png"  />
- Presenter：实现应用程序的业务逻辑，控制视图和数据模型中的同步
- View：变得轻量，不再需要注册并检测Model
MVP架构由于降低了耦合，各部分可以进行单元测试。
MVP的缺点是，Presenter还是要控制V和M的同步逻辑处理。

### MVVM
<img src="/images/mvvm.png"  />
- Model：应用程序数据的抽象和封装。
- ViewModel：显示在页面上的数据的抽象和封装

MVVM将各种同步处理的工作进行了进一步的抽象，ViewModel和View之间通过数据绑定，实现数据的同步。
以前由Presenter实现的数据同步操作，现在交给了数据绑定来实现。
只需要在View中使用数据绑定语法，证明View的显示内容和ViewModel的数据关系。
当ViewModel进行数据更新的时候，会将数据更新的内容实时在View上更新。
同时当用户对View进行操作的时候，数据绑定也会将View上的数据变化同步到数据模版上。

## 定位
<img src="/images/vdw.png"  />
vue 在MVVM中的定位是ViewModel层。
MVVM的View相当于页面的DOM树，而ViewModel层就由vue来实现，vue实例中的vm.$Data对页面或部分页面的数据进行抽象，映射页面的视图内容和状态。在Model层通过原生的JS来对业务逻辑进行抽象和封装。
在开发过程中，只需要关注如何合理地设置ViewModel的数据，来实现业务视图的封装和抽象，以及实现View和ViewModel的交互和同步。

## 开发
实现一个下拉选择列表
CSS
```
.form-select {
    cursor: pointer;
    display: inline-block;
}
.form-select-text {
    display: inline-block;
    width: 100px;
    padding-left: 5px;
    border-bottom: 1px dashed black;
}
.form-select-text .glyphicon {
    float: right;
}
.form-select-list {
    background-color: white;
    position: absolute;
    border: 1px solid black;
    width: 100px;
}
.form-select-list ul {
    margin: 0;
    padding: 0;
    list-style: none;
}
.form-select-list li {
    padding: 0 0 0 5px;
}
.form-select-list li:hover {
    color: blue;
    background-color: ghostwhite;
}
```

### jquery实现方式

DOM
```
<div>
  <span>请选择：</span>
  <div id="mySelector" class="form-select">
    <span class="form-select-text"></span>
    <div class="form-select-list">
      <ul>
      </ul>
    </div>
  </div>
</div>
```
JS
```
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.1/jquery.min.js" ></script>
<script>
$(function () {
  var options = ('选项一,选项二,选项三,选项四,选项五').split(',')

  var $mySelector = $('#mySelector')
  var $mySelectorText = $mySelector.find('.form-select-text')
  var $mySelectorListDiv = $mySelector.find('.form-select-list')
  var $mySelectorList = $mySelector.find('.form-select-list ul')

  $mySelectorText.html(options[0])
  $mySelectorListDiv.hide()
  $.each(options, function (idx, option) {
    $mySelectorList.append($('<li>' + option + '</li>'))
  })

  $mySelectorList.on('click', 'li', function (e) {
    $mySelectorText.html(options[$(this).index()])
    $mySelectorListDiv.hide()
  })

  $('.form-select').on('mouseover', function (e) {
    $(this).find('.form-select-list').show()
  }).on('mouseout', function (e) {
    $(this).find('.form-select-list').hide()
  })
})
</script>
```

### 使用vue
DOM
```
<div id="vueEle">
  <span>请选择：</span>
  <div class="form-select" v-on:mouseover="listStyle.display = 'block'" v-on:mouseout="listStyle.display = 'none'">
    <span class="form-select-text">{{selected}}</span>
    <div class="form-select-list" v-bind:style="listStyle">
      <ul>
        <li v-for="option in options" v-on:click="selectOption(option)">{{option}}</li>
      </ul>
    </div>
  </div>
</div>
```
JS
```
<script src="http://cdnjs.cloudflare.com/ajax/libs/vue/2.0.0-rc.8/vue.js" ></script>
<script>
  var options = ('选项一,选项二,选项三,选项四,选项五').split(',')

  var vm = new Vue({
    el: '#vueEle',
    data: {
      options: options,
      selected: options[0],
      listStyle: {
        display: 'none'
      }
    },
    methods: {
      selectOption (opt) {
        this.selected = opt
        this.listStyle.display = 'none'
      },
      log (info) {
        console.log(info)
      }
    }
  })
</script>
```

# 数据绑定 与 计算属性

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

# 条件渲染

- Vue.js 条件渲染的作用是在不同条件下显示或隐藏某些元素，可以使用 v-if 和 v-show 指令实现不同的条件渲染。
- 这两个指令在作用、编译方式及使用方法上都各有不同，此外，还有一个 v-else 指令可以分别和 v-if、v-show 配合使用。
- v-if为true就在DOM中显示，为false则在DOM中移除。
- v-show为true在DOM中display为block，为false在DOM中display为none。

## v-if例子：
```
<link rel="stylesheet" href="http://cdn.bootcss.com/bootstrap/3.3.6/css/bootstrap.min.css">
<div class="container">
    <h2>条件渲染</h2>
    <div id="app">
        <div>
            <img v-if="user.photo" :src="user.photo">
            <img v-else src="../images/defaultAvatar.png">
        </div>

        <hr>

        <div>
            产品描述、产品价格、产品规格、产品库存……
        </div>
        <template v-if="product.bookmarked">
            <span class="glyphicon glyphicon-bookmark text-success"></span>
            <span class="text-info">已收藏{{product.bookmarkedTimes}}次</span>
        </template>
    </div>
</div>
```
```
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            user: {
                photo: 'http://tva1.sinaimg.cn/crop.0.0.400.400.180/6d2828b2jw8f193hei9uij20b40b4mxd.jpg'
            },
            product: {
                bookmarked: true,
                bookmarkedTimes: 26
            }
        }
    })
</script>
```

## v-show例子：
```
<style>
.item {
    cursor: pointer;
}
.subitems div {
    padding-left: 15px;
}
</style>
<link rel="stylesheet" href="http://cdn.bootcss.com/bootstrap/3.3.6/css/bootstrap.min.css">

<div class="container">
    <h2>条件渲染</h2>
    <div id="app">
        <div>
            <div class="item" @click="subitemsExpanded = !subitemsExpanded">
                成人票套餐
                <span class="glyphicon glyphicon-chevron-up" v-show="subitemsExpanded"></span>
                <span class="glyphicon glyphicon-chevron-down" v-else></span>
            </div>
            <div class="subitems" v-show="subitemsExpanded">
                <div>成人票</div>
                <div>门票抵用券</div>
                <div>酒店抵用券</div>
            </div>
        </div>
    </div>
</div>
```
```

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            subitemsExpanded: true
        }
    })
</script>
```

v-if 指令的切换性能消耗较高，当条件切换时，v-if 指令会根据条件创建或删除 DOM 元素，在条件满足时编译生成相应的元素并插入 DOM 结构，在条件不满足时将元素从 DOM 结构中删除；当使用 template元素包装多个元素时，最终的渲染结果仅包含 template 元素的内容。

v-show 指令的初始性能消耗较高，v-show 指令会在初始编译时创建元素并插入 DOM 结构，同时根据条件决定该元素是否显示；v-show 指令不能使用 template 元素包装多个元素；v-if 和 v-show 指令后面都可以紧跟一个 v-else 指令表示当条件不满足时渲染/显示的内容。

## 扩展：
template 元素是 HTML5 中引入的新元素，天然具有 display:none 的样式且无法改变，因此v-if 指令使用 template元素包装时，编译结果仅使用其所包装的内容而不保留 template 元素（脱壳）。
v-show 指令通过 display 样式来控制元素是否显示，若使用 template 元素包装，编译时同样需要经过“脱壳”的过程，无法实现一个父级元素设置多个子元素的 display 样式，因此不能使用 template 元素包装多个元素，若希望能在条件变化时单独设置多个元素的 display 样式，则会带来较大的实现复杂度。当希望控制多个元素显示/隐藏时，要么使用非 template（如 div ）元素进行包装，并消除使用该包装元素对结构、样式带来的影响，要么分别对每个元素应用 v-show 指令，或者在切换性能消耗可接受的条件下，改为使用 v-if 指令。

## 注意事项：

- 多于一个元素的 v-if 需要使用 template
- v-show 不支持 template
- v-else 必须紧跟在 v-if/v-show 指令后面
- 组件条件下不能使用 v-else

当 v-if 用于多个元素时，若随意在元素外面使用 div> 元素包装，当在 css 文件中使用了元素结构较为明确的选择子时（如.nav > span），则有可能会导致元素的样式发生变化；
v-if 和 v-show 指令编译时，会查找紧跟其后的元素是否带有 v-else 指令，因此带 v-else 指令的元素与带 v-if/v-show 指令的元素之间不能间隔其他元素；
组件条件下，由于指令优先级的不同，会导致 v-else 出现问题，因此只能通过使用相反条件的 v-if 指令来实现 v-else 的效果。

## 比较：
v-if 指令是惰性的，只有条件为真时才开始局部编译（编译后缓存），当条件变化时将编译生成的元素从 DOM 结构中插入/删除；
v-show 是非惰性的，初始编译时编译和渲染元素，并根据条件设置 display 样式来控制其显示/隐藏；v-if 指令切换消耗较高，v-show 指令初始渲染消耗较高。
选择 v-if 还是 v-show 指令主要考虑条件变化是否频繁及当条件切换时对性能的消耗。

# 列表渲染

## 指令与索引：
可以使用 v-for 指令基于一个数组渲染一个列表。这个指令使用特殊的语法，形式为 item in items，items 是数据数组，item 是当前数组元素的别名。
从1.0.17版开始，为兼容 Javascript 写法，也可以使用 item of items 语法；
在 v-for 块内我们能完全访问父组件作用域内的属性，另有一个特殊变量 $index，正如你猜到的，它是当前数组元素的索引：
```
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }} - {{ $index }}
  </li>
</ul>
```
```
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

## 嵌套：
类似于 template v-if，也可以将 v-for 用在 <template> 标签上，以渲染一个包含多个元素的块。
```
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider"></li>
  </template>
</ul>
```

## 对象属性：
除了 $index 之外，作用域内还可以访问另外一个特殊变量 $key。
在遍历对象时，是按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下是一致的。
```
<ul id="repeat-object" class="demo">
  <li v-for="value in object">
    {{ $key }} : {{ value }}
  </li>
</ul>
```
```
new Vue({
  el: '#repeat-object',
  data: {
    object: {
      FirstName: 'John',
      LastName: 'Doe',
      Age: 30
    }
  }
})
```

## 值域：
v-for 也可以接收一个整数，此时它将重复模板数次。
```
<div>
  <span v-for="n in 10”>{{ n }} </span>
</div>
```

## 检测：
<img src="/images/vcheck.png"  />
受ES5限制，vue不能监测到数据元素的或对象属性的添加或者删除。所以Vue.js 包装了被观察数组的变异方法，故它们能触发视图更新。被包装的方法有：

### 变异方法：

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

### 非变异方法
不会修改原始数组而是返回一个新数组：

- concat()
- slice()
- filter()

#### 重新赋值
在使用非变异方法时，可以直接用新数组替换旧数组：
```
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

## 对象扩充方法:
因为 JavaScript 的限制，Vue.js 不能检测到下面数组变化：
直接用索引设置元素，如 vm.items[0] = {}；
修改数据的长度，如 vm.items.length = 0。
所以，Vue.js 扩展了观察数组：
- $set           添加数据
- $remove    删除数据
```
example1.items.$set(0, { childMsg: 'Changed!'})
this.items.$remove(item)
```

## track-by：
当对数组重新赋值时，如果不加特殊设置，vue会重新渲染所有列表上的DOM元素。
为了提升性能，所以提供了track-by属性，对v-for指令的元素设置这个属性，可以提示vue在什么情况下可以复用已有的DOM元素。
使用 track-by 可以增加复用，提升数据变动时的渲染性能；
可以 track-by 数组中的唯一键值来指示当键值一致时复用使用域和 DOM 元素；
若数组没有唯一键值，可以使用 track-by="$index" 指示 v-for 指令进行原位更新模式，该模式也可用于处理数组中的重复值。
http://cn.vuejs.org/guide/list.html#track-by