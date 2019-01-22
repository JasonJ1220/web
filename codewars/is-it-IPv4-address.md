# Regexp Basics - is it IPv4 address?
## Detail
Implement String#ipv4_address?, which should return true if given object is an IPv4 address - four numbers (0-255) separated by dots.

It should only accept addresses in canonical representation, so no leading 0s, spaces etc.

Example
```
solution('camelCasing') // => should return 'camel Casing'
```

## Answer
**My Solution**
```
String.prototype.ipv4Address=function(){
  return /^((2[0-4]\d|25[0-5]|0|1?[1-9]\d?)\.){3}(2[0-4]\d|25[0-5]|0|1?[1-9]\d?)$/g.test(this);
}
```
**Best Solutions**
```
String.prototype.ipv4Address = function() {
  return /^(([1-9]?\d|1\d\d|2[0-4]\d|25[0-5])(\.(?!$)|$)){4}$/.test(this);
};
```