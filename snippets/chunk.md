# Intro
Chunks an array into smaller arrays of a specified size.
```
chunk([1, 2, 3, 4, 5], 2); // [[1,2],[3,4],[5]]
```
# Implement
## My Implement
### thinking
按照指定大小切分数组；
首先想到的是使用reduce方法，实现完了发现有点复杂，更进一步思考用slice函数实现更优雅一些。
### coding
```
// 实现一：
const chunk = (arr, size) => arr.reduce((result,target,index)=>{
    let targetArray = result[result.length-1];
    if(targetArray === undefined){
        targetArray = []
        result.push(targetArray);
    }
    targetArray.length<size?targetArray.push(target):result.push([target]);
    return result;
},[]);
// 实现二：
const chunk = (arr, size) => (new Array(Math.ceil(arr.length/size))).fill(null).map((item,i)=>arr.slice(i*size,i*size+size))
```
### test
```
chunk([1, 2, 3, 4, 5], 2); // [[1,2],[3,4],[5]]
```

## lodash Implement
```
const chunk = (arr, size) =>
  Array.from({ length: Math.ceil(arr.length / size) }, (v, i) =>
    arr.slice(i * size, i * size + size)
  );
```
# Wrapping up
没有想到更方便的 Array.from 方法。
