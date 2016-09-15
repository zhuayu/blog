---
title: 浏览器的JavaScript引擎
date: 2016-09-15 11:34:11
tags:
    - 浏览器的JavaScript引擎
---

## 浏览器的组成
浏览器的核心是两个部分：渲染引擎和JavaScript解释器（又称JavaScript引擎）。
<!-- more -->
### 渲染引擎
渲染引擎的作用是，将页面从代码渲染为视觉上可以感知的平面文档。不同浏览器有不同的渲染引擎。
- Firefox：Gecko引擎
- Safari：WebKit引擎
- Chrome：Blink引擎

渲染引擎处理网页，通常分为四个阶段。

- 解析代码：HTML代码解析为DOM，CSS代码解析为CSSOM（CSS Object Model）
- 对象合成：将DOM和CSSOM合成一颗渲染树（render tree）
- 布局：计算出渲染树的布局（layout）
- 绘制：将渲染树绘制到屏幕

以上四步并非严格按顺序执行，往往第一步没有完成，第二步和第三步就已经开始了。所以，会看到这种情况：网页的HTML代码还没下载完，但浏览器已经显示出内容了。

### JavaScript引擎
JavaScript引擎的主要作用是，读取页面中的JavaScript代码，对其处理后运行。

## JavaScript代码嵌入网页的方法
### 通过 script 标签直接添加代码块
<script>
// some JavaScript code
</script>

script 标签有一个type 属性，用来指定脚本类型。

- text/javascript :默认值。老式浏览器推荐。
- application/javascript : 较新浏览器推荐。

### 外部加载脚本
通过 script 标签的src属性置顶外部文件路径，如果使用了非英文字符，还应该注明编码。外部加载脚本和直接添加代码块不能混用，直接添加的代码块会被忽略。
```
<script src='example.js'></script>
<script charset='utf-8' src='中文.js'></script>
<script src='example.js'>
     console.log('test');
     // 会被忽略，不会运行
</script>
```

### 行内代码
HTML语言运行在某些事件属性和a元素的href属性中，直接写入JavaScript。( 不建议使用）
```
<div onclick=“alert(‘hello')"></div>
<a href=“javascript:alert(‘hello')"></a>
```

## script标签的工作原理
正常的网页加载流程

- 浏览器一边下载HTML网页，一边开始解析
- 解析过程中，发现script标签
- 暂停解析，网页渲染控制权交给javascript引擎
- 如果script标签应用了外部脚本，就下载该脚本，否则直接执行
- 执行完毕，控制权交还渲染引擎，恢复往下解析HTML网页

也就是说，加载外部脚本时，浏览器会暂停页面渲染，等脚本下载并且执行完成后，再继续渲染。原因是javascript可以修改DOM，所以必须把控制权让给它，否则会导致复杂的线程竞赛的问题。

如果外部脚本加载时间比较长（比如一直无法完成下载），就会照成网页长时间失去响应，浏览器就会呈现假死状态，这被称为“阻塞效应”。

为了避免这种情况，较好的做法是将script标签都放在页面底部，而不是头部。这样即使遇到脚本失去响应，页面主体的渲染也已经完成了，用户至少可以看到内容，而不是面对一张空白的页面。

如果某些脚本代码非常重要，一定要放在页面头部的话，最好直接将代码嵌入页面，而不是连接外部脚本文件，这样能缩短加载时间。

在DOM结构生成之前就调用DOM的话javascript会报错，如果脚本在尾部加载，就不会存在这个问题。

如果多个script标签引入外部脚本，浏览器会同时平行下载，但是执行时会保证从上而下，即使后者先下载完成。这是为了保证脚本之间的依赖不受破坏。

解析和执行CSS，也会产生阻塞。Firefox会等到脚本前面所有样式表，都下载并解析完，再执行脚本；Webkit则是一旦发现脚本引用了央视，就会暂停执行脚本，等到样式表下载并解析完，再恢复执行。

此外，对于来自同一个域名的资源，不如脚本文件、样式表文件、图片文件等，浏览器一半最多同时下载六个（IE11允许同时下载13个）。如果是来自不同域名的资源，就没有这个限制。所以，通常把静态资源放在不同的域名之下，以加快下载速度。

## defer和async属性
一般来说如果脚本之间没有依赖关系，就是用async，如果脚本之间有依赖关系就使用defer。如果同时使用async和defer属性，后者不起作用，浏览器行为由async属性决定。

### defer属性
defer属性告诉浏览器，等DOM完成加载后，再执行脚本，而且保证执行顺序。对于内置而不是外部的script标签，以及动态生成的script标签，defer属性不起作用。

### async属性
async属性作用时，使用另一个进程下载脚本，下载时不会阻塞渲染，浏览器继续渲染。哪个脚本下载先结束，就先执行哪个脚本。

## 重流和重绘
渲染树转换为网页布局，称为“布局流”（flow），布局显示到页面的整个过程，称为“绘制”（paint）。它们都有阻塞效应，并且会耗费很多时间和计算资源。

页面生成以后，脚本操作和样式表操作，都会触发重流（reflow）和重绘（repaint）。用户的交互，也会触发，比如设置鼠标悬停（a：hover）效果、页面滚动、输入框中输入文本、改变窗口大小等等。

重流和重绘并不一定一起发生，重流必然导致重绘，重绘不一定需要重流。比如改变元素颜色，只会导致重绘，而不会导致重流。改变元素布局，则会导致重绘和重流。

大多数情况下，浏览器绘智能判断，将重流和重绘只限制在相关的子树上面，最小化所耗费的代价，而不会全局重新生成页面。

作为开发者，应该尽量设法降低重绘的次数和成本。比如，尽量不要变动高层的DOM元素，而低层DOM元素的变动代替；再比如，重绘table布局和flex布局，开销都会比较大。

### 举个例子：
```
var foo = document.getElementById('foobar');
foo.style.color = 'blue';
foo.style.marginTop= '30px';
```

上面代码只会导致一次重绘，因为浏览器会累积DOM变动，然后一次执行。
下面代码则会导致2次重绘。
```
var foo = document.getElementById('foobar');
foo.style.color = 'blue';
var margin = parseInt(foo.style.marginTop);
foo.style.marginTop= （margin + 10) + 'px';
```

### 优化建议：
- 读取DOM或者写入DOM，尽量写在一起，不要混杂
- 缓存DOM信息
- 不要一项一项地改变样式，而是使用CSS class 一次性改变样式
- 使用document fragment操作DOM
- 动画时使用absolute定位或者fixed定位，这样可以减少对其他元素的影响
- 只要必要时才显示元素
- 使用window.requestAnimationFrame() ，因为它可以把代码推迟到下次重流时执行，而不是理科要求页面重流。
- 使用虚拟DOM（virtual DOM）库

### 再举个例子：
```
／／ 重绘代价高
function doubleHeight(element){
     var currentHeight = element.clientHeight;
     element.style.height = (currentHeight * 2 ) + 'px';
}

／／ 重绘代价低
function doubleHeight(element){
     var currentHeight = element.clientHeight;
     window.requestAnimationFrame(function(){
          element.style.height = (currentHeight * 2 ) + 'px';
     });
}
```

## 脚本动态嵌入
除了用静态的script标签，还可以动态嵌入script标签
```
['1.js’,'2.js'].forEach(function(src){
     var script = document.createElement('script’);
     script.src = src;
     doucment.head.appendChild(script);
}
```
这种方法的好处是，动态生成的script标签不会阻塞页面渲染，也不会造成浏览器假死。问题在于这种方法无法保证脚本的执行顺序，哪个脚本先下载完成，就先执行哪个。
如果要想避免这个问题，可以设置async属性为false。
```
['1.js’,'2.js'].forEach(function(src){
     var script = document.createElement('script’);
     script.src = src;
     script.async = false;
     doucment.head.appendChild(script);
}
```
上面代码依然不会阻塞页面，而且可以保证执行顺序。不过要注意的是，这段代码后面加载的文件，会因此都等待2.js执行完后再执行。

我们可以把上面的方法，封装成一个函数。
```
(function() {

    var scripts = document.getElementsByTagName('script')[0];
    function load(url) {
        var script = document.createElement('script');
        script.async = true;
        script.src = url;
        scripts.parentNode.insertBefore(script, scripts);
    }
    load('//apis.google.com/js/plusone.js');
    load('//platform.twitter.com/widgets.js');
    load('//s.thirdpartywidget.com/widget.js');

}());
```
上述代码中，async属性为true，是因为加载的脚本没有相互依赖关系。而且，这样就不会发生阻塞。

此外，动态嵌入还有一个地方需要注意。动态嵌入必须等待CSS文件加载完成以后，才会去下载外部脚本文件。静态加载就不存在这个问题，script标签指定的外部脚本文件，都是与CSS文件同时并发下载的。

## 加载使用协议
如果不指定协议，浏览器默认采用HTTP协议下载。
```
<script scr="example.js"></script>
```
如果采用HTTPS协议下载，必需写明（假定服务器支持）。
```
<script scr=“https://example.js"></script>
```
如果我们希望，更具页面本身的协议来决定加载协议，可以采用下面双斜杠写法。
```
<script scr=“//example.js"></script>
```

## JavaScript虚拟机
JavaScript是一种解释型语言，也就是说，他不需要编译，可以由解析器实时运行。这样的好处是运行和修改都比较方便，刷新页面就可以重新解释。缺点是每次运行都要调用解析器，系统开销较大，运行速度慢于编译型语言。为了提高速度，目前的浏览器都将JavaScript进行一定程度的编译，生成类似字码（bytecode）的中间代码，以提高运行速度。

早期，浏览器内部对JavaScript的处理过程如下：

- 读取代码，进行词法分析（Lexical analysis），将代码分解成词元（token）。
- 对词元进行语法分析（parsing），将代码整理成“语法树”（syntax tree）。
- 使用翻译器（translator），将代码转为字节码（bytecode）。
- 使用字节码解析器（bytecode interpreter），将字节码转为机器码。

逐行解析将字节码转为机器码，是很低效的。为了提高运行速度，现代浏览器改为采用“即时编译”（Just In Time compiler ，JIT），即字节码只在运行时编译，用到哪一行就编译到哪一行，并且八编译结果缓存。通常，一个程序经常用到的，知识其中一小部分代码，有了缓存的编译结果，整个程序的运行速度就会显著提升。

不同的浏览器呦不同的编译策略。有的浏览器只编译最经常用到的部分，比如循环的部分；有的浏览器索性省略了字节码的翻译步骤，直接编译成机器码，比如chrome的V8引擎。

字节码不能直接运行，而是运行在一个虚拟机上，一般也把虚拟机成为JavaScript引擎。因为JS运行时未必有字节码，所以JavaScript虚拟机并不完全基于字节码，而是部分基于源码，即制药有可能，就通过JIT编译器直接把源码编译成机器码运行，省略字节码的步骤。这一点与其它采用虚拟机（比如Java）的语言不同。这样做的目的，是为了尽可能地优化代码、提高性能。

- IE：CHakra
- Safari：Nitro／JavaScript Core
- Opera：Carakan
- Firefox：SpiderMonkey
- Chrome：V8
- Chromium：V8

## 单线程模型
含义：
JavaScript只能在一个线程上运行，不代表JavaScript引擎只有一个线程。
事实上，JavaScript引擎有多个线程，其中单个脚本只能在一个线程上运行，其他线程都是在后台配合。
JavaScript脚本在一个线程里运行，这意味着，一次只能运行一个任务，其他任务都必须在后面排队等待。

JavaScript之所以采用单线程，而不是多线程，跟历史有关。JavaScript从诞生九十单线程，原因是不想让浏览器变得太复杂，因为多线程需要共享资源、并且有可能修改彼此的运行结果，对于一个页面脚本来说，这太复杂。比如，假定JavaScript同时有2个线程，一个线程在某个DOM节点上添加内容，另一个线程删除这个节点，这时浏览器不知道以哪个为准，所以为了避免复杂性，这已经成为这门语言的核心特征。

为了利用多核CPU的计算能力，HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程，但是子线程完全受主线控制，而且不得操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质。

线程模型带来了一些问题，主要是新的任务被加在队列的尾部，只有前面的所有任务运行结束，才会轮到它执行。如果一个任务特别耗时，后面的任务都会停在那里等待，照成浏览器失去响应，又称为假死。

如果派对是因为计算量大，CPU忙不过来，还好。但是很多时候CPU是闲着的，因为IO设备很慢，不得不等着结果出来，再往下执行。JavaScript语言的设计者意识到，这CPU完全可以不管IO设备，挂起处于等待中的任务，先运行排在后面的任务。等到IO设备返回了结果，再回过头，把挂起的任务继续执行下去。这种机制就是JavaScript内部采用的Event Loop。

## 消息队列
JavaScript 运行时，出了一根运行线程，系统还提供一个消息队列，里面十各种需要当前程序处理的消息。新的消息进入队列的时候，会自动排在队列的尾端。

运行线程之遥发现消息队列不为空，就会取出排在第一位的那个消息，执行它对应的回调函数。等到执行完了，再取出排在第二位的消息，不断循环，知道消息队列变为空为止。

每条消息与一个回调函数相联系，也就是说，程序只要收到这条消息，就会执行对应的函数。另一个方面，进入消息列队的消息，必须有对应的回调函数。否则这个消息就会遗失，不会进入消息队列。举个例子，鼠标点击会产生一条消息，报告click事件发生了。如果没有回调函数，这个消息就遗失了。如果有回调函数，这个消息就进入消息队列。等到程序收到这个消息，就会执行click事件的回调函数。

另一种情况是setTimeout会制定时间向消息队列添加一条消息。如果消息队列之中，此时没有其他消息，这条消息会立即得到处理。否则，这条消息会不得不等到其他消息处理完，才会得到处理。否则，这条消息不得不等到其他消息处理完，才会得到处理。因此，setTimeout指定的执行时间，只是一个最早可能发生的时间，并不能保证一定会在那个时间发生。

## Event Loop
所谓的Event Loop，指的是一种内部循环，用来一轮又一轮地处理消息队列之中的消息，即执行对应的回调函数。
Wikipedia定义：Event Loop是一个程序结构，用于等待和发送消息和事件。
可以把Event Loop理解成动态更新的消息队列本身。

常见的JavaScript认为：

- 执行JavaScript代码
- 对用户的输入做出反应
- 处理异步的网络请求

所以任务可以分成两种，一种是同步任务，另一种是异步任务。
同步任务：在JavaScript执行进程上排队执行任务，只有前一个任务执行完毕，才能执行后一个任务。
异步任务：不进入JavaScript执行进程、而进入任务队列的任务，只有任务队列通知主进程，某个异步任务可以执行了，该任务才回进入JavaScript进程执行。

以AJAX操作为例，它可以当作同步任务处理，也可以当作异步任务处理，有开发者决定。如果是同步任务，主线程就等着ajax操作返回的结果，再往下执行。如果是异步任务，改任务直接进入任务队列，JavaScript进程跳过ajax操作，直接往下执行，等到ajax操作有了结果，JavaScript进程再执行对应的回调函数。

也就是说，虽然JavaScript只有一根进程用来执行，但是并行的还有其他进程（比如，处理定时器的进程、处理用户输入的进程、处理网络通信的进程等等）。这些进程通过向任务队列添加任务，实现与JavaScript进程通信。

运行以后的程序叫作进程，一般情况下，一个进程一次只能执行一个任务。如果有很多任务需要执行，不外乎三种解决办法。

- 排队。因为一个进程一次只能执行一个任务，只好等前面的任务执行完了，再执行后面的程序。
- 新建进程。使用fork命令，为每一个任务新建一个进程。
- 新建线程。因为进程太耗费资源，所以如今的程序汪汪允许一个进程包含多个线程，由线程去完成任务。

每当遇到I/O的时候，主线程就让Event Loop线程去通知相应的I/O程序，然后接着往后运行，等到I/O程序操作完成，EventLoop线程再把结果返回主线程。主线程就调用事先设定的回调函数，完成这个任务，这种运行方式称为异步模式。如果有大量的异步任务，它们会在消息队列中产生大量的消息。这些消息排成队，等候进入主线程。本质上，消息队列就是一个先进先出的数据结构。

单线程模型虽然对JavaScript构成很大的限制，但也因此它具备了其他语言不具备的优势。如果部署得好，JavaScript程序是不会出现堵塞的，这就是为什么node.js平台可以很少的资源，应付大流量访问的原因。