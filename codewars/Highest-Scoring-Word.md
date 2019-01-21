# Highest Scoring Word
## Detail
Given a string of words, you need to find the highest scoring word.

Each letter of a word scores points according to it's position in the alphabet: `a = 1, b = 2, c = 3` etc.

You need to return the highest scoring word as a string.

If two words score the same, return the word that appears earliest in the original string.

All letters will be lowercase and all inputs will be valid.
## Answer
**My Solution**
```
function high(x){
  return x.split(" ").reduce(function(a,b,index){
    return getScore(a) >= getScore(b)?a:b;
  });
}

function getScore(a){
  var sum = 0;
  for(var i=0;i<a.length;i++){
      sum += a[i].charCodeAt() - 96;
    }
    console.log(a,sum);
    return sum;
}
```
**Best Solutions**
```
function high(x){
  //transform the input string into array & define a string of alphabetical latin characters
  var arr = x.split(' ');
  var str = 'abcdefghijklmnopqrstuvwxyz';
  //Iterate through the array with input words to find the one with the greatest sum
  var newArr = arr.map(function(word){
    var sum = 0;
    for (var i = 0; i < word.length; i++) {
      sum += str.indexOf(word[i]);
    }
    return sum;
  });
  //Return the word with the greatest sum
  return arr[newArr.indexOf(Math.max(...newArr))];
}
```