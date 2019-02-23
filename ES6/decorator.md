# Decorator
## 类的修饰
许多面向对象的语言都有修饰器（Decorator）函数，用来修改类的行为。目前，有一个提案将这项功能，引入了 ECMAScript。
```
@testable
class MyTestableClass {
  // ...
}

function testable(target) {
  target.isTestable = true;
}

MyTestableClass.isTestable // true
```
最简原型
```
@decorator
class A {}

// 等同于

class A {}
A = decorator(A) || A;
```
也就是说，修饰器是一个对类进行处理的函数。修饰器函数的第一个参数，就是所要修饰的目标类。

如果觉得一个参数不够用，可以在修饰器外面再封装一层函数。
```
function testable(isTestable) {
  return function(target) {
    target.isTestable = isTestable;
  }
}

@testable(true)
class MyTestableClass {}
MyTestableClass.isTestable // true

@testable(false)
class MyClass {}
MyClass.isTestable // false
```
注意，修饰器对类的行为的改变，是代码编译时发生的，而不是在运行时。这意味着，修饰器能在编译阶段运行代码。也就是说，修饰器本质就是编译时执行的函数。

例子：
```
// mixins.js
export function mixins(...list) {
  return function (target) {
    Object.assign(target.prototype, ...list)
  }
}

// main.js
import { mixins } from './mixins'

const Foo = {
  foo() { console.log('foo') }
};

@mixins(Foo)
class MyClass {}

let obj = new MyClass();
obj.foo() // 'foo'
```


## 方法的修饰
修饰器不仅可以修饰类，还可以修饰类的属性。
```
class Person {
  @readonly
  name() { return `${this.first} ${this.last}` }
}
```
修饰器函数readonly一共可以接受三个参数。
```
function readonly(target, name, descriptor){
  // descriptor对象原来的值如下
  // {
  //   value: specifiedFunction,
  //   enumerable: false,
  //   configurable: true,
  //   writable: true
  // };
  descriptor.writable = false;
  return descriptor;
}

readonly(Person.prototype, 'name', descriptor);
// 类似于
Object.defineProperty(Person.prototype, 'name', descriptor);
```

## Mixin
在修饰器的基础上，可以实现Mixin模式。所谓Mixin模式，就是对象继承的一种替代方案，中文译为“混入”（mix in），意为在一个对象之中混入另外一个对象的方法。
```
const Foo = {
  foo() { console.log('foo') }
};

class MyClass {}

Object.assign(MyClass.prototype, Foo);

let obj = new MyClass();
obj.foo() // 'foo'
```

下面，我们部署一个通用脚本mixins.js，将 Mixin 写成一个修饰器。
```
export function mixins(...list) {
  return function (target) {
    Object.assign(target.prototype, ...list);
  };
}
```
然后，就可以使用上面这个修饰器，为类“混入”各种方法。
```
import { mixins } from './mixins';

const Foo = {
  foo() { console.log('foo') }
};

@mixins(Foo)
class MyClass {}

let obj = new MyClass();
obj.foo() // "foo"
```

不过，上面的方法会改写MyClass类的prototype对象，如果不喜欢这一点，也可以通过类的继承实现 Mixin。
```
class MyClass extends MyBaseClass {
  /* ... */
}
```
上面代码中，MyClass继承了MyBaseClass。如果我们想在MyClass里面“混入”一个foo方法，一个办法是在MyClass和MyBaseClass之间插入一个混入类，这个类具有foo方法，并且继承了MyBaseClass的所有方法，然后MyClass再继承这个类。
```
let MyMixin = (superclass) => class extends superclass {
  foo() {
    console.log('foo from MyMixin');
  }
};
```
上面代码中，MyMixin是一个混入类生成器，接受superclass作为参数，然后返回一个继承superclass的子类，该子类包含一个foo方法。

接着，目标类再去继承这个混入类，就达到了“混入”foo方法的目的。
```
class MyClass extends MyMixin(MyBaseClass) {
  /* ... */
}

let c = new MyClass();
c.foo(); // "foo from MyMixin"
```
如果需要“混入”多个方法，就生成多个混入类。
```
class MyClass extends Mixin1(Mixin2(MyBaseClass)) {
  /* ... */
}
```
这种写法的一个好处，是可以调用super，因此可以避免在“混入”过程中覆盖父类的同名方法。
```
let Mixin1 = (superclass) => class extends superclass {
  foo() {
    console.log('foo from Mixin1');
    if (super.foo) super.foo();
  }
};

let Mixin2 = (superclass) => class extends superclass {
  foo() {
    console.log('foo from Mixin2');
    if (super.foo) super.foo();
  }
};

class S {
  foo() {
    console.log('foo from S');
  }
}

class C extends Mixin1(Mixin2(S)) {
  foo() {
    console.log('foo from C');
    super.foo();
  }
}
```

上面代码中，每一次混入发生时，都调用了父类的super.foo方法，导致父类的同名方法没有被覆盖，行为被保留了下来。
```
new C().foo()
// foo from C
// foo from Mixin1
// foo from Mixin2
// foo from S
```