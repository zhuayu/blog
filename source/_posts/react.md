---
title: react
date: 2017-10-15 16:23:15
tags:
---

## 概述

React是由Facebook主导开发的JavaScript框架，和之前流行过的MVVM框架例如Angular不同，Reac只专注MVC中的V（视图层），优点在于轻、块、并且组件化的思想在构建开发用户界面时有很大的帮助。React规范化的代码风格，能减少每个开发者开发出来的代码风格的差异，在团队协作中益处显著。

前置知识

* JavaScript基础（变量／对象／函数）
* ES6 基础
* 熟悉 HTML
* 基本掌握 CSS

参考文档

* [react官网](https://facebook.github.io/react/docs/hello-world.html)
<!-- more -->

## 环境搭建

[node](https://nodejs.org/zh-cn/) 环境下载安装，如网速较慢可尝试 [yarn](https://yarnpkg.com/zh-Hans/) 或切换 [淘宝镜像](http://npm.taobao.org/)

```bash
npm install -g create-react-app
create-react-app my-app

cd my-app
npm start
```

## 组建开发

### JSX入门
JSX是基于JavaScript在React当中的一种语法扩展，不是单纯的字符串，不是模版语言，也不是HTML。被用于创建React当中的Element，通过React当中的方法渲染成DOM。

采用JSX及ES6语法的js代码需要转移才能在浏览器中运行，实际生产中会利用BABEL之类的转译器，转译成浏览器可以直接运行的js。JSX在转移过程中，会变成React.creatElement的一个方法，其中的标签语言都会被转化为参数传入 。

```javascript
//JSX
ReactDOM.render(
    <p>Hello world!</p>,
    document.getElementById('container')
)
//BABEL
React.createElement('p',null,`Hello world!`),
    document.getElementById('container')
)
```

用JSX创建的元素一般情况下都是不可变的，所以一般采用const关键字来声明一个JSX元素。（ const 关键字在ES6中被用来声明一个常量，但这里所谓的常量和其他编程语言中的概念稍有不同，它事实上是声明一个只读的指针。 

JSX中的标签可以携带属性，例如我们可以添加id，但是和HTML又不是完全相同，为标签使用class的时候需要添加className。JSX支持的所有属性可以在这里找到：[All Supported HTML Attributes](https://facebook.github.io/react/docs/dom-elements.html#all-supported-html-attributes)

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

const title = `<h1 id="appTitle" className="h1-title">React Learning</h1>`;
ReactDOM.render(title,document.getElementById('root');
```

JSX的标签可以相互嵌套，但嵌套时最外层有且只能有一个标签。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

const title = (
    <div>
        <h1 id="appTitle" className="h1-title">React Learning</h1>
        <p>lets learn JSX</p>
    </div>
);
ReactDOM.render(title,document.getElementById('root');
```

JSX创建的元素中可以使用js表达式，表达式需要用大括号{}括起来。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

function sayhi(name){
    return `Hi , ${name}`;
}

const title = (
    <div>
        <h1 id="appTitle" className="h1-title">React Learning</h1>
        <p>{sayhi('react')}</p>
    </div>
);
ReactDOM.render(title,document.getElementById('root');
```

小结：

* JSX是React当中实现的一种扩展语法
* JSX既不是字符串也不是HTML
* 通过JSX可以方便的在React中声明元素
* JSX可以通过React当中的方法被渲染成页面的DOM
* JSX的标签可以拥有属性，但与HTML不完全相同
* JSX的标签也可以像HTML一样相互嵌套，但最外层只能有一个标签
* 我们可以在JSX当中使用JS表达式

### 组件

#### 元素与组件 Element & Component

##### React元素
元素是构建React应用的描述界面的最小单位，在作用上，我们可以把React元素理解为DOM元素，但实际中，React元素只是js中普通的对象。在React内部有一套React DOM的机制，我们可以称之为Virtual DOM，通过js中树状结构的对象来模拟真实DOM。
React之所以轻和快就是因为这虚拟DOM的存在，React内部还实行了一个低复杂度高效率的Diff算法。不同于以往的框架如Angular使用的脏检查，在应用改版数据的时候，React会尽量进行少的比较，然后更具虚拟DOM只改变真实DOM当中需要被改变的部分。
通过这成单独抽象的逻辑，让React有了无限的可能，例如ReactNative、ReactVR、ReactXP的实现。

##### 组件
组件是可以讲UI切分称为一些独立的、可服用的部分，类似于js当中对function函数的定义。

#### 函数定义与类定义组件 Functional & Class
新版本的React里提供了两种定义组件的方法

* 通过JS的function来定义组件
* 通过ES6中的class来定义组件

函数定义组件需要定义一个接收props作为参数，返回react元素的方法即可。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

const Title = (props) => <h1>hello,{props.name}</h1>
ReactDOM.render(<Title name='react' />,
    document.getElementById('app')
);
```

类定义组件使用ES6中class类的方法来定义

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

class Title extends React.Component {
    render() {
        return <h1>hello,{props.name}</h1>
    }
}
ReactDOM.render(<Title name='react' />,
    document.getElementById('app')
);
```

#### 展示与容器组件 Presentational & Container

* 展示组件主要负责组件内容如何展示，数据来自props或者与数据无关的state，大多情况使用函数声明组件
* 容器组件主要关注数据如何交互，数据来自state和后端，多数使用class类声明组件

☹️ no well ，这类组件负担的功能太多了，他只是一个单一的组件，同时需要初始化state，通过ajax获取服务器数据，渲染列表内容。在实际引用中可能还会有更多的功能依赖，在后续无论要修改样式内容、服务器交互都需要修改同一个组件，逻辑严重的耦合。如果功能在同一个组件维护，也不利于团队间的协作。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'


class CommentList extends React.Component{
    contructor(props) {
        super(props)
        this.state = {
            comments:[]
        }
    }
    
    componentDidMount() {
        $.ajax({
            url:'my-contents.json',
            dataType:'json',
            sucess:function(comments){
                this.setState({
                    coments:coments
                })
            }.bind(this)
        })
    }
    
    renderComment({body,author}) {
        return <li>{body}-{author}</li>
    }
    
    render() {
        renturn <ul>{this.state.comments.map(this.renderComment)}</ul>
    }
    
}

```

😊 good , 区分展示组件和容器组件。回复列表如何展示、如何获取的逻辑就被分离到2个组件中。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'


//展示组件
class CommentList extends React.Component {
    contruct(props) {
        super(props)
    }
    
    renderComment({body,author}) {
        return <li>{body}-{author}</li>
    }
    
    render() {
        renturn <ul>{this.state.comments.map(this.renderComment)}</ul>
    }
}

//容器组件
class CommentListContainer extends React.Component {
    contructor() {
        super()
        this.state = {
            comments:[]
        }
    }
    
    componentDidMount() {
        $.ajax({
            url:'my-contents.json',
            dataType:'json',
            sucess:function(comments){
                this.setState({
                    coments:coments
                })
            }.bind(this)
        })
    }
    
    render() {
        return <CommentList comtents={this.state.comments} />
    }
}
```

#### 有状态与无状态组件 Stateful & Stateless
* 有状态组件可以获取、存储、改变引用本身的数据，伴有state的变化。
* 无状态组件只接收来自其他组件的传值，只对this.props的调用。
* 并不是所有的展示组件都是无状态组件，所有的容器组件都是有状态组件。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

//有状态组件
class StatefullLink extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            active:false
        }
    }
    
    handleClick() {
        this.setState({
            active: !this.state.active
        })
    }
    
    render() {
        return <a
            style={{ color: this.state.active ? 'red' : 'black' }}
            onClick={this.handleClick.bind(this)}
        >
            Stateful Link
        </a>
    }
}

// 无状态组件
class StatelessLink extends React.Component {
    contructor(props) {
        super(props)
    }
    
    handleClick() {
        this.props.handleClick(this.props.router)
    }
    
    render() {
        const active = this.props.activeRouter === this.props.router
        return (
            <li>
                <a
                    style={{ color : active ? 'red' : 'black' }}
                    onClick={this.handleClick.bind(this)}
                >
                    Stateless Link
                </a>
            </li>
        )
    }
}
```

事实上我们编写的组件大部分都是无状态组件

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

function SimpleButtonES5(props) {
    return <button>{props.text}</button>
}

const SimpleButtonES6 = props => <button{props.text}</button>
```

#### 受控与非受控组件 Controlled & Uncontrolled
一般涉及到表单元素时对组件类型的划分方法

* 一般情况下所有React表单元素都是受控组件，每个受控组件都需要对应的事件处理函数
* React通过特殊属性ref来获取非受控组件

受控组件的值一般由props或者static传入，用户在元素上交互或者输入内容会引起static的变化，在static改变之后重新渲染组件，我们才能在页面中看到值的变化。假如没有绑定事件处理函数，用户的输入是不会有效果的，这就是受控的含义所在。

```javascript
class ControlledInput extends React.Component {
    constructor() {
        super()
        this.state = {
            value:'Please type here...'
        }
    }
    
    handleChange(event){
        console.log('Controlled change:',event.target.value)
        this.setState({
            value:event.target.value
        })
    }
    
    render() {
        return (
            <label>
                Controlled Component:
                <input type="text" value={this.state.value} onChange={(e)=> this.handleChange(e)}/>
            </label>
        )
    }
}
```

非受控组件类似于传统的DOM表单控件，用户输入不会引起static的改变，我们也不会直接为非受控组件传入值。获取非受控组件的值我们需要使用特殊的ref属性，

```javascript
class UncontrolledInput extends React.Component {
    constructor() {
        super()
    }
    
    handleChange(event){
        console.log('Controlled change:',event.target.value)
    }
    
    render() {
        return (
            <label>
                Uncontrolled Component:
                <input type="text" defaultValue='Please type here...' ref={(input)=> this.input = input} onChange={(e)=> this.handleChange(e)}/>
            </label>
        )
    }
}
```

通常情况下，react当中所有的表单控件都需要是受控组件，我们需要为每一个受控组件编写事件处理函数。比如说注册，你需要写出所有姓名、电话、邮箱的逻辑。当然也有小技巧可以让同一个事件处理函数引用在多个表单组件中，但生产开发中并没有多大的意义，多是在重构的时候使用。还有一些别的库，需要和表单进行交互，这时候使用非受控组件就会方便一些。

#### 组合与继承 Composition & Inheritance
* 通过组合和props传值几乎可以满足所有的场景需求，同样也更符合组件化的理念，就好像相互嵌套的DOM元素一样。
* 继承的写法虽然生效，但是不符合React的理念，在React中，props可以传入任何东西：变量、函数、甚至是组件本身。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

class InheritedButton extends React.Component {
    contructor() {
        super()
        this.state = {
            color: 'red'
        }
    }
    
    render() {
        return (
            <button style={{backgroundColor: this.state.color}} >Inherited Button</button>
        )
    }
}

class BlueButton extends InheritedButton {
    contructor() {
        super()
        this.state = {
            color:'blue'
        }
    }
}

const CompositedButton = props => <button style={{backgroundColor:props.color}}Composited Button</button>
const YellowButton = () =>  <CompositedButton color="yellow" />

ReactDOM.render(
    <div>
        <BlueButton/>
        <br/>
        <YellowButton>
    </div>,
    document.getElementById('app')
)

```

如何想实现一些非洁面类型函数的服用，可以单独写在其他的模块当中，再引入组件进行使用。

### 组件数据

三种数据应用类型

* Props
* State
* Context

#### Props
props 就是属性prototype的缩写，在JSX中props就像HTML的attribute一样。在概念上props对于组件，就相当于js当中的参数对应函数一样。组件函数接收props作为参数，然后返回视图的内容。props比起原生的html属性强大许多，可以传入变量、函数、组件 ...


```javascript
//Function
Component(props) = View

//JSX
const SimpleButton = props => <button className={props.color}>Submit</button>
ReactDOM.render(<SimpleButton color="blue"/>,document.getElementById('app'))

//HTML
<button class="blue">Submit</button>
```

Props的值自上向下传递，从父组件传入到子组件当中，并且props是自读的，不能在组件中修改props的内容。 就是说组件只能通过传入的props值渲染界面，内部不能对props值进行修改。

```javascript
const Children = props => <p>{props.name}</p>
const Father = <Children name="Bob"/>
```

Props有类型检查和默认值

```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import PropTypes from 'prop-types'

const Title = props => <h1>{props.title}</h1>

Title.defaultProps = {
    title: 'Hello world!'
}

Title.propTypes = {
    title: PropTypes.string.isRequired
}

ReactDOM.render(<Title/>,document.getElementById('app'))
```

#### State
state翻译过来可以为状态，一个组件可以有自身的状态，也可以包含整个react应用的状态。例如：通过API从服务器获取数据，然后渲染到页面，就是我们用到state的时候。

* 初始化
* setState方法
* 向下传递数据
* State management 状态管理
* 服务器数据交互
* 用户输入交互
* 多个组件共用的State存放在其公有的父组件里

简单的计数器例子 ，通过class类定义组件来声明一个有状态组件，然后在构造方法中初始化状态的state并为它赋默认值。之后就能通过this.state来访问它，我们在添加一个负责处理state变化的函数addOne，通过setState的方法来改变state值。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

class Conter extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            counter:0
        }
    }
    
    addOne() {
        this.setState({
            counter:this.state.counter +1
        })
    }
    
    render() {
        return(
            <div>
                <p>{this.state.counter}</p>
                <button onClick={()=>this.addOne()}>Increment</button>
            </div>
        )
    }
}

ReactDOM.render(<Counter />,document.getElementById('app'))
```

复杂点的点赞例子 。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

const Title = props => <h1>{props.title}</h1>
const UserInfo = props => <p>{props.firstName + ' ' + props.lastName}</p>

class LikeButton extends React.Component {
    constructor(props) {
        super(props)
    }
    
    handleClick() {
        this.props.handleClick()
    }
    
    render() {
        return <button onClick{()=>this.handleClick()}>{this.props.mark}</button>
    }
}

class App extends React.Component {
    constructor(props){
        super(props)
        this.state = {
            mark:'☆'
        }
    }
    
    handleButtonClick(){
        const mark = (this.state.mark === '☆') ? '★' : '☆';
        this.setState({
            mark:mark
        })
    }
    
    render() {
        return (
            <div>
                <Title title="Hello , jikexueyuan"/>
                <UserInfo firseName="Jax" lastName="Chu" />
                <LikeButton mark={this.state.mark} handleClick{()=>this.handleButtonClick()}
            </div>
        )
    }
}

const Root = props => <div className="container">{props.children}</div>

ReactDOM.render(
    <Root>
        <App/>
    </Root>
    ,document.getElementById('app'))

```

假如State分散到应用的各个部分，有的复杂后端数据的交互，有的处理用户输入，有的负责页面变化的逻辑，后期维护是相当困难的。因此在开发React应用中，我们应尽量把State统一管理，通过props把state的数据传递到组件当中。货币汇率转换器 

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

class CurrencyConvertor extends React.Componnet {
    constructor(){
        super()
    }
    
    this.state = {
        money : 100
    }
    
    handleInputChange(event) {
        this.setState({
            money:event.target.value
        })
    }
    
    render() {
        return (
            <div>
                <h2>货币兑换</h2>
                <YuanInput money={this.state.money} handleChange={(e)=> this.handleInputChange(e)} />
                <MoneyCovertor type='美元' unit='dollar' money={this.state.money} rate={0.1453} />
                <MoneyCovertor type='日元' unit='yen' money={this.state.money} rate={16.1814} />
        )
    }
}

class YuanInput extends React.Component {
    constructor(props){
        super(props)
    }
    
    handleChange(event){
        this.props.handleChange(event)
    }
    
    render() {
        return (
            <p>
                <lable>
                人民币
                <input name='yuan' onChange={(e)=>this.handleChange(e)} value={this.props.money}
                </lable>
            </p>
        )
    }
}

class MoneyCovertor = props => (
    <p>
        <lable>
            {props.type}
            <input name={props.unit} value={(props.money * props.rate).toFixed(2)} disabled />
        </lable>
    </p>
)

ReactDOM.render(<CurrencyConvertor /> ,document.getElementById('app'))
```
#### context
集中管理state会引发另一个问题，state的数据都是自上而下传递的，假如我们在最外一层进行组件管理，而我们使用的子组件确在N层之下，那么我们就需要在每一层的组件都传递props下去。所以React提供 Context 带有实验性质，用于跨层级传递数据。必须进行类型检查。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'
import PropTypes from 'prop-types'

const UserInfo = (props,content) => (
    <div className="UserInfo-name">{context.lastName +','+ props.firstName}</div>
)
UserInfo.contextTypes = {
    lastName:PropTypes.string
}

const Column = props => <div className='column'><UserInfo firseName={props.firstName} /><div>
const Row    = props => <div className='row'><Columen firseName={props.firstName} /></div>
const Container = props => <div className='container'><Row firstName={props.firstName} /></div>

calss App extends React.component {
    constructor() {
        super()
        this.state = {
            fisetName: 'Chu',
        }
    }
    
    getChildContent() {
        return {
            lastName: 'Jax'
        }
    }
    
    render() {
        return (
            <div className="app">
                <Container first={this.state.firstName} />
            </div>
        )
    }
}

App.childContextTypes = {
    lastName: PropTypes.string
}

ReactDOM.render(<App/>,document.getElementById('root'))
```

### 组件生命周期

#### React 是如何渲染组件的？
一般来讲，我们都会先定义一个组件，我们想要在页面当中看到这个组件渲染的结果的话，需要用JSX的形式把组件传入到ReactDOM.render方法中作为第一个参数。JSX通过React转化其实是一个react.createElement的方法。render方法获取到react元素中会将它实例化，更具React实例的元素创建出真实DOM。再更具第二个参数的指向，讲创建好的元素插入到目标DOM容器当中。

再新版本的React当中，React的底层被重写，换上新的引擎，React Fiber。它将React更新应用界面分成了2个主要的部分，调度过程与执行过程。

在调度过程中，有四个生命周期会被触发：

* componentWillMount
* conponentWillReceiveProps
* shouldComponentUpdate
* componentWillUpdate

在执行过程中，有三个生命周期会被触发：

* componentDidMount
* componentDidUpdate
* componentWillUnmount

React为了方便我们控制自己的引用提供了许多预知的生命周期方法，就想以上的生命周期方法，会在组件挂载流程、更新流程、卸载流程中触发。React初次渲染时，属于挂载流程，后续更新时的都是更新流程，最后我们还可以通过ReactDOM.unmountComponentAtNode将组件卸载，其中进行的就是卸载流程。

举个栗子 。这里我们定义2个组件，父组件会传递state到子组件当中，顺便state的变化和props的传递流程我们也可以从这个 中看到。子组件每一个生命周期函数中，都在控制台输出相关的信息。

```html
<div id="root"></div>
<button id="render">渲染</button>
```

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'

class Number extends React.Component {
  constructor(props) {
    super(props)
    console.log('%cconstructor' + '%c 子组件已构造', 'font-weight:bold', 'color: blue')
  }

  componentWillMount() {
    console.group("%c React 挂载", 'color: #00d8ff')
    console.log('%ccomponentWillMount' + '%c 组件即将挂载', 'font-weight:bold', '')
  }

  componentDidMount() {
    console.log('%ccomponentDidMount' + '%c 组件已挂载', 'font-weight:bold', '')
    console.groupEnd();
    console.log('\n')
  }

  componentWillReceiveProps(newProps) {
    console.group("%c React 更新", 'color: green')
    console.log('%ccomponentWillReceiveProps' + '%c 组件即将接收props', 'font-weight:bold', '')
    console.log('newProps', newProps.counter)
    console.log('this.props', this.props.counter)
  }

  shouldComponentUpdate(newProps, newState) {
    const result = true
    console.info('%cshouldComponentUpdate' + '%c 返回判断是否要更新组件' + `%c ${result}`, 'font-weight:bold', 'color: #ff3c41', 'font-weight:bold;color: #236fd4')
    if (!result) console.groupEnd()
    return result;
  }

  componentWillUpdate(nextProps, nextState) {
    console.log('%ccomponentWillUpdate' + '%c 组件即将更新', 'font-weight:bold', '')
    console.log('nextProps', nextProps.counter)
    console.log('this.props', this.props.counter)
  }

  componentDidUpdate(prevProps, prevState) {
    console.log('%ccomponentDidUpdate' + '%c 组件已更新', 'font-weight:bold', '')
    console.log('prevProps', prevProps.counter)
    console.log('this.props', this.props.counter)
    console.groupEnd();
    console.log('\n')
  }

  componentWillUnmount() {
    console.group("%c React 卸载", 'color: brown')
    console.log('%ccomponentWillUnmount' + '%c 组件即将卸载', 'font-weight:bold', 'color: gray')
    console.groupEnd();
    console.log('\n')
  }

  render() {
    console.log('%crender' + '%c 组件渲染中...', 'font-weight:bold', '')
    console.log('this.props', this.props.counter)
    return <p>{ this.props.counter }</p>
  }
}

class Counter extends React.Component {
  constructor(props) {
    super(props)
    console.log('%cconstructor' + '%c 父组件已构造', 'font-weight:bold', 'color: #ae63e4')

  }

  componentWillMount() {
    this.state = {
      counter: 0
    }
    console.log('this.state', this.state.counter)
  }

  shouldComponentUpdate(newProps, newState) {
    const result = true
    console.info('%cshouldComponentUpdate' + '%c 返回判断是否要更新父组件' + `%c ${result}`, 'font-weight:bold', 'color: #ff3c41', 'font-weight:bold;color: #236fd4')
    return result;
  }

  addOne() {
    console.log('%caddOne()' + '%c 事件处理函数触发', 'font-weight:bold', '')
    console.log('prevState', this.state.counter)
    this.setState((prevState) => ({
      counter: prevState.counter + 1
    }))
  }

  unMount() {
    ReactDOM.unmountComponentAtNode(document.getElementById('root'));
  }

  update() {
    this.forceUpdate()
  }

  render() {
    console.log('%crender' + '%c 父组件渲染中...', 'font-weight:bold', '')
    console.log('nextState', this.state.counter)
    console.log('\n')
    return (
      <div>
        <Number counter={this.state.counter} />
        <button
          onClick={() => this.addOne()}>
          增加
        </button>
        <button
          onClick={() => this.update()}>
          强制更新
        </button>
        <button
          onClick={() => this.unMount()}>
          卸载
        </button>
      </div>
    )
  }
}

const render = () => ReactDOM.render(
  <Counter />,
  document.getElementById('root')
)
//render()

document.getElementById('render').addEventListener('click', render)
```

首先是组件初次渲染的挂载流程，我们通过一个按钮，绑定一个渲染函数来主动触发组件的渲染。初次挂载组件时，我们构建的组件类会被挂载声明，在渲染之前会触发``componentWillMount``，在渲染之后会触发``componentDidMount``这2个生命周期函数。

``componentWillMount``会在渲染之前被触发，并且只会被组件被渲染之前触发一次。如果使用ES6的class声明组件的话，时机和作用几乎和``constructor``相同。在``componentWillMount``对state的定义或者改变，是不会触发新的渲染的。

``componentDidMount``会在渲染完成之后被触发，同样自会触发一次。在这个方法中，已经可以访问到渲染到页面的DOM元素了，所以官方推荐在这个方法中可以进行一些类似于AJAX请求的操作，是我们最经常使用的生命周期函数。

父组件会向子组件更新props的数值，这时候会触发``componentWillReceiveProps``。组件在运行这一流程，props还没有被改变。然后就会就会触发``shouldComponentUpdate``，默认返回true，如果返回false的话，更新流程会被跳过，渲染也不会继续被触发。假如要提升应用的性能，可以在这个函数中进行自定义的判断来跳过不需要被更新的组件。组件在初次渲染时，和forceUpdate时，是不会触发这个函数的。

``componentWillUpdate``和``componentDidUpdate``会在更新渲染的前后触发。在``componentWillUpdate``里无法调用this.setState，可以理解为更新流程到这一步想要更改state已经晚了，如果有需要可以在``componentWillReceiveProps``中更新state，React会把改变合并到同一个更新流程里面执行。而``componentDidUpdate``是另一个适合我们发起AJAX请求的地方，在这里我们还可以比较前后的props变化，再决定是否发起网络请求。比较实用的场景就是保存用户的输入到服务器，用户可能会来来回回修改内容，如果我们判断修改前后数据最终没有被改变，那就没有必要发起不必要的网络请求了。

通过``ReactDOM.unmountComponentAtNode``来卸载已经挂载了的React组件。在卸载流程的``componentWillUnmount``是我们解绑事件最合适的位置。

```bash
>> constructor 父组件以及构造
>> this.state 0
>> render 父组件渲染中...
>> nextState 0

>>子组件已构造
>>componentWillMount 组件即将挂载
>>render 组件渲染中
>>this.props 0
>>componentDidMount 组件已挂载 

>>addOne() 事件处理函数触发
>>prevState 0
>>shouldComponentUpdate 返回判断是否要更新父组件 true
>>render 父组件渲染中...
>>nextState 1

>>componentWillReceiveProps 组件即将接受Props
>>nextProps 1
>>this.props 0
>>shouldComponentUpdate 返回判断是否要更新组件 true
>>>>componentWillUpdate 组件即将更新
>>nextProps 1
>>this.props 0
>>render 组件渲染中
>>this.props 1
>>componentDidUpdate 组件已更新
>>preProps 0
>>this.props 1

>> componentWillUnmount 组件即将卸载
```

[React异步获取文章小 ](https://codepen.io/discountry/pen/wJNpop)

通过const的关键字声明，使用arrowFunction 定义一个简单的 RedditList 展示组件。一般组件包含多层嵌套的JSX的时候，都会加上一个小括号，因为浏览器在某些情况下可能会对换行的代码加上分号。 JSX当中时可以使用js表达式的，一般在渲染列表中都会拿到一个数组数据，通过map的方法渲染出列表中的每一项。JSX在渲染列表项的时候，每一项都必须带有key属性，作为识别列表中某一项的标识，React在渲染列表的时候会通过key来判断每一个的内容。

使用类定义的方法来定义一个 RedditFetch 容器组件。首先把posts初始化为数组，定义一个从服务器获取数据的方法fetchFromApi，在请求成功之后重新设置state，获取到帖子的数据。在componentDidMount中调取fetchFromApi，在componentWillUnmount中解除向服务器发起的请求。

```javascript
const RedditList = props => (
    <div>
        <h1>{`/r/${props.subreddit}`}</h1>
        <ul>
            {props.posts.map(post =>
                <li key={post.id}>
                    <a href={post.url}>{post.title}</a>  
                </li>
            )}
        </ul>
    </div>
)

class RedditFetch extends React.Component {
    constructor(props) {
        super(props)
            this.state = {
                posts: []
        }
    }
  
    fetchFromApi() {
        this.serverRequest = axios.get(`https://www.reddit.com/r/${this.props.subreddit}.json`)
            .then(res => {
                const posts = res.data.data.children.map(obj => obj.data)
                this.setState({
                    posts
            })
        })
    }
  
    componentDidMount() {
        this.fetchFromApi()
    }
  
    componentWillUnmount() {
        this.serverRequest.abort()
    }
  
    render() {
        return <RedditList subreddit={this.props.subreddit} posts={this.state.posts} />
    }
}

ReactDOM.render(<RedditFetch subreddit="reactjs" />,document.getElementById('root'))

```

小结

* React组件渲染包括三个流程：挂载流程、更新流程、卸载流程。
* 各个生命周期函数会在特定的时候触发并适用于不同的使用场景。
* 通过使用生命周期函数我们可以对应用进行更精准的控制。
* 如果你需要发起网络请求，将其安排在适合的生命函数周期中。

### 表单及事件处理

JSX中使用的和HTML同名的元素，并不等同于原生的HTML标签，只是React抽象出来的一种标签的写法。举个例子 

```javascript
<textarea value={this.state.value} onChange={this.handleChange}/>

<select value={this.state.value} onChange={this.handleChange}>
    <option value="grapefruit">Grapefruit</option>
    <option value="lime">Lime</option>
    <option value="coconut" >Coconut</option>
</select>
```
```html
<textarea >
    Hello , React !
</textarea>

<select>
    <option value="grapefruit">Grapefruit</option>
    <option value="lime">Lime</option>
    <option value="coconut" select >Coconut</option>
</select>
```

事件也是React内部封装的，由小驼峰命名，并不等同于HTML DOM的原生事件，可以通过以下地址[link](https://facebook.github.io/react/docs/events.html)查看。

## Redux 数据处理

### Redux简介
React技术栈的主要适用情景是Web应用，Web应用涉及非常多的用户交互和状态数据改变。我们应该尽量控制应用中有状态组件的个数，应用数据应该尽量集中统一管理，否则后期维护成本较高。之前介绍有状态组件与无状态组件，React也推荐我们控制有状态组件的个数，在我们开发过程中编写的90%都应该是无状态组件。React本身提供给我们控制引用数据的方式只有props、state 及带有实验性质的context。React本身并没有提供应用状态管理的解决方案，在小的应用中可能感受不到状态管理是一个问题，但随着应用复杂度的增加，这个问题就会暴露得越来越明显，这个时候就可以考虑使用Redux。

既然React推荐我们尽量少的编写有状态组件，为何不干脆把一个应用所有的状态数据集中到一个地方管理呢？这便是Redux的理念。Redux把应用的数据统一储存在一个对象当中，把应用的所有的数据交互及引用状态的改变统一用固定形式的action动作对象来描述，并由reducer的方法来判断不同的动作如何改变引用状态，最后通过store对象来执行action动作获取state应用状态，并触发改变应用状态的事件。

实际上Redux提供的是一种应用状态的解决思路，我们可以用基本的原生JS方法来实现Redux的全部功能。并且我们就算引入Redux编写的大部分的是原生的JS函数和对象。

举个例子 ，原生JS模拟实现Redux。
首先定义一个counter的函数，接受state和action参数。state就是状态数据，而action是一个带有type的js对象。通过对type的判断，返回新的state，这样的函数在redux中被称为redcer。通过counter返回的默认值作为容器的初始state值，为组件添加increment、decrement方法，事实上都是调用dispatch方法。在dispatch方法中调用counter函数，将先用的应用状态和action方法传递

```javascript
import React, { Component } from 'react';

// Reducer
const counter = (state = { value: 0 }, action) => {
    switch (action.type) {
        case 'INCREMENT':
            return { value: state.value + 1 };
        case 'DECREMENT':
            return { value: state.value - 1 };
        default:
            return state;
    }
}

class Counter extends Component {
    // State
    constructor(props) {
        super(props)
        this.state = counter(undefined,{})
    }
  
    dispatch(action) {
        this.setState(prevState => counter(prevState, action));
    }
    
    // Action
    increment = () => {
        this.dispatch({ type: 'INCREMENT' });
    };

    decrement = () => {
        this.dispatch({ type: 'DECREMENT' });
    };
  
    render() {
        return (
            <div>
                {this.state.value}
                <button onClick={this.increment}>+</button>
                <button onClick={this.decrement}>-</button>
            </div>
        )
    }
}

```

最后：

* 不要为了“用框架”而用框架
* 真正遇到需要这些工具来解决问题时再去实际应用
* [Redux官网](http://cn.redux.js.org/)
* 你可能不需要用到Redux


### Action
Action 是带有type属性的js对象。``{ type: 'INCREMENT' }``在Redux的应用操作中，我们要把所有改变应用状态的操作，规范为一个个Action。在Action当中，我们也可以赋上要用来修改应用状态state的具体数据``{ type: 'INCREMENT', num: 1 }``。上述我们可以看到两点Action的作用：

* 定义我们的应用可以进行操作的动作或者操作的类型
* 传递改变引用状态的数据

Action的定义要遵从一定的标准：[Flux 标准 Action](https://github.com/acdlite/flux-standard-action)

```javascript
{
    // Action 类型
    type: 'INCREMENT',
    // payload 中返回我们要传递的数据，用来修改state
    paload: {
        num: 1
    },
    // payload 数据未获取成功时返回true
    error:false,
    // 一些不必要在 paload 中传递的其他数据
    meta: {
        success: true
    }
}
```

为了防止我们代码中出现很多写死的数值，减少hard code，可以使用Redux当中action creator的方法。

```javascript
const counterActionGenerator = (type, num) => (num) => {
    let action = { type , num }
    return action
}

const addNumber = counterActionGenerator('INCREMENT', null )
const minsNumber = counterActionGenerator('DECREMENT', null )
```

为了清晰的表达我们所有action动作，可以在应用的开头，或者一个独立的文件中定义所有的action type。

```javascript
const INCREMENT = 'INCREMENT'
const DECREMENT = 'DECREMENT'
```

* 定义action类型的文件类似于一个说明文档，当你想要为应用添加新特性时，可以查阅应用已有的action类型，避免冲突。
* 在版本管理上也可以清晰看到action的记录的增删改查。
* 如果发现错误，在import就会发现代码报错，更快的进行调试。

### Reducer
在之前函数中定义了 counter 函数，它的内部时一个switch结构，接收两个参数，state 和 action ，并返回一个新的state 。

```javascript
// Reducer
const counter = (state = { value: 0 }, action) => {
    switch (action.type) {
        case 'INCREMENT':
            return { value: state.value + 1 };
        case 'DECREMENT':
            return { value: state.value - 1 };
        default:
            return state;
    }
}
```
我们将上述的这个函数抽象为这个公式，更具业务现有的状态，更具触发的action ，返回新的状态的函数称之为 Reducer。

```javascript
(previousState, action ) => newState
```

之所以将这样的函数称之为reducer，是因为这种函数与被传入 [Array.prototype.reduce(reducer, ?initialValue)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)里的回调函数属于相同的类型。Reducer可以勉强翻译为缩减器或折叠器。

reduce属于一个高阶函数，它将其中的回调函数reducer递归应用到数组中的所有元素上，并返回一个独立的值，这就是缩减折叠的意义所在。

```javascript
var sum = [0, 1, 2, 3].reduce(function(acc, val) {
    return acc + val
}, 0)

/* 注意这里当中的回调函数(prev,curr) => prev + curr
*  与我们redux当中的reducer模型 (previousState, action ) => newState 看起来是不是很相似？
*/
[0, 1, 2, 3, 4].reduce( (prev, curr) => prev + curr )

const todos = (state = [], action ) => {
    switch (action.type) {
        case 'ADD_TODO':
            return [
            //ES7对象扩展运算符写法
            ...state,
            {
                id:action:id,
                text:action.text,
                completed:false
            }
            ]
        // 不知道action类型的话返回默认state
        default:
            return state
    }
}
```

reducer的主体，是一个switch结构的运算,他只是更具传入的state和action，来判断返回一个新的state。它必须是一个纯函数，不可以修改影响输入值（需要深拷贝），并且没有副作用（异步调用、影响作用域...)。永远不要在 reducer 里做这些操作：

* 修改传入参数；
* 执行有副作用的操作，如 API 请求和路由跳转；
* 调用非纯函数，如 Date.now() 或 Math.random()。

React应用当中数据发生改变，界面也要跟随相应改变，重新渲染，这这一步之间需要一个比较差异的diff操作，因此改变前后的数据都要被使用到。另外，实现一些诸如旅行的功能也需要依赖不可变数据。

### Store
Store是我们存储状态数据state的地方，我们通过redux中createStore的方法，传入reducer函数来创建store。

* getState 获取当前状态数据
* dispath 执行action
* subscribe 订阅当前状态数据发生改变时触发的事件

原生模拟createStore 

```javascript
const createStore = (reducer) => {
    let state;
    let listeners = [];
    // 返回当前的state
    const getState = () => state;
    // 根据action调用reducer返回新的state并触发listener
    const dispatch = (action) => {
        state = reducer(state,action);
        listeners.forEach(listeners => listener())
    }
    
    /* 这里的subscribe有两个功能
    * 调用 subscribe(listener)会使用listeners.push(listener)注册一个listener
    * 调用 subscribe 的返回函数则会注销掉listener
    */
    const subscribe = (listener) => {
        listeners.push(listerer);
        return () => {
            listeners = listerers.filter( 1 => 1 !== listener);
        }
    }
    
    // 返回三个主要方法
    return { getState, dispatch, subscribe }
}
```

### Middleware

Middleware可以翻译为中间件，例如网站中某些访问限制的验证功能模块就可以当作一个middleware。Redux也可以应用middleware的概念实现一些例如日志记录调试输出等功能。

例如我们想知道数据前后发生的变化，最直接的办法 。

```javascript
console.log('prev state',store.getState())
store.dispatch(action)
console.log('next state',store.getState())
```

手动修改dispath的方法 

```javascript
const store = createStore(counter)
// 将原本的dispatch方法保留并附上控制台语句
let next = store.dispatch
store.dispatch = function dispatchAndLog(action) {
    console.log('prev state',store.getState())
    let result = next(action)
    console.log('next state',store.getState())
    return result
}
```
直接修改dispath原生方法是比较脏的处理，并且有时候我们会使用多个middleware，所以我们需要把它与dispatch的逻辑拆分开来。在实际中可以再redux中引入applyMiddleware 方法 

```javascript
const { createStore , applyMiddleware } = Redux;
const loggerMiddleware = store => next => action => {
    console.log('dispatching', action)
    let result = next(action)
    console.log('next state', store.getState())
    return result
}
const store = createStore(counter,
    applyMiddleware(loggerMiddleware))
```

### react-redux

react 和 redux 的使用 

在之前使用reat时，我们把数据放到state，通过setState可以触发react的更新渲染，而目前状态数据都交由redux管理。redux数据状态发生改变时，相应的是我们通过subscribe方法绑定的listener。所以我们需要绑定一个比较合适的监听函数作为状态变化时的相应，react提供了主动触发react更新渲染的方法``forceUpdate``。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'
import { createStore } from 'redux'

const INCREMENT = 'INCREMENT'
const DECREMENT = 'DECREMENT'

const counter = (state = 0, action) => {
  switch (action.type) {
    case INCREMENT:
      return state + 1;
    case DECREMENT:
      return state - 1;
    default:
      return state;
  }
}

const store = createStore(counter)

//展示组件
const Counter = ({ 
  count, onIncrement, onDecrement }) => (
  <div>
    <h1>{count}</h1>
    <button onClick={onIncrement}>+</button>
    <button onClick={onDecrement}>-</button>
  </div>
)

// 容器组件
class CounterContainer extends React.Component {
    componentDidMount() {
        this.unsubscribe = store.subscribe( () = > {
            this.forceUpdate()
        })
    }
    
    componentWillUnmount() {
        this.unsubscribe()
    }
    
    render(){
        return <Counter 
            value={ store.getState() } 
            onIncrement = { () => store.dispatch({type: INCREMENT}) } 
            onDecrement = { () => store.dispatch({type: DECREMENT}) } 
        />
    }
    
}

ReactDOM.render(<CounterContainer />,document.getElementById('root'))
```

React是构建用户界面的框架，而Redux是为了解决状态管理问题专门开发的库，两者适合搭配但使用不是必须。React官方专门提供了连接react和redux的库，react-redux。react-redux主要提供两个方法

* provider 是向整个应用传递store容器组件
* connect 方法可以更具用户展示组件自动生成对应容器组件

provider就像我们传递store用的容器组件，一般都会套在应用组件的最外层。而connect方法可以根据我们现有的展示组件，自动生成容器组件。connect需要传递2个参数，mapStateToProps，用来对应state和props，并且传入参数时，在我们redux发生改变时，就会自动触发组件的更新。mapDispatchToProps，通过dispatch来传递对应的方法来触发action事件。connect在获取了redux的store之后，再更具我们传入的方法把我们需要的部分对应到props属性当中，再传递到我们的组件里。

```javascript
import React from 'reat'
import ReactDOM from 'react-dom'
import { createStore } from 'redux'
import { Provider ,connect } = 'react-redux'
import PropTypes from 'prop-types'


const INCREMENT = 'INCREMENT'
const DECREMENT = 'DECREMENT'

const counter = (state = 0, action) => {
  switch (action.type) {
    case INCREMENT:
      return state + 1;
    case DECREMENT:
      return state - 1;
    default:
      return state;
  }
}

const store = createStore(counter)

//展示组件
const Counter = ({ 
  count, onIncrement, onDecrement }) => (
  <div>
    <h1>{count}</h1>
    <button onClick={onIncrement}>+</button>
    <button onClick={onDecrement}>-</button>
  </div>
)

Counter.propTypes = {
    count: PropTypes.number.isRequired,
    onIncrement: PropTypes.func.isRequired,
    onDecrement: PropTypes.func.isRequired,
}

const = mapStateToProps = (state) => (
    {
        count:state
    }
)

const = mapDispatchToProps = (dispatch) => {
    onIncrement: () => {
        dispatch({type: INCREMENT})
    },
    onDecrement: () => {
        dispatch({type: DECREMENT})
    }
}

// 容器组件
const CounterContainer = connect(mapStateToProps, mapDispatchToProps)(Counter)

ReactDOM.render(
    <Provider store={store}>
        <CounterContainer />
    </Provider>
    ,document.getElementById('root'))
```
react-redux实现了很多优化，不会像ReactDOM.render和 foreUpdate一样耗费效率，正式开发中建议使用react-redux。

## react-router 4

### 简介
在web开发中，前端扮演着越来越重要的角色。路由功能可以由前端来实现，无需后端返回页面，通过前端路由实现URL的改变以及对页面的切换。较早之前实现前端路由的方法，在Angular1中时通过hash(#)来处理，也就是带#号的锚点。前端路由多种多样，我们还可以通过操作DOM的[histroy](https://developer.mozilla.org/zh-CN/docs/Web/API/History)对象，使用``history.pushState``,``window.onpopstate``这2个方法来实现。

前端路由主要实现2个部分的功能：

* 改变当前URL地址
* 更具URL地址对当前页面进行切换

```html
<ul>
    <li><a href="/home">Home</a></li>
    <li><a href="/blog">Blog</a></li>
    <li><a href="/about">About</a></li>
</ul>
<h2>History state:</h2>
<p id="state"></p>
```
```javascript
// 修改 history pushState
(function(history){
    var pushState = history.pushState;
    history.pushState = function(state) {
        if (typeof history.onpushstate == "function") {
            history.onpushstate({state: state});
        }
        return pushState.apply(history, arguments);
    }
})(window.history);
// 添加触发事件
window.onpopstate = history.onpushstate = function(event) {
  document.getElementById('state').innerHTML = "location: " + document.location + ", state: " + JSON.stringify(event.state);
}
// 为链接绑定事件处理函数，阻止页面跳转，触发pushState
var links = document.getElementsByTagName('a');
for(var i = 0; i < links.length; i++) {
    links[i].onclick = function (event) {
        event.preventDefault();
        var route = event.target.getAttribute('href');
        history.pushState({page: route}, route, route);
        console.log('current state', history.state)
    }
}
```

react-router为我们提供预制的，可以实现功能的组件，使用方法和react组件保持一致，只需要我们在应用的时候加上一些JSX的标签。react-router目前已出了4个版本，而且不同版本直接差异也比较大。目前4.0版本中包含三个主要的库：

* react-router-dom(web)
* react-router-native(rn)
* react-router(core)

react-router-dom和react-router-native相当于在react-router之上又封装了一层。让我们来关注并且手动配置的部分更少了，一般如果开发web应用的话，react-router-dom就足够了。

我们从react-router-dom中引入三个组件，BroswerRouter、Router、Link，这个组件都是react组件。首先把BrowswerRouter放到最外层，这是给我们应用添加路由功能的容器组件，之后在我们觉得合适的地方添加link组件，link组件的to属性就是对应路由的地址，最后使用Router来添加正式的路由。每个Router的path属性也是对应路由的地址，点击对应的link，就会跳转到对应的路由中。Router的component属性，就是路由跳转过来显示的组件。

```javascript
const { BrowserRouter, Route, Link } = ReactRouterDOM

const BasicExample = () => (
    <BrowserRouter>
        <div>
            <ul>
                <li><Link exact to="/">Home</Link></li>
                <li><Link exact to="/about">About</Link></li>
                <li><Link to="/topics">Topics</Link></li>
            </ul>

            <Route exact path="/" component={Home}/>
            <Route exact path="/about" component={About}/>
            <Route path="/topics" component={Topics}/>
        </div>
    </BrowserRouter>
)

const Home = () => (
    <div><h2>Home</h2></div>
)

const About = () => (
    <div><h2>About</h2></div>
)

const Topics = ({ match }) => (
    <div><h2>Topics</h2></div>
)

ReactDOM.render(<BasicExample />, document.getElementById('root'))
```

### 配置

#### BrowserRouter
主要实现页面功能与URL同步的功能，新版本的BrowserRouter内置对浏览器histroy的支持，不需要我们做任何的配置就可以正常使用，并且没有#号，也不会生成随机字符串。

```javascript
import { BrowserRouter } from 'react-router-dom'

const Root = () => (
    <BrowserRouter>
        <App/>
    </BrowserRouter>
)

const App = () => (
    <div><h1>Hello, react-route !</h1></div>
)

ReactDOM.render(<Root/>,document.getElementById('root'))
```

#### Route

Route是路由组件，一般接受2个参数，path 和 component，用来制定路由的URL已经所要渲染的组件。exact 属性，添加后只会匹配到path的根地址。

```javascript
const BasicExample = () => (
    <BrowserRouter>
        <div>
            <ul>
                <li><Link exact to="/">Home</Link></li>
                <li><Link exact to="/about">About</Link></li>
                <li><Link to="/topics">Topics</Link></li>
            </ul>

            <Route exact path="/" component={Home}/>
            <Route exact path="/about" component={About}/>
            <Route path="/topics" component={Topics}/>
        </div>
    </BrowserRouter>
)

```

#### Link

Link是最基本的导航连接组件，我们只需要使用to参数，指向我们要使用的路由即可。在我们使用当中，会经常为我们的导航使用不同的样式，这就需要使用的NavLink，并加上导航使用的激活属性activeStyle。组件接收match对象包含以下几项属性：

* params 预设url中传递的参数
* isExact 当前url是否绝对匹配此路由
* path 路由设定的path值
* url 当前url地址

```javascript
import { BrowserRouter } from 'react-router-dom'

const Root = () => (
    <BrowserRouter>
        <App/>
    </BrowserRouter>
)

const App = () => (
    <div>
        <ul>
            <li><NavLink activeStyle={{fontWeight:'Bold'}}  to="/repo/react>Repo:react</NavLink></li>
            <li><NavLink activeStyle={{fontWeight:'Bold'}}  to="/repo/vue>Repo:vue</NavLink></li>
            <li><NavLink activeStyle={{fontWeight:'Bold'}}  to="/repo/angular>Repo:angular</NavLink></li>
        </ul>
        
        <Route path='/repo/:repoName?' component={Repo} />
    </div>
)

const Repo = ({match}) => (
    <div><p>{match.params.repoName}</p></div>
)

ReactDOM.render(<Root/>,document.getElementById('root'))

```

除了通过组件的参数获取外，还可以通过withRouter方法来获取

```javascript
const Title = ({match}) => (
    <h1>{match.params.repo}</h1>
)
Title = withRouter(Title)
```

## CSS

### css样式编写的方法

* css in js [(css名称)](https://developer.mozilla.org/en-US/docs/web/css/css_properties_reference)
* 外部引用css，然后为组件定义对应的className属性。

css in js,是由facebook提出的在由React开发的背景下。css in js，允许我们在变量当中定义样式，通过定义内联样式直接将每个组件的样式写在每个组件的js当中。通过利用js语言本身的特性，不必担心样式的作用域或者是复用性之类的问题。在传统的web开发认为应该尽量使用外联样式，与页面结构分离。但组件化的思想中，同一个组件应该在内部定义逻辑／结构／样式。分而治之，进近维护，独立文件，外部引用。


#### Modules/Sass/PostCSS 在React项目中的应用

#### Modules
create-react-app已经把我们把所有配置都封装号，如果我们希望改变的话，就需要获取配置文件，在项目中``run eject``,在目录下生成config文件，webpack.config.dev.js和webpack.config.prod.js。

我们用命令行创建的react项目当中，默认已经包含了使用CSS Modules的所有依赖，需要我们修改配置才能生效。

```javascript
{
    loader: require.resolve('css-loader'),
    options: {
        importLoaders: 1,
        modules:true,
        localIdentName: '[path][name]__[local]--[hash:base64:5]'
    },
}
```

然后就可以通过import来引入样式了，就可以在className对应的类名下使用了。

```javascript
import styles from './App.css'
```
#### sass
如果像使用sass，就要添加对应的sass加载器。

```bash
npm install sass-loader node-sass --save-dev
```

```javascript
{
    test:/\.scss$/,
    include:paths.appSrc,
    loaders:[
        require.resolve('style-loader'),
        require.resolve('css-loader'),
        require.resolve('sass-loader'),
    ]
}
```

#### postcss
是一个加载css插件的平台，比起一些预先定制好的加载器，postcss更加的灵活。

```bash
npm install autoprefixer postcss-initial postcss-import postcss-mixins postcss-nested postcss-simple-vars postcss-math postcss-color-function --save-dev
配置 postcss.config.js
新建src文件夹，配置 variables.js 和 mixins.js
在 webpack.config 中引入 postcss 配置
```

**webpack.config**

```javascript
{
    loader: require.resolve('postcss-loader'),
    options: {
        config: {
            path:'./config/postcss.config.js'
        }
    },
}
```

**postcss.config.js**

```javascript
module.exports = {
  plugins: {
    /* autoprefix for different browser vendors */
    'autoprefixer': {},
    /* reset inherited rules */
    'postcss-initial':{
      reset: 'inherited' // reset only inherited rules
    },
    /* enable css @imports like Sass/Less */
    'postcss-import': {},
    /* enable mixins like Sass/Less */
    'postcss-mixins':{
      mixins: require('./src/mixins')
    },
    /* enable nested css selectors like Sass/Less */
    'postcss-nested':{},
    /* require global variables */
    'postcss-simple-vars':{
      variables: function variables() {
        return require('./src/variables')
      },
      unknown: function unknown(node, name, result) {
        node.warn(result, 'Unknown variable ' + name)
      }
    },
    /* PostCSS plugin for making calculations with math.js  */
    'postcss-math':{},
    /* transform W3C CSS color function to more compatible CSS. */
    'postcss-color-function':{}
  }
}
```

**mixins.js**

```javascript
// src/mixins.js
var globalMixins = {
  /* noSelect is a static mixin  */
  noSelect: {
    '-webkit-touch-callout': 'none',
    '-webkit-user-select': 'none',
    '-khtml-user-select': 'none',
    '-moz-user-select': 'none',
    '-ms-user-select': 'none',
    'user-select': 'none'
  },
  /* OpenSans is a dynamic mixin  */
  OpenSans: function (obj, value) {
    return {
      'font-family': 'Open Sans, sans-serif',
      'font-style': 'normal',
      'font-size': value,
      'font-weight': 200,
      '-webkit-font-smoothing': 'antialiased',
      '-moz-osx-font-smoothing': 'grayscale'
    }
  }
}
module.exports = globalMixins
```

variables.js

```javascript
var globalVariable = {
  primary: 'blue'
}
module.exports = globalVariable
```

**App.css**

```css
.App-intro{
    color:$primary;
    @mixin noSelect;
    @mixin OpenSans 30px;
}
```

## 实战TodoList

### 开发前准备

环境

* [node](https://nodejs.org/zh-cn/)
* [create-react-app](https://github.com/facebookincubator/create-react-app)

初始化项目

```bash
cd Desktop 
create-react-app todo
```

删除src目录上的所有文件

```bash
cd src  && rm -rf *
```

创建Components 及 Containers 文件夹

```bash
mkdir Components
mkdir Containers
```

在Components上创建 TodoApp.js

```bash
cd Components && touch TodoApp.js
vim TodoApp.js
```

**TodoApp.js**

```javascript
import React, { Component } from 'react';

class TodoApp extends Component {
    render() {
        return (
            <div>Hello, React!</div>
        );
    }
}

export default TodoApp;
```

在src目录下创建index.js并引用TodoApp.js

```bash
cd .. && touch index.js
vim index.js
```

**index.js**

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import TodoApp from './Components/TodoApp'

ReactDOM.render(<TodoApp />, document.getElementById('root'));
```

### 组件开发

在📁Component中创建AddTodo.js，把标题和输入框都放在其中。

**AddTodo.js**

```javascript
import React from 'react';

const AddTodo = () => (
    <header>
        <h1>Todos</h1>
        <input placeholder="接下来做什么？" autoFocus />
    </header>
);

export default AddTodo;
```

在📁Component中创建TodoList.js，列表组件。涉及到React当中的列表渲染，我们把列表中的每一个元素设为Todo.js组件，然后TodoList接收props，使用map方法渲染出每一项Todo。

**TodoList.js**

```javascript
import React from 'react';
import Todo from './Todo';

const TodoList = ({ todos }) => (
    <ul>
        {todos.map((todo) => (
            <Todo key={todo.id} content={todo.content} />
        ))}
    </ul>
);

export default TodoList;
```

**Todo.js**

```javascript
import React from 'react';

const Todo = ({ content }) => (
    <li>
        <div>
            <input type="checkbox" />
            <label href="/#">{content}</label>
        </div>
    </li> 
);

export default Todo;
```

在📁Component中创建Footer.js，导航组件。包含全部、代办、已完成，并且把当中的每一项封装为Link组件。由于这三项的内容都是固定的，所以我们直接重复引用三次Link组件即可。

**Footer.js**

```javascript
import React from 'react';
import Link from './Link';

const Footer = () => (
    <footer>
        <ul>
            <Link filter="all">全部</Link>
            <Link filter="active">待办</Link>
            <Link filter="completed">已完成</Link>
        </ul>
    </footer>
);

export default Footer;
```

**Link.js**

```javascript
import React from 'react';

const Link = ({ children }) => (
    <li>
        <a href="/#">{children}</a>
    </li>
);

export default Link;
```

在src目录下创建store.js，用来模拟临时数据

**store.js**

```javascript
const todos = [
    { 
        id: 1,
        content: "学习 React"
    },
    { 
        id: 2,
        content: "学习 Redux"
    },
    { 
        id: 3,
        content: "学习 react-router"
    },
];

export default todos;
```

修改应用主文件TodoApp.js，引入编写的三个模块，以及store数据。把store数据传入TodoList中。

**TodoApp.js**

```javascript
import React    from 'react';

import AddTodo  from './AddTodo';
import TodoList from './TodoList';
import Footer   from './Footer';
import todos    from '../store.js';


const TodoApp = () => (
    <section>
        <AddTodo/>
        <TodoList todos={todos}/>
        <Footer/>
    </section>
);

export default TodoApp;
```

### 样式
借用 [todomvc-app-css](https://github.com/tastejs/todomvc-app-css) 来进行样式填充，然后对应的添加className。

> 此处我们仅仅是为了页面的美观，如果对样式没有要求，只关注功能，可以跳过。

通过npm安装

```bash
npm install todomvc-app-css --save
```

然后更具 [todomvc-html](https://github.com/tastejs/todomvc-app-template/blob/master/index.html)里为我们的Todo应用对应添加上className。

**TodoApp.js**

```javascript
const TodoApp = () => (
    <section className="todoapp" >
        <AddTodo/>
        <TodoList todos={todos}/>
        <Footer/>
    </section>
);
```

**AddTodo.js**

```javascript
const AddTodo = () => (
    <header className="header">
        <h1>Todos</h1>
        <input className="new-todo" placeholder="接下来做什么？" autoFocus />
    </header>
);
```

**TodoList.js**

```javascript
const TodoList = ({ todos }) => (
    <ul className="todo-list">
        {todos.map((todo) => (
            <Todo key={todo.id} content={todo.content} />
        ))}
    </ul>
);
```

**Todo.js**

```javascript
const Todo = ({ content }) => (
    <li>
        <div className="view">
            <input className="toggle" type="checkbox" />
            <label href="/#">{content}</label>
        </div>
    </li> 
);
```

**Foot.js**

```javascript
const Footer = () => (
    <footer className="footer">
        <ul className="filters">
            <Link filter="all">
                全部
            </Link>
            <Link filter="active">
                待办
            </Link>
            <Link filter="completed">
                已完成
            </Link>
        </ul>
    </footer>
);
```

在TodoApp.js中引入todomvc-app-css文件

**TodoApp.js**

```javascript
import 'todomvc-app-css/index.css';
```

### 数据处理

首先为项目安置redux及react-redux，新建action.js、reducer.js、store.js

```bash
npm install redux react-redux --save
```

**action.js**

action是一个具有type属性的js对象。我们为其添加一个addTodo功能，更具用户的文本输入，添加一条待办事项。由于待办事项是在react列表中渲染，所以每一项内容都需要一个数据作为key值，定义为todoId 处始为 4（默认有3条）。toggleTodo方法为标记待办事项，我们必须要传递进id即可。

```javascript
let todoId = 4;

export const addTodo = (content) => ({
    type: 'ADD_TODO',
    content,
    id: todoId++
});

export const toggleTodo = (id) => ({
    type: 'TOGGLE_TODO',
    id
});
```

**reducer.js**

reducer是一个switch结构的函数，接收state和action作为参数，判断不同的action.type的类型，返回一个新的state。当action.type === 'ADD_TODO'的时候，我们为用户新增一条代办事项，用展开操作符号的方法(...state)，可以保证我们返回的是一个新的state数据。当action === 'TOGGLE_TODO' 时，当某一个待办事项切换时，将它切换为已完成或者未完成的状态，然后返回。

```javascript
const todo = (state, action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        id: action.id,
        content: action.content,
        completed: false
      };
    case 'TOGGLE_TODO':
      if (state.id !== action.id) {
        return state;
      };

      return {
        ...state,
        completed: !state.completed
      };
    default:
      return state;
  }
};

export const todos = (state = [], action) => {
    switch (action.type) {
        case 'ADD_TODO':
            return [
                ...state,
                todo(undefined, action)
            ];
        case 'TOGGLE_TODO':
            return state.map(t => {
                return todo(t, action)
            })
        default:
            return state;
    }
};
```
**store**

引用redux调用createStore生成stroe。

```javascript
import { todos } from './reducer';
import { createStore } from 'redux';

const defaultTodos = [
    { 
        id: 1,
        content: "学习 React"
    },
    { 
        id: 2,
        content: "学习 Redux"
    },
    { 
        id: 3,
        content: "学习 react-router"
    },
];

const store = createStore(todos, defaultTodos);

export default store;
```

为了区分逻辑，新建Root.js。使用react-redux把react和redux连接在一起。

Root.js

```javascript
import React from 'react';
import { Provider } from 'react-redux';
import TodoApp from './TodoApp';
import store from '../store';

const Root = () => {
    return (
       <Provider store={store}>
           <TodoApp />
        </Provider>
    );
};

export default Root;
```

之前编写的组件都是展示组件，只负责页面的结构，要获取用户数据需要使用的容器组件。在Containers中创建一个VisibleTodoList.js，作为TodoList的容器组件。除了引入TodoList之外，还有引入react-redux中的connect方法，connect的作用就是将store当中的数据和方法，映射到我们组件的props当中。connect方法接收2个函数，mapStateToProps和mapDispatchToProps。分别用来处理store当中的state数据和dispatch方法。这里我们使用简单的mapStateToProps方法，将state数据映射到todos当中。

**VisibleTodoList.js**

```javascript
import { connect } from 'react-redux';
import TodoList from '../Components/TodoList';
import { toggleTodo } from '../action';

const mapStateToProps = (state, ownProps) => ({
    todos: state
});

const mapDispatchToProps = (dispatch) => ({
    onTodoClick: (id) => {
        dispatch(toggleTodo(id));
    }
});

const VisibleTodoList = connect(
    mapStateToProps,
    mapDispatchToProps
)(TodoList);

export default VisibleTodoList;
```

通过**todoList**为每个todo组件传入onClick方法

```javascript
import React from 'react';
import Todo from './Todo';

const TodoList = ({ todos, onTodoClick }) => (
    <ul className="todo-list">
        {todos.map((todo) => (
            <Todo key={todo.id} onClick={() => onTodoClick(todo.id)} {...todo} content={todo.content} />
        ))}
    </ul>
);

export default TodoList;
```

然后在**todo**组件中，把事件绑定在checkbox上。我们也需要界面的改变来观察数据的切换，因此我们把checkbox的选中状态和completed的属性绑定在一起，同时为todo组件加上css效果。

```javascript
import React from 'react';

const Todo = ({ content, onClick, completed }) => (
    <li style={{
        textDecoration:
            completed ?
            'line-through' :
            'none',
        color:
            completed ?
            '#d9d9d9' :
            '#4d4d4d'
        }
    }>
        <div className="view">
            <input onChange={onClick} className="toggle" type="checkbox" checked={completed ? 'checked' : ''} />
            <label href="/#">{content}</label>
        </div>
    </li> 
);

export default Todo;
```


为AddTodo添加待办事项的功能，把AddTodo移动到Containers当中，引入react-redux中的connect方法。如果我们不为connect传递任何参数，connect会把state和dispatch传递到我们的组件当中，所以我们能从props中获取dispatch方法。为input绑定事件处理函数，当我们按下回车的时候，触发dispatch方法传入ADD_TODO动作（从action中引入ADD_TODO方法）。我们使用非受控组件的方法来获取input的输入值，所以需要在组件中添加ref的属性来获取到input元素，把它暂存到我们声明的input变量当中。

**AddTodo.js**

```javascript
import React from 'react';
import { connect } from 'react-redux';
import { addTodo } from '../action';

let AddTodo = ({dispatch}) => {
    let input
    return (
        <header className="header">
            <h1>Todos</h1>
            <input onKeyDown={(e) => {
                    if (input.value && e.keyCode === 13) {
                        dispatch(addTodo(input.value))
                        input.value = ''
                    }
                    }}
                    ref={node => input = node}
            className="new-todo" placeholder="接下来做什么？" autoFocus />
        </header>
    )
};

AddTodo = connect()(AddTodo)

export default AddTodo;
```


修改入口主文件**TodoApp.js**

```javascript
import React    from 'react';

import AddTodo  from '../Containers/AddTodo';
import VisibleTodoList from '../Containers/VisibleTodoList';
import Footer   from './Footer';
import todos    from '../store.js';

import 'todomvc-app-css/index.css';


const TodoApp = () => (
    <section className="todoapp">
        <AddTodo/>
        <VisibleTodoList/>
        <Footer/>
    </section>
);

export default TodoApp;
```

修改渲染文件index.js，让其渲染最外层的**Root.js**

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import Root from './Components/Root';

ReactDOM.render(<Root />, document.getElementById('root'));
```

### 路由配置
react 实现路由需要用到前端路由的技术，这里我们使用react-router4.0。我们需要用到导航的信息，来显示何种待办事项，所以我们以路由参数来设置路径，显示所有待办事项在根路径。

```bash
npm install react-router react-router-dom --save
```

在**Root.js**中引入react-router，需要用到的组件有 BrowserRouter 和 Route。然后放置在Provider组件之内。

```javascript
import React from 'react';
import { Provider } from 'react-redux';
import {
    BrowserRouter as Router,
    Route
} from 'react-router-dom';
import TodoApp from './TodoApp';
import store from '../store';

const Root = () => {
    return (
       <Provider store={store}>
           <Router>
               <Route path='/:filter?' component={TodoApp}/>
           </Router>
        </Provider>
    );
};

export default Root;
```

在底部导航**Link.js**组件中，我们需要把底部导航加上NavLink组件，并把filter参数传递到to属性当中。

```javascript
import React from 'react';
import { NavLink } from 'react-router-dom';

const Link = ({ filter, children }) => (
    <li>
        <NavLink to={"/" + filter}>{children}</NavLink>
    </li>
);

export default Link;
```

在src创建**Selector.js**，selector结构和reducer很像，但不会改变应用更改的数据状态，更具传入的参数返回state的部分内容。selector接收2个参数，state和filter。同样也是switc的结构，更具不同的filter类型处理返回新的state数据。

```javascript
const getVisibleTodos = (state, filter) => {
    switch(filter) {
        case 'all':
            return state;
        case 'active':
            return state.filter(t => !t.completed);
        case 'completed':
            return state.filter(t => t.completed);
        default:
            return state;
    }
};

export default getVisibleTodos;
```

在**VisibleTodoList.js**中，我们需要使用到路由的参数，并且需要通过路由的信息，重新切换展示出待办事项。所以我们要使用withRouter来生成新的容器组件。reater-router完全兼容redux，只需要在redux的connect外面套上即可。引入我们已经编写号的selector，修改mapStateToProps,将todo设置为被selector设置过滤后的值。这里我们需要使用到保存在match对象中的路由参数，并设置默认值为all。

```javascript
import { connect } from 'react-redux';
import { withRouter } from 'react-router';
import TodoList from '../Components/TodoList';
import { toggleTodo } from '../action';
import getVisibleTodos from '../selector';

const mapStateToProps = (state, {match}) => ({
    todos: getVisibleTodos(
        state,
        match.params.filter || "all"
    )
});

const mapDispatchToProps = (dispatch) => ({
    onTodoClick: (id) => {
        dispatch(toggleTodo(id));
    }
});

const VisibleTodoList = withRouter(connect(
    mapStateToProps,
    mapDispatchToProps
)(TodoList));

export default VisibleTodoList;
```

小结：

我们建立了2个文件夹，Components和Containers，展示组件负责界面的结构样式，容器组件负责处理数据交互，这样可以更好的理清我们业务的逻辑，也方便开发维护。接下来我们使用redux来进行应用的状态管理，我们使用三个独立的文件来处理应用状态数据 action.js 、reducer.js、store.js。action对应这我们页面的每一个功能，这里我们主要实现了添加事项和标记事项是否完成的状态的功能，reducer中一个处理数组中的数据和另一个负责处理单独的todo对象，通过createStore的方法生成store。使用react-router实现前端路由的功能，最常使用的组件BrowserRouter、Route、Link。为了切换显示不同状态的效果，我们还使用到了路由参数，通过withRouter的方法传递到目标组件的方法当中，通过match对象里获取到具体的路由参数。拿到路由参数后，通过redux的selector方法来有选择性的展示数据。









