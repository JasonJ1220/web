# Simple array rotation
## Detail
In this Kata, you will be given an array and your task will be to determine if an array is in ascending or descending order and if it is rotated or not.

Consider the array [1,2,3,4,5,7,12]. This array is sorted in Ascending order. If we rotate this array once to the left, we get [12,1,2,3,4,5,7] and twice-rotated we get [7,12,1,2,3,4,5]. These two rotated arrays are in Rotated Ascending order.

Similarly, the array [9,6,5,3,1] is in Descending order, but we can rotate it to get an array in Rotated Descending order: [1,9,6,5,3] or [3,1,9,6,5] etc.

Arrays will never be unsorted, except for those that are rotated as shown above. Arrays will always have an answer, as shown in the examples below.

Example
```
solve([1,2,3,4,5,7]) = "A" -- Ascending
solve([7,1,2,3,4,5]) = "RA" -- Rotated ascending
solve([4,5,6,1,2,3]) = "RA" -- Rotated ascending
solve([9,8,7,6]) = "D" -- Descending
solve([5,9,8,7,6]) = "RD" -- Rotated Descending
```

## Answer
**My Solution**
```
function solve(arr){
  var max = Math.max.apply(null,arr),
      min = Math.min.apply(null,arr),
      maxIndex = arr.indexOf(max),
      minIndex = arr.indexOf(min);
      console.log(max,min,maxIndex,minIndex);
  if(maxIndex === arr.length-1){
    return "A";
  }
  else if(minIndex === arr.length-1){
    return "D";
  }
  else if(maxIndex < minIndex){
    return "RA";
  }
  else if(maxIndex > minIndex){
    return "RD";
  }
};
```
**Best Solutions**
```
function solve(arr) {
  const
    a = arr[0] < arr[1], b = arr[1] < arr[2], c = arr[arr.length - 1] < arr[0],
    m = a == b ? a : c;
  return (c == m ? 'R' : '') + (m ? 'A' : 'D');
}
```