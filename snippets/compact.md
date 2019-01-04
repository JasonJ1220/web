# Intro
Creates an array with all falsey values removed. The values false, null, 0, "", undefined, and NaN are falsey.
```
compact([0, 1, false, 2, '', 3]);
// => [1, 2, 3]
```
# Implement
## My Implement
### thinking
由于返回数组的元素个数和参数数组的元素个数不一致，不能采用map方法
### coding
```
方法一：
const compact = (arr) =>
arr&&arr.reduce((a,b)=>{return b&&a.push(b),a},[])
方法二
const compact = (arr) =>
arr&&arr.filter((a)=>Boolean(a))
```
### test
```
compact([0, 1, false, 2, '', 3]);
// => [1, 2, 3]
```

## lodash Implement
```
function compact(array) {
  let resIndex = 0
  const result = []

  if (array == null) {
    return result
  }

  for (const value of array) {
    if (value) {
      result[resIndex++] = value
    }
  }
  return result
}
```
# Wrapping up

