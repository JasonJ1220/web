# Reversed Words
## Detail
Complete the solution so that it reverses all of the words within the string passed in.

Example:
```
reverseWords("The greatest victory is that which requires no battle")
// should return "battle no requires which that is victory greatest The"
```
## Answer
**My Solution**
```
function reverseWords(str){
  return str.split(" ").reverse().join(" ");; // reverse those words
}
```
**Best Solutions**
```
function reverseWords(str){
  return str.split(' ').reverse().join(' ');
}
```