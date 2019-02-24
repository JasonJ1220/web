# ES5继承
## 原型链继承
将基类的构造函数赋给子类的prototype

特点：
1. 子类实例共享属性
2. 无法给基类构造函数传参数

## 借用构造函数
在子类中调用基类构造函数，采用 Parent.call(this)

特点：
1. 避免了引用类型的属性被所有实例共享
2. 可以在 Child 中向 Parent 传参
3. 方法都在构造函数中定义，每次创建实例都会创建一遍方法。

## 组合继承
原型链和借用构造函数的技术组合到一块
使用原型链实现对原型属性和方法的继承
通过借用构造函数来实现对实例属性的继承.
```
function Parent (name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Parent.prototype.getName = function () {
    console.log(this.name)
}

function Child (name, age) {
    Parent.call(this, name);
    this.age = age;
}

Child.prototype = new Parent();

var child1 = new Child('张三', '18');

child1.colors.push('black');

console.log(child1.name); // 张三
console.log(child1.age); // 18
console.log(child1.colors); // ["red", "blue", "green", "black"]

var child2 = new Child('李四', '20');

console.log(child2.name); // 李四
console.log(child2.age); // 20
console.log(child2.colors); // ["red", "blue", "green"]
```

特点：
融合原型链继承和构造函数的优点，是 JavaScript 中最常用的继承模式。
会调用两次父类构造函数。（一次是设置子类型实例的原型的时候；一次是创建子类的时候。）

## 寄生式继承
创建一个仅用于封装继承过程的函数，该函数在内部以某种形式来做增强对象，最后返回对象。
```
function createObj (o) {
    function F(){}
    F.prototype = o;
    var clone = new F();

    clone.sayName = function () {
        console.log('hello world');
    }
    return clone;
}
```
特点：
跟借用构造函数模式一样，每次创建对象都会创建一遍方法。

## 寄生组合式继承
```
function Parent (name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Parent.prototype.getName = function () {
    console.log(this.name)
}

function Child (name, age) {
    Parent.call(this, name);
    this.age = age;
}

// 关键的三步
var F = function () {};

F.prototype = Parent.prototype;

Child.prototype = new F();


var child1 = new Child('kevin', '18');

console.log(child1);
```

最后我们封装一下这个继承方法：
```
function object(o) {
    function F() {}
    F.prototype = o;
    return new F();
}

function prototype(child, parent) {
    var prototype = object(parent.prototype);
    prototype.constructor = child;
    child.prototype = prototype;
}

// 当我们使用的时候：
prototype(Child, Parent);
```

这种方式的高效率体现它只调用了一次 Parent 构造函数，并且因此避免了在 Parent.prototype 上面创建不必要的、多余的属性。与此同时，原型链还能保持不变；因此，还能够正常使用 instanceof 和 isPrototypeOf。开发人员普遍认为寄生组合式继承是引用类型最理想的继承范式。
