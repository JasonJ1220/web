# toString()、toLocaleString()、valueOf()
Array、Boolean、Date、Number等对象都具有toString()、toLocaleString()、valueOf()三个方法，那这三个方法有什么区别？

基于不同的数据类型，举例说明，原始数据如下：
```
//Object 
var object = {
    'a':1,
    'b':'efg',
    'c':false
};
//Array
var array = [1,'abcd',new Date('2019-1-1'),true,{'a':1,'b':'efg','c':false}];
//Number
var number = 100;
//String
var string = 'jasonj';
//Date
var date = new Date('2019-1-1');
//Boolean
var boolean = Boolean(false);
//Symbol
var symbol = Symbol('jasonj');
```


## valueOf
```
object.valueOf()//{a: 1, b: "efg", c: false}
array.valueOf()//[1, "abcd", Tue Jan 01 2019 00:00:00 GMT+0800 (中国标准时间), true, {a: 1, b: "efg", c: false}]
number.valueOf()//100
string.valueOf()//'jasonj'
date.valueOf()//1546272000000
boolean.valueOf()//false
symbol.valueOf()//Symbol(jasonj)
```

## toString
```
object.toString()//"[object Object]"
array.toString()//"1,abcd,Tue Jan 01 2019 00:00:00 GMT+0800 (中国标准时间),true,[object Object]"
number.toString()//'100'
string.toString()//'jasonj'
date.toString()//"Tue Jan 01 2019 00:00:00 GMT+0800 (中国标准时间)"
boolean.toString()//"false"
symbol.toString()//Symbol(jasonj)
```

## toLocaleString
```
object.toLocaleString()//"[object Object]"
array.toLocaleString()//[1, "abcd", 2019/1/1 上午12:00:00, true, {a: 1, b: "efg", c: false}]
number.toLocaleString()//'100'
string.toLocaleString()//'jasonj'
date.toLocaleString()//'2019/1/1 上午12:00:00'
boolean.toLocaleString()//'false'
symbol.toLocaleString()//Symbol(jasonj)
```

## 对比
