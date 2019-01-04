# Intro
A convenient version of what is perhaps the most common use-case for map: extracting a list of property values.
```
var stooges = [{name: 'moe', age: 40}, {name: 'larry', age: 50}, {name: 'curly', age: 60}];
pluck(stooges, 'name');
=> ["moe", "larry", "curly"]
```
# Implement
## My Implement
### thinking
获取数组中数据的某个字段，注意异常判断。
### coding
```
const pluck = (arr,path) =>
arr.map(item=>item&&item[path]||null)
```
### test
```
var stooges = [{name: 'moe', age: 40}, {name: 'larry', age: 50}, {name: 'curly', age: 60},null,{}];
pluck(stooges, 'name');
//  ["moe", "larry", "curly", null, null]
```

## lodash Implement
```
const pluck = (arr,key)=>{
  var newArr = [];
  for (var i = 0, x = arr.length; i < x; i++){
    if (arr[i].hasOwnProperty(key)){
      newArr.push(arr[i].key)
    }
  }
  return newArr;
}
```
# Wrapping up
不会返回null对象了。