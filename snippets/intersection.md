# Intro
Returns a list of elements that exist in both arrays.
```
intersection([1, 2, 3], [4, 3, 2]); // [2, 3]
```
# Implement
## My Implement
### thinking
取数组交集，首先想到的是reduce 和 indexOf。
### coding
```
const intersection = (arr1,arr2) =>
arr1.reduce((a,b)=>(arr2.indexOf(b)>0&&a.push(b),a),[])
```
### test
```
intersection([1, 2, 3], [4, 3, 2]); // [2, 3]
```

## lodash Implement
```
const intersection = (a, b) => {
  const s = new Set(b);
  return a.filter(x => s.has(x));
};
```
# Wrapping up
利用了 Set 的 has方法 以及数组的 filter 方法过滤。
