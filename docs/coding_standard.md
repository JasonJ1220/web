[TOC]

# 编程风格
## 基本格式化
- 层级缩进：推荐 4个空格字符;
- 语句结尾需要加上分号，避免没必要的BUG；
- 行的长度：限定每行的长度在80个字符之内；
- 换行：在运算符后面换行，第二行追加两个缩进；变量赋值换行特殊，第二行的位置应当和赋值运算符的位置保持对齐；
```
// Good: Break after operator, following line indented two levels
callAFunction(document, element, window, "some string value", true, 123,
        navigator);
var result = something + anotherThing + yesAnotherThing + someThingElse +
             anotherSomethingElse;
// Bad: Following line indented only one level
callAFunction(document, element, window, "some string value", true, 123,
    navigator);
// Bad: Break before operator
callAFunction(document, element, window, "some string value", true, 123
        , navigator);
```

- 空行
在方法之间；
在方法中局部变量和第一条语句之前；
在多行或单行注释之前；
在方法内的逻辑片段之间；
- 命名
首先要语义化，使用驼峰式命名法，小驼峰即首字母小写，之后每个单词首字母大写，通常用于给变量和方法命名；大驼峰即首字母大写，之后同小驼峰；
1. 变量和函数的命名：变量名前缀应该是名词（myName），函数名前缀应该是动词（getName），常量全部使用大写字母加下划线（MY_NAME），非常直观。
2. 常量使用全大写命名（MAX_COUNT）；
3. 构造函数使用大驼峰命名法（DateFormat）；

- 字符串：
推荐使用双引号括起字符串；不要使用“\”来连接字符串，推荐使用“+”；
```
// Good
var name = "Nicholas";
// Bad: Single quotes
var name = 'Nicholas';
// Bad: Wrapping to second line
var longString = "Here's the story, of a man \
named Brady.";
```

- 数字：
不要省略小数点之前或之后的数字；

- null
用来初始化一个变量，这个变量可能赋值一个对象。
用来和一个已经初始化的变量比较，这个变量可以是也可以不是一个对象。
当函数的参数期望是对象时，用作参数传入。
当函数的返回值期望是对象时，用作返回值传出。

- undefined
避免在代码中使用undefined；

- 对象直接量
```
//Bad
var book = new Object();
book.title = "a";
book.author = "b";
//Good
var book = {
    title:"a",
    author:"b"
};
```

- 数组直接量
```
//Bad
var colors = new Array("red","green","blue");
var numbers = new Array(1,2,3,4);
//Good
var colors = ["red","green","blue"];
var numbers = [1,2,3,4];
```

## 注释
注释：单行注释用//，多行注释用/**/，单行注释//之后总有一个空格，下一行代码缩进层级应与注释保持一致。难以理解或是容易被认为是错误的代码前应该加上注释。
```
/*
 *注意前面的空格（不好）
 * 注意前面的空格（好）
 *／
```

## 语句和表达式
- 花括号使用
```
if(  ){
    //....
}else{
    //....
}
```

- switch
```
switch(condition){
    //依次执行
    case "first":
    /*fall through*/
    case "second":
    //代码
    break;
    case "third":
    break;
    //如果不需要默认行为，请省略default
    default:
    break;
}
```

- for
尽可能避免使用continue，单页没有理由完全禁止使用，可根据代码的可读性来决定。

- 避免使用with语句;
- for-in 语句是用来遍历对象的属性值的，遍历数组请使用for循环;
for-in循环不仅遍历对象的实例属性，同样也遍历从原型链继承来的属性。推荐如下写法(如果想包含对原型链的遍历则给出注释)：
```
for(var prop in object){
    if(object.hasOwnProperty(prop)){
        console.log("Property name is:" + prop);
        console.log("Property value is:" + object[prop]);
    }
}
```

## 变量、函数和运算符
- 将局部变量的定义作为函数内第一条语句；并且合并var语句；
- 请先声明函数再使用，而且函数声明不应该出现在条件语句块之内。
- 严格模式：只在需要的地方添加，尽量不要全局使用，除非你已经明确都要严格模式。
```
// 推荐使用方式
function fn() {
    "use strict";
    //代码
}
//或者
(function() {
    "use strict";
    function fn1() {
        //....
    }
    function fn2() {
        //....
    }
})();
```

- 立即执行函数写法；
```
var value = (function(){
    //函数体
    return {
        result:"hi"
    }
}());
```

- 判断两个值是否相等时，使用===或!==，不推荐使用==和!=，因为javascript具有强制类型转换机制会造成意想不到的结果。

- 尽量不使用 eval , Function, 以及不要给 setTimeout，setInterval 传入方法字符串（当然如果实在没有别的方法完成当前任务，推荐使用eval）;如下：
```
var functionA=new Function( "alert('Hi')" )
functionA()
setTimeout( "document.body.style.background='red'",100 );
setInterval( "document.write('It is now"+ (new Date()) +"')",1000 )
```

- 禁止使用原始包装类型来创建对象

# 编程实践
## UI层的松耦合
- 将JavaScript从CSS中抽离；禁用CSS expression；
- 将CSS从JavaScript中抽离；用 js 修改样式的时候，采用添加或者移除类名的方式，而不是直接使用.style.color .style.cssText；特殊情况是当你需要给元素定位时，在CSS里无法实现，可以在JavaScript中实现；
```
// Bad
element.style.color = "red";
element.style.left = "10px";
element.style.top = "100px";
element.style.visibility = "visible";
element.style.cssText = "color: red; left: 10px; top: 100px; visibility: hidden";
//样式
.reveal {
    color: red;
    left: 10px;
    top: 100px;
    visibility: visible;
}
// Good - HTML5
element.classList.add("reveal");
// Good - jQuery
$(element).addClass("reveal");
```

- 在HTML中不要使用javascript；比如嵌入在HTML中的onClick
```
<!-- Bad -->
<button onclick="doSomething()" id="action-btn">Click Me</button>
```

- 将HTML从JavaScript中 抽离；
```
// Bad
var div = document.getElemenetById("my-div");
div.innerHTML = "<h3>Erroe</h3>";
```
- 解决方法：1、从服务器加载；
```
function loadDialog(name, oncomplete) {
    var xhr = new XMLHttpRequest();
    xhr.open("get", "/js/dialog/" + name, true);
    xhr.onreadystatechange = function() {
        if (xhr.readyState == 4 && xhr.status == 200) {
            var div = document.getElementById("dlg-holder");
            div.innerHTML = xhr.responseText;
            oncomplete();
        } else {
            // handle error
        }
    };
    xhr.send(null);
}
```

- 解决方法：2、使用简单客户端模板；
```
// 模板
<li><a href="%s">%s</a></li>
function sprintf(text) {
    var i=1, args=arguments;
    return text.replace(/%s/g, function() {
        return (i < args.length) ? args[i++] : "";
    });
}
// usage
var result = sprintf(templateText, "/item/4", "Fourth item");
```

- 解决方法：3、使用复杂客户端模板； 比如使用 Handlebars；
```
// 模板放在 script 中
<script type="text/x-handlebars-template" id="list-item">
    <li><a href="{{url}}">{{text}}</a></li>
</script>
function addItem(url, text) {
    var mylist = document.getElementById("mylist"),
    script = document.getElementById("list-item"),
    templateText = script.text,
    template = Handlebars.compile(script.text),
    div = document.createElement("div"),
    result;
    result = template({
        text: text,
        url: url
    });
    div.innerHTML = result;
    list.appendChild(div.firstChild);
}
// 使用
addItem("/item/4", "Fourth item");
```
## 避免使用全局变量
- 避免使用全局变量，存在问题：命名冲突；代码的脆弱性；难以测试；
- 意外的全局变量：忘了var;  解决办法： 使用 JSLint 或者 JSHint ;
- 单全局变量： 比如YUI定义了一个YUI全局变量； jQuery 定义了两个全局变量，$ 和 jQuery;
- 命名空间： 比如 YUI.event 下的所有方法都是和事件相关的。
- 模块： 例如AMD模块；传入参数是模块名称（可省略，直接把js文件作为模块名称），依赖和工厂方法；
```
define("module-name", [ "dependency1", "dependency2" ],
     function(dependency1, dependency2) {
          var Books = {};
          Books.MaintainableJavaScript = {
          author: "Nicholas C. Zakas"
     };
    return Books;
  });
```
- 零全局变量：使用前提是内部代码与外部代码没有任何依赖关系。
```
  (function(win) {
        var doc = win.document;

    })(window)
```
## 事件处理

```
//Bad
function handleClick(event) {
    var popup = document.getElementById("popup");
    popup.style.left = event.clientX + "px";
    popup.style.top = event.clientY + "px";
    popup.className = "reveal";
}
// addListener() from Chapter 7
addListener(element, "click", handleClick);
```

- 隔离应用逻辑
```
var MyApplication = {
    handleClick: function(event) {
        this.showPopup(event);
    },
    showPopup: function(event) {
        var popup = document.getElementById("popup");
        popup.style.left = event.clientX + "px";
        popup.style.top = event.clientY + "px";
        popup.className = "reveal";
    }
};
addListener(element, "click", function(event) {
    MyApplication.handleClick(event);
});
```

- 不要分发事件对象
```
// Good
var MyApplication = {
    handleClick: function(event) {
        this.showPopup(event.clientX, event.clientY);
    },
    showPopup: function(x, y) {
        var popup = document.getElementById("popup");
        popup.style.left = x + "px";
        popup.style.top = y + "px";
        popup.className = "reveal";
    }
};
addListener(element, "click", function(event) {
    MyApplication.handleClick(event); // this is okay
});
```
当处理事件时候，最好让事件处理程序成为接触到event对象的唯一的函数。
```
// Good
var MyApplication = {
    handleClick: function(event) {
        // 阻止默认行为
        event.preventDefault();
        //阻止冒泡
        event.stopPropagation();
        // pass to application logic
        this.showPopup(event.clientX, event.clientY);
    },
    showPopup: function(x, y) {
        var popup = document.getElementById("popup");
        popup.style.left = x + "px";
        popup.style.top = y + "px";
        popup.className = "reveal";
    }
};
addListener(element, "click", function(event) {
    MyApplication.handleClick(event); // this is okay
});
```
## 避免空比较
- 下面是不好的示范，你很多都与null不相等，包括数字，字符串，对象等，从而带来隐藏的错误。只有当期待的值是null的时候，可以和null进行比较。
```
//Bad
var Controller = {
    process: function(items) {
        if (items !== null) { // Bad
            items.sort();
            items.forEach(function(item) {
                // do something
            });
        }
    }
};
```
这段代码中，process方法显然希望items是一个数组，然后当items的值为1时，不等于null，依然进入if语句内了，执行到items.sort()就报错了;
- 检测基本类型使用typeof, 引用类型使用instanceof
```
//检测字符串
if(typeof name === "string"){
    //....
}
//检测数字
if(typeof count === "number"){
    //....
}
//检测布尔值
if(typeof isComplated === "boolean"){
    //....
}
//检测数组
function isArray(value) {
    return Object.prototype.toString.call(value) === "[object Array]";
}
//检测引用值
//日期
if(value instanceof Date){
    //....
}
//error
if(value instanceof Error){
    //....
}
//检测函数
if(typeof myFunc === "function"){
    //....
}
//检测属性是否存在
// Bad，当属性值是0, "", false, null或者 undefined 
if (object[propertyName]) {
    // do something
}
// Bad: Compare against null
if (object[propertyName] != null) {
    // do something
}
// Bad: Compare against undefined
if (object[propertyName] != undefined) {
    // do something
}
var object = {
    count: 0,
    related: null
};
// Good
if ("count" in object) {
    // this executes
}
if ("related" in object) {
    // this executes
}
//当你只想检查实例对象的某个属性是否存在的时候使用hasOwnProperty()
// Good for all non-DOM objects
if (object.hasOwnProperty("related")) {
    //this executes
}
// Good when you're not sure if a DOM object
if ("hasOwnProperty" in object && object.hasOwnProperty("related")) {
    //this executes
}
```

## 将配置数据从代码中分离出来
什么是配置数据？ 就是JavaScript代码中有可能发生改变的。比如：url, 需要展现给用户的字符串， 重复的值， 设置等， 任何可能发生变化的值。

## 抛出自定义错误
- 推荐在错误消息中包含函数名称，以及函数失败的原因。
```
function getDivs(element) {
    if (element && element.getElementsByTagName) {
        return element.getElementsByTagName("div");
    } else {
        throw new Error("getDivs(): Argument must be a DOM element.");
    }
}
```

-  何时抛出错误? 辨别代码中哪些部分在特定的情况下最优可能导致失败，并只在哪些地方抛出错误才是关键所在。我们的目的不是防止错误，而是错误发生的时候更加容易地调试。
```
// Bad 太多检查
function addClass(element, className) {
    if (!element || typeof element.className != "string") {
        throw new Error("addClass(): First argument must be a DOM element.");
    }
    if (typeof className != "string") {
        throw new Error("addClass(): Second argument must be a string.");
    }
    element.className += " " + className;
}
// Good 第二个参数是null 或者一个数字或者一个布尔值时是不会抛出错误的
function addClass(element, className) {
    if (!element || typeof element.className != "string") {
        throw new Error("addClass(): First argument must be a DOM element.");
    }
    element.className += " " + className;
}
```

-  使用try catch 语句，当在try中 发生一个错误，会立即跳到catch语句，传入一个错误对象。还可以增加一个finally 模块，不管错误发不发生都会执行。如果在try块中包含一个return语句，实际上它也是等到finally执行完成之后才能返回。
```
try {
    somethingThatMightCauseAnError();
} catch (ex) {
    handleError(ex);
} finally {
    continueDoingOtherStuff();
}
```

- ECMA-262规范指出了7种错误类型
1）Error ： 所有错误的基本类型
2）EvalError ： 通过eval()函数执行代码出错时抛出
3）RangeError ： 一个数字超出了它的边界时抛出，比如试图创建一个长度为-20的数组（new Array(-20)）。
4）ReferenceError： 期望的对象不存在时抛出，比如试图在一个null对象引用上调用一个函数
5）SyntaxError： 语法错误
6）TypeError： 变量不是期望的类型时抛出错误
7）URIError： 给encodeURI()、encodeURIComponent()、decodeURI()、decodeURIComponent()等函数传递格式非法的URI字符串时抛出

-自定义错误
```
function MyError(message){
    this.message = message;
}
MyError.protoType = new Error();
```

## 不是你的对象不要动
以下这些对象不要尝试去修改他们，因为他们已经存在了： 
- 原生对象（Object, Array）
- DOM对象（document）
- 浏览器对象模型（BOM）
- 对象（window）
- 类库的对象 

对待他们的原则是：
- 不覆盖方法； 
- 不新增方法； 
- 不删除方法； 

下面是一些不好的示例：
```
document.getElementById = function() {
    return null; // talk about confusing
};
document._originalGetElementById = document.getElementById;
document.getElementById = function(id) {
    if (id == "window") {
        return window;
    } else {
        return document._originalGetElementById(id);
    }
};
document.sayImAwesome = function() {
    alert("You're awesome.");
};
Array.prototype.reverseSort = function() {
    return this.sort().reverse();
};
YUI.doSomething = function() {
    // code
};
document.getElementById = null;
```

- 更好的途径
- 基于对象继承
- 基于类型继承
- 门面模式

```
//基于对象的继承
var person = {
    name: "Nicholas",
    sayName: function() {
    alert(this.name);
}
};
var myPerson = Object.create(person);
myPerson.sayName(); // pops up "Nicholas"

//基于类型的继承，在开发者定义了构造函数的前提下；
function MyError(message) {
    this.message = message;
}
MyError.prototype = new Error();
var error = new MyError("Something bad happened.");
console.log(error instanceof Error); // true
console.log(error instanceof MyError); // true

//门面模式  与适配器唯一不同是 其创建新的接口，后者实现已经存在的接口
function DOMWrapper(element) {
    this.element = element;
}
DOMWrapper.prototype.addClass = function(className) {
    this.element.className += " " + className;
};
DOMWrapper.prototype.remove = function() {
    this.element.parentNode.removeChild(this.element);
};

// Usage
var wrapper = new DOMWrapper(document.getElementById("my-div"));

// add a CSS class
wrapper.addClass("selected");

// remove the element
wrapper.remove();
```

- 阻止修改
1. 阻止拓展(preventExtension)
禁止为对象添加属性和方法，但已存在的属性和方法是可以被修改或删除的。
```
var person = {
    name:"kevin"
};
Object.preventExtension(person);
```
2. 密封对象(seal)
类似preventExtension，而且禁止对象删除已存在的属性和方法。
```
var person = {
    name:"kevin"
};
Object.seal(person);
```
3. 冻结对象(freeze)
类似seal，而且禁止对象修改已存在的属性和方法（所有字段均为只读）
```
var person = {
    name:"kevin"
};
Object.freeze(person);
```
