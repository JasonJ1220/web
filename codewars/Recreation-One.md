# Integers: Recreation One
## Detail
Divisors of 42 are : 1, 2, 3, 6, 7, 14, 21, 42. These divisors squared are: 1, 4, 9, 36, 49, 196, 441, 1764. The sum of the squared divisors is 2500 which is 50 * 50, a square!

Given two integers m, n (1 <= m <= n) we want to find all integers between m and n whose sum of squared divisors is itself a square. 42 is such a number.

The result will be an array of arrays or of tuples (in C an array of Pair) or a string, each subarray having two elements, first the number whose squared divisors is a square and then the sum of the squared divisors.

Examples:
```
list_squared(1, 250) --> [[1, 1], [42, 2500], [246, 84100]]
list_squared(42, 250) --> [[42, 2500], [246, 84100]]
```
The form of the examples may change according to the language, see Example Tests: for more details.

Note

In Fortran - as in any other language - the returned string is not permitted to contain any redundant trailing whitespace: you can use dynamically allocated character strings.
## Answer
**My Solution**
```
function listSquared(m, n) {
  var result = [];
  for(var i =m ; i<=n ; i++){
    var divisors = getDivisors(i),
        sum = divisors.reduce(function(a,b){return a+b;}),
        sqrtNum = Math.sqrt(sum);
    if(Math.floor(sqrtNum) === sqrtNum){
      result.push([i,sum]);
    }
  }
  return result;
}

function getDivisors(num){
  var result = [];
  for(var i = 1;i<=num; i++){
    if(num % i === 0){
      result.push(Math.pow(i,2));
    }
  }
  return result;
}
```
**Best Solutions**
```
function listSquared (m, n) {
  var matches = [];

  for (var i = m; i <= n; ++i) {
    var sum = getDivisors(i).reduce((sum, n) => sum + n * n, 0);
    var ok = Number.isInteger(Math.sqrt(sum));

    if (ok) {
      matches.push([i, sum]);
    }
  }

  return matches;
}

function getDivisors (n) {
  var divisors = [];

  for (var i = 1; i <= n / 2; ++i) {
    if (n % i) {
      continue;
    }

    divisors.push(i);
  }

  return divisors.concat([n]);
}
```