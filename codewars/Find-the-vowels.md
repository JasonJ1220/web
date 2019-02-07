# Find the vowels
## Detail
We want to know the index of the vowels in a given word, for example, there are two vowels in the word super (the second and fourth letters).

So given a string "super", we should return a list of [2, 4].

Example
```
Mmmm  => []
Super => [2,4]
Apple => [1,5]
YoMama -> [1,2,4,6]
```
NOTE: Vowels in this context refers to English Language Vowels - a e i o u y

NOTE: this is indexed from [1..n] (not zero indexed!)

## Answer
**My Solution**
```
function vowelIndices(word){
  var result = [],
      vowels = ["a","e","i","o","u","y"],
      index = 1;
  
  word.split("").forEach(function(letter){
    if(vowels.includes(letter.toLowerCase())){
      result.push(index);
    }
    index++;
  });
  return result;
}
```
**Best Solutions**
```
function vowelIndices(word) {
  var arr = [];
  for(var i = 0; i < word.length; i++) {
    if(/[aeioyu]/i.test(word[i])) {
      arr.push(i+1);
    }
  }
  return arr;
}
```