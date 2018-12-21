# Intro
Creates a function that returns the result of invoking the given functions with the this binding of the created function, where each successive invocation is supplied the return value of the previous.
```
function square(n) {
  return n * n;
}
 
var addSquare = _.flow([_.add, square]);
addSquare(1, 2);
// => 9
```
# Implement
## My Implement
### thinking
总结下来要注意的有以下几点:
1. flow函数的参数是一个函数的数组，返回值是一个函数；
2. 参数中，除了函数数组的第一个函数可以接收多个参数，其他函数的接受参数都是上一个函数的返回值，所以初始函数的参数是**多元**的，而其他函数的接受值是**一元**的。
3. 作为参数的函数数组，以此从左向右执行，前一个函数的返回值作为下一个函数的参数，直到最后一个函数执行完成返回结果。

### coding
```
var flow = function (args) {
    var index = 0,
        result = null;

    return function recursion(...args1) {
        if (index === args.length) {
            return result;
        } else {
            result = args[index].apply(null, args1);
            index++;
            return recursion.call(null, result);
        }
    };
}
```
### test
```
var greeting = (firstName, lastName) => 'hello, ' + firstName + ' ' + lastName
var toUpper = str => str.toUpperCase()
var fn = flow([greeting, toUpper])
console.log(fn('jack', 'smith'))//HELLO, JACK SMITH
```

## lodash Implement
```
/**
 * Composes a function that returns the result of invoking the given functions
 * with the `this` binding of the created function, where each successive
 * invocation is supplied the return value of the previous.
 *
 * @since 3.0.0
 * @category Util
 * @param {Function[]} [funcs] The functions to invoke.
 * @returns {Function} Returns the new composite function.
 * @see flowRight
 * @example
 *
 * function square(n) {
 *   return n * n
 * }
 *
 * const addSquare = flow([add, square])
 * addSquare(1, 2)
 * // => 9
 */
function flow(funcs) {
  const length = funcs ? funcs.length : 0
  let index = length
  while (index--) {
    if (typeof funcs[index] != 'function') {
      throw new TypeError('Expected a function')
    }
  }
  return function(...args) {
    let index = 0
    let result = length ? funcs[index].apply(this, args) : args[0]
    while (++index < length) {
      result = funcs[index].call(this, result)
    }
    return result
  }
}
```
# Wrapping up
lodash的本来实现是从左到右的，但也提供了从右到左的flowRight，还多了一层函数的校验，而且接收的是数组，不是参数序列,而且从这行var result = length ? funcs[index].apply(this, args) : args[0]可以看出允许数组为空，可以看出还是非常严谨的。我写的就缺少这种严谨的异常处理。