# 综合
## 同源策略
同源策略可防止 JavaScript 发起跨域请求。源被定义为 URI、主机名和端口号的组合。此策略可防止页面上的恶意脚本通过该页面的文档对象模型，访问另一个网页上的敏感数据。

## 跨域
- 原因
> 浏览器的同源策略导致了跨域
- 作用
> 用于隔离潜在恶意文件的重要安全机制
- 解决
> 1. jsonp ，允许 script 加载第三方资源
> 2. 反向代理（nginx 服务内部配置 Access-Control-Allow-Origin *）
> 3. cors 前后端协作设置请求头部，Access-Control-Allow-Origin 等头部信息
> 4. iframe 嵌套通讯，postmessage

## 域名收敛
PC 时代为了突破浏览器的域名并发限制。有了域名发散。
浏览器有并发限制，是为了防止DDOS攻击。
域名收敛：就是将静态资源放在一个域名下。减少DNS解析的开销。
域名发散：是将静态资源放在多个子域名下，就可以多线程下载，提高并行度，使客户端加载静态资源更加迅速。
域名发散是pc端为了利用浏览器的多线程并行下载能力。而域名收敛多用与移动端，提高性能，因为dns解析是是从后向前迭代解析，如果域名过多性能会下降，增加DNS的解析开销。

## viewport
字面意思为视图窗口，在移动web开发中使用。表示将设备浏览器宽度虚拟成一个特定的值（或计算得出），这样利于移动web站点跨设备显示效果基本一致。移动版的 Safari 浏览器最新引进了 viewport 这个 meta tag，让网页开发者来控制 viewport 的大小和缩放，其他手机浏览器也基本支持。

在移动端浏览器当中，存在着两种视口，一种是可见视口（也就是我们说的设备大小），另一种是视窗视口（网页的宽度是多少）。 举个例子：如果我们的屏幕是320像素 * 480像素的大小（iPhone4），假设在浏览器中，320像素的屏幕宽度能够展示980像素宽度的内容。那么320像素的宽度就是可见视口的宽度，而能够显示的980像素的宽度就是视窗视口的宽度。

为了显示更多的内容，大多数的浏览器会把自己的视窗视口扩大，简易的理解，就是让原本320像素的屏幕宽度能够容下980像素甚至更宽的内容（将网页等比例缩小）。

**Viewport属性值**
- width 设置layout viewport 的宽度，为一个正整数，或字符串"width-device"
- initial-scale 设置页面的初始缩放值，为一个数字，可以带小数
- minimum-scale 允许用户的最小缩放值，为一个数字，可以带小数
- maximum-scale 允许用户的最大缩放值，为一个数字，可以带小数
- height 设置layout viewport 的高度，这个属性对我们并不重要，很少使用
- user-scalable 是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许这些属性可以同时使用，也可以单独使用或混合使用，多个属性同时使用时用逗号隔开就行了。

## 如何处理不让别人盗用你的图片，访问你的服务器资源
- http header, 对refer做判断看来源是不是自己的网站，如果不是就拒绝
- 通过session校验，如果不通过特定服务生成cookie和session就不能请求得到资源

## 从输入 URL 到页面加载完成的过程中都发生了什么事情？
### 1. 浏览器接收URL
URL包含的信息：协议、网络地址:端口号、资源路径、查询字符串？、片段标识符#
### 2. 将URL与缓存进行比对如果请求的页面在缓存中且未过期，则直接进行第8步
缓存分为彻底缓存和缓存协商，这里的确认是否过期是指彻底缓存（缓存失效之前不再需要跟服务器交互）。

**彻底缓存的机制（http首部字段）：cache-control，Expires**

- Expires是一个绝对时间，即服务器时间。浏览器检查当前时间，如果还没到失效时间就直接使用缓存文件。但是该方法存在一个问题：服务器时间与客户端时间可能不一致。因此该字段已经很少使用，现在基本用cache-control进行判断。

- cache-control中的max-age保存一个相对时间。例如Cache-Control: max-age = 484200，表示浏览器收到文件后，缓存在484200s内均有效。 如果同时存在cache-control和Expires，浏览器总是优先使用cache-control。

**当缓存过期时，浏览器会向服务器发起请求询问资源是否真正过期，这就是缓存协商。对应http首部字段：last-modified，Etag**

- last-modified是第一次请求资源时，服务器返回的字段，表示最后一次更新的时间。下一次浏览器请求资源时就发送if-modified-since字段。服务器用本地Last-modified时间与if-modified-since时间比较，如果不一致则认为缓存已过期并返回新资源给浏览器；如果时间一致则发送304状态码，让浏览器继续使用缓存。当然，用该方法也存在问题，比如修改时间有变化但实际内容没有变化，而服务器却再次将资源发送给浏览器。所以，使用Etag进行判断更好。

- Etag：资源的实体标识（哈希字符串），当资源内容更新时，Etag会改变。服务器会判断Etag是否发生变化，如果变化则返回新资源，否则返回304。

![缓存机制](https://upload-images.jianshu.io/upload_images/3160413-fb75e5af66606680.png?imageMogr2/auto-orient/strip|imageView2/2/w/692/format/webp)

### 3. 如果网络地址不是一个 IP 地址，通过DNS解析域名返回一个IP地址
### 4. 浏览器与服务器通过三次握手(SYN,SYN/ACK,ACK)建立TCP 连接
![三次握手](https://upload-images.jianshu.io/upload_images/3160413-5a9f596e6bde10c6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

### 5. 浏览器向服务器发送HTTP请求。
数据经过应用层、传输层、网络层、物理层逐层封装，传输到下一个目的地。
每一层作用如下：
>应用层：为应用进程提供服务，加应用层首部封装为协议数据单元。
> 传输层：实现端到端通信，加TCP首部封装为数据包，TCP控制了数据包的发送序列的产生，不断的调整发送序列，实现流控和数据完整。
> 网络层：转发分组并选择路由；加IP首部封装为IP分组。
> 数据链路层：相邻的节点间的数据传输；加首部[mac地址]和尾部封装为帧。
> 物理层：具体物理媒介中的数据传送，数据转换为比特流。

### 6. 服务器收到请求，从它的文档空间中查找资源并返回HTTP响应。

### 7. 浏览器接受 HTTP 响应，检查 HTTP header 里的状态码，并做出不同的处理方式。
比如404显示错误页面，304使用缓存，200下一步解码和渲染， 204页面不会发生更新。

常见状态码：200 ok, 204 no content, 206 partial content

301 moved permanently(资源已分配新的uri)，302 found(本次使用新uri访问)，303 see other(以get定向到另一个uri)，304 not modified, 307 temporary redirect(不会从post改为get)

400 bad request，402 unauthorized，403 forbidden, 404 not found

500 internal server error，503 service unavailable

### 8. 如果是可以缓存的，这个响应则会被存储起来
根据首部字段判断是否进行缓存。例如，Cache-Control, no-cache(每次使用缓存前和服务器确认)，no-store 绝对禁止缓存

### 9. 解码
- 浏览器拿到index.html文件后，就开始解析其中的html代码，遇到js/css/image等静态资源时，就向服务器端去请求下载

- 解析成对应的树形数据结构DOM树、CSS规则树，Javascript脚本通过DOM API和CSSOM API来操作DOM树、CSS规则树。

### 10. 渲染
- 计算CSS样式（JS可动态修改dom或css,进一步改变渲染树和分布）
- 构建渲染树（Repaint：屏幕的一部分要重画，比如某个CSS的背景色变了，元素的几何尺寸没有变。Reflow：几何尺寸变了，我们需要重新验证并计算Render Tree。
- 确认布局（定位坐标和大小，是否换行，各种position, overflow, z-index属性 ……）
- 绘制（调用操作系统Native GUI的API绘制，将每个节点转化为实际像素绘制到视口上）

### 11. 关闭TCP连接或继续保持连接
通过四次挥手关闭连接
![三次握手](https://upload-images.jianshu.io/upload_images/3160413-1d8c843335a5f419.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

## 谈谈性能优化问题
代码层面：避免使用css表达式，避免使用高级选择器，通配选择器。

缓存利用：缓存Ajax，使用CDN，使用外部js和css文件以便缓存，添加Expires头，服务端配置Etag，减少DNS查找等

请求数量：合并样式和脚本，使用css图片精灵，初始首屏之外的图片资源按需加载，静态资源延迟加载。

请求带宽：压缩文件，开启GZIP

## web storage和cookie的区别
Web Storage的概念和cookie相似，区别是它是为了更大容量存储设计的。Cookie的大小是受限的，并且每次你请求一个新的页面的时候Cookie都会被发送过去，这样无形中浪费了带宽，另外cookie还需要指定作用域，不可以跨域调用。

除此之外，Web Storage拥有setItem,getItem,removeItem,clear等方法，不像cookie需要前端开发者自己封装setCookie，getCookie。

但是cookie也是不可以或缺的：cookie的作用是与服务器进行交互，作为HTTP规范的一部分而存在 ，而Web Storage仅仅是为了在本地“存储”数据而生

## cookie 和session 的区别
1. cookie数据存放在客户的浏览器上，session数据放在服务器上。
2. cookie不是很安全，别人可以分析存放在本地的cookie并进行cookie欺骗
考虑到安全应当使用session。
3. session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能
考虑到减轻服务器性能方面，应当使用cookie。
4、单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。
5、建议：
将登陆信息等重要信息存放为session
其他信息如果需要保留，可以放在cookie中

# HTML
## DOCTYPE有什么用？
DOCTYPE是“document type”的缩写。它是 HTML 中用来区分标准模式和怪异模式的声明，用来告知浏览器使用标准模式渲染页面。

从中获得的启发：在页面开始处添加<!DOCTYPE html>即可。

## 请描述`<script>、<script async>和<script defer>`的区别。
- `<script>` - HTML 解析中断，脚本被提取并立即执行。执行结束后，HTML 解析继续。
- `<script async>` - 脚本的提取、执行的过程与 HTML 解析过程并行，脚本执行完毕可能在 HTML 解析完毕之前。当脚本与页面上其他脚本独立时，可以使用async，比如用作页面统计分析。
- `<script defer>` - 脚本仅提取过程与 HTML 解析过程并行，脚本的执行将在 HTML 解析完毕后进行。如果有多个含defer的脚本，脚本的执行顺序将按照在 document 中出现的位置，从上到下顺序执行。
注意：没有src属性的脚本，async和defer属性会被忽略。

# CSS
## 请解释什么是精灵图（css sprites），以及如何实现？
精灵图，也称雪碧图。因常见碳酸饮料雪碧的英文名也是 Sprite，因此也有人会使用雪碧图的非正式译名。

精灵图是把多张图片整合到一张上的图片。它被运用在众多使用了很多小图标的网站上（Gmail 在使用）。实现方法：

1. 使用生成器将多张图片打包成一张精灵图，并为其生成合适的 CSS。
2. 每张图片都有相应的 CSS 类，该类定义了background-image、background-position和background-size属性。
3. 使用图片时，将相应的类添加到你的元素中。

好处：
- 减少加载多张图片的 HTTP 请求数（一张精灵图只需要一个请求）。但是对于 HTTP2 而言，加载多张图片不再是问题。
- 提前加载资源，防止在需要时才在开始下载引发的问题，比如只出现在:hover伪类中的图片，不会出现闪烁。

## 编写高效的 CSS（待补充）

## { box-sizing: border-box; }会产生怎样的效果？
- 元素默认应用了box-sizing: content-box，元素的宽高只会决定内容（content）的大小。
- box-sizing: border-box改变计算元素width和height的方式，border和padding的大小也将计算在内。
- 元素的height = 内容（content）的高度 + 垂直方向的padding + 垂直方向border的宽度
- 元素的width = 内容（content）的宽度 + 水平方向的padding + 水平方向border的宽度

## relative、fixed、absolute和static四种定位有什么区别？
经过定位的元素，其position属性值必然是relative、absolute、fixed或sticky。

- static：默认定位属性值。该关键字指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时 top, right, bottom, left 和 z-index 属性无效。
- relative：该关键字下，元素先放置在未添加定位时的位置，再在不改变页面布局的前提下调整元素位置（因此会在此元素未添加定位时所在位置留下空白）。
- absolute：不为元素预留空间，通过指定元素相对于最近的非 static 定位祖先元素的偏移，来确定元素位置。绝对定位的元素可以设置外边距（margins），且不会与其他边距合并。
- fixed：不为元素预留空间，而是通过指定元素相对于屏幕视口（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变。打印时，元素会出现在的每页的固定位置。fixed 属性会创建新的层叠上下文。当元素祖先的 transform 属性非 none 时，容器由视口改为该祖先。
- sticky：盒位置根据正常流计算(这称为正常流动中的位置)，然后相对于该元素在流中的 flow root（BFC）和 containing block（最近的块级祖先元素）定位。在所有情况下（即便被定位元素为 table 时），该元素定位均不对后续元素造成影响。当元素 B 被粘性定位时，后续元素的位置仍按照 B 未定位时的位置来确定。position: sticky 对 table 元素的效果与 position: relative 相同。

# JavaScript/ES
## 什么是闭包（closure），为什么使用闭包？（待补充）
闭包是函数和声明该函数的词法环境的组合。词法作用域中使用的域，是变量在代码中声明的位置所决定的。闭包是即使被外部函数返回，依然可以访问到外部（封闭）函数作用域的函数。
1. 函数嵌套函数
2. 函数内部可以引用外部的参数和变量
3. 参数和变量不会被垃圾回收机制回收

## 继承



## AMD/CMD/commonJS
```
//AMD
define(function () {
    var add = function (x, y) {
        return x + y;
    }
    return {
        add: add
    };
})

require(['./add', './square'], function (addModule, squareModule) {
    addModule.add(1, 1);
    squareModule.square(3);
})

//CMD
define(function (require, exports, module) {
    var add = function (x, y) {
        return x + y;
    };
    module.exports = {
        add: add
    }
});

define(function (require, exports, module) {
    var addModule = require('./add');
    console.log(addModule.add(1, 1));
});
```

AMD 与 CMD 的区别
1. CMD推崇依赖就近，AMD推崇依赖前置
2. AMD是提前执行，CMD是延迟执行
3. AMD是将需要将模块加载完成再执行代码，而CMD是在require的时候加载模块，加载完成再去执行

```
//commonjs
var add = function (x, y) {
    return x + y;
};
module.exports.add = add;
// 引入模块的方式
var add = require('./add.js');
console.log(add.add(1, 1));
```

## 使用let、var和const创建变量有什么区别
- 用var声明的变量的作用域是它当前的执行上下文，它可以是嵌套的函数，也可以是声明在任何函数外的变量。let和const是块级作用域，意味着它们只能在最近的一组花括号（function、if-else 代码块或 for 循环中）中访问。
- var会使变量提升，这意味着变量可以在声明之前使用。let和const不会使变量提升，提前使用会报错。
- 用var重复声明不会报错，但let和const会。
- let和const的区别在于：let允许多次赋值，而const只允许一次。

## 你对 Promises 及其 polyfill 的掌握程度如何？
掌握它的工作原理。Promise是一个可能在未来某个时间产生结果的对象：操作成功的结果或失败的原因（例如发生网络错误）。 Promise可能处于以下三种状态之一：fulfilled、rejected 或 pending。 用户可以对Promise添加回调函数来处理操作成功的结果或失败的原因。

一些常见的 polyfill 是$.deferred、Q 和 Bluebird，但不是所有的 polyfill 都符合规范。ES2015 支持 Promises，现在通常不需要使用 polyfills。

- 优点：

避免可读性极差的回调地狱。
使用.then()编写的顺序异步代码，既简单又易读。
使用Promise.all()编写并行异步代码变得很容易。

- 缺点：

轻微地增加了代码的复杂度（这点存在争议）。
在不支持 ES2015 的旧版浏览器中，需要引入 polyfill 才能使用。

## 柯里化
柯里化（currying）是一种模式，其中具有多个参数的函数被分解为多个函数，当被串联调用时，将一次一个地累积所有需要的参数。这种技术帮助编写函数式风格的代码，使代码更易读、紧凑。值得注意的是，对于需要被 curry 的函数，它需要从一个函数开始，然后分解成一系列函数，每个函数都需要一个参数。

```
function curry(fn) {
  if (fn.length === 0) {
    return fn;
  }

  function _curried(depth, args) {
    return function(newArgument) {
      if (depth - 1 === 0) {
        return fn(...args, newArgument);
      }
      return _curried(depth - 1, [...args, newArgument]);
    };
  }

  return _curried(fn.length, []);
}

function add(a, b) {
  return a + b;
}

var curriedAdd = curry(add);
var addFive = curriedAdd(5);

var result = [0, 1, 2, 3, 4, 5].map(addFive); // [5, 6, 7, 8, 9, 10]
```

## 详细地解释 Ajax
Ajax（asynchronous JavaScript and XML）是使用客户端上的许多 Web 技术，创建异步 Web 应用的一种 Web 开发技术。借助 Ajax，Web 应用可以异步（在后台）向服务器发送数据和从服务器检索数据，而不会干扰现有页面的显示和行为。通过将数据交换层与表示层分离，Ajax 允许网页和扩展 Web 应用程序动态更改内容，而无需重新加载整个页面。实际上，现在通常将 XML 替换为 JSON，因为 JavaScript 对 JSON 有原生支持优势。

XMLHttpRequest API 经常用于异步通信。此外还有最近流行的fetch API。

```
let xmlhttp
if (window.XMLHttpRequest) {
    //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
    xmlhttp = new XMLHttpRequest()
} else {
    // IE6, IE5 浏览器执行代码
    xmlhttp = new ActiveXObject("Microsoft.XMLHTTP")
}
xmlhttp.onreadystatechange = () => {
    if (xmlhttp.readyState === 4 && xmlhttp.status === 200) {
        document.getElementById("myDiv").innerHTML = xmlhttp.responseText
    }
}
xmlhttp.open("GET", "/ajax/test.txt", true)
xmlhttp.send()
```

## Ajax和Fetch区别
- ajax是使用XMLHttpRequest对象发起的，但是用起来很麻烦，所以ES6新规范就有了fetch，fetch发一个请求不用像ajax那样写一大堆代码。
- 使用fetch无法取消一个请求，这是因为fetch基于Promise，而Promise无法做到这一点。
- 在默认情况下，fetch不会接受或者发送cookies
- fetch没有办法原生监测请求的进度，而XMLHttpRequest可以
- fetch只对网络请求报错，对400，500都当做成功的请求，需要封装去处理
- fetch由于是ES6规范，兼容性上比不上XMLHttpRequest

## 深拷贝
```
function deepCopy(s) {
    const d = {}
    for (let k in s) {
        if (typeof s[k] == 'object') {
            d[k] = deepCopy(s[k])
        } else {
            d[k] = s[k]
        }
    }

    return d
}
```

## 如何实现文件断点续传
断点续传最核心的内容就是把文件“切片”然后再一片一片的传给服务器，但是这看似简单的上传过程却有着无数的坑。

首先是文件的识别，一个文件被分成了若干份之后如何告诉服务器你切了多少块，以及最终服务器应该如何把你上传上去的文件进行合并，这都是要考虑的。

因此在文件开始上传之前，我们和服务器要有一个“握手”的过程，告诉服务器文件信息，然后和服务器约定切片的大小，当和服务器达成共识之后就可以开始后续的文件传输了。

前台要把每一块的文件传给后台，成功之后前端和后端都要标识一下，以便后续的断点。

当文件传输中断之后用户再次选择文件就可以通过标识来判断文件是否已经上传了一部分，如果是的话，那么我们可以接着上次的进度继续传文件，以达到续传的功能。 有了HTML5 的 File api之后切割文件比想想的要简单的多的多。

只要用slice 方法就可以了

```
var packet = file.slice(start, end);
```

参数start是开始切片的位置，end是切片结束的位置 单位都是字节。通过控制start和end 就可以是实现文件的分块。如：

```
file.slice(0,1000);
file.slice(1000,2000);
file.slice(2000,3000);
// ......
```

在把文件切成片之后，接下来要做的事情就是把这些碎片传到服务器上。 如果中间掉线了，下次再传的时候就得先从服务器获取上一次上传文件的位置，然后以这个位置开始上传接下来的文件内容。

## 首屏时间、白屏时间
Performance 接口可以获取到当前页面中与性能相关的信息。
该类型的对象可以通过调用只读属性 window.performance 来获得。
白屏时间：
```
performance.timing.responseStart - performance.timing.navigationStart
```
首屏时间
```
window.onload = () => {
    new Date() - performance.timing.responseStart
}
```

## escape、encodeURI和encodeURIComponent的区别
### escape
对字符串(string)进行编码(而另外两种是对URL)，作用是让它们在所有电脑上可读。
编码之后的效果是%XX或者%uXXXX这种形式。
其中 ASCII字母、数字、@*/+ ，这几个字符不会被编码，其余的都会。
最关键的是，当你需要对URL编码时，请忘记这个方法，这个方法是针对字符串使用的，不适用于URL。
### encodeURI和encodeURIComponent
对URL编码是常见的事，所以这两个方法应该是实际中要特别注意的。
它们都是编码URL，唯一区别就是编码的字符范围，其中
encodeURI方法**不会**对下列字符编码 ASCII字母、数字、~!@#$&*()=:/,;?+'
encodeURIComponent方法**不会**对下列字符编码 ASCII字母、数字、~!*()'
也就是encodeURIComponent编码的范围更广，会将http://XXX中的//也编码，会导致URL不可用。(其实java中的URLEncoder.encode(str,char)也类似于这个方法，会导致URL不可用)


## Javascript垃圾回收方法
- 标记清除（mark and sweep）

这是JavaScript最常见的垃圾回收方式，当变量进入执行环境的时候，比如函数中声明一个变量，垃圾回收器将其标记为“进入环境”，当变量离开环境的时候（函数执行结束）将其标记为“离开环境”。

垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在标记的就是要删除的变量了

- 引用计数(reference counting)

在低版本IE中经常会出现内存泄露，很多时候就是因为其采用引用计数方式进行垃圾回收。引用计数的策略是跟踪记录每个值被使用的次数，当声明了一个变量并将一个引用类型赋值给该变量的时候这个值的引用次数就加1，如果该变量的值变成了另外一个，则这个值得引用次数减1，当这个值的引用次数变为0的时 候，说明没有变量在使用，这个值没法被访问了，因此可以将其占用的空间回收，这样垃圾回收器会在运行的时候清理掉引用次数为0的值占用的空间。

在IE中虽然JavaScript对象通过标记清除的方式进行垃圾回收，但BOM与DOM对象却是通过引用计数回收垃圾的，
也就是说只要涉及BOM及DOM就会出现循环引用问题。

## Mixin模式
Mixin 指的是多个对象合成一个新的对象，新对象具有各个组成成员的接口。
```
function mix(...mixins) {
  class Mix {
    constructor() {
      for (let mixin of mixins) {
        copyProperties(this, new mixin()); // 拷贝实例属性
      }
    }
  }

  for (let mixin of mixins) {
    copyProperties(Mix, mixin); // 拷贝静态属性
    copyProperties(Mix.prototype, mixin.prototype); // 拷贝原型属性
  }

  return Mix;
}

function copyProperties(target, source) {
  for (let key of Reflect.ownKeys(source)) {
    if ( key !== 'constructor'
      && key !== 'prototype'
      && key !== 'name'
    ) {
      let desc = Object.getOwnPropertyDescriptor(source, key);
      Object.defineProperty(target, key, desc);
    }
  }
}
```

上面代码的mix函数，可以将多个对象合成为一个类。使用的时候，只要继承这个类即可。
```
class DistributedEdit extends mix(Loggable, Serializable) {
  // ...
}
```

# DOM

# HTTP

# 安全
## XSS
### XSS是什么
XSS是一种经常出现在web应用中的计算机安全漏洞，它允许恶意web用户将代码植入到提供给其它用户使用的页面中。<br>
比如这些代码包括HTML代码和客户端脚本。攻击者利用XSS漏洞旁路掉访问控制——例如同源策略(same origin policy)。<br>
这种类型的漏洞由于被黑客用来编写危害性更大的网络钓鱼(Phishing)攻击而变得广为人知。<br>
对于跨站脚本攻击，黑客界共识是：跨站脚本攻击是新型的“缓冲区溢出攻击“，而JavaScript是新型的“ShellCode”。

```
示例：
<script>alert(document.cookie)</script>
```

### 特点
能注入恶意的HTML/JavaScript代码到用户浏览的网页上，从而达到Cookie资料窃取、会话劫持、钓鱼欺骗等攻击。
<攻击代码不一定（非要）在 <script></script> 中>
 
### 原因
* Web浏览器本身的设计不安全。浏览器能解析和执行JS等代码，但是不会判断该数据和程序代码是否恶意。
* 输入和输出是Web应用程序最基本的交互，而且网站的交互功能越来越丰富。如果在这过程中没有做好安全防护，很容易会出现XSS漏洞。
* 程序员水平参差不齐，而且大都没有过正规的安全培训，没有相关的安全意识。
* XSS攻击手段灵活多变。

### 危害
* 盗取各类用户帐号，如机器登录帐号、用户网银帐号、各类管理员帐号
* 控制企业数据，包括读取、篡改、添加、删除企业敏感数据的能力
* 盗窃企业重要的具有商业价值的资料
* 非法转账
* 强制发送电子邮件
* 网站挂马
* 控制受害者机器向其它网站发起攻击

### 如何防范
* 将重要的cookie标记为http only, 这样的话Javascript 中的document.cookie语句就不能获取到cookie了.
* 表单数据规定值的类型，例如：年龄应为只能为int、name只能为字母数字组合。。。。
* 对数据进行Html Encode 处理
* 过滤或移除特殊的Html标签， 例如: `<script>, <iframe> , &lt; for <, &gt; for >, &quot for`
* 过滤JavaScript 事件的标签。例如 "onclick=", "onfocus" 等等。

参考资料：<br>
https://www.cnblogs.com/phpstudy2015-6/p/6767032.html<br>
https://www.cnblogs.com/443855539-wind/p/6055816.html<br>
https://baike.baidu.com/item/XSS%E6%94%BB%E5%87%BB/954065?fr=aladdin


## CSRF
CSRF（Cross-site request forgery）跨站请求伪造，也被称为“One Click Attack”或者Session Riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。尽管听起来像跨站脚本（XSS），但它与XSS非常不同，XSS利用站点内的信任用户，而CSRF则通过伪装来自受信任用户的请求来利用受信任的网站。与XSS攻击相比，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比XSS更具危险性。

### 特点
* 依靠用户标识危害网站
* 利用网站对用户标识的信任
* 欺骗用户的浏览器发送HTTP请求给目标站点
* 另外可以通过IMG标签会触发一个GET请求，可以利用它来实现CSRF攻击。

### 防御
* 通过referer、token或者验证码来检测用户提交。
* 尽量不要在页面的链接中暴露用户隐私信息。
* 对于用户修改删除等操作最好都使用post操作 。
* 避免全站通用的cookie，严格设置cookie的域。


# React

# TypeScript

# Webpack

## 有哪些常见的Loader？他们是解决什么问题的？

- file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件
- url-loader：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去
- source-map-loader：加载额外的 Source Map 文件，以方便断点调试
- image-loader：加载并且压缩图片文件
- babel-loader：把 ES6 转换成 ES5
- css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
- style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。
- eslint-loader：通过 ESLint 检查 JavaScript 代码

## 有哪些常见的Plugin？他们是解决什么问题的？
- define-plugin：定义环境变量
- commons-chunk-plugin：提取公共代码
- uglifyjs-webpack-plugin：通过UglifyES压缩ES6代码

## webpack的构建流程是什么?从读取配置到输出文件这个过程尽量说全
Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：

1. 初始化参数：从配置文件和 Shell 语句中读取与合并参数，得出最终的参数；
2. 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译；
3. 确定入口：根据配置中的 entry 找出所有的入口文件；
4. 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理；
5. 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系；
6. 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；
7. 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

在以上过程中，Webpack 会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果。

## 是否写过Loader和Plugin？描述一下编写loader或plugin的思路？
1. Loaders是用来告诉webpack如何转化处理某一类型的文件，并且引入到打包出的文件中
2. Plugin是用来自定义webpack打包过程的方式，一个插件是含有apply方法的一个对象，通过这个方法可以参与到整个webpack打包的各个流程(生命周期)。

Loader像一个"翻译官"把读到的源文件内容转义成新的文件内容，并且每个Loader通过链式操作，将源文件一步步翻译成想要的样子。

编写Loader时要遵循单一原则，每个Loader只做一种"转义"工作。 每个Loader的拿到的是源文件内容（source），可以通过返回值的方式将处理后的内容输出，也可以调用this.callback()方法，将内容返回给webpack。 还可以通过 this.async()生成一个callback函数，再用这个callback将处理后的内容输出出去。 此外webpack还为开发者准备了开发loader的工具函数集——loader-utils。

相对于Loader而言，Plugin的编写就灵活了许多。 webpack在运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

## 如何利用webpack来优化前端性能？（提高性能和体验）
用webpack优化前端性能是指优化webpack的输出结果，让打包的最终结果在浏览器运行快速高效。

- 压缩代码。删除多余的代码、注释、简化代码的写法等等方式。可以利用webpack的UglifyJsPlugin和ParallelUglifyPlugin来压缩JS文件， 利用cssnano（css-loader?minimize）来压缩css
- 利用CDN加速。在构建过程中，将引用的静态资源路径修改为CDN上对应的路径。可以利用webpack对于output参数和各loader的publicPath参数来修改资源路径
- 删除死代码（Tree Shaking）。将代码中永远不会走到的片段删除掉。可以通过在启动webpack时追加参数--optimize-minimize来实现
- 提取公共代码。

## 如何提高webpack的构建速度？
- 多入口情况下，使用CommonsChunkPlugin来提取公共代码
- 通过externals配置来提取常用库
- 利用DllPlugin和DllReferencePlugin预编译资源模块 通过DllPlugin来对那些我们引用但是绝对不会修改的npm包来进行预编译，再通过DllReferencePlugin将预编译的模块加载进来。
- 使用Happypack 实现多线程加速编译
- 使用webpack-uglify-parallel来提升uglifyPlugin的压缩速度。 原理上webpack-uglify-parallel采用了多核并行压缩来提升压缩速度
- 使用Tree-shaking和Scope Hoisting来剔除多余代码

# 面试题

# leetcode



# 简历规范
1. 体现前端工程化
2. 体现职责

# 公共项目
1. 国际化项目
2. 组件化平台项目
3. 创建代码命令行工具
4. 技术文章