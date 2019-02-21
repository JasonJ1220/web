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
- Content-内容
- Padding-块内部与Content的距离
- Border-边框
- Margin-margin用于控制块与块（可以理解成块级元素）之间的距离

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
---
存储容量限制比使用cookie要大得多（至少5MB），而且速度更快。

localStorage：存储的数据是永久性的
sessionStorage：与存储数据的顶级窗口或浏览器选项卡具有相同的生命周期

## JavaScript
### 1.What is the difference between the equality operators == and ===?
---
=== 更严格，要求类型和数值都相等
== 只进行值比较 如 `0==false` 为 `true`
尽可能的使用 === 

### 2.What is the difference between an element and a component in React?
---
1. element就是React实现界面内容的最小单元，它代表了虚拟DOM中的一个对象。它描述了组件实例和DOM节点的关系在React中，通过使用React.createElment可以实现虚拟DOM节点。
2. component 就是一个方法或者一个类，可以接受一定的输入，之后返回一个React element 树结构。

### 3.What is the difference between the postfix i++ and prefix ++i increment operators?
---
作为赋值的结果不一样，如：
```
let i = 0,
    a = i++; // a = 0
// i === 1

let i = 0,
    a = ++i; // a = 1
// i === 1
```

### 4.In which states can a Promise be?
---
- pending
- fulfilled
- rejected

### 5.What is a stateless component?
---
无状态组件 是一种只负责展示的纯组件,依赖props渲染显示，避免使用this关键字。

### 6.What is a stateful component in React?
---
有状态组件 在无状态组件的基础上，如果组件内部包含状态（state）且状态随着事件或者外部的消息而发生改变的时候，这就构成了有状态组件（Stateful Component）。有状态组件通常会带有生命周期(lifecycle)，用以在不同的时刻触发状态的更新。这种组件也是通常在写业务逻辑中最经常使用到的，根据不同的业务场景组件的状态数量以及生命周期机制也不尽相同。

### 7.Create a function batches that returns the maximum number of whole batches that can be cooked from a recipe.
```
/**
It accepts two objects as arguments: the first object is the recipe
for the food, while the second object is the available ingredients.
Each ingredient's value is number representing how many units there are.

`batches(recipe, available)`
*/

// 0 batches can be made
batches(
  { milk: 100, butter: 50, flour: 5 },
  { milk: 132, butter: 48, flour: 51 }
)
batches(
  { milk: 100, flour: 4, sugar: 10, butter: 5 },
  { milk: 1288, flour: 9, sugar: 95 }
)

// 1 batch can be made
batches(
  { milk: 100, butter: 50, cheese: 10 },
  { milk: 198, butter: 52, cheese: 10 }
)

// 2 batches can be made
batches(
  { milk: 2, sugar: 40, butter: 20 },
  { milk: 5, sugar: 120, butter: 500 }
)
```
---

```
const batches = (recipe, available) =>
  Math.floor(
    Math.min(...Object.keys(recipe).map(k => available[k] / recipe[k] || 0))
  )
```

### 8.Create a standalone function bind that is functionally equivalent to the method Function.prototype.bind.
```
function example() {
  console.log(this)
}
const boundExample = bind(example, { a: true })
boundExample.call({ b: true }) // logs { a: true }
```
---
```
let bind=(fn,context)=>(...args)=>fn.apply(context,args)
```


### 9.What is the purpose of callback function as an argument of setState?
---
1. setState是异步的, 执行完成之后会调用callback;
2. 推荐使用生命周期的 componentDidUpdate 方法替代callback ;

### 10.What is a callback? Can you show an example using one?
---
1. 事件订阅
```
function onClick() {
  console.log("The user clicked on the page.")
}
document.addEventListener("click", onClick)
```
2. 回调函数
```
const map = (arr, callback) => {
  const result = []
  for (let i = 0; i < arr.length; i++) {
    result.push(callback(arr[i], i))
  }
  return result
}
map([1, 2, 3, 4, 5], n => n * 2) // [2, 4, 6, 8, 10]
```

### 11.How do you clone an object in JavaScript?
---
1. 使用...结构赋值
```
const obj = { a: 1, b: 2 }
const shallowClone = { ...obj }
```
2. Object.assign
3. JSON.parse(JSON.stringify(obj)

### 12.How do you compare two objects in JavaScript?
```
function isDeepEqual(obj1, obj2, testPrototypes = false) {
  if (obj1 === obj2) {
    return true
  }

  if (typeof obj1 === "function" && typeof obj2 === "function") {
    return obj1.toString() === obj2.toString()
  }

  if (obj1 instanceof Date && obj2 instanceof Date) {
    return obj1.getTime() === obj2.getTime()
  }

  if (
    Object.prototype.toString.call(obj1) !==
      Object.prototype.toString.call(obj2) ||
    typeof obj1 !== "object"
  ) {
    return false
  }

  const prototypesAreEqual = testPrototypes
    ? isDeepEqual(
        Object.getPrototypeOf(obj1),
        Object.getPrototypeOf(obj2),
        true
      )
    : true

  const obj1Props = Object.getOwnPropertyNames(obj1)
  const obj2Props = Object.getOwnPropertyNames(obj2)

  return (
    obj1Props.length === obj2Props.length &&
    prototypesAreEqual &&
    obj1Props.every(prop => isDeepEqual(obj1[prop], obj2[prop]))
  )
}
```

### 13.What is CORS?
---
出于保护用户安全的原因，浏览器限制了跨域的HTTP请求。XMLHttpRequest 和 fetch 都遵循同域的原则，也就是说跨域调用的对端的response 要包含CORS 头，才可以。

### 14.What is event delegation and why is it useful? Can you show an example of how to use it?
---
Event.target - 返回触发事件的元素
Event.currentTarget- 返回绑定事件的元素

target事件委托的定义：本来该自己干的事，但是自己不干，交给别人来干（通常都是子节点交给父节点去监听事件，然后通过事件冒泡的特性和e.target 来控制相应的子节点去处理事件）。一般用到for循环遍历节点添加事件的时候都可以用事件委托来做，可以提高性能。
```
document.addEventListener("click", e => {
  if (e.target.closest("button")) {
    handleButtonClick()
  }
})
```

### 15.What is the difference between an expression and a statement in JavaScript?
---
如果你能把它打印出来或者赋值给一个变量，它就是一个表达式。如果你不能，这是一个声明。

```
// 函数声明
function funDeclaration(type){
    return type==="Declaration";
}
// 函数表达式
var funExpression = function(type){
    return type==="Expression";
}
```
　Javascript 中函数声明和函数表达式是存在区别的，函数声明在JS解析时进行函数提升，因此在同一个作用域内，不管函数声明在哪里定义，该函数都可以进行调用。而函数表达式的值是在JS运行时确定，并且在表达式赋值完成后，该函数才能调用。

### 16.What are truthy and falsy values in JavaScript?
---
false:
1. false
1. null
1. undefined
1. 0
1. ''
1. NaN

### 17.Generate an array, containing the Fibonacci sequence, up until the nth term.
---
```
(n)=>[...Array(n+1)].reduce((result,a,i)=>result.concat(i>1?result[i-2]+result[i-1]:i),[]).slice(1)
```

### 18.What does 0.1 + 0.2 === 0.3 evaluate to?
---
false;
```
(a,b,m)=>Math.abs(a-b)<m
```

### 19.What is the difference between the array methods map() and forEach()?
---
map 构造数组
forEach 轮询数组

### 20.What will the console log in this example?
```
var foo = 1
var foobar = function() {
  console.log(foo)
  var foo = 2
}
foobar()
```
---
undefined

状态提升，不伴随着赋值提升。
相当于
```
var foo = 1
var foobar = function() {
  var foo;
  console.log(foo)
  foo = 2
}
foobar()
```

### 21.How does hoisting work in JavaScript?
---
1. 声明提升是JavaScript默认行为。
1. JavaScript 会自动把 var 和 function 声明移动到顶部。
1. 函数声明会被完整地提升。 变量只会被部分提升。例如只有 var 的声明会被提升，而赋值则不会。
1. 声明方法提升 优先于 声明变量提升
```
myFunction() // No error; logs "hello"
function myFunction() {
  console.log("hello")
}
myFunction() // Error: `myFunction` is not a function
var myFunction = function() {
  console.log("hello")
}
```

### 22.What is the reason for wrapping the entire contents of a JavaScript source file in a function that is immediately invoked?
---
1. 在主流JavaScript框架中经常使用。
2. 提供私有的命名空间

### 23.What are inline conditional expressions?
---
```
\\ error
function App({ messages, isVisible }) {
  return (
    <div>
      if (messages.length > 0) {
        <h2>You have {messages.length} unread messages.</h2>
      } else {
        <h2>You have no unread messages.</h2>
      }
      if (isVisible) {
        <p>I am visible.</p>
      }
    </div>
  )
}

\\correct
function App({ messages, isVisible }) {
  return (
    <div>
      {messages.length > 0 ? (
        <h2>You have {messages.length} unread messages.</h2>
      ) : (
        <h2>You have no unread messages.</h2>
      )}
      {isVisible && <p>I am visible.</p>}
    </div>
  )
}
```

### 24.What is a key? What are the benefits of using it in lists?
---
1. Keys 可以在 DOM 中的某些元素被增加或删除的时候帮助 React 识别哪些元素发生了变化。因此你应当给数组中的每一个元素赋予一个确定的标识。
2. 不要用index
3. 如果 list中的 item是子组件的话，那么它的key应该由它的父组件决定。

### 25.What is the difference between lexical scoping and dynamic scoping?
---
词法作用域，也叫静态作用域，它的作用域是指在词法分析阶段就确定了，不会改变。动态作用域是在运行时根据程序的流程信息来动态确定的，而不是在写代码时进行静态确定的。

动态作用域是在运行时根据程序的流程信息来动态确定的，而不是在写代码时进行静态确定的。
```
var a = 2;

function foo() {
  console.log(a); // 会输出2还是3？
}

function bar() {
  var a = 3;
  foo();
}

bar();
```
如果是静态作用域 会输出2；
如果是动态作用域 会输出3；

**词法作用域关注函数在何处声明，而动态作用域关注函数从何处调用。**

### 26.Create a function that masks a string of characters with # except for the last four (4) characters.
---
N多方案
```
const mask = (str, maskChar = "#") =>
  str.slice(-4).padStart(str.length, maskChar)
```

### 27.What is a MIME type and what is it used for?
---
Multi-purpose Internet Mail Extensions-两部分构成 类型/子类型

### 28.What is the difference between null and undefined?
---
null是显式赋值，而undefined定义为未赋值。
undefined == null
typeof null === 'object'

### 29.Describe the different ways to create an object. When should certain ways be preferred over others?
class Person 两个属性 name,age 一个方法 birthday
---
对象字面量方式：
```
const person = {
  name: "John",
  age: 50,
  birthday() {
    this.age++
  }
}
person.birthday() // person.age === 51
```
构造函数
```
function Person(name, age){
  this.name = name
  this.age = age
}
Person.prototype.birthday = function() {
  this.age++
}
const person1 = new Person("John", 50)
const person2 = new Person("Sally", 20)
person1.birthday() // person1.age === 51
person2.birthday() // person2.age === 21
```
工厂方法
```
const createPerson = (name, age) => {
  const birthday = () => person.age++
  const person = { name, age, birthday }
  return person
}
const person = createPerson("John", 50)
person.birthday() // person.age === 51
```
Object.create()
```
const personProto = {
  birthday() {
    this.age++
  }
}
const person = Object.create(personProto)
person.age = 50
person.birthday() // person.age === 51
```

### 30.What is the difference between a parameter and an argument?
---
argument是一个类数组的对象，包含了调用函数的参数。

### 31.Does JavaScript pass by value or by reference?
---
值传递

### 32.How do you pass an argument to an event handler or callback?
---
```
<button onClick={() => this.handleClick(id)} />
<button onClick={this.handleClick.bind(this, id)} />
```

### 33.What are Promises?
---
```
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("result")
  }, 100)
})
  .then(console.log)
  .catch(console.error)
```

### 34.How does prototypal inheritance differ from classical inheritance?
---
classical inheritance
构造函数 new

prototypal inheritance
继承自其他对象 工厂方法 object.create()

### 35.What is the output of the following code?
```
const a = [1, 2, 3]
const b = [1, 2, 3]
const c = "1,2,3"

console.log(a == c)
console.log(a == b)
```
---
true false
JavaScript  ==会自动进行类型转换，对比string的值
而两个对象比较 则为referance比较。

### 36.What does the following function return?
```
function greet() {
  return
  {
    message: "hello"
  }
}
```
undefined 
return在当前行自动加分号

### 37.Are semicolons required in JavaScript?
---
我认为必须加。

### 38.What is short-circuit evaluation in JavaScript?
---
```
true || false // true
false && true // false
```
逻辑操作不会产生布尔值，除非操作数的值是布尔值。

### 39.What is the difference between synchronous and asynchronous code in JavaScript?
---
Synchronous means each operation must wait for the previous one to complete.
Asynchronous means an operation can occur while another operation is still being processed.

JavaScript 事件轮询

### 40.What does the following code evaluate to?
```
typeof typeof 0
```
---
string

### 41.What are JavaScript data types?
---
Boolean, Null, Undefined, Number, String, Symbol

### 42.What are the differences between var, let, const and no keyword statements?
---
- var
在 ES6 之前，只有 var 一种声明变量的方法。在某一函数内部声明的变量和方法只能在其函数作用域内部访问到。
一些在例如 if 或者 for 语句的块作用域内声明的变量，能够在其包含的大括号外被访问到。

- let
它不会被提升，且可以在块级作用域内生效。
```
let g1 = 'global 1'
let g2 = 'global 2'
{   /* Creating a new block scope */
  g1 = 'new global 1'
  let g2 = 'local global 2'
  console.log(g1)   // 'new global 1'
  console.log(g2)   // 'local global 2'
  console.log(g3)   // ReferenceError: g3 is not defined
  let g3 = 'I am not hoisted';
}
console.log(g1)    // 'new global 1'
console.log(g2)    // 'global 2' 
```


**注意：未使用 var，let 或 const 关键字声明的变量会自动变成全局变量。**

### 43.What is a cross-site scripting attack (XSS) and how do you prevent it?
---
1. 一旦在DOM解析过程成出现不在预期内的改变（JS代码执行或样式大量变化时），就可能发生XSS攻击
1. XSS分为反射型XSS，存储型XSS和DOM XSS
1. 反射型XSS是在将XSS代码放在URL中，将参数提交到服务器。服务器解析后响应，在响应结果中存在XSS代码，最终通过浏览器解析执行。
1. 存储型XSS是将XSS代码存储到服务端（数据库、内存、文件系统等），在下次请求同一个页面时就不需要带上XSS代码了，而是从服务器读取。
1. DOM XSS的发生主要是在JS中使用eval造成的，所以应当避免使用eval语句。
1. XSS危害有盗取用户cookie，通过JS或CSS改变样式，DDos造成正常用户无法得到服务器响应。
1. XSS代码的预防主要通过对数据解码，再过滤掉危险标签、属性和事件等。

解决方案
1. 转义关键字 如'/,<,>,\'等等
2. 正如其名称，innerHTML 返回 HTML 文本。通常，为了在元素中检索或写入文本，人们使用innerHTML。但是，textContent通常具有更好的性能，因为文本不会被解析为HTML。此外，使用textContent可以防止 XSS 攻击。

### 44.What is Big O Notation?
---
当执行时间呈指数增长时，要小心嵌套循环。

### 45.How can you avoid callback hells?
---
1. promise
2. generator函数
3. async/await语法
4. 模块化:将回调分解为独立的函数

### 46.Which is the preferred option between callback refs and findDOMNode()?
---
callback refs 
```
class MyComponent extends Component {
  componentDidMount() {
    this.node.scrollIntoView()
  }

  render() {
    return <div ref={node => (this.node = node)} />
  }
}
```

### 47.What is the children prop?
---
1. children 是 props 的一个特殊属性。专门用来传递子组件，可以为任何数据类型。
2. react提供了特殊的API与这个属性协同工作。如React.Children.map, React.Children.forEach, React.Children.count, React.Children.only 和 React.Children.toArray.


```
function GenericBox({ children }) {
  return <div className="container">{children}</div>
}

function App() {
  return (
    <GenericBox>
      <span>Hello</span> <span>World</span>
    </GenericBox>
  )
}
```

### 48.What is a closure? Can you give a useful example of one?
---
闭包就是能够读取其他函数内部变量的函数。
```
function f1(){
　　var n=999;
　　nAdd=function(){n+=1}
　　function f2(){
　　　　alert(n);
　　}
　　return f2;//f2就是闭包
}
var result=f1();
result(); // 999
nAdd();
result(); // 1000
```

### 49.What is context?(React)
---
1. 用于共享数据
2. 等同于react-redux中的provider
```
<Provider store={store}>
    <App />
</Provider>
```

### 50.What is event-driven programming?
---
拥有事件驱动方法 on 和 emit
```
document.addEventListener("click", function(event) {
  // This callback function is run when the user
  // clicks on the document.
})

const hub = createEventHub()
hub.on("message", function(data) {
  console.log(`${data.username} said ${data.text}`)
})
hub.emit("message", {
  username: "John",
  text: "Hello?"
})
```

### 51.What are fragments?
---
相当于一个虚拟DOM对象，允许从组件返回的多个元素分组，而Fragment自身并不算添加任何实际DOM元素。
```
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}
```

### 52.What is functional programming?
---
1. 支持函数式编程的JavaScript特性(map,reduce等)
2. 更干净、更简洁的开发经验
3. 如redux

### 53.Explain the differences between imperative and declarative programming.
---
**命令式编程：**一步一步告诉计算机先做什么再做什么。
```
const numbers = [1, 2, 3, 4, 5]
const numbersDoubled = []
for (let i = 0; i < numbers.length; i++) {
  numbersDoubled[i] = numbers[i] * 2
}
```
**声明式编程：**应该做什么，但不指定具体要怎么做。如sql：
```
SELECT * FROM collection WHERE num > 5
```
又如：
```
const numbers = [1, 2, 3, 4, 5]
const numbersDoubled = numbers.map(n => n * 2)
```

声明式编程通常使用函数和表达式。命令式编程经常使用语句并依赖于导致突变的低级特性，而声明式编程则非常注重抽象和纯粹性。
声明式编程更简洁，更容易处理。

### 54.What is memoization?
---
Memoization是缓存函数调用的输出以使后续调用更快的过程。再次使用相同的输入调用函数将返回缓存的输出，而不需要再次进行计算。
```
const memoize = fn => {
  const cache = new Map()
  return value => {
    const cachedResult = cache.get(value)
    if (cachedResult !== undefined) return cachedResult
    const result = fn(value)
    cache.set(value, result)
    return result
  }
}
```

### 55.How do you ensure methods have the correct `this` context in React component classes?
---
1. 在 constructor 里 对函数 执行bind(this)操作
```
constructor(props) {
  super(props);
  this.handleClick = this.handleClick.bind(this);
}
handleClick() {
  // Perform some logic
}
```
2. 在事件订阅后 使用=>表达式 handler函数
```
handleClick = () => {
  console.log('this is:', this);
}
render() {
  return (
    <button onClick={this.handleClick}>
      Click me
    </button>
  );
}
```
3. 直接在事件订阅处 使勇=> 封装 handler函数
```
<button onClick={e => this.handleClick(e)}>Click me</button>
```


### 56.Contrast mutable and immutable values, and mutating vs non-mutating methods.
---
说明一些会引发原数组变化的数组方法
```
const myString = "hello!"
myString.replace("!", "") // returns a new string, cannot mutate the original value

const originalArray = [1, 2, 3]
originalArray.push(4) // mutates originalArray, now [1, 2, 3, 4]
originalArray.concat(4) // returns a new array, does not mutate the original
```

### 57.What is the only value not equal to itself in JavaScript?
---
NaN
要使用Number.isNaN()去判断是否为NaN
```
const isNaN = x => x !== x
```

### 58.Create a function pipe that performs left-to-right function composition by returning a function that accepts one argument.
```
const square = v => v * v
const double = v => v * 2
const addOne = v => v + 1
const res = pipe(square, double, addOne)
res(3) // 19; addOne(double(square(3)))
```
---
```
var pipe = (...fns)=>(v)=>fns.reduce((a,b)=>b(a),v)
```

### 59.What are portals in React?
---
Portals 提供了一种很好的将子节点渲染到父组件以外的 DOM 节点的方式。
ReactDOM.createPortal(child, container)
第一个参数（child）是任何可渲染的 React 子元素，例如一个元素，字符串或碎片。第二个参数（container）则是一个 DOM 元素。
应用场景：左右移动组件，模态框组件


### 60.What is a pure function?
---
1.给定输入 有确定性的输出
2.没有副作用(对函数之外的数据或提供给函数的参数进行改变)的函数

**Pure**
```
const a = (x, y) => x + y
const b = (arr, value) => arr.concat(value)
const c = arr => [...arr].sort((a, b) => a - b)
```
**Impure**
```
const a = (x, y) => x + y + Math.random()
const b = (arr, value) => (arr.push(value), arr)
const c = arr => arr.sort((a, b) => a - b)
```

### 61.What is recursion and when is it useful?
---
举例：
```
const nest = (items, id = null, link = "parent_id") =>
  items
    .filter(item => item[link] === id)
    .map(item => ({ ...item, children: nest(items, item.id) }))

const comments = [
  { id: 1, parent_id: null, text: "First reply to post." },
  { id: 2, parent_id: 1, text: "First reply to comment #1." },
  { id: 3, parent_id: 1, text: "Second reply to comment #1." },
  { id: 4, parent_id: 3, text: "First reply to comment #3." },
  { id: 5, parent_id: 4, text: "First reply to comment #4." },
  { id: 6, parent_id: null, text: "Second reply to post." }
]

nest(comments)
/*
[
  { id: 1, parent_id: null, text: "First reply to post.", children: [...] },
  { id: 6, parent_id: null, text: "Second reply to post.", children: [] }
]
*/
```

在处理包含未知数量嵌套结构的数据结构时，递归非常有用。
递归必须满足一个跳出循环的基本条件，否则它将无限期地调用自己。

### 62.What are refs in React? When should they be used?
---
1. 返回element的引用
2. 不应该过度使用
3. 使用方法：
```
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    this.myRef = React.createRef()
  }

  render() {
    return <div ref={this.myRef} />
  }
}
```

什么时候应该用：
- Managing focus, text selection, or media playback.
- Triggering imperative animations.
- Integrating with third-party DOM libraries.

### 63.Explain the difference between a static method and an instance method.
---
```
Array.isArray // static method of Array
Array.prototype.push // instance method of Array
```

### 64.What is the this keyword and how does it work?
---
1. 对象字面量
```
var myObject = {
  property: this,
  regularFunction: function() {
    return this
  },
  arrowFunction: () => {
    return this
  },
  iife: (function() {
    return this
  })()
}
myObject.regularFunction() // myObject
myObject["regularFunction"]() // my Object
myObject.property // NOT myObject; lexical `this`
myObject.arrowFunction() // NOT myObject; lexical `this`
myObject.iife // NOT myObject; lexical `this`
const regularFunction = myObject.regularFunction
regularFunction() // NOT myObject; lexical `this`
```

2. 事件监听
```
document.body.addEventListener("click", function() {
  console.log(this) // document.body
})
```

3. 构造函数
```
class Example {
  constructor() {
    console.log(this) // myExample
  }
}
const myExample = new Example()
```

4. call() 和 apply()
```
var myFunction = function() {
  return this
}
myFunction.call({ customThis: true }) // { customThis: true }
```

5. this的改变
```
var obj = {
  arr: [1, 2, 3],
  doubleArr() {
    return this.arr.map(function(value) {
      // this is now this.arr
      return this.double(value)
    })
  },
  double() {
    return value * 2
  }
}
obj.doubleArr() // Uncaught TypeError: this.double is not a function
```

### 65.What is the purpose of JavaScript UI libraries/frameworks like React, Vue, Angular, Hyperapp, etc?
---
1. 虚拟DOM是以普通对象的形式表示实际DOM树的，它允许库编写代码，就好像整个文档在每次更改时都被丢弃并重新构建，而实际DOM只更新需要更改的内容。由于与重新计算虚拟DOM相比，更改实际DOM节点的开销更大，因此将新虚拟DOM与前一个虚拟DOM进行比较可以提高效率。
2. JSX是JavaScript的扩展，它提供类似xml的语法来创建虚拟DOM对象，通过转换器将这些对象转换为函数调用。与标记的模板文本相比，它简化了控制流(if语句/三元表达式)。

### 66. What does 'use strict' do and what are some of the key benefits to using it?
---
- 消除 this 强制赋值，当引用null或undefined值时抛出错误。
- 在无效使用delete时抛出错误。
- 禁止在ECMAScript的未来版本中定义某些语法



### 67. What is a virtual DOM and why is it used in libraries/frameworks?
---
修改虚拟dom比修改真实dom效率高