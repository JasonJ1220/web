# Replace-With-Alphabet-Position
## Detail
Welcome.
In this kata you are required to, given a string, replace every letter with its position in the alphabet.
If anything in the text isn't a letter, ignore it and don't return it.
```
"a" = 1, "b" = 2, etc.
```
Example
```
alphabet_position("The sunset sets at twelve o' clock.")
```
Should return "20 8 5 19 21 14 19 5 20 19 5 20 19 1 20 20 23 5 12 22 5 15 3 12 15 3 11" (as a string)
## Answer
**My Solution**
```
function alphabetPosition(text) {
  var alphabet="abcdefghijklmnopqrstuvwxyz",
      result = "";
  for(var i = 0; i< text.length;i++){
    var index = alphabet.indexOf(text[i].toLowerCase());
    if(index > -1 ){
      result += index+1 + " ";
    }
  }
  return result.slice(0,result.length-1);
}
```
**Best Solutions**
```
function alphabetPosition(text) {
  return text
    .toUpperCase()
    .match(/[a-z]/gi)
    .map( (c) => c.charCodeAt() - 64)
    .join(' ');
}
```