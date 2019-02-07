# Human Readable Time
## Detail
Write a function, which takes a non-negative integer (seconds) as input and returns the time in a human-readable format (HH:MM:SS)

- HH = hours, padded to 2 digits, range: 00 99
- MM = minutes, padded to 2 digits, range: 00 59
- SS = seconds, padded to 2 digits, range: 00 59

The maximum time never exceeds 359999 (99: 59 :59)

You can find some examples in the test fixtures.

## Answer
**My Solution**
```
function humanReadable(seconds) {
  var hour = ("00" + Math.floor(seconds/60/60)).slice(-2),
      min = ("00" + Math.floor((seconds/60 - hour*60))).slice(-2),
      second = ("00" + Math.floor(seconds - min * 60 - hour*60 * 60)).slice(-2);
      
  return hour + ":" + min + ":"+ second;
}
```
**Best Solutions**
```
function humanReadable(seconds) {
  var pad = function(x) { return (x < 10) ? "0"+x : x; }
  return pad(parseInt(seconds / (60*60))) + ":" +
         pad(parseInt(seconds / 60 % 60)) + ":" +
         pad(seconds % 60)
}
```