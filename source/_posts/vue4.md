---
title: VUE （四）列表渲染
date: 2016-10-05 21:09:23
categories:
    - vue
---

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

<!-- more -->

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