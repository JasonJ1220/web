[TOC]

# HTML
## HTML标签

### canvas
```js
//画线
function drawLines(points) {
  //创建画布
  var canvas = new Canvas(100,100);  
  //获取上下文
  var ctx = canvas.getContext('2d'); 
  ctx.fillStyle="#ffffff";
  ctx.fillRect(0,0,100,100);  //Draw background
  ctx.strokeStyle="#ff0000";  //Set pen's color
  ctx.beginPath();
  
  points.forEach((point,index)=>{
    if(index===0){
      ctx.moveTo.apply(ctx,point);
    }
    else{
      ctx.lineTo.apply(ctx,point);
    }
  });
  //This is an example, it will draw a line from point(0,0) to 
  //Don't delete or modify the following code
  ctx.stroke();              //Draw the path you made above
  return canvas.toDataURL(); //Returns the image data
```

# JavaScript
## 基础
### call/apply/bind
call和apply是为了动态改变this而出现的，当一个object没有某个方法，但是其他的有，我们可以借助call或apply用其它对象的方法来操作。

**obj.doSomething.call(thisObj, arg1, arg2);**

**obj.doSomething.apply(thisObj, [arg1, arg2]);**

**obj.doSomething.bind(thisObj)();**

当参数明确时可用call, 当参数不明确时可用apply给合arguments。
如：
```js
function cat(){};
cat.prototype={     
    food:"fish",     
    say: function(){           
        alert("I love "+this.food);     
    }};
var blackCat = new cat;
blackCat.say();
var whiteDog = {food:"bone"};

blackCat.say.call(whiteDog);
```
基础应用：
```js
var add = function (a, b) {
    return a + b;
}
add.call(null, 1, 2);

var add = function (a, b) {
    return a + b;
}
add.apply(null, [1, 2]);
```

bind 应用
```js
var foo = {
    bar : 1,
    eventBind: function(){
        $('.someClass').on('click',function(event) {
            /* Act on the event */
            console.log(this.bar);      //1
        }.bind(this));
    }
}
```

比较：

```js
//bind 是非立即执行方法，需要调用
var obj = {
    x: 81,
};
 
var foo = {
    getX: function() {
        return this.x;
    }
}
 
console.log(foo.getX.bind(obj)());  //81
console.log(foo.getX.call(obj));    //81
console.log(foo.getX.apply(obj));   //81
```

## 编码规范
## 面向对象
### 公有属性和公共方法
属性在构造函数内赋值到this上，方法通过原型模式赋值。

```js
function User(name,age){
  this.name = name;//公有属性
  this.age = age;
}
User.prototype.getName = function(){//公有方法
  return this.name;
}
var user = new User('name',26);
console.log(user.getName());//output:name
```

### 私有属性和私有方法
属性在构造函数内初始化为变量，方法直接初始化为成员。
```js
function User(name,age){
  var name = name;//私有属性
  var age = age;
  function alertAge(){//私有方法
     alert(age);
  }
  alertAge(age); //弹出26
}
var user = new User('name',26);
```

### 静态属性和静态方法
直接给对象构造器赋值。
```js
function User(){}
User.age = 26;//静态属性
User.myname = 'name';
//静态方法
User.getName =function(){
  return this.myname;
  //如果这里使用this.name，返回的将是User，所有改用了myname，
}
console.log(User.getName());//output:name
```

### get/set方法
```js
function User(name,age){
  var name = name;//私有属性
  var age = age;
  this.getName = function(){ //get方法
     return name;//私有属性和方法不能使用this调用
  }
}
var user = new User('name',26);
console.log(user.getName());//output:name
```

### 函数声明和函数表达式

```js
alert(sum(3, 11)); //弹出14
function sum(x, y) {
    return x + y;
}

alert(sum(3, 11)); // unexpected identifier 意外标识符错误
var sum = function (x, y) {
    return x + y;
};
//除了什么时候通过变量访问函数这一点区别外，函数声明与函数表达式的语法是等价的。
```

### 合并对象

###

## DOM
### 浏览器相关
- 打开新的浏览窗口

```js
//打开一个新的浏览器窗口或查找一个已命名的窗口
window.open(URL, name, features, replace);
//name属性
//_blank - URL加载到一个新的窗口。这是默认
//_parent - URL加载到父框架
//_self - URL替换当前页面
//_top - URL替换任何可加载的框架集
myWindow = window.open('', '_blank', 'width=200,height=100');
myWindow.document.write("<p>这是'我的窗口'</p>");
myWindow.focus();
```

### DOM相关
1. DOM元素查找

```js
//获取元素
var el = document.getElementById('xxx');
var els = document.getElementsByClassName('highlight');
var els = document.getElementsByTagName('td');
//查找选择器
document.querySelector("#test");
document.querySelectorAll("#test")[0];
// 获取父元素、父节点
var parent = ele.parentElement;
var parent = ele.parentNode;//只读，没有兼容性问题
var offsetParent=ele.offsetParent;//只读，找到最近的有定位的父节点。
// 获取子节点，子节点可以是任何一种节点，可以通过nodeType来判断
var nodes = ele.children;//标准下、非标准下都只含元素类型，但对待非法嵌套的子节点，处理方式与childNodes一致。
var nodes = ele.childNodes;//非标准下：只包含元素类型，不会包含非法嵌套的子节点。
　　　　　　　　　　　　　　　　//标准下：包含元素和文本类型，会包含非法嵌套的子节点。 
//获取元素属性列表
var attr = ele.attributes;

```

2. 修改DOM元素

```js
//创建div元素
var ele=document.createElement("div");  
//创建文本节点
var ele = document.createTextNode('hello world!');
//修改DOM
document.getElementById("p1").innerHTML = "New text!";
//把input的属性type值改变为button
document.getElementById("changes").type = "button";
// 添加、删除子元素
ele.appendChild(el);
ele.removeChild(el);
// 替换子元素
ele.replaceChild(el1, el2);
// 插入子元素
parentElement.insertBefore(newElement, referenceElement);
//克隆元素
//该参数指示被复制的节点是否包括原节点的所有属性和子节点
ele.cloneNode(true) 

//替换元素
$("#me").replaceWith("<div>走上人生巅峰</div>");
$("<div>那是我逝去的青春</div>").replaceAll("#yi");
```


3. 属性操作

```js
// 获取一个{name, value}的数组
var attrs = el.attributes;

// 获取、设置属性
var c = el.getAttribute('class');
el.setAttribute('class', 'highlight');

// 判断、移除属性
el.hasAttribute('class');
el.removeAttribute('class');

// 是否有属性设置
el.hasAttributes();

//设置内容 - text()、html() 、attr()以及 val()
$("#test1").text("Hello world!");
$("#test2").html("<b>Hello world!</b>");
$("#test3").val("Dolly Duck");
$("#w3s").attr("href", "http://www.w3school.com.cn/jquery");
$("#w3s").removeAttr("href");
//添加内容
$("p").append("Some appended text.");
$("p").prepend("Some prepended text.");
$("img").after("Some text after");
$("img").before("Some text before");
//删除内容
$("#div1").remove();
$("#div1").empty();
//操作class
$("div").addClass("important");
$("div").removeClass("important");
$("div").toggleClass("important");

```

4. 包裹DOM元素

```js
//包裹元素
$("p").wrap("<b></b>"); //所有段落标记字体加粗
$("span").wrapInner("<i></i>"); //所有段落中的span标记斜体
```

5. 遍历DOM元素

```js
//遍历元素
$("div").each(function (index) {
    if (index == 2) {
        $(this).addClass("red");
    }
});
```

6. DOM事件

```js
//绑定事件
$("p").on("click", function(){
    alert($(this).text());
});
//一次性事件
$("p").one("click", function () {
    $(this).animate({
        fontSize: "+=6px"
    });
});
//手动触发事件
$("div").on("click", function () {
    $(this).append("ClickTest");
});
$("div").trigger("click");
//焦点事件、失去焦点事件
$("input").focus(function () { //获得焦点
    $("input").css("background-color", "#FFFFCC");
});
$("input").blur(function () { //失去焦点
    $("input").css("background-color", "#D6D6FF");
});

//阻止默认浏览器动作
if (e && e.preventDefault) {
    //阻止默认浏览器动作(W3C)
    e.preventDefault();
} else {
    //IE中阻止函数器默认动作的方式
    window.event.returnValue = false;
    return false;
}
```

7. CSS相关

```js
//js方式
document.getElementById("id").style.color="red"

//返回CSS属性值
$("p").css("color");
//设置CSS属性值
$("p").css("color","red");
$("p").css({ "color": "#ff0011", "background": "blue" });
//元素尺寸
$("#div1").width();
$("#div1").height();
$("#div1").innerWidth();//内部区域宽度（包括补白、不包括边框）
$("#div1").innerHeight();//内部区域高度（包括补白、不包括边框）
$("#div1").outerWidth();
$("#div1").outerHeight();
//遍历元素层级
$("span").parent();
$("span").parents();
$("span").parents("div");
$("span").parentsUntil("div");//查找当前元素的所有的父辈元素，直到遇到匹配的那个元素为止。
$("div").children();
$("div").children("p.1");
$("div").find("span");
$("div").find("*");
$("h2").siblings();
$("h2").siblings("p");
$("h2").next();
$("h2").nextAll();
$("h2").nextUntil("h6");
$("h2").prev();
$("h2").prevAll();
$("h2").prevUntil("h1");
$("div").first();
$("div").last();
$("p").eq(0);
$("p").filter(".intro");
$("p").not(".intro");
//换肤 找出你希望切换的媒体类型（media-type），然后把href设置成新的样式表。
$('link[media="screen"]').attr('href', 'Alternative.css');
```

8. 动画

```js
//隐藏/显示元素
//注意：display:none;
$(selector).hide(speed, callback);
$(selector).show(speed, callback);
//淡入/淡出元素
//注意：display:none;
$(selector).fadeIn(speed, callback);
$(selector).fadeOut(speed, callback);
$(selector).fadeToggle(speed, callback);
$(selector).fadeTo(speed, opacity, callback);
//元素上下滑动
//注意：display:none;
$(selector).slideDown(speed, callback);
$(selector).slideUp(speed, callback);
$(selector).slideToggle(speed, callback);
//动画
$(selector).animate(params, speed, callback);
$("#btn").click(function () {
    $("div").animate({
        height: "200px",
        width: "660px"
    }, "slow", function () {
        $("div").animate({
            height: "80px",
            width: "80px"
        }, "slow")
    });
});
//停止动画
$(selector).stop(stopAll, goToEnd);
$("#flip").click(function () {
    $("#panel").slideToggle(5000);
});
$("#stop").click(function () {
    $("#panel").stop();
});
```


## 对象
### 创建对象
- 对象字面量

```js
var clock = {
    hour: 12,
    minute: 10,
    second: 10,
    showTime: function () {
        alert(this.hour + ":" + this.minute + ":" + this.second);
    }
}
clock.showTime(); //调用
```
- 创建Object实例

```js
var clock = new Object();
 clock.hour = 12;
 clock.minute = 10;
 clock.showHour = function () {
     alert(clock.hour);
 };
 clock.showHour(); //调用
```
- 构造函数模式

```js
 function clock(hour, minute, second) {
     if (this instanceof clock) {
         this.hour = hour;
         this.minute = minute;
         this.second = second;
         this.showTime = function () {
             alert(this.hour + ":" + this.minute + ":" + this.second);
         }
     } else {
         throw new Error("please add 'new' to make a instance");
     }
 }
 //这个new关键字是必须，如果不加，clock就不会当成构造函数调用，而只是一个普通的函数。
 //同时，还会意外地给他的外部作用域即window添加属性，因为此时构造函数内部的this已经映射到了外部作用域了。所以为了安全起见，可以这样创建。
 //缺点：由于this指针在对象实例的时候发生改变指向新的实例。这时新实例的方法也要重新创建，如果n个实例就要n次重建相同的方法。
 var newClock = new clock(12, 12, 12);
 alert(newClock.hour);
```
- 原型模式

```js
  //这种纯原型的模式问题也很明显，所有的属性，方法都是共享的，不能让对象具体化。常常我们想每个对象有自己的属性。
  function clock(hour, minute, second) {}
  clock.prototype.hour = 12;
  clock.prototype.minute = 12;
  clock.prototype.second = 12;
  clock.prototype.showTime = function () {
      alert(this.hour + ":" + this.minute + ":" + this.second);
  }
  //或者
  clock.prototype = {
      constructor: clock, //必须手动设置这个属性，不然就断了与构造函数的联系了。没有实例共享原型的意义了。
      hour: 12,
      minute: 12,
      second: 12,
      showTime: function () {
          alert(this.hour + ":" + this.minute + ":" + this.second)
      }
  }
  var newClock = new clock();
  newClock.showTime();
```
- 构造-原型组合模式

```js
function clock(hour, minute, second) {
     this.hour = hour;
     this.minute = minute;
     this.second = second;
 }

 clock.prototype.showTime = function () {
     alert(this.hour + ":" + this.minute + ":" + this.second);
 }
 var newClock = new clock(12, 12, 12);
 newClock.showTime();
```

### 对象检测
- 判断是否为 null || undefined

```js
function isNone(obj) {
    return obj === null || obj === undefined;
}

Ember.isNone();              // true
Ember.isNone(null);          // true
Ember.isNone(undefined);     // true
Ember.isNone('');            // false
Ember.isNone([]);            // false
Ember.isNone(function() {}); // false
```

- 判断是否为 null || undefined || 空字符串 || 空数组

```js
function isEmpty(obj) {
    var none = isNone(obj);
    if (none) {
      return none;
    }
    
    if (typeof obj.size === 'number') {
      return !obj.size;
    }
    
    var objectType = typeof obj;
    
    if (objectType === 'object') {
      var size = obj.size;
      if (typeof size === 'number') {
        return !size;
      }
    }
    
    if (typeof obj.length === 'number' && objectType !== 'function') {
      return !obj.length;
    }
    
    if (objectType === 'object') {
      var _length = obj.length;
      if (typeof _length === 'number') {
        return !_length;
      }
    }
    return false;
}


Ember.isEmpty();                // true
Ember.isEmpty(null);            // true || 
Ember.isEmpty(undefined);       // true
Ember.isEmpty('');              // true
Ember.isEmpty([]);              // true
Ember.isEmpty({});              // false
Ember.isEmpty('Adam Hawkins');  // false
Ember.isEmpty([0,1,2]);         // false
Ember.isEmpty('\n\t');          // false
Ember.isEmpty('  ');            // false
```

- 判断是否为 null || undefined || 空字符串 || 空数组 || 空格字符串

```js
function isBlank(obj) {
    return isEmpty(obj) || typeof obj === 'string' && obj.match(/\S/) === null;
}
Ember.isBlank();                // true
Ember.isBlank(null);            // true
Ember.isBlank(undefined);       // true
Ember.isBlank('');              // true
Ember.isBlank([]);              // true
Ember.isBlank('\n\t');          // true
Ember.isBlank('  ');            // true
Ember.isBlank({});              // false
Ember.isBlank('\n\t Hello');    // false
Ember.isBlank('Hello world');   // false
Ember.isBlank([1,2,3]);         // false
```

- 判断是否为空对象

```js
isEmptyObject: function( obj ) {
    var name;
    for ( name in obj ) {
        return false;
    }
    return true;
}

$.isEmptyObject({ });                    //true
$.isEmptyObject(new Object());           //true
$.isEmptyObject([]);                     //false
$.isEmptyObject([0]);                    //false
$.isEmptyObject({name: "CodePlayer"});   //false
$.isEmptyObject({sayHi: function(){} }); //false
```

### 属性相关操作
- 删除属性，检测属性

```js
var cat = {
    "name" : "tom",
    "sex" : "man",
    "color" : "yellow"
};
Object.prototype.size = 10;
delete cat.name;
cat.sex = undefined;
cat.color = null;
//in操作符只要通过对象能访问到属性(包括原型)就返回true。
//hasOwnProperty()只在属性存在于实例中(不包括原型)时才返回true。
console.log("name属性是否存在：" + cat.hasOwnProperty("name")); //false
console.log("sex属性是否存在：" + cat.hasOwnProperty("sex")); //true
console.log("color属性是否存在：" + cat.hasOwnProperty("color")); //true
console.log("size属性是否存在：" + cat.hasOwnProperty("size")); //false
console.log("size属性是否存在：" + ("size" in cat)); //true
for (name in cat) {
    document.write(cat[name] + "\n");
}
```

- 遍历属性

1. for...in 遍历对象自身的和继承的可枚举属性。
2. Object.keys(obj)
Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性。
3. Object.getOwnPropertyNames(obj)
Object.getOwnPropertyNames返回一个数组，包含对象自身的所有属性（包括不可枚举的属性）。
4. Object.getOwnPropertySymbols(obj)
Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有 Symbol 属性。
5. Reflect.ownKeys(obj)
Reflect.ownKeys返回一个数组，包含对象自身的所有属性，不管属性名是 Symbol 或字符串，也不管是否可枚举。

### Object的属性方法
- Object.create 

创建一个原型链干净的对象

```js
var o = Object.create({
            "say": function () {
                alert(this.name);
            },
            "name":"Byron"
        });
```
- Object.defineProperty(O,Prop,descriptor)
value:属性的值

writable:当且仅当该属性的 writable 为 true 时，该属性才能被赋值运算符改变。默认为 false。

configurable:当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，也能够被删除。默认为 false。

enumerable:当且仅当该属性的 enumerable 为 true 时，该属性才能够出现在对象的枚举属性中。默认为 false。

set:一个给属性提供 getter 的方法，如果没有 getter 则为undefined。当我们读取某个属性的时候，其实是在对象内部调用了该方法，此方法必须要有return语句。该方法返回值被用作属性值。默认为 undefined。

get: 一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。该方法将接受唯一参数，并将该参数的新值分配给该属性。默认为 undefined。也就是说，当我们设置某个属性的时候，实际上是在对象的内部调用了该方法。

在 descriptor 中不能同时设置访问器（get 和 set）和 wriable 或 value，否则会错，就是说想用 get 和 set，就不能用 writable 或 value 中的任何一个。

```js
Object.defineProperty(o,'age', {
    value: 24,
    writable: true,
    enumerable: true,
    configurable: true
});

Object.defineProperty(o, 'sex', {
    value: 'male',
    writable: false,
    enumerable: false,
    configurable: false
});

console.log(o.age); //24
o.age = 25;

for (var obj in o) {
    console.log(obj + ' : ' + o[obj]);
    /*
    age : 25  //没有把sex ： male 遍历出来
    say : function () {
        alert(this.name);
    } 
    name : Byron 
    */
}
delete o.age;
console.log(o.age);//undefined 

console.log(o.sex); //male
//o.sex = 'female'; //Cannot assign to read only property 'sex' of #<Object> 
delete o.age; 
console.log(o.sex); //male ,并没有被删除

//Object.defineProperties(O,descriptors)
Object.defineProperties(o, {
    'age': {
        value: 24,
        writable: true,
        enumerable: true,
        configurable: true
    },
    'sex': {
        value: 'male',
        writable: false,
        enumerable: false,
        configurable: false
    }
});
```

- Object.getOwnPropertyDescriptor(O,property)

会返回某个对象属性的描述对象（descriptor）


```js
var obj = {
  foo: 123,
  get bar() { return 'abc' }
};

Object.getOwnPropertyDescriptors(obj)
// { foo:
//    { value: 123,
//      writable: true,
//      enumerable: true,
//      configurable: true },
//   bar:
//    { get: [Function: bar],
//      set: undefined,
//      enumerable: true,
//      configurable: true } }

```

- Object.getOwnPropertyNames(obj)

返回一个数组，包含对象自身的所有属性(包括不可枚举属性）。

- Object.keys(obj)

和getOwnPropertyNames方法类似，但是获取所有的可枚举的属性，返回一个数组

- Object.preventExtensions(O) / Object.isExtensible(O)

Object.preventExtensions方法用于锁住对象属性，使其不能够拓展，也就是不能增加新的属性，但是属性的值仍然可以更改，也可以把属性删除，Object.isExtensible用于判断对象是否可以被拓展

```js
console.log(Object.isExtensible(o)); //true
o.lastName = 'Sun';
console.log(o.lastName); //Sun ,此时对象可以拓展

Object.preventExtensions(o);
console.log(Object.isExtensible(o)); //false

o.lastName = "ByronSun";
console.log(o.lastName); //ByronSun，属性值仍然可以修改

//delete o.lastName;
console.log(o.lastName); //undefined仍可删除属性

 o.firstname = 'Byron'; //Can't add property firstname, object is not extensible 不能够添加属性
```

- Object.seal(O) / Object.isSealed
Object.seal方法用于把对象密封，也就是让对象既不可以拓展也不可以删除属性（把每个属性的configurable设为false）,单数属性值仍然可以修改，Object.isSealed由于判断对象是否被密封

```js
Object.seal(o);
o.age = 25; //仍然可以修改
delete o.age; //Cannot delete property 'age' of #<Object>

//Object.freeze(O) / Object.isFrozen
```

- Object.freeze(O) / Object.isFrozen

终极神器，完全冻结对象，在seal的基础上，属性值也不可以修改（每个属性的wirtable也被设为false）

```js 
Object.freeze(o);
o.age = 25; //Cannot assign to read only property 'age' of #<Object>
```

## 数值

### 数值检测
- 检测数值是否是有限数字
如果 number 是有限数字（或可转换为有限数字），那么返回 true。否则，如果 number 是 NaN（非数字），或者是正、负无穷大的数，则返回 false。

```js
Number.isFinite(15); // true
Number.isFinite(0.8); // true
Number.isFinite(NaN); // false
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite('foo'); // false
Number.isFinite('15'); // false
Number.isFinite(true); // false

//ES5 可以通过下面的代码，部署Number.isFinite方法。
(function (global) {
  var global_isFinite = global.isFinite;

  Object.defineProperty(Number, 'isFinite', {
    value: function isFinite(value) {
      return typeof value === 'number' && global_isFinite(value);
    },
    configurable: true,
    enumerable: false,
    writable: true
  });
})(this);
```

- 判断是否为NaN

```js
Number.isNaN(NaN) // true
Number.isNaN(15) // false
Number.isNaN('15') // false
Number.isNaN(true) // false
Number.isNaN(9/NaN) // true
Number.isNaN('true'/0) // true
Number.isNaN('true'/'true') // true

//ES5 通过下面的代码，部署Number.isNaN()。
(function (global) {
  var global_isNaN = global.isNaN;

  Object.defineProperty(Number, 'isNaN', {
    value: function isNaN(value) {
      return typeof value === 'number' && global_isNaN(value);
    },
    configurable: true,
    enumerable: false,
    writable: true
  });
})(this);
```

- 判断是否为整数
```js
Number.isInteger(25) // true
Number.isInteger(25.0) // true
Number.isInteger(25.1) // false
Number.isInteger("15") // false
Number.isInteger(true) // false

ES5 可以通过下面的代码，部署Number.isInteger()。

(function (global) {
  var floor = Math.floor,
    isInteger = global.isInteger;

  Object.defineProperty(Number, 'isInteger', {
    value: function isInteger(value) {
      return typeof value === 'number' &&
        isInteger(value) &&
        floor(value) === value;
    },
    configurable: true,
    enumerable: false,
    writable: true
  });
})(this);
```


### 数值转换

- Number.parseInt() / Number.parseFloat() / toFixed()

```js
// ES5的写法
parseInt('12.34',10);// 12
parseFloat('123.45#'); // 123.45

// ES6的写法
Number.parseInt('12.34',10); // 12
Number.parseFloat('123.45#'); // 123.45

//保留几位小数
var num = 3.14;
num.toFixed(1);//3.1
```

### Math对象

- 返回 x 的 y 次幂的值。

```js
Math.pow(x, y);
```

- 返回绝对值

```js
Math.abs(x);
```


- 返回 x 和 y 中的最大值和最小值

```js
Math.max(x, y);
Math.min(x, y);
```

- 返数组最大值

```js
var arr = [5, 7, 9, 1];
Math.max.apply(null, arr);
```

- 随机数

```js
//返回介于 0 ~ 1 之间的随机数
Math.random();
//返回介于min~max之间的随机整数
Math.trunc(Math.random() * (max - min) + min)
```

- 对数进行下舍入

```js
Math.floor(x);
```
- 对数进行上舍入

```js
Math.ceil(x);
```

- 四舍五入

```js
Math.round(4.4); //4
Math.round(4.5); //5
```

- 去除一个数的小数部分，返回整数部分。

```js
Math.trunc(4.1) // 4
Math.trunc(4.9) // 4
Math.trunc(-4.1) // -4
Math.trunc(-4.9) // -4
Math.trunc(-0.1234) // -0
```

- 判断一个数到底是正数、负数、还是零。

```js
Math.sign(-5) // -1
Math.sign(5) // +1
Math.sign(0) // +0
Math.sign(-0) // -0
Math.sign(NaN) // NaN
Math.sign('9'); // +1
Math.sign('foo'); // NaN
Math.sign();      // NaN
```

- 平方根函数

```js
Math.sqrt(0)//0;
Math.sqrt(1)//1;
Math.sqrt(9)//3;
Math.sqrt(0.64)//0.8;
Math.sqrt//NaN;
```

## 日期

- 获取日期相关
```js
var mydate = new Date();
mydate.getYear(); //获取当前年份(2位)
mydate.getFullYear(); //获取完整的年份(4位,1970-????)
mydate.getMonth(); //获取当前月份(0-11,0代表1月)
mydate.getDate(); //获取当前日(1-31)
mydate.getDay(); //获取当前星期X(0-6,0代表星期天)
mydate.getTime(); //获取当前时间(从1970.1.1开始的毫秒数)
mydate.getHours(); //获取当前小时数(0-23)
mydate.getMinutes(); //获取当前分钟数(0-59)
mydate.getSeconds(); //获取当前秒数(0-59)
mydate.getMilliseconds(); //获取当前毫秒数(0-999)
mydate.toLocaleDateString(); //获取当前日期
var mytime=mydate.toLocaleTimeString(); //获取当前时间
mydate.toLocaleString( ); //获取日期与时间
```

- 判断闰年
```js
Date.prototype.isLeapYear = function()
{
    return (0==this.getYear()%4&&((this.getYear()0!=0)||(this.getYear()@0==0)));
}
```

## 字符串
- 字符串Unicode码

```js
var str = "Hello world!";
str.charCodeAt(0);//72
str.charCodeAt(1);//101
str.charCodeAt(2);//108

String.fromCharCode(72);// H
```

- 字符串替换

```js
var str="Visit Microsoft!"
str.replace(/Microsoft/, "W3School");
//第一个字母转大写
var name = 'aaa bbb ccc';
var uw=name.replace(/\b\w+\b/g, function(word){
    return word.substring(0,1).toUpperCase()+word.substring(1);}
);
//贪婪匹配 查找最后一个,
var str = "a,b,c,d,e";
str.replace(/(.*),/,"$1 &");//"a,b,c,d&e"
```

- 分割字符串

```js
"|a|b|c".split("|"); //["", "a", "b", "c"]
"abc".split(""); //["", "a", "b", "c"]
```
- 大小写转换

```js
var strVariable = 'test';
strVariable.toLowerCase();
strVariable.toUpperCase();
```
- 字符串转义

```js
var str = "你好javascript123;#$%";
var str1 = encodeURIComponent(str);
console.log(str1); //%u4F60%u597Djavascript123%3B%23%24%25
var str2 = decodeURIComponent(str1);
console.log(str2); //你好javascript123;#$%
```

- 字符串查询

indexof()：返回某个指定的字符串值在字符串中首次出现的位置。

includes()：返回布尔值，表示是否找到了参数字符串。

startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。

endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。

```js
var str="Hello world!"
str.indexOf("Hello");//0
str.indexOf("abc");//-1
str.startsWith('Hello') // true
str.endsWith('!') // true
str.includes('o') // true
str.startsWith('world', 6) // true
str.endsWith('Hello', 5) // true
str.includes('Hello', 6) // false
```

- 提取字符串

```js
var str="Hello happy world!"
str.slice(6); //happy world!
str.slice(6,11); //happy
str.slice(-6); //world!
```

- 字符串重复

```js
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
```

- ES6模板字符串

```js
// 字符串中嵌入变量
var name = "Bob", 
    time = "today";
var result = `Hello ${name}, how are you ${time}?`
```

- toString妙用

```js
//进制转换
var a =123;
a.toString(2);//"1111011"
a.toString(8);//"173"
a.toString(16);//"7b"
```

## 数组

- 判断是否是数组

```js
Array.isArray([1, 2, 3]);  // true
Array.isArray({foo: 123}); // false
Array.isArray('foobar');   // false
Array.isArray(undefined);  // false
```

- 排序

```js
//a,b表示数组中的任意两个元素，若return > 0 b前a后；reutrn < 0 a前b后；
//简化一下：a-b输出从小到大排序，b-a输出从大到小排序。
var arr = [1, 2, 1, 3, 3, 2, 4, 6, 3];
var sortArr = arr.sort(function (a, b) {
    return a - b
});
sortArr.reverse();
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort(); 
//排序改变原有数组，并且返回自身引用
//排序结果是Apple,Banana,Mango,Orange
//若要得到结果Orange,Mango,Banana,Apple只需要将上步得到的fruits结果反转即可
fruits.reverse(); //排序结果是Orange,Mango,Banana,Apple
```
- 数组连接 or 复制

```js
var array0 = [1, 2, 3].concat();
var array0 = [1, 2, 3].slice();
var array1 = [1, 2, 3];
array1.concat([4, 5]);
var array2 = [6, 7];
var array3 = [8, 9];
array1.concat(array2, array3);
```

- 数组-字符串

```js
//把数组中的所有元素放入一个字符串,指定分隔符
var arr = new Array(3);
arr[0] = "George";
arr[1] = "John";
arr[2] = "Thomas";
arr.join(','); //George,John,Thomas
```
- 提取数组

```js
//从已有的数组中返回选定的元素
var arr = [0,1,2,3,4,5];
arr.slice(3); //3,4,5
arr.slice(1, 3); //1,2
```

- 插入/删除

```js
var arr = new Array(6);
arr[0] = "1";
arr[1] = "2";
arr[2] = "3";
arr[3] = "4";
arr[4] = "5";
arr[5] = "6";
arr.push("7");//1,2,3,4,5,6,7 并返回修改后数组的长度。
arr.pop();//移除最后一项 arr=[1,2,3,4,5,6] return 7 
//插入操作 splice会修改原数组
arr.splice(2, 0, "7"); //1,2,7,3,4,5,6
//shift 和 unshift
//shift()：删除原数组第一项，并返回删除元素的值；如果数组为空则返回undefined 。 
//unshift:将参数添加到原数组开头，并返回数组的长度 。

//删除替换操作
arr.splice(2, 3, "0"); //1,2,0,5,6
arr.splice(2, 3); //1,2,5,6
```

- 查找

```js
arr = ["1", "2", "3"];
arr.indexOf("3"); //2
arr.indexOf("0"); //-1

[1, 4, -5, 10].find(function(n) {return n < 0;});// -5
[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10

[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2

[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true

function isBiggerThan10(element, index, array) {
  return element > 10;
}

[2, 5, 8, 1, 4].some(isBiggerThan10);  // false
[12, 5, 8, 1, 4].some(isBiggerThan10); // true
[12, 5, 8, 1, 4].every(isBiggerThan10); // false


[1,2,3,4,5,6,7,8].filter(function(item){
    return item <5;
});//[1,2,3,4]
```

- 填充数组

```js
['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]

['a', 'b', 'c'].fill(7, 1, 2)
// ['a', 7, 'c']
```




## 函数

## 异常捕获

```js
try {
  throw new Error("Error!");
} catch (error) {
  alert(error.name + ": " + error.message);
}
```

## 正则表达式

```js
var reg = new RegExp();//RegExp是一个对象,和Aarray一样  
//但这样没有任何效果,需要将正则表达式的内容作为字符串传递进去  
reg =new RegExp("a");//最简单的正则表达式,将匹配字母a  
//第二个参数 g （全文查找） i （忽略大小写） m （多行查找）
reg=new RegExp("a","gi");//第二个参数,表示匹配时全局搜索、不分大小写  

// 正则表达式的字面量方式
var reg = /a/gi;  

//test方法测试字符串,符合模式时返回true,否则返回false  
var re = /he/;//最简单的正则表达式,将匹配he这个单词  
var str = "he";  
alert(re.test(str));//true  
str = "we";  
alert(re.test(str));//false  
 
//exec方法返回的数组第1到n元素中包含的是匹配中出现的任意一个子匹配  
var re = /[A-Z]/;  
//exec方法执行后,修改了re的lastIndex属性,  
var str = "Hello,World!!!";  
var arr = re.exec(str);  
alert(re.lastIndex);//0,因为没有设置全局标志  
re = /[A-Z]/g;  
arr = re.exec(str);  
alert(re.lastIndex);//1  
arr = re.exec(str);  
alert(re.lastIndex);//7 

// String对象的match方法也用于将字符串与正则表达式进行匹配并返回结果数组
var str = "My name is CJ.Hello everyone!";  
var re = /[A-Z]/;//匹配所有大写字母  
var arr = str.match(re);//返回数组  
alert(arr);//数组中只会包含一个M,因为我们没有使用全局匹配  
re = /[A-Z]/g;  
arr = str.match(re);  
alert(arr);//M,C,J,H  
//从字符串中抽取单词  
re = /\b[a-z]*\b/gi;//\b表示单词边界  
str = "one two three four";  
alert(str.match(re));//one,two,three,four  

``` 

- $1...$9 属性
返回在模式匹配期间找到的，所存储的最近的九个部分。只读。 

RegExp.$n 

每当产生一个带括号的成功匹配时，$1...$9属性的值就被修改。可以在一个正则表达式模式中指定任意多个带括号的子匹配，但只能存储最新的九个。



## Console

```js
/*
 *****console日志 支持的占位符有：字符（%s）、整数（%d或%i）、浮点数（%f）和对象（%o）
 */
console.log('hello');
console.info('information');
console.error('error');
console.warn('warn');
console.log("%d年%d月%d日", 2011, 3, 26);
//console日志 分组
console.group("first group info");
console.log("1.myblog");
console.log("2.CSDN");
console.groupEnd();
console.group("second group info");
console.log("1.QQ");
console.log("2.welcome");
console.groupEnd();
//console日志 dir 显示对象数据
var info = {
    blog: "www",
    QQGroup: 1111111,
    message: "welcome"
};
console.dir(info);
//console日志 来显示网页的某个节点
var info = document.getElementById('info');
console.dirxml(info);
//console日志 断言
var result = 1;
console.assert(result);
var year = 2014;
console.assert(year == 2018);
//console日志 console.trace()用来追踪函数的调用轨迹。
console.trace();
//console日志 显示代码的运行时间。
console.time("counter1");
for (var i = 0; i < 1000; i++) {
    for (var j = 0; j < 1000; j++) {}
}
console.timeEnd("counter1");
//profile 开始一个带有可选标签的JavaScript CPU性能分析。以console.profileEnd()作为整个性能分析结束的标记。每次性能分析结果被附加到性能分析面板。
console.profile("processPixels function excute!");
// later, after processing pixels
console.profileEnd();
//在一个记录会话期间，添加一个事件到时间表。
console.timeStamp('check out this custom timestamp thanks to console.timeStamp()!');
```

## 异步/多线程
### 定时器

```js
//间隔调用
var timer = setInterval(doSomething(), 3000);
clearInterval(timer);
//延时调用
var timer = setTimeout(doSomething(), 3000);
clearTimeout(timer);
```

### worker

```js
//检测是否可用Worker
if (typeof(Worker) !== "undefined") {
    // Yes! Web worker support!
    // Some code.....
} else {
    // Sorry! No Web Worker support..
}
//使用Worker
var worker = new Worker("xxxxx.js");
worker.postMessage("parameter");
worker.onmessage = function (evt) {
    console.log(evt.data);
};

//worker内部
onmessage =function (evt){
  var data = evt.data;//通过evt.data获得发送来的数据
  postMessage(data);//将获取到的数据发送会主线程
}
```

## 本地存储

```js
//localStorage
localStorage.lastname = "Smith";
localStorage.setItem("name","Smith");
console.log(localStorage.lastname);
console.log(localStorage.getItem("name"));
//删除
localStorage.removeItem("name");
localStorage.clear();
//sessionStorage
sessionStorage.lastname = "Smith";
console.log(sessionStorage.lastname);
```


## Map/Set
Map是一组键值对的结构，具有极快的查找速度。
```js
//遍历方法
//keys()：返回键名的遍历器
//values()：返回键值的遍历器
//entries()：返回键值对的遍历器
//forEach()：使用回调函数遍历每个成员

//空map设值key-value
var m = new Map();
m.set("A",99);
m.set("B",66);
//构造参数传key-value
var m = new Map([['A', 99], ['B', 66]]);

var m = new Map(); // 空Map
m.set('XiaoMing', 99); // 添加新的key-value
m.has('XiaoMing'); // 是否存在key 'XiaoMing': true
m.get('XiaoMing'); // 99
m.delete('XiaoMing'); // 删除key 'XiaoMing'
m.get('XiaoMing'); // undefined

var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    alert(value);
});
```

Set和Map类似，但set只存储key，且key不重复。

```js
//遍历方法
//keys()：返回键名的遍历器
//values()：返回键值的遍历器
//entries()：返回键值对的遍历器
//forEach()：使用回调函数遍历每个成员

var s1 = new Set(); // 空Set
s1.add(1);
s1.add(2);
s1.add(3);

var s2 = new Set([1, 2, 3]); // 含1, 2, 3

var s = new Set([1, 2, 3]);
s.add(3);// Set{1,2,3}
s.delete(3);// Set([1,2]);
s.has(3);//false

var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    alert(element);
});
```


# CSS