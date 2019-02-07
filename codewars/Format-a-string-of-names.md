# Format a string of names like 'Bart, Lisa & Maggie'.
## Detail
Given: an array containing hashes of names

Return: a string formatted as a list of names separated by commas except for the last two names, which should be separated by an ampersand.

Example
```
list([ {name: 'Bart'}, {name: 'Lisa'}, {name: 'Maggie'} ])
// returns 'Bart, Lisa & Maggie'

list([ {name: 'Bart'}, {name: 'Lisa'} ])
// returns 'Bart & Lisa'

list([ {name: 'Bart'} ])
// returns 'Bart'

list([])
// returns ''
```

## Answer
**My Solution**
```
function list(names){
  return names.length <=1? names[0] === undefined?"":names[0].name || "":names.reduce(
    function(a,b,index){
      if(index === 1) {
        return a.name + ", " + b.name;
      } 
      else {
        return a +  ", " + b.name
      }}).replace(/(.*),/,"$1 &");
}
```
**Best Solutions**
```
function list(names){
  return names.reduce(function(prev, current, index, array){
    if (index === 0){
      return current.name;
    }
    else if (index === array.length - 1){
      return prev + ' & ' + current.name;
    } 
    else {
      return prev + ', ' + current.name;
    }
  }, '');
 }
//or
function list(names) {
  var xs = names.map(p => p.name)
  var x = xs.pop()
  return xs.length ? xs.join(", ") + " & " + x : x || ""
}
//or
var list = (names) =>  names.map(x => x.name).join(', ').replace(/(.*),(.*)$/, "$1 &$2")
```