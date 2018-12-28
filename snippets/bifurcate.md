# Intro
Splits values into two groups. If an element in second array is truthy, the corresponding element in the collection belongs to the first group; otherwise, it belongs to the second group.
```
bifurcate(['beep', 'boop', 'foo', 'bar'], [true, true, false, true]); // [ ['beep', 'boop', 'bar'], ['foo'] ]
```
# Implement
## My Implement
### thinking
使用 reduce 方法解决。
### coding
```
const bifurcate = (arr, filter)=> arr.reduce((result,target,index)=>{
    filter[index]?result[0].push(target):result[1].push(target);
    return result;
},[[],[]]);
```
### test
```
bifurcate(['beep', 'boop', 'foo', 'bar'], [true, true, false, true]); // [ ['beep', 'boop', 'bar'], ['foo'] ]
```

## lodash Implement
```
const bifurcate = (arr, filter) =>
  arr.reduce((acc, val, i) => (acc[filter[i] ? 0 : 1].push(val), acc), [[], []]);
```
# Wrapping up
思路一致，lodash 实现方式比我更简练。