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


## toString()方法与toLocalString()方法区别
- toLocalString()是调用每个数组元素的 toLocaleString() 方法，然后使用地区特定的分隔符把生成的字符串连接起来，形成一个字符串。
- toString()方法获取的是String(传统字符串),而toLocaleString()方法获取的是LocaleString(本地环境字符串)。
- 如果你开发的脚本在世界范围都有人使用，那么将对象转换成字符串时请使用toString()方法来完成。
- LocaleString()会根据你机器的本地环境来返回字符串，它和toString()返回的值在不同的本地环境下使用的符号会有微妙的变化。
- 所以使用toString()是保险的，返回唯一值的方法,它不会因为本地环境的改变而发生变化。如果是为了返回时间类型的数据，推荐使用LocaleString()。若是在后台处理字符串，请务必使用toString()。