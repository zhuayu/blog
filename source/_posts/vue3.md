---
title: VUE （三）条件渲染
date: 2016-10-05 20:16:59
categories:
    - vue
---

Vue.js 条件渲染的作用是在不同条件下显示或隐藏某些元素，可以使用 v-if 和 v-show 指令实现不同的条件渲染。
<!-- more -->
这两个指令在作用、编译方式及使用方法上都各有不同，此外，还有一个 v-else 指令可以分别和 v-if、v-show 配合使用。
v-if为true就在DOM中显示，为false则在DOM中移除。
v-show为true在DOM中display为block，为false在DOM中display为none。

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