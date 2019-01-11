# Two to One
## Detail
Take 2 strings s1 and s2 including only letters from ato z. Return a new sorted string, the longest possible, containing distinct letters,each taken only once - coming from s1 or s2. 
Examples: 
```
let a = "xyaabbbccccdefww",
    b = "xxxxyyyyabklmopq";
longest(a, b) => "abcdefklmopqwxy";
a = "abcdefghijklmnopqrstuvwxyz";
longest(a, b) => "abcdefghijklmnopqrstuvwxyz"
```
## Answer
**My Solution**
```
function longest(s1, s2) {
  var target = s1.concat(s2);
      instinctList = [];
  for(var i =0;i<target.length;i++){
    if(instinctList.indexOf(target[i]) === -1){
      instinctList.push(target[i]);
    }
  }
  return instinctList.sort().join('');
}
```
**Best Solutions**
```
const longest = (s1, s2) => [...new Set(s1+s2)].sort().join('')
```