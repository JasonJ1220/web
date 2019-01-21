# Break camelCase
## Detail
Complete the solution so that the function will break up camel casing, using a space between words.

Example
```
solution('camelCasing') // => should return 'camel Casing'
```

## Answer
**My Solution**
```
function solution(string) {
    return string.replace(/[A-Z]/g,function(item){
          return ' '+item
   });
}
```
**Best Solutions**
```
function solution(string) {
  return(string.replace(/([A-Z])/g, ' $1'));
}
```