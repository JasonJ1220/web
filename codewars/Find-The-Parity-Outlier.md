# Find The Parity Outlier
## Detail
You are given an array (which will have a length of at least 3, but could be very large) containing integers. The array is either entirely comprised of odd integers or entirely comprised of even integers except for a single integer N. Write a method that takes the array as an argument and returns this "outlier" N.

Examples
```
[2, 4, 0, 100, 4, 11, 2602, 36]
Should return: 11 (the only odd number)

[160, 3, 1719, 19, 11, 13, -21]
Should return: 160 (the only even number)
```
## Answer
**My Solution**
```
function findOutlier(integers){
var total = integers.reduce(function(a,b,index){if(index ===1){a = Math.abs(a)%2;}return a+Math.abs(b)%2;});
  var isOddArray =  total > 1;
  for(var i =0; i < integers.length; i++){
    if(isOddArray && integers[i] %2 === 0){
      return integers[i];
    }
    else if(!isOddArray && Math.abs(integers[i]) %2 === 1){
      return integers[i];
    }
  }
}
```
**Best Solutions**
```
function findOutlier(int){
  var even = int.filter(a=>a%2==0);
  var odd = int.filter(a=>a%2!==0);
  return even.length==1? even[0] : odd[0];
}
```