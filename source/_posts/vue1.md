---
title: VUE (一) 概览
date: 2016-10-02 14:53:12
categories:
    - vue
---

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
