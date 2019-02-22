# fetch
在 AJAX 时代，进行 API 等网络请求都是通过 XMLHttpRequest 或者封装后的框架进行网络请求。
现在产生的 fetch 框架简直就是为了提供更加强大、高效的网络请求而生，虽然在目前会有一点浏览器兼容的问题。

虽然 fetch 还不是作为一个稳定的标准发布，但是在其一直迭代更新的 标准描述 中，我们发现已经包含了很多的好东西。
fetch 支持了大部分常用的 HTTP 的请求以及和 HTTP 标准的兼容，如 HTTP Method，HTTP Headers，Request，Response。

## 基本用法
```
fetch('http://example.com/movies.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(myJson);
  });
```
这里我们通过网络获取一个JSON文件并将其打印到控制台。最简单的用法是只提供一个参数用来指明想fetch()到的资源路径，然后返回一个包含响应结果的promise(一个 Response 对象)。

当然它只是一个 HTTP 响应，而不是真的JSON。为了获取JSON的内容，我们需要使用  json()方法（在Bodymixin 中定义，被 Request 和 Response 对象实现）。

## 请求参数
```
// Example POST method implementation:

postData('http://example.com/answer', {answer: 42})
  .then(data => console.log(data)) // JSON from `response.json()` call
  .catch(error => console.error(error))

function postData(url, data) {
  // Default options are marked with *
  return fetch(url, {
    body: JSON.stringify(data), // must match 'Content-Type' header
    cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'same-origin', // include, same-origin, *omit
    headers: {
      'user-agent': 'Mozilla/4.0 MDN Example',
      'content-type': 'application/json'
    },
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, cors, *same-origin
    redirect: 'follow', // manual, *follow, error
    referrer: 'no-referrer', // *client, no-referrer
  })
  .then(response => response.json()) // parses response to JSON
}
```

一个配置项对象，包括所有对请求的设置。可选的参数有：
- method: 请求使用的方法，如 GET、POST。
- headers: 请求的头信息，形式为 Headers 的对象或包含 ByteString 值的对象字面量。
- body: 请求的 body 信息：可能是一个 Blob、BufferSource、FormData、URLSearchParams 或者 USVString 对象。注意 GET 或 HEAD 方法的请求不能包含 body 信息。
- mode: 请求的模式，如 cors、 no-cors 或者 same-origin。
- credentials: 请求的 credentials，如 omit、same-origin 或者 include。为了在当前域名内自动发送 cookie ， 必须提供这个选项， 从 Chrome 50 开始， 这个属性也可以接受 FederatedCredential 实例或是一个 PasswordCredential 实例。
- cache:  请求的 cache 模式: default 、 no-store 、 reload 、 no-cache 、 force-cache 或者 only-if-cached 。
- redirect: 可用的 redirect 模式: follow (自动重定向), error (如果产生重定向将自动终止并且抛出一个错误), 或者 manual (手动处理重定向). 在Chrome中，Chrome 47之前的默认值是 follow，从 Chrome 47开始是 manual。
- referrer: 一个 USVString 可以是 no-referrer、client或一个 URL。默认是 client。
- referrerPolicy: Specifies the value of the referer HTTP header. May be one of no-referrer、 no-referrer-when-downgrade、 origin、 origin-when-cross-origin、 unsafe-url 。
- integrity: 包括请求的  subresource integrity 值 （ 例如： sha256-BpfBw7ivV8q2jLiT13fxDYAe2tJllusRSZ273h2nFSE=）。

## 返回值
一个 Promise，resolve 时回传 Response 对象。

## 实例
### 上传JSON
```
var url = 'https://example.com/profile';
var data = {username: 'example'};

fetch(url, {
  method: 'POST', // or 'PUT'
  body: JSON.stringify(data), // data can be `string` or {object}!
  headers: new Headers({
    'Content-Type': 'application/json'
  })
}).then(res => res.json())
.catch(error => console.error('Error:', error))
.then(response => console.log('Success:', response));
```

### 上传文件
```
var formData = new FormData();
var fileField = document.querySelector("input[type='file']");

formData.append('username', 'abc123');
formData.append('avatar', fileField.files[0]);

fetch('https://example.com/profile/avatar', {
  method: 'PUT',
  body: formData
})
.then(response => response.json())
.catch(error => console.error('Error:', error))
.then(response => console.log('Success:', response));
```

### 上传多文件
```
var formData = new FormData();
var photos = document.querySelector("input[type='file'][multiple]");

formData.append('title', 'My Vegas Vacation');
formData.append('photos', photos.files);

fetch('https://example.com/posts', {
  method: 'POST',
  body: formData
})
.then(response => response.json())
.then(response => console.log('Success:', JSON.stringify(response)))
.catch(error => console.error('Error:', error));
```

### 检测请求是否成功
如果遇到网络故障，fetch() promise 将会 reject，带上一个 TypeError 对象。虽然这个情况经常是遇到了权限问题或类似问题——比如 404 不是一个网络故障。想要精确的判断 fetch() 是否成功，需要包含 promise resolved 的情况，此时再判断 Response.ok 是不是为 true。类似以下代码：
```
fetch('flowers.jpg').then(function(response) {
  if(response.ok) {
    return response.blob();
  }
  throw new Error('Network response was not ok.');
}).then(function(myBlob) { 
  var objectURL = URL.createObjectURL(myBlob); 
  myImage.src = objectURL; 
}).catch(function(error) {
  console.log('There has been a problem with your fetch operation: ', error.message);
});
```

## 自定义请求对象
除了传给 fetch() 一个资源的地址，你还可以通过使用 Request() 构造函数来创建一个 request 对象，然后再作为参数传给 fetch()：
```
var myHeaders = new Headers();

var myInit = { method: 'GET',
               headers: myHeaders,
               mode: 'cors',
               cache: 'default' };

var myRequest = new Request('flowers.jpg', myInit);

fetch(myRequest).then(function(response) {
  return response.blob();
}).then(function(myBlob) {
  var objectURL = URL.createObjectURL(myBlob);
  myImage.src = objectURL;
});


//Request() 和 fetch() 接受同样的参数。你甚至可以传入一个已存在的 request 对象来创造一个拷贝：
var anotherRequest = new Request(myRequest,myInit);
```

## Headers
使用 Headers 的接口，你可以通过 Headers() 构造函数来创建一个你自己的 headers 对象。一个 headers 对象是一个简单的多名值对：
```
var content = "Hello World";
var myHeaders = new Headers();
myHeaders.append("Content-Type", "text/plain");
myHeaders.append("Content-Length", content.length.toString());
myHeaders.append("X-Custom-Header", "ProcessThisImmediately");

//or
myHeaders = new Headers({
  "Content-Type": "text/plain",
  "Content-Length": content.length.toString(),
  "X-Custom-Header": "ProcessThisImmediately",
});
```
变更header的内容:
```
console.log(myHeaders.has("Content-Type")); // true
console.log(myHeaders.has("Set-Cookie")); // false
myHeaders.set("Content-Type", "text/html");
myHeaders.append("X-Custom-Header", "AnotherValue");
 
console.log(myHeaders.get("Content-Length")); // 11
console.log(myHeaders.getAll("X-Custom-Header")); // ["ProcessThisImmediately", "AnotherValue"]
 
myHeaders.delete("X-Custom-Header");
console.log(myHeaders.getAll("X-Custom-Header")); // [ ]
```
如果使用了一个不合法的HTTP Header属性名，那么Headers的方法通常都抛出 TypeError 异常。如果不小心写入了一个不可写的属性，也会抛出一个 TypeError 异常。除此以外的情况，失败了并不抛出异常。例如：
```
var myResponse = Response.error();
try {
  myResponse.headers.set("Origin", "http://mybank.com");
} catch(e) {
  console.log("Cannot pretend to be a bank!");
}
```

最佳实践是在使用之前检查 content type 是否正确，比如：
```
fetch(myRequest).then(function(response) {
  if(response.headers.get("content-type") === "application/json") {
    return response.json().then(function(json) {
      // process your JSON further
    });
  } else {
    console.log("Oops, we haven't got JSON!");
  }
});
```

## Guard
由于 Headers 可以在 request 请求中被发送或者在 response 请求中被接收，并且规定了哪些参数是可写的，Headers 对象有一个特殊的 guard 属性。这个属性没有暴露给 Web，但是它影响到哪些内容可以在 Headers 对象中被操作。
可能的值如下：
- none：默认的
- request：从 request 中获得的 headers（Request.headers）只读
- request-no-cors：从不同域（Request.mode no-cors）的 request 中获得的 headers 只读
- response：从 response 中获得的 headers（Response.headers）只读
- immutable：在 ServiceWorkers 中最常用的，所有的 headers 都只读。

## Response
如上述, Response 实例是在fetch()处理完promises之后返回的.

它的实例也可用通过JavaScript来创建, 但只有在ServiceWorkers中才真正有用,当使用respondWith()方法并提供了一个自定义的response来接受request时:
```
var myBody = new Blob();

addEventListener('fetch', function(event) {
  event.respondWith(new Response(myBody, {
    headers: { "Content-Type" : "text/plain" }
  });
});
```

Response() 构造方法接受两个可选参数—response的数据体和一个初始化对象(与Request()所接受的init参数类似.)
你会用到的最常见的response属性有:
- Response.status — 整数(默认值为200) 为response的状态码.
- Response.statusText — 字符串(默认值为"OK"),该值与HTTP状态码消息对应.
- Response.ok — 如上所示, 该属性是来检查response的状态是否在200-299(包括200,299)这个范围内.该属性返回一个Boolean值.

## Body
不管是请求还是响应都能够包含body对象. body也可以是以下任意类型的实例.
- ArrayBuffer
- ArrayBufferView (Uint8Array and friends)
- Blob/File
- string
- URLSearchParams
- FormData

Body 类定义了以下方法 (这些方法都被 Request 和Response所实现)以获取body内容. 这些方法都会返回一个被解析后的promise对象和数据.
```
arrayBuffer()
blob()
json()
text()
formData()
```

比起XHR来，这些方法让非文本化的数据使用起来更加简单。
请求体可以由传入body参数来进行设置:
```
var form = new FormData(document.getElementById('login-form'));
fetch("/login", {
  method: "POST",
  body: form
})
```
request和response（包括fetch() 方法）都会试着自动设置Content-Type。如果没有设置Content-Type值，发送的请求也会自动设值。

## 特性检测
Fetch API 的支持情况，可以通过检测Headers, Request, Response 或 fetch()是否在Window 或 Worker 域中。例如：
```
if(self.fetch) {
    // run my fetch request here
} else {
    // do something with XMLHttpRequest?
}
```