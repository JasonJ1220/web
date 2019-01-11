# Isogram
## Detail
An isogram is a word that has no repeating letters, consecutive or non-consecutive. Implement a function that determines whether a string that contains only letters is an isogram. Assume the empty string is an isogram. Ignore letter case.
```
isIsogram( "Dermatoglyphics" ) == true
isIsogram( "aba" ) == false
isIsogram( "moOse" ) == false // -- ignore letter case
```
## Answer
**My Solution**
```
function isIsogram(str){
  return new Set(str.toLowerCase()).size === str.length
}
```
**Best Solutions**
```
function isIsogram(str){ 
  return !/(\w).*\1/i.test(str)
}
```