# 事件对象的公共成员
## DOM中的event的公共成员
event对象包含与创建它的特定事件有关的属性和方法。触发的事件类型不一样，可用的属性和方法不一样。但是，DOM中所有事件都有以下公共成员。【注意bubbles属性和cancelable属性】

![](https://i.imgur.com/rbipwbp.png)

### 对比currentTarget和target
在事件处理程序内部，对象this始终等于currentTarget的值，而target则只是包含事件的实际目标。

举例：页面有个按钮，在body（按钮的父节点）中注册click事件，点按钮时click事件会冒泡到body进行处理。
```
<body>
<input id="btn" type="button" value="click"/>
<script>
    document.body.onclick=function(event){
        console.log("body中注册的click事件");
        console.log("this===event.currentTarget? "+(this===event.currentTarget)); //true
        console.log("currentTarget===document.body?"+(event.currentTarget===document.body)); //true
        console.log('event.target===document.getElementById("btn")? '+(event.target===document.getElementById("btn"))); //true
    }
</script>
</body>
```

### 通过type属性，可以在一个函数中处理多个事件。
原理：通过检测event.type属性，对不同事件进行不同处理。

举例：定义一个handler函数用来处理3种事件：click,mouseover,mouseout。
```
<body>
<input id="btn" type="button" value="click"/>
<script>
var handler=function(event){
    switch (event.type){
        case "click":
            alert("clicked");
            break;
        case "mouseover":
            event.target.style.backgroundColor="pink";
            break;
        case "mouseout":
            event.target.style.backgroundColor="";
    }
};
var btn=document.getElementById("btn");
btn.onclick=handler;
btn.onmouseover=handler;
btn.onmouseout=handler;
</script>
</body>
```

### stopPropagation()和stopImmediatePropagation()对比
同：stopPropagation()和 stopImmediatePropagation()都可以用来取消事件的进一步捕获或冒泡。
异：二者的区别在于当一个事件有多个事件处理程序时，stopImmediatePropagation()可以阻止之后事件处理程序被调用。

举例：
```
<body>
<input id="btn" type="button" value="click"/>
<script>
    var btn=document.getElementById("btn");
    btn.addEventListener("click",function(event){
        console.log("buttn click listened once");
//    event.stopPropagation();//取消注释查看效果
//    event.stopImmediatePropagation();//取消注释查看效果
    },false);
    btn.addEventListener("click",function(){
        console.log("button click listened twice");
    },false);
    document.body.onclick= function (event) {
        console.log("body clicked");
    }
</script>
</body>
```

![](https://i.imgur.com/pHrKxia.jpg)

### eventPhase
eventPhase值在捕获阶段为1，处于目标阶段为2，冒泡阶段为3。
可以通过下面代码查看：
```
var btn=document.getElementById("btn");
btn.onclick= function (event) {
console.log(event.CAPTURING_PHASE); //1
console.log(event.AT_TARGET); //2
console.log(event.BUBBLING_PHASE); //3
```

例子：
```
<body>
<input id="btn" type="button" value="click"/>
<script>
var btn=document.getElementById("btn");
btn.onclick= function (event) {
    console.log("按钮DOM0级方法添加事件处理程序eventPhase值为？"+event.eventPhase);
}
btn.addEventListener("click",function(event){
    console.log("按钮DOM2级方法添加事件处理程序，且addEventListener第三个参数为true时eventPhase值为？"+event.eventPhase);
},true);
btn.addEventListener("click",function(event){
    console.log("按钮DOM2级方法添加事件处理程序，且addEventListener第三个参数为false时eventPhase值为？"+event.eventPhase);
},false);
    document.body.addEventListener("click", function (event) {
        console.log("body上添加事件处理程序，且在捕获阶段eventPhase值为？"+event.eventPhase);
    },true);
document.body.addEventListener("click", function (event) {
    console.log("body上添加事件处理程序，且在冒泡阶段eventPhase值为？"+event.eventPhase);
},false);
</script>
```

运行结果：
![](https://i.imgur.com/NcmkyFL.jpg)