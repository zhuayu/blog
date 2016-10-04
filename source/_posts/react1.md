---
title: react (一)概览
date: 2016-09-02 14:53:12
categories:
    - react
---

## 简介

fackbook   这是一个视图层的JS库。

虚拟DOM、组件化、可复用，多平台

React 在全局定义一个唯一的数据模型，模型中只要数据有任何变化，都将整个数据模型应用到UI界面的DOM树上，react会负责更新需要更新的的界面部分。

虚拟DOM，是在浏览器端用 JS 实现了一套 DOM API。
基于 React 的所有 DOM 构造都是通过虚拟 DOM 进行的。每当数据变化时，React 会重 新构建整个 DOM 树，并将当前的树和上一次的 DOM 树进行对比，得到 DOM 结构的区别。然后仅仅将需要变化的部分进行实际的浏览器 DOM 更新。且，React 能够批处理虚拟 DOM 的刷新，在一个事件循环内的两次数据变化会被合并。

组件，状态机器。
React 讲用户界面看成多个组件的状态机器，当组件处于某个状态是，就输出这个状态所对应的界面。只需要简单更新某个组件的状态，即可基于新状态输出整个页面，然后React 对比前后2个界面的不同，局部更新需要变化的DOM树。

多平台，react Native。
可以打包成 android 及iOS 应用。
<!-- more -->

### 通俗地说：
使用JS模版的虚拟DOM，通过状态来进行数据的局部更新。
组件有自己独立的功能和样式，一次写完，重复调用，大量现成的UI组件库，前端可以更加专注于布局和逻辑。

### 使用场景：
要在webapp、android、iOS多个平台发布。
某个功能，要在多个地方或多个页面复用。
数据复杂且不断变化的页面的大型应用。


## 配置
建议在React 中使用 CommonJS 模块系统，通过 npm下载依赖包，使用 webpack 打包。

### package.json
```
{
  "name": "react",
  "version": "1.0.0",
  "description": "react test",
  "main": "webpack.config.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "react"
  ],
  "author": "jax",
  "license": "ISC",
  "dependencies": {
    "react": "^15.3.0",
    "react-dom": "^15.3.0",
  },
  "devDependencies": {
    "babel-core": "^6.13.2",
    "babel-loader": "^6.2.4",
    "babel-preset-es2015": "^6.13.2",
    "babel-preset-react": "^6.11.1",
    "babel-preset-stage-0": "^6.5.0",

    "css-loader": "^0.23.1",
    "file-loader": "^0.9.0",
    "style-loader": "^0.13.1",
    "url-loader": "^0.5.7",

    "webpack": "^1.13.1"
  }
}
```

- react
- react-dom

     2015.10.7日react v0.14版本中，为了使React能在更多的不同环境下更快、更容易构建。于是把react分成了react和react-dom两个部分。这样就为web版的react和移动端的React Native共享组件铺平了道路。也就是说我们可以跨平台使用相同的react组件。
     新的 react 包含了React.createElement，.createClass，.Component，.PropTypes，.children以及其他元素和组件类。
而 react-dom 包括ReactDOM.render，.unmountComponentAtNode和.findDOMNode。在 react-dom/server，有ReactDOMServer.renderToString和.renderToStaticMarkup服务器端渲染支持。
     总的来说，两者的区别就是：ReactDom是React的一部分。ReactDOM是React和DOM之间的粘合剂，一般用来定义单一的组件，或者结合ReactDOM.findDOMNode（）来使用。更重要的是ReactDOM包已经允许开发者删除React包添加的非必要的代码，并将其移动到一个更合适的存储库。

- babel-core                    ——调用babel的API转码
- babel-loader                 ——调用babel和webpack来翻译语法
- babel-preset-es2015    ——ES6转码规则
- babel-preset-react        ——react转码规则
- babel-preset-stage-0    ——ES6+不同阶段语法提案的转码规则（ 0～4 四个阶段 ）

     安装babel的loader让其支持ES6等更高语法，ES6+的React语法有些超出了ES6规范，比如ES7 property initializers 和 static 关键字。

- css-loader                     ——处理CSS文件中的 url( ) 表达式
- style-loader                   ——将CSS代码插入页面的 style 标签中
- file-loader                      ——引入文件资源，如图像、字体、样式
- url-loader                      ——对file-loader的封装，常用语抽取url中的图片资源转base64

- webpack                       ——打包构建工具

### webpack.config.js
```
var path = require('path');
var webpack = require("webpack");
var uglifyJsPlugin = webpack.optimize.UglifyJsPlugin;

module.exports = {
  entry: path.resolve(__dirname, 'app/app.js'),
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'bundle.js'
  },
  plugins: [
      new webpack.BannerPlugin("极客前端\n jax.zhu"),
      new uglifyJsPlugin({
          compress: {
              warnings: false
          }
      })
  ],
  module:{
    loaders:[{
        test: /\.jsx?$/,
        exclude: /node_modules/,
        loader: "babel",
        query:{
            presets:['es2015','react']
        }
    },{
      test:/\.(png|jpg|gif)$/,
      loader:'url-loader?limit=8192'
    },{
      test: /\.css$/,
      loader: 'style!css'
    }]
  }
}
```

- entry                         ——入口文件
- output                       ——出口文件
- plugins                      ——插件
- loaders                      ——模块和资源转换器

## 生命周期

- Mounted        实例化    ReactCompoments 被 JS render解析生成DOM节点，并被插入浏览器的DOM结构
    - getDefaultProps
    - getInitalState
    - componentWillMount
    - render
    - componentDidMount
- Update          存在期    一个Mounted的ReactCompoments被重新render的过程
    - componentWillReceiveProps
    - shouldComponentUpdate
    - componentWillUpdate
    - render
    - componentDidUpdate
- Unmounted   清理期    一个Mounted的ReactCompoments对应的DOM节点，被从DOM结构中移除
    - componentWillUnmount

## 数据流
在React中，数据的葱父节点到子节点单向流动。子节点通过获取props渲染父节点传来的数据，如果顶部组件某个props发生改变，React会递归向下遍历整颗树，重新渲染整个组件。
组件内部也有自己的状态，state，这个状态只能在组件内修改。
```
var React = require('react');
var ReactDOM = require('react-dom');

var Timer = React.createClass({
    propTypes:{
      propsData:React.PropTypes.string
    },
    getDefaultProps:function(){
      return {
        propsData:'morenName'
      }
    },
    getInitialState: function() {
      return {
        stateData: 0
      };
    },
    tick: function() {
      this.setState({
        stateData: this.state.stateData + 1
      });
    },
    componentDidMount: function() {
      this.interval = setInterval(this.tick, 1000);
    },
    componentWillUnmount: function() {
      clearInterval(this.interval);
    },
    render: function() {
      return (
        <div>{this.props.propsData}: {this.state.stateData}</div>
    );
  }
});

ReactDOM.render(<Timer propsData="jax" />, document.getElementById('app'));
```

- propTypes     定义配置对象，用于验证获取的props是否匹配。
- getDefaultProps    定义默认props值
- getInitialState    定义默认state值
- 避免在组件内部修改this.props，把props作为只读，用来做事件处理，与子组件通讯。
- 使用state存储简单的视图状态，比如下拉框是否可见。
- 实用this.setState来设置状态，而不要使用this.state直接修改状态

## 事件绑定
React 通过事件处理器绑定到组件上处理事件。在事件被触发的同时，更新组件的内部状态。组件内部状态的更新会触发组件重绘。
React 处理事件的本质上和原生JavaScript事件一样，所以的事件在命名上与原生JavaScript规范一致，并且会在相同的情景触发。
React 绑定事件处理器的语法和HTML语法非常相似，但底层并没有使用HTML的属性，只是用这种写法来绑定事件处理器，其内部则按照需要高效滴维护着事件处理器。
```
var React = require('react');
var ReactDOM = require('react-dom');

var TodoList = React.createClass({
  render: function() {
    var createItem = function(item) {
      return <li key={item.id}>{item.text}</li>;
    };
    return <ul>{this.props.items.map(createItem)}</ul>;
  }
});
var TodoApp = React.createClass({
  getInitialState: function() {
    return {items: [], text: ''};
  },
  onChange: function(e) {
    this.setState({text: e.target.value});
  },
  handleSubmit: function(e) {
    e.preventDefault();
    var nextItems = this.state.items.concat([{text: this.state.text, id: Date.now()}]);
    var nextText = '';
    this.setState({items: nextItems, text: nextText});
  },
  handleClick:function(){
    console.log('点击了一下')
  },
  render: function() {
    return (
      <div>
        <h3 onClick={this.handleClick}>TODO</h3>
        <TodoList items={this.state.items} />
        <form onSubmit={this.handleSubmit}>
          <input onChange={this.onChange} value={this.state.text} />
          <button>{'Add #' + (this.state.items.length + 1)}</button>
        </form>
      </div>
    );
  }
});

ReactDOM.render(<TodoApp />, document.getElementById('app'));
```

参考：
React Doc：https://facebook.github.io/react/docs/getting-started.html
React package ：https://facebook.github.io/react/blog/2015/10/07/react-v0.14.html
React ES6+ ：https://babeljs.io/blog/2015/06/07/react-on-es6-plus