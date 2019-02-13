# interview

## CSS
### 1.Using flexbox, create a 3-column layout where each column takes up a col-{n} / 12 ratio of the container.
```
<div class="row">
  <div class="col-2"></div>
  <div class="col-7"></div>
  <div class="col-3"></div>
</div>
```
---
```
.row {
  display: flex;
}
.col-2 {
  flex-growth: 2;
}
```

### 2.What is CSS BEM?
---
BEM代表块（Block），元素（Element），修饰符（Modifier）。一种前端命名方法论，让你的CSS类对其他开发者来说更加透明而且更有意义。
```
/* block component */
.block {
}
/* element */
.block__element {
}
/* modifier */
.block__element--modifier {
}
```

### 3.What are the advantages of using CSS preprocessors?
CSS 预处理器定义了一种新的语言，其基本思想是：用一种专门的编程语言，为CSS增加了一些编程的特性，将CSS作为目标生成文件，然后开发者就只要使用这种语言进行编码工作。比如：Sass（SCSS）、LESS、Stylus等等都是CSS预处理器。
**优点**
- 开发速度提升；
- 减少代码冗余；
- 提供函数，使得样式开发更加编码化；
- 维护简单便捷；
- 代码更干净，优美；

**缺点**
- 有学习成本
- 调试变得更麻烦
- 可以预编译好CSS，如果不预编译好则需要多一个编译器来重新编译一次CSS代码，也就是给浏览器多了一道工序，网页显示的速度会减慢；.

### 3.Can you name the four types of @media properties?
---
- all
用于所有设备
- screen
用于电脑屏幕，平板电脑，智能手机等。
- print
用于打印机和打印预览
- speech
应用于屏幕阅读器等发声设备

### 4.Describe the layout of the CSS Box Model and briefly describe each component.
---
Content-内容
Padding-块内部与Content的距离
Border-边框
Margin-margin用于控制块与块（可以理解成块级元素）之间的距离

### 5.What is the difference between em and rem units?
---
em - 相对长度，相对于父组件的`font-size`
rem - 相对长度，相对于根容器的`font-size`

### 6.What are the advantages of using CSS sprites and how are they utilized?
---
将一个页面内所需要显示的图片全部整合到一张大图中，并使用css属性background进行调用。这样的话，当页面渲染时，可以减少请求次数，在一定程度上能够加快网页加载速度。

缺点：
- 分辨率的屏幕下的自适应页面，你的图片如果不够宽，很容易出现背景断裂；
- 维护麻烦

background-image(背景图片), background-position(确定图片显示的位置) , background-size(背景图片的大小)

### 7.What is the difference between '+' and '~' sibling selectors?.
---
如
```
div+p

p~ul
```
`+` 选择紧接在 div 元素之后的所有 p 元素。
`~` 选择前面有 p 元素的每个 ul 元素。

### 8.What is a focus ring? What is the correct solution to handle them?
对焦环是可对焦元素(如按钮和锚标记)的可见轮廓。它根据供应商的不同而不同，但是通常它会以蓝色的轮廓出现在元素周围，以表明它当前是重点关注的。

最近，Bootstrap等框架选择使用box-shadow轮廓来替代默认的焦点环。然而，这对于鼠标用户来说仍然不是理想的。

最好的解决方案是即将出现的伪选择器:focus-visible，现在可以用JavaScript对其进行填充。只有当用户使用键盘时，它才会显示一个对焦环，并将其隐藏给鼠标用户。这保持了鼠标使用的美观性和键盘使用的可访问性。

## NodeJS
### 1.NodeJS often uses a callback pattern where if an error is encountered during execution, this error is passed as the first argument to the callback. What are the advantages of this pattern?
```
fs.readFile(filePath, function(err, data) {
  if (err) {
    // handle the error, the return is important here
    // so execution stops here
    return console.log(err)
  }
  // use the data object
  console.log(data)
})
```
---
这只是个约定，然而，你应该遵守它。
1. 如果发现的异常则无需传递继续数据；
2. 适应更多的callback模式，更易于维护的代码

### 2.What is REST?
---
所谓REST就是用URL定位资源，用HTTP描述操作。
- Reading: /posts/ => GET
- Creating: /posts/new => POST
- Updating: /posts/:id => PUT
- Destroying: /posts/:id => DELETE

### 3.How can you avoid callback hells?
```
getData(function(a) {
  getMoreData(a, function(b) {
    getMoreData(b, function(c) {
      getMoreData(c, function(d) {
        getMoreData(d, function(e) {
          // ...
        })
      })
    })
  })
})
```
---
- Modularization: break callbacks into independent functions
- Use a control flow library, like async
- Use generators with Promises
- Use async/await (from v7 on)

### 4.What is the event loop in Node.js?
事件轮询允许nodeJS执行非阻塞的I/O操作，而其他操作继续执行。

**延伸阅读：**

**1.为什么JavaScript是单线程？**
JavaScript语言的一大特点就是单线程，也就是说，同一个时间只能做一件事。那么，为什么JavaScript不能有多个线程呢？这样能提高效率。

JavaScript的单线程，与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？

所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变。

为了利用多核CPU的计算能力，HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程，但是子线程完全受主线程控制，且不得操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质。

**2.任务队列**
单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。

如果排队是因为计算量大，CPU忙不过来，倒也算了，但是很多时候CPU是闲着的，因为IO设备（输入输出设备）很慢（比如Ajax操作从网络读取数据），不得不等着结果出来，再往下执行。

JavaScript语言的设计者意识到，这时主线程完全可以不管IO设备，挂起处于等待中的任务，先运行排在后面的任务。等到IO设备返回了结果，再回过头，把挂起的任务继续执行下去。

于是，所有任务可以分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）。同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；异步任务指的是，不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

具体来说，异步执行的运行机制如下。（同步执行也是如此，因为它可以被视为没有异步任务的异步执行。）
（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
（4）主线程不断重复上面的第三步。

只要主线程空了，就会去读取"任务队列"，这就是JavaScript的运行机制。这个过程会不断重复。

**3.事件和回调函数**
"任务队列"是一个事件的队列（也可以理解成消息的队列），IO设备完成一项任务，就在"任务队列"中添加一个事件，表示相关的异步任务可以进入"执行栈"了。主线程读取"任务队列"，就是读取里面有哪些事件。

"任务队列"中的事件，除了IO设备的事件以外，还包括一些用户产生的事件（比如鼠标点击、页面滚动等等）。只要指定过回调函数，这些事件发生时就会进入"任务队列"，等待主线程读取。

所谓"回调函数"（callback），就是那些会被主线程挂起来的代码。异步任务必须指定回调函数，当主线程开始执行异步任务，就是执行对应的回调函数。

"任务队列"是一个先进先出的数据结构，排在前面的事件，优先被主线程读取。主线程的读取过程基本上是自动的，只要执行栈一清空，"任务队列"上第一位的事件就自动进入主线程。但是，由于存在后文提到的"定时器"功能，主线程首先要检查一下执行时间，某些事件只有到了规定的时间，才能返回主线程。

**4.Event Loop**
主线程从"任务队列"中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为Event Loop（事件循环）。
为了更好地理解Event Loop，请看下图
![](https://i.imgur.com/F1McXqn.png)
上图中，主线程运行的时候，产生堆（heap）和栈（stack），栈中的代码调用各种外部API，它们在"任务队列"中加入各种事件（click，load，done）。只要栈中的代码执行完毕，主线程就会去读取"任务队列"，依次执行那些事件所对应的回调函数。

执行栈中的代码（同步任务），总是在读取"任务队列"（异步任务）之前执行。请看下面这个例子。
```
    var req = new XMLHttpRequest();
    req.open('GET', url);    
    req.onload = function (){};    
    req.onerror = function (){};    
    req.send();
```
上面代码中的req.send方法是Ajax操作向服务器发送数据，它是一个异步任务，意味着只有当前脚本的所有代码执行完，系统才会去读取"任务队列"。所以，它与下面的写法等价。
```
    var req = new XMLHttpRequest();
    req.open('GET', url);
    req.send();
    req.onload = function (){};    
    req.onerror = function (){};   
```
也就是说，指定回调函数的部分（onload和onerror），在send()方法的前面或后面无关紧要，因为它们属于执行栈的一部分，系统总是执行完它们，才会去读取"任务队列"。

**5.定时器**
除了放置异步任务的事件，"任务队列"还可以放置定时事件，即指定某些代码在多少时间之后执行。这叫做"定时器"（timer）功能，也就是定时执行的代码。

定时器功能主要由setTimeout()和setInterval()这两个函数来完成，它们的内部运行机制完全一样，区别在于前者指定的代码是一次性执行，后者则为反复执行。以下主要讨论setTimeout()。

setTimeout()接受两个参数，第一个是回调函数，第二个是推迟执行的毫秒数。
```
console.log(1);
setTimeout(function(){console.log(2);},1000);
console.log(3);
```
上面代码的执行结果是1，3，2，因为setTimeout()将第二行推迟到1000毫秒之后执行。

如果将setTimeout()的第二个参数设为0，就表示当前代码执行完（执行栈清空）以后，立即执行（0毫秒间隔）指定的回调函数。
```
setTimeout(function(){console.log(1);}, 0);
console.log(2);
```
上面代码的执行结果总是2，1，因为只有在执行完第二行以后，系统才会去执行"任务队列"中的回调函数。

总之，setTimeout(fn,0)的含义是，指定某个任务在主线程最早可得的空闲时间执行，也就是说，尽可能早得执行。**它在"任务队列"的尾部添加一个事件，因此要等到同步任务和"任务队列"现有的事件都处理完，才会得到执行。**

HTML5标准规定了setTimeout()的第二个参数的最小值（最短间隔），不得低于4毫秒，如果低于这个值，就会自动增加。在此之前，老版本的浏览器都将最短间隔设为10毫秒。另外，对于那些DOM的变动（尤其是涉及页面重新渲染的部分），通常不会立即执行，而是每16毫秒执行一次。这时使用requestAnimationFrame()的效果要好于setTimeout()。

需要注意的是，setTimeout()只是将事件插入了"任务队列"，必须等到当前代码（执行栈）执行完，主线程才会去执行它指定的回调函数。**要是当前代码耗时很长，有可能要等很久，所以并没有办法保证，回调函数一定会在setTimeout()指定的时间执行。**

**6.Node.js的Event Loop**
Node.js也是单线程的Event Loop，但是它的运行机制不同于浏览器环境。
![](https://i.imgur.com/YTty35a.png)
根据上图，Node.js的运行机制如下。
（1）V8引擎解析JavaScript脚本。
（2）解析后的代码，调用Node API。
（3）libuv库负责Node API的执行。它将不同的任务分配给不同的线程，形成一个Event Loop（事件循环），以异步的方式将任务的执行结果返回给V8引擎。
（4）V8引擎再将结果返回给用户。

除了setTimeout和setInterval这两个方法，Node.js还提供了另外两个与"任务队列"有关的方法：process.nextTick和setImmediate。它们可以帮助我们加深对"任务队列"的理解。

process.nextTick方法可以在当前"执行栈"的尾部----下一次Event Loop（主线程读取"任务队列"）之前----触发回调函数。**也就是说，它指定的任务总是发生在所有异步任务之前**。setImmediate方法则是在当前"任务队列"的尾部添加事件，也就是说，它指定的任务总是在下一次Event Loop时执行，这与setTimeout(fn, 0)很像。请看下面的例子
```
process.nextTick(function A() {
  console.log(1);
  process.nextTick(function B(){console.log(2);});
});

setTimeout(function timeout() {
  console.log('TIMEOUT FIRED');
}, 0)
// 1
// 2
// TIMEOUT FIRED
```
上面代码中，由于process.nextTick方法指定的回调函数，总是在当前"执行栈"的尾部触发，所以不仅函数A比setTimeout指定的回调函数timeout先执行，而且函数B也比timeout先执行。这说明，如果有多个process.nextTick语句（不管它们是否嵌套），将全部在当前"执行栈"执行。

现在，再看setImmediate。
```
setImmediate(function A() {
  console.log(1);
  setImmediate(function B(){console.log(2);});
});

setTimeout(function timeout() {
  console.log('TIMEOUT FIRED');
}, 0);
```
上面代码中，setImmediate与setTimeout(fn,0)各自添加了一个回调函数A和timeout，都是在下一次Event Loop触发。那么，哪个回调函数先执行呢？答案是不确定。运行结果可能是1--TIMEOUT FIRED--2，也可能是TIMEOUT FIRED--1--2。

令人困惑的是，Node.js文档中称，setImmediate指定的回调函数，总是排在setTimeout前面。实际上，这种情况只发生在递归调用的时候。
```
setImmediate(function (){
  setImmediate(function A() {
    console.log(1);
    setImmediate(function B(){console.log(2);});
  });

  setTimeout(function timeout() {
    console.log('TIMEOUT FIRED');
  }, 0);
});
// 1
// TIMEOUT FIRED
// 2
```
上面代码中，setImmediate和setTimeout被封装在一个setImmediate里面，它的运行结果总是1--TIMEOUT FIRED--2，这时函数A一定在timeout前面触发。至于2排在TIMEOUT FIRED的后面（即函数B在timeout后面触发），是因为**setImmediate总是将事件注册到下一轮Event Loop，所以函数A和timeout是在同一轮Loop执行，而函数B在下一轮Loop执行**。

我们由此得到了process.nextTick和setImmediate的一个重要区别：多个process.nextTick语句总是在当前"执行栈"一次执行完，多个setImmediate可能则需要多次loop才能执行完。事实上，这正是Node.js 10.0版添加setImmediate方法的原因，否则像下面这样的递归调用process.nextTick，将会没完没了，主线程根本不会去读取"事件队列"！
```
process.nextTick(function foo() {
  process.nextTick(foo);
});
```
事实上，现在要是你写出递归的process.nextTick，Node.js会抛出一个警告，要求你改成setImmediate。

另外，由于process.nextTick指定的回调函数是在本次"事件循环"触发，而setImmediate指定的是在下次"事件循环"触发，所以很显然，前者总是比后者发生得早，而且执行效率也高（因为不用检查"任务队列"）。

## HTML
### 1.What is the purpose of the alt attribute on images?
---
如果无法显示图像，浏览器将显示替代文本

### 2.What is the purpose of cache busting and how can you achieve it?
---
This is done by naming the file something different to the old file.
如：
```
src="js/script.js" => src="js/script.js?v=2"
```
### 3.Can a web page contain multiple `<header>` elements? What about `<footer>` elements?
---
当然可以。
这两种标签代表了离它们最近的祖先元素的 头部 和 脚部。 所以不仅仅是`<body>`标签可以包含一个header和footer，而且每一个`<article>`和`<section>`元素都可以包含一个header和footer标签。但是，一个 section 中只能各有一个。

### 4.Briefly describe the correct usage of the following HTML5 semantic elements: 
```
<header>, <article>,<section>, <footer>
```
---
- header
标签定义文档的页眉（介绍信息）。
- article
定义独立的文章内容。
- section
标签定义文档中的节，比如章节、页眉、页脚或文档中的其他部分。
- footer
标签定义文档或节的页脚。页脚通常包含文档的作者、版权信息、使用条款链接、联系信息等等。可以在一个文档中使用多个 `<footer>` 元素。

### 5.What are defer and async attributes on a `<script>` tag?
---
```
<script src="myscript.js"></script>
<script src="myscript.js" defer></script>
<script src="myscript.js" async></script>
```
没有 defer 或 async，文档的解析将停止，并立即下载并执行脚本。
有 async，文档的解析不会停止，其他线程将下载脚本，脚本下载完成后开始执行脚本，脚本执行的过程中文档将停止解析，直到脚本执行完毕。
有 defer，文档的解析不会停止，其他线程将下载脚本，待到文档解析完成，脚本才会执行。

所以async和defer的最主要的区别就是async是异步下载并立即执行，然后文档继续解析，defer是异步加载后解析文档，然后再执行脚本，这样说起来是不是理解了一点了；

### 6.What is the DOM?
---
DOM 是 W3C的标准。
document.getElementById() 和 document.querySelector()  用来获取DOM

### 7.Discuss the differences between an HTML specification and a browser’s implementation thereof.
---
诸如HTML5之类的HTML规范定义了文档必须遵守的一组规则，符合该规则的被称为有效。此外，规范提供了有关浏览器如何解释和呈现此类文档的说明。
如果浏览器根据规范的规则处理有效文档，则称其为“支持”规范。到目前为止，没有任何浏览器支持HTML5的所有规范（尽管所有主流浏览器都支持其中的大部分），因此，开发人员必须确认所使用的属性是否所有的浏览器都支持。这就是为什么跨浏览器支持仍然是开发人员头痛的原因，尽管具体情况有所改善。

### 8.What is the difference between HTML and React event handling?
---
1. 大小写及绑定写法不同
```
//HTML
<button onclick="handleClick()"></button>
//React
<button onClick={handleClick}></button>
```
2. react 不能通过 return false;来阻止元素的默认行为，需要通过e.preventDefault();
```
<a href="#" onclick="console.log('The link was clicked.'); return false" />
function handleClick(e) {
  e.preventDefault()
  console.log("The link was clicked.")
}
```

### 9.What are some differences that XHTML has compared to HTML?
---
1. XHTML 的目标是取代 HTML。
2. XHTML 是更严格更纯净的 HTML 版本。
3. 任何元素都可以自闭合
4. 标签和属性区分大小写，通常为小写

### 10.Where and why is the rel="noopener" attribute used?
---
rel ="noopener"属性是`<a>`元素中使用的属性。它会阻止页面具有window.opener属性，该属性将指向打开链接的页面，并允许从超链接打开的页面操作超链接所在的页面。

### 11.What is HTML5 Web Storage? Explain localStorage and sessionStorage.
存储容量限制比使用cookie要大得多（至少5MB），而且速度更快。

localStorage：存储的数据是永久性的
sessionStorage：与存储数据的顶级窗口或浏览器选项卡具有相同的生命周期





## JavaScript
### 6.What is the difference between the equality operators == and ===?
=== 更严格，要求类型和数值都相等
== 只进行值比较 如 `0==false` 为 `true`
尽可能的使用 === 

