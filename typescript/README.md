# TypeScript

## 基础类型

ts 的类型主要有布尔值、数字、字符串、数组、元组、枚举、Any、Void、Null和Undefined、Never。

```
 # Boolean 
  let boo : boolean = true; || let boo : boolean = false;

  # Number(数字和各种进制)
  let num : number = 6; || let num : number = 0xf00d;

  # String
  let str : string = 'string';

  # Array
  let arr : number[] = [ 1, 2, 3];  // 此类是表示由此类元素组成的一个数组
  let arr : Array<number> = [ 1, 2, 3]; // 数组泛型 Array<元素类型>

  # Tuple
  > 元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。
  let arr : [ string, number ];
  arr = [ 'hello', 10 ];  // ok
  arr = [ 10, 'hello' ]; // error
  > 访问已知索引的元素，会得到正确的类型
  arr[1].slice(0);  // error, 'number' does not have 'slice';
  > 访问越界元素，使用联合类型替代。
  arr[3] = 'hello'; || arr[3] = 10;
  arr[4] = true;  // 不是联合类型中的一个，报错。
  arr[6].toString();  // ok,联合类型都具备这个方法。

  # enum
  > enum类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。
  enum Name { 'Tom', 'Jack' }
  let currName : Name = Name.Tom;
  > 默认情况下，从0开始为元素编号。 你也可以手动的指定成员的数值。
  enum Name { 'Tom' = 1, 'Jack' }
  let currName : Name = Name.Jack;
  > 或全部手动赋值
  enum Name { 'Tom' = 1, 'Jack' = 4 }
  > 枚举类型提供的一个便利是你可以由枚举的值得到它的名字。
  enum Name { 'Tom' = 1, 'Jack' = 4 }
  let currName : Name = Name[4];
  console.log(currName); // Jack

  # Any
  > 有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 any类型来标记这些变量：
  let anytype : any = 3;
  anytype = 'also can be string';
  anytype = false; 
  > 不定类型的array
  let anyArray : any[] = [ 1, true, '123'];

  # Void
  > 某种程度上来说，void类型像是与any类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是void
  function warning() : void {
    console.log('warning');
  }
  > void的变量只能赋值为`undefined`或`null`;
  let unsure : void = undefined;

  # undefined 和 null
  > TypeScript里，undefined和null两者各自有自己的类型分别叫做undefined和null。
  let un : undefined = undefined;
  let nu : null = null;
  > 默认情况下null和undefined是所有类型的子类型。 就是说你可以把 null和undefined赋值给number类型的变量.当你指定了--strictNullChecks标记，null和undefined只能赋值给void和它们各自。 这能避免 很多常见的问题。 也许在某处你想传入一个 string或null或undefined，你可以使用联合类型string | null | undefined。

  # never
  > never类型表示的是那些永不存在的值的类型。 例如， never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使 any也不可以赋值给never。

  function error( message : string ) : never {
    throw new Error(message);
  }

  function fail(){
    return error('something error');
  }

  function loop() : never {
    while (true) {

    }
  }
```

## 类型断言
有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

主要有两种表现形式。
- 尖括号语法（在tsx中无法使用）
```
let unsure : any = 'this is a string';
let strlength : number = (<string>unsure).length;
```

- as 语法(推荐)
```
let unsure : any = 'this is a string';
let strlength : number = (unsure as string).length;
```

## 变量声明
使用const或者let,var和let的区别:
```
# 分析下列代码结果
  for (var i = 0; i < 10; i++) {
    setTimeout(function() { console.log(i); }, 100 * i);
  }
  // 10 10 10 10 10 10 10 10 10 10 
  for (var i = 0; i < 10; i++) {
    (function(i) {
        setTimeout(function() { console.log(i); }, 100 * i);
    })(i);
  }
  // 0 1 2 3 4 5 6 7 8 9
  for (let i = 0; i < 10 ; i++) {
    setTimeout(function() {console.log(i); }, 100 * i);
  }
  // 0 1 2 3 4 5 6 7 8 9
```

## 解构

```
# 对于函数参数
  function arg([ fir, sec ] : [ number, number ]) :void {
    console.log( fir, sec );
  }
  let input : Array<number> = [ 1, 2 ];
  arg(input);

  # 属性重命名
  let { a : name1 , b : name2 } : { a : string, b : number }= { a : 'a' , b : 100 };

  # 默认值
  function default( defaultObj : { a : string, b : number } ) : void {
    let { a , b  = 100 } = defaultObj;
  }
```

> 不声明的赋值需要用括号括起来，不然一对花括号会被解析成一个块。

```
( { a, b } : { a : string, b : number } = { a : 'a', b: 100 } );
```
扩展对象的一些小问题，它仅包含对象 自身的可枚举属性。 

## 接口

TypeScript的核心原则之一是对值所具有的结构进行类型检查。 它有时被称做“鸭式辨型法”或“结构性子类型化”。 在TypeScript里，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

> 需要注意的是，我们传入的对象参数实际上会包含很多属性，但是编译器只会检查那些必需的属性是否存在，并且其类型是否匹配。 然而，有些时候TypeScript却并不会这么宽松

```
# 简例
  interface LabelledValue {
    label : string
  };
  function printLabel (labelledObj : labelledValue) : void {
    console.log( labelledObj.label );
  }
  let myObj = {size: 10, label: "Size 10 Object"};
  printLabel(myObj);
  > 类型检查器不会去检查属性的顺序，只要相应的属性存在并且类型也是对的就可以。

  # 可选属性
  interface SquareConfig {
    color?: string;
    width?: number;
  }
  function createSquare(config: SquareConfig): {color: string; area: number} {
    let newSquare = {color: "white", area: 100};
    if (config.color) {
      newSquare.color = config.color;
    }
    if (config.width) {
      newSquare.area = config.width * config.width;
    }
    return newSquare;
  }
  let mySquare = createSquare({color: "black"});

  # 只读属性
  一些对象属性只能在对象刚刚创建的时候修改其值。 你可以在属性名前用 readonly来指定只读属性:
  interface Point {
    readonly x: number;
    readonly y: number;
  }
  TypeScript具有ReadonlyArray<T>类型，它与Array<T>相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改：
  let a: number[] = [1, 2, 3, 4];
  let ro: ReadonlyArray<number> = a;
  > readonly vs const 
   做为变量使用的话用 const，若做为属性则使用readonly。

  # 额外属性检查
  interface SquareConfig {
    color?: string;
    width?: number;
  }

  function createSquare(config: SquareConfig): { color: string; area: number } {
      // ...
  }
  1 类型断言
  let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
  2 添加字符串索引签名,这种是你前提可以确定这个对象可能具有某些特殊用途的额外属性。
  interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
  }

  # 函数类型
  为了使用接口表示函数类型，我们需要给接口定义一个调用签名。 它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。
  interface SearchFunc {
    (source: string, subString: string): boolean;
  }
  创建一个函数类型的变量，并将一个同类型的函数赋值给这个变量
  let mySearch: SearchFunc;
  mySearch = function(source: string, subString: string) : boolean {
    let result = source.search(subString);
    return result > -1;
  }
  函数的参数会逐个进行检查，要求对应位置上的参数类型是兼容的。但函数的参数名不需要与接口里定义的名字相匹配。

  # 可索引的类型
  可索引类型具有一个 索引签名，它描述了对象索引的类型，还有相应的索引返回值类型。 
  interface StringArray {
    [index: number]: string;
  }
  let myArray: StringArray;
  myArray = ["Bob", "Fred"];
  let myStr: string = myArray[0];
  共有支持两种索引签名：字符串和数字。 可以同时使用两种类型的索引，但是数字索引的返回值必须是字符串索引返回值类型的子类型。 这是因为当使用 number来索引时，JavaScript会将它转换成string然后再去索引对象。 也就是说用 100（一个number）去索引等同于使用"100"（一个string）去索引，因此两者需要保持一致。
  class Animal {
      name: string;
  }
  class Dog extends Animal {
      breed: string;
  }
  // 错误：使用数值型的字符串索引，有时会得到完全不同的Animal!
  interface NotOkay {
      [x: number]: Animal;
      [x: string]: Dog;
  }
  字符串索引签名能够很好的描述dictionary模式，并且它们也会确保所有属性与其返回值类型相匹配。 因为字符串索引声明了 obj.property和obj["property"]两种形式都可以。
  interface NumberDictionary {
    [index: string]: number;
    length: number;    // 可以，length是number类型
    name: string       // 错误，`name`的类型与索引类型返回值的类型不匹配
  }
  索引签名可以设置为只读，这样可以防止给索引赋值
  interface ReadonlyStringArray {
      readonly [index: number]: string;
  }
  let myArray: ReadonlyStringArray = ["Alice", "Bob"];
  myArray[2] = "Mallory"; // error!

  # 类类型
  1. 简例
  interface ClockInterface {
      currentTime: Date;
  }
  class Clock implements ClockInterface {
      currentTime: Date;
      constructor(h: number, m: number) { }
  }
  接口描述了类的公共部分，而不是公共和私有两部分。 它不会帮你检查类是否具有某些私有成员。
  2. 类静态部分与实例部分的区别
  当你操作类和接口的时候，你要知道类是具有两个类型的：静态部分的类型和实例的类型。当一个类实现了一个接口时，只对其实例部分进行类型检查。 constructor存在于类的静态部分，所以不在检查的范围内。我们应该直接操作类的静态部分。
  interface ClockConstructor {
      new (hour: number, minute: number): ClockInterface;
  }
  interface ClockInterface {
      tick();
  }

  function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
      return new ctor(hour, minute);
  }

  class DigitalClock implements ClockInterface {
      constructor(h: number, m: number) { }
      tick() {
          console.log("beep beep");
      }
  }
  class AnalogClock implements ClockInterface {
      constructor(h: number, m: number) { }
      tick() {
          console.log("tick tock");
      }
  }

  let digital = createClock(DigitalClock, 12, 17);
  let analog = createClock(AnalogClock, 7, 32);
  3. 继承接口
  和类一样，接口也可以相互继承。 这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里。
  interface Shape {
      color: string;
  }
  interface Square extends Shape {
      sideLength: number;
  }
  let square = <Square>{};
  square.color = "blue";
  square.sideLength = 10;
  可继承多个接口
  interface Shape {
      color: string;
  }
  interface PenStroke {
      penWidth: number;
  }
  interface Square extends Shape, PenStroke {
      sideLength: number;
  }
  let square = <Square>{};
  square.color = "blue";
  square.sideLength = 10;
  square.penWidth = 5.0;
  4. 混合类型
  有时你希望一个对象可以同时具有多种类型。
  interface Counter {
      (start: number): string;
      interval: number;
      reset(): void;
  }
  function getCounter(): Counter {
      let counter = <Counter>function (start: number) { };
      counter.interval = 123;
      counter.reset = function () { };
      return counter;
  }
  let c = getCounter();
  c(10);
  c.reset();
  c.interval = 5.0;
  ( 在使用JavaScript第三方库的时候，你可能需要像上面那样去完整地定义类型。 )

  5. 接口继承类
  当接口继承了一个类类型时，它会继承类的成员但不包括其实现。 就好像接口声明了所有类中存在的成员，但并没有提供具体实现一样。 接口同样会继承到类的private和protected成员。 这意味着当你创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现（implement）。
  class Control {
      private state: any;
  }

  interface SelectableControl extends Control {
      select(): void;
  }

  class Button extends Control implements SelectableControl {
      select() { }
  }

  class TextBox extends Control {
      select() { }
  }

  // 错误：“Image”类型缺少“state”属性。
  class Image implements SelectableControl {
      select() { }
  }
```

## 类

```
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");
```

## 修饰符
- 公共 public(默认)
可以自由的访问程序里定义的成员。

- 私有 private
不能在声明它的类的外部访问。当我们比较带有 private或 protected成员的类型的时候，情况就不同了。 如果其中一个类型里包含一个 private成员，那么只有当另外一个类型中也存在这样一个 private成员， 并且它们都是来自同一处声明时，我们才认为这两个类型是兼容的。

- 受保护 protected
protected 修饰符与private修饰符的行为很像，但是，protected成员在派生类中仍然可以访问。构造函数也可以被标记成 protected。 这意味着这个类不能在包含它的类外被实例化，但是能被继承。

- 只读 readonly 使用readonly关键字将属性设置为只读的。只读属性必须在声明时或构造函数里被初始化。

```
# 综合栗子
  class Parent {
    public name : string; // 默认是public,这里举例所以显式的写出来了。
    private age : number; // 只可以在Parent类内部访问
    protected sex : string; // 可以在基类和派生类内部访问
    readonly height : number; // 此时是只读的，不允许修改;
    constructor( name : string, age : number, sex : string = 'man' ){
      this.name = name;
      this.age = age;
      this.sex = sex;
      this.height = 180;
    }
    changeName( name : string = '' ){
      this.name = name;
      console.log( `name 改为 ${ name }` );
    }

    getAge(){
      console.log(`${ this.name } 的年龄为 ${ this.age }`);
    }

    getSex(){
      console.log(`${ this.name } 的性别为 ${ this.sex }`)
    }
  }

  class Son extends Parent {
    constructor( name : string, age : number, sex : string = 'man' ){
      // 派生类包含构造函数钱必须调用super(),他会执行基类的构造。而且在构造里访问this之前必须调用super();
      super(name,age,sex);
    }
    run(distance : number = 10){
      console.log(`${ this.name } run ${ distance }m`);
    }
    changeName( name : string = ''){
      // 重写父类的changeName方法。
      console.log('rewrite...');
      super.changeName(name);
    }
    getSonSex(){
      // 此处访问基类的sex属性
      console.log(`${ this.name } 的性别为 ${ this.sex } ~ 派生类`)
    }
  }

  class ProtectParent {
    protected constructor(){
      this.name = 'protected';
    }
  }

  // 参数属性
  class Argument {
    // 这个地方参数被声明为private类型之后会在初始化时声明赋值合并。参数属性通过给构造函数参数添加一个访问限定符来声明。 使用 private限定一个参数属性会声明并初始化一个私有成员；对于 public和 protected来说也是一样。
    constructor( private name : string ){}
    printf( time : number ){
      console.log( `${ this.name } call ${ time } time;` )
    }
  }

  
  let parent : Parent = new Parent('jack',50, 'woman');
  let son : Son = new Son('jackson',10);
  parent.changeName('jacks');
  parent.getAge();
  parent.age; // error,age为私有属性，只可以在Parent类中访问
  parent.getSex();
  parent.sex; // error, sex为被保护的属性，只可以在基类和派生类内部访问
  parent.height; // 180
  parent.height = 120; // error, height是只读的。
  son.run();
  let PP = new ProtectParent(); // error,构造是被保护的。
  
  // 存取器
  首先，存取器要求你将编译器设置为输出ECMAScript 5或更高。 不支持降级到ECMAScript 3。 其次，只带有 get不带有 set的存取器自动被推断为 readonly。 这在从代码生成 .d.ts文件时是有帮助的，因为利用这个属性的用户会看到不允许够改变它的值。
  let passcode = "secret passcode";
  class Employee {
      private _fullName: string;

      get fullName(): string {
          return this._fullName;
      }

      set fullName(newName: string) {
          if (passcode && passcode == "secret passcode") {
              this._fullName = newName;
          }
          else {
              console.log("Error: Unauthorized update of employee!");
          }
      }
  }
```

## 抽象类
抽象类做为其它派生类的基类使用。 它们一般不会直接被实例化。 不同于接口，抽象类可以包含成员的实现细节。abstract关键字是用于定义抽象类和在抽象类内部定义抽象方法。抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。抽象方法的语法与接口方法相似。 两者都是定义方法签名但不包含方法体。 然而，抽象方法必须包含abstract关键字并且可以包含访问修饰符。

```
abstract class Department {

    constructor(public name: string) {
    }

    printName(): void {
        console.log('Department name: ' + this.name);
    }

    abstract printMeeting(): void; // 必须在派生类中实现
}

class AccountingDepartment extends Department {

    constructor() {
        super('Accounting and Auditing'); // 在派生类的构造函数中必须调用 super()
    }

    // 派生类中必须实现抽象基类中的抽象方法
    printMeeting(): void {
        console.log('The Accounting Department meets each Monday at 10am.');
    }

    generateReports(): void {
        console.log('Generating accounting reports...');
    }
}
let department: Department; // 允许创建一个对抽象类型的引用
department = new Department(); // 错误: 不能创建一个抽象类的实例
department = new AccountingDepartment(); // 允许对一个抽象子类进行实例化和赋值
department.printName();
department.printMeeting();
department.generateReports(); // 错误: 方法在声明的抽象类中不存在
```

## 高级技巧
- 构造函数 在Typescript中声明一个类的时候，实际上声明了很多东西，首先是类的实例的类型。构造函数的类型包含了类的所有静态属性。换个角度说，我们可以认为类具有实例部分和静态部分这两个部分。

```
class Greeter {
    static standardGreeting = "Hello, there";
    greeting: string;
    greet() {
        if (this.greeting) {
            return "Hello, " + this.greeting;
        }
        else {
            return Greeter.standardGreeting;
        }
    }
}

let greeter1: Greeter;
greeter1 = new Greeter();
console.log(greeter1.greet());

let greeterMaker: typeof Greeter = Greeter; 
// typeof Greeter 意思为取Greeter类的类型。而不是实例的类型。
// 或者更确切的说，‘告诉我Greeter标识符的类型’，也就是构造函数的类型。
greeterMaker.standardGreeting = "Hey there!";

let greeter2: Greeter = new greeterMaker();
console.log(greeter2.greet());
```

- 把类当做接口使用 类定义会创建两个东西：类的实例类型和一个构造函数。 因为类可以创建出类型，所以你能够在允许使用接口的地方使用类。

```
class Point {
    x: number;
    y: number;
}

interface Point3d extends Point {
    z: number;
}
let point3d: Point3d = {x: 1, y: 2, z: 3};
```

## 函数

```
# 完整的函数类型书写
function add (x : number, y : number) : number {
    return x + y;
}
let orgAdd : ( x : number, y : number ) => number = function ( x : number, y : number ) : number { return x + y; }
参数类型的名字不必与参数名相同，只要类型是匹配的就好了，就认为它是有效的参数类型。

# 推断类型(按上下文归类)
赋值语句一边指定了类型，另一边没有指定类型，这个时候typescript编译器会自动识别出来。

# 可选参数
function arguments( fir : string, sec ?: string ) : string {
    if(sec)
    return fir + '' + sec;
    else
    return fir;
}
区别js和ts的可选参数
js中的参数都是可选的，不传的时候则为undefined，在ts里在参数旁边使用?可以实现可选参数的功能。
可选参数必须跟在必须参数后面

# 剩余参数
js中如果操作多个参数可以使用arguments,而ts中你可以把所有参数收集到一个变量里。
function rest ( fir : string, ...rest : string[] ) : string {
    return fir + ' ' + rest.join(' ');
}

# 方法重载
let suits = ["hearts", "spades", "clubs", "diamonds"];

function pickCard(x: {suit: string; card: number; }[]): number;
function pickCard(x: number): {suit: string; card: number; };
function pickCard(x): any {
    // Check to see if we're working with an object/array
    // if so, they gave us the deck and we'll pick the card
    if (typeof x == "object") {
        let pickedCard = Math.floor(Math.random() * x.length);
        return pickedCard;
    }
    // Otherwise just let them pick the card
    else if (typeof x == "number") {
        let pickedSuit = Math.floor(x / 13);
        return { suit: suits[pickedSuit], card: x % 13 };
    }
}

let myDeck = [{ suit: "diamonds", card: 2 }, { suit: "spades", card: 10 }, { suit: "hearts", card: 4 }];
let pickedCard1 = myDeck[pickCard(myDeck)];
alert("card: " + pickedCard1.card + " of " + pickedCard1.suit);

let pickedCard2 = pickCard(15);
alert("card: " + pickedCard2.card + " of " + pickedCard2.suit);
为了让编译器能够选择正确的检查类型，它与JavaScript里的处理流程相似。 它查找重载列表，尝试使用第一个重载定义。 如果匹配的话就使用这个。 因此，在定义重载的时候，一定要把最精确的定义放在最前面。
```

> 函数的类型只是由参数类型和返回值组成的。 函数中使用的捕获变量不会体现在类型里。 实际上，这些变量是函数的隐藏状态并不是组成API的一部分。

## this

```
# 提供显式的`this`
interface Card {
    suit: string;
    card: number;
}
interface Deck {
    suits: string[];
    cards: number[];
    createCardPicker(this: Deck): () => Card;
}
let deck: Deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    // NOTE: The function now explicitly specifies that its callee must be of type Deck
    createCardPicker: function(this: Deck) {
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);

            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
        }
    }
}

let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();

alert("card: " + pickedCard.card + " of " + pickedCard.suit);
现在TypeScript知道createCardPicker期望在某个Deck对象上调用。 也就是说 this是Deck类型的，而非any，因此--noImplicitThis不会报错了。
```

## 泛型

```
# 举个栗子
function identity<T>(arg: T): T {
    return arg;
}
这里的T为类型变量，表示类型而不是值。
这里T帮助ts捕捉用户输入的类型，之后就可以使用这个类型。使用T当做返回值类型，这样可以知道传入值和返回值类型是相同的。这样就可以跟踪函数里使用的类型的信息。这个就叫做泛型。可以适用于多个类型，不会丢失信息。保持准确性，传入数值类型并返回数值类型。

# 使用泛型函数
1. 传入所有参数，包含类型参数
let output = identity<string>('mystring');
2. 利用类型推论 -- 即编译器会根据传入的参数自动的帮助我们确定T的类型
let output = identity('mystring');
类型推论帮助我们保持代码精简和高可读性。如果编译器不能够自动地推断出类型的话，只能像上面那样明确的传入T的类型，在一些复杂的情况下，这是可能出现的。

# 泛型接口
interface genFn<T> {
    ( arg : T ) : T;
};
let ide : genFn<number> = identity;

# 泛型类
class Gen <T> {
    zero : T;
    add : ( x : T, y : T ) => T;
}
let gen = new Gen<Number>();
泛型类指的是实例部分的类型，所以类的静态属性不能使用这个泛型类型。

# 泛型约束
利用接口来描述约束条件，然后使用这个接口和extends关键字实现约束
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);  // Now we know it has a .length property, so no more error
    return arg;
}
这时的泛型函数被定义了约束，因此它不再是适用于任何类型。

# 泛型约束中使用类型参数
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, "a"); // okay
getProperty(x, "m"); // error: Argument of type 'm' isn't assignable to 'a' | 'b' | 'c' | 'd'.

# 在泛型里使用类类型
class BeeKeeper {
    hasMask: boolean;
}

class ZooKeeper {
    nametag: string;
}

class Animal {
    numLegs: number;
}

class Bee extends Animal {
    keeper: BeeKeeper;
}

class Lion extends Animal {
    keeper: ZooKeeper;
}

function createInstance<A extends Animal>(c: new () => A): A {
    return new c();
}

createInstance(Lion).keeper.nametag;  // typechecks!
createInstance(Bee).keeper.hasMask;   // typechecks!
使用原型属性推断并约束构造函数与类实例的关系。
```

## 枚举
使用枚举我们可以定义一些带名字的常量。 使用枚举可以清晰地表达意图或创建一组有区别的用例。 TypeScript支持数字的和基于字符串的枚举。

```
# 数字枚举
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}
Up初始值为1，其余的成员会从1开始自动增长；或者不给Up初始值，这样Up默认为0，依然其他成员从0开始自动增加。要注意每个枚举成员的值都是不一样的。不带初始化器的枚举或者被放在第一的位置，或者被放在使用了数字常量或其它常量初始化了的枚举后面。

enum Er {
  A  = getVal(),
  B // 这个时候因为A没有值，这样B需要赋值才行，不然会报错。
}

# 使用枚举: 通过枚举属性访问枚举成员，枚举名字来访问枚举类型。
function respond( rec : string, message : Direction ) : void {}
respond( 'rec', Direction.Up );

# 字符串枚举
在一个字符串枚举里，每个成员都必须用字符串字面量，或另外一个字符串枚举成员进行初始化。(简言之: 必须初始化)因为字符串是没有自增长的行为的，但是可以很好的序列化，如果你正在调试并且必须要读一个数字枚举的运行时的值，这个值通常是很难读的，字符串美剧允许你提供一个运行时有意义的并且可读的值，独立于枚举成员的名字。
enum Direction {
  Up = 'Up',
  Down = 'Down',
  Left = 'Left',
  Right = 'Right'
};

# 异构枚举
## 计算的和常量成员
每个枚举成员都带有一个值，它可以是常量或计算出来的。
1. 当满足如下条件时，枚举成员被当做是常量:
* 它是美剧的第一个成员且没有初始化器，这种情况下他被赋值为0
* 它不带有初始化器且它之前的枚举成员是一个数字常量，这种情况下，当前枚举成员的值为它上一个枚举成员的值加1。
* 枚举成员使用常量枚举表达式初始化。常量枚举表达式是Typescript表达式的子集，它可以在编译阶段求值。当一个表达式满足下面条件之一时，他就是一个常量枚举表达式。
  * 一个枚举表达式字面量(主要是字符串字面量或数字字面量)
  * 一个对之前定义的常量枚举成员的引用(可以是在不同的枚举类型中定义的)
  * 带括号的常量枚举表达式
  * 一元运算符 + ， - ， ~ 其中之一应用在了常量枚举表达式
  * 常量枚举表达式作为二元运算符 + ， - ， * ， / ， % ， << , >> , >>> , &, | , ^的操作对象。若常数枚举表达式求值后为NaN或Infinity,则会在编译阶段报错。
2. 除以上情况的枚举成员都被当做是需要计算得出的值。
enum FileAccess {
  // constant members
  None,
  Read = 1 << 1,
  Write = 1 << 2,
  ReadWrite = Read | Write,
  // computed member
  G = '123'.length
}
## 联合枚举与枚举成员的类型
存在一种特殊的非计算的常量枚举成员的子集:字面量枚举成员。字面量枚举成员是指不带有初始值的常量枚举成员，或者是值被初始化为:
1. 任何字符串字面量(例如: 'foo')
2. 任何数字字面量(例如: 1)
3. 应用了一元 - 符号的数字字面量(例如: -1)
当所有枚举成员都拥有字面量枚举值时，它就带有了一种特殊的语义
>1 枚举成员成为了类型(某些成员只能是枚举成员的值))
enum Shape {
  Circle,
  Square
}
interface Circle {
  kind : Shape.Circle,
  radius : number
}
let c : Circle = {
  kind : Shape.Square, // error
  radius : 100
}
>2 枚举类型本身变成了每个枚举成员的联合
enum E {
  Foo,
  Bar
}
function diff(x : E) : void {
  if(x !== E.Foo || x !== E.Bar){
    // error,!==操作不能用于E.Foo和E.Bar类型
  }
}
这个例子里，我们先检查 x是否不是 E.Foo。 如果通过了这个检查，然后 ||会发生短路效果， if语句体里的内容会被执行。 然而，这个检查没有通过，那么 x则 只能为 E.Foo，因此没理由再去检查它是否为 E.Bar。
## 运行时的枚举
枚举是运行时真正存在的对象。
1. 反向映射
数字枚举成员具有反向映射，从枚举值到枚举名字。
enum Core {
  A
}
let a = Core.A;
let nameOfA = Core[a]; // 'A'
引用枚举成员总会生成对属性访问并且永远也不会内联代码。但不会为字符串枚举成员生成反向映射。
2. const 枚举
为了避免在额外生成的代码上的开销和额外的非直接的对枚举成员的访问，我们可以使用const枚举。常量枚举通过在美剧上使用const修饰符来定义。
const enum E {
  A = 1,
  B =  A * 2 
}
常量枚举只能使用常量枚举表达式，并且不同于常规的枚举，它们在编译阶段会被删除。 常量枚举成员在使用的地方会被内联进来。 之所以可以这么做是因为，常量枚举不允许包含计算成员。
3. 外部枚举
用来描述已经存在的枚举类型的形状
declare enum E {
  A = 1,
  B,
  C = 2
}
外部枚举和非外部枚举之间有一个重要的区别，在正常的枚举里，没有初始化方法的成员被当成常数成员。 对于非常数的外部枚举而言，没有初始化方法时被当做需要经过计算的。
```

## 类型推论

TypeScript里，在有些没有明确指出类型的地方，类型推论会帮助提供类型。

```
# 自动推断
let X = 3;
变量X会被推断为数字。这种推断发生在初始化变量和成员，设置默认参数值和决定函数返回值时。

# 最佳通用类型
当需要从几个表达式中推断类型时候，会使用这些表达式的类型来推断出一个最合适的通用类型。
let x = [ 0, 1, null];
当前元素的类型为: number和null，计算通用类型算法会考虑所有的候选类型，并给出一个兼容所有候选类型的类型。若没有找到最佳通用类型的话，类型推断的结果为联合类型。

# 上下文类型
Typescript类型推论也可能按照相反的方向进行。按上下文归类会发生在表达式的类型与相处的位置相关时。
window.onmousedown = function(mouseEvent) {
    console.log(mouseEvent.button);  //<- Error
};
此时检查器使用window.onmousedown函数的类型来判断右边函数表达式的类型。若上下文类型表达式包含了明确的类型信息，上下文的类型则被忽略。
function createZoo(): Animal[] {
    return [new Rhino(), new Elephant(), new Snake()];
}
上下文归类会在很多情况下使用到。 通常包含函数的参数，赋值表达式的右边，类型断言，对象成员和数组字面量和返回值语句。 上下文类型也会做为最佳通用类型的候选类型。这个例子里，最佳通用类型有4个候选者：Animal，Rhino，Elephant和Snake。 当然， Animal会被做为最佳通用类型。
```

## 类型兼容性
TypeScript里的类型兼容性是基于结构子类型的。 结构类型是一种只使用其成员来描述类型的方式。 它正好与名义（nominal）类型形成对比。 
在基于名义类型的类型系统中，数据类型的兼容性或等价性是通过明确的声明和/或类型的名称来决定的。这与结构性类型系统不同，它是基于类型的组成结构，且不要求明确地声明。

```
interface Named {
    name: string;
}

class Person {
    name: string;
}

let p: Named;
// OK, because of structural typing
p = new Person();
```

TypeScript的结构性子类型是根据JavaScript代码的典型写法来设计的。 因为JavaScript里广泛地使用匿名对象，例如函数表达式和对象字面量，所以使用结构类型系统来描述这些类型比使用名义类型系统更好。TypeScript的类型系统允许某些在编译阶段无法确认其安全性的操作。当一个类型系统具此属性时，被当做是“不可靠”的。TypeScript允许这种不可靠行为的发生是经过仔细考虑的。
TypeScript结构化类型系统的基本规则是，如果x要兼容y，那么y至少具有与x相同的属性。

```
interface Named {
    name: string;
}

let x: Named;
// y's inferred type is { name: string; location: string; }
let y = { name: 'Alice', location: 'Seattle' };
x = y;
function greet(n: Named) {
    alert('Hello, ' + n.name);
}
greet(y); // OK
这里的比较过程是递归进行的，检查每个成员及子成员
```

## 比较两个函数

首先看他们的参数列表。是否前者的每个参数都可以再后者中找到对应类型的参数。注意的是参数的名字相同与否无所谓，只看他们的类型。

```
let x = (a: number) => 0;
let y = (b: number, s: string) => 0;

y = x; // OK
x = y; // Error
```

类型系统强制源函数的返回值类型必须是目标函数返回值类型的子类型。

```
let x = () => ({name: 'Alice'});
let y = () => ({name: 'Alice', location: 'Seattle'});

x = y; // OK
y = x; // Error because x() lacks a location property
```

## 函数参数双向协变

当比较函数参数类型时，只有当源函数参数能够赋值给目标函数或者反过来时才能赋值成功。 这是不稳定的，因为调用者可能传入了一个具有更精确类型信息的函数，但是调用这个传入的函数的时候却使用了不是那么精确的类型信息。 实际上，这极少会发生错误，并且能够实现很多JavaScript里的常见模式。

```
enum EventType { Mouse, Keyboard }

interface Event { timestamp: number; }
interface MouseEvent extends Event { x: number; y: number }
interface KeyEvent extends Event { keyCode: number }

function listenEvent(eventType: EventType, handler: (n: Event) => void) {
    /* ... */
}

// Unsound, but useful and common
listenEvent(EventType.Mouse, (e: MouseEvent) => console.log(e.x + ',' + e.y));

// Undesirable alternatives in presence of soundness
listenEvent(EventType.Mouse, (e: Event) => console.log((<MouseEvent>e).x + ',' + (<MouseEvent>e).y));
listenEvent(EventType.Mouse, <(e: Event) => void>((e: MouseEvent) => console.log(e.x + ',' + e.y)));

// 参数类型不正确
listenEvent(EventType.Mouse, (e: number) => console.log(e));
```


## 可选参数和剩余参数

比较函数兼容性的时候，可选参数与必须参数是可互换的。 源类型上有额外的可选参数不是错误，目标类型的可选参数在源类型里没有对应的参数也不是错误。当一个函数有剩余参数时，它被当做无限个可选参数。常见的函数接受一个回调函数并用对于程序员来说是可预知的参数但对类型系统来说是不确定的参数来调用。

```
function invokeLater(args: any[], callback: (...args: any[]) => void) {
    /* ... Invoke callback with 'args' ... */
}

// Unsound - invokeLater "might" provide any number of arguments
invokeLater([1, 2], (x, y) => console.log(x + ', ' + y));

// 这里回调里x和y都被使用了，但是在回调中作为了可选参数。对于类型系统是不友好的
invokeLater([1, 2], (x?, y?) => console.log(x + ', ' + y));
```

## 函数重载
对于有重载的函数，源函数的每个重载都要在目标函数上找到对应的函数签名。 这确保了目标函数可以在所有源函数可调用的地方调用。

## 枚举兼容
枚举类型与数字类型兼容，并且数字类型与枚举类型兼容。不同枚举类型之间是不兼容的。

## 类兼容
类与对象字面量和接口差不多，但有一点不同: 类有静态部分和实例部分的类型。比较两个类类型的对象时，只有实例的成员会被比较。静态成员和构造函数不在比较的范围内。

```
class Animal {
    feet: number;
    constructor(name: string, numFeet: number) { }
}

class Size {
    feet: number;
    constructor(numFeet: number) { }
}

let a: Animal;
let s: Size;

a = s;  //OK
s = a;  //OK
私有成员会影响兼容性判断。 当类的实例用来检查兼容时，如果目标类型包含一个私有成员，那么源类型必须包含来自同一个类的这个私有成员。 这允许子类赋值给父类，但是不能赋值给其它有同样类型的类。
```

## 泛型兼容

因为TypeScript是结构性的类型系统，类型参数只影响使用其做为类型一部分的结果类型。

```
interface Empty<T> {
}
let x: Empty<number>;
let y: Empty<string>;

x = y;  // okay, y matches structure of x
```

上面代码里，x和y是兼容的，因为它们的结构使用类型参数时并没有什么不同。 把这个例子改变一下，增加一个成员，就能看出是如何工作的了：
```
interface NotEmpty<T> {
    data: T;
}
let x: NotEmpty<number>;
let y: NotEmpty<string>;

x = y;  // error, x and y are not compatible
```
在这里，泛型类型在使用时就好比不是一个泛型类型。

对于没指定泛型类型的泛型参数时，会把所有泛型参数当成any比较。 然后用结果类型进行比较，就像上面第一个例子。

```
let identity = function<T>(x: T): T {
    // ...
}

let reverse = function<U>(y: U): U {
    // ...
}

identity = reverse;  // Okay because (x: any)=>any matches (y: any)=>any
```

# React 中的 TypeScript
## 类组件的使用
```
interface IProps {
  name: string;
}

interface IState {
  color: "red" | "blueviolet"
}
class Home extends React.Component<IProps, IState> {
  constructor(props: IProps){
    super(props);
    this.state = {
      color: "red"
    }
  }

  public onClickColor = () => {
    const { color } = this.state;
    if (color === "red") {
      this.setState({
        color: "blueviolet"
      });
    }
    if (color === "blueviolet") {
      this.setState({
        color: "red"
      });
    }
  }

  public render(){
    const { name } = this.props;
    const { color } = this.state;
    return (
      <div>
        <span style={{ color }}>{ name }</span>
        <button onClick={this.onClickColor}>变颜色</button>
      </div>
    );
  }
}


export default Home;
```

## 函数式组件
```
interface IFuncProps {
  name: string;
}
const FuncComp: React.FC<IFuncProps> = ({ name }) => {
  return (
    <div>{ name }</div>
  )
}
```

With hooks:
```
interface IFuncProps {
  name: string;
}

const FuncComp2: React.SFC<IFuncProps> = ({ name }) => {
  const [ num, setNum ] = React.useState<number>(0);
  return (
    <div>
      { name } { num }
      <button onClick={() => {
        setNum(num + 1);
      }}>+</button>
    </div>
  )
}
function useState<S>(initialState: S | (() => S)): [S, Dispatch<SetStateAction<S>>];
```
