# 跨域demo
所有demo都是使用基于nodeJs的Web开发框架Express，需要一点点nodeJS的知识，如果不会nodeJS也没关系，注释里面会详细解释说明。
用node的web框架express在`3000端口`和`3001端口`分别开启一个静态服务器，在两个端口间进行跨域传输数据。

## Set Up
运行环境
- node.js 全局安装
- npm 全局安装

运行步骤
1. cross-domain目录下npm install，下载项目所需的依赖
2. 在对应的demo目录下，dos窗口输入`run.bat`，直接一步完成上面的

## demo 说明

### 1. CORS跨域
示例如何通过 Access-Control-Allow-Origin 解决跨域。

### 2. JSONP跨域
通过JSONP跨域

### 3. postMessage跨域
这是html5的新API，适用于不同窗口iframe之间的跨域

### 4. window.name跨域
在 http://localhost:3000 使用js动态生成一个隐藏的iframe，设置src属性为' http://localhost:3001 '，等这个iframe加载完之后，重新设置src属性为同源的地址' http://localhost:3000 '(或者其他同源地址)，现在站点与iframe同源，那就可以访问window.name属性，而name值没有变化，因为window.name属性在不同的页面（甚至不同域名）加载后依旧存在。

### 5. location.hash跨域
在 http://localhost:3000/a.html 使用js动态生成一个隐藏的iframe，设置src属性为' http://localhost:3001/c.html#getdata '，在c.html判断hash值是否为'#getdata'，如果为'#getdata'，则在当前的iframe(c.html)中再生成一个隐藏的iframe，其src属性指向' http://localhost:3000/b.html '，因为a.html和b.html同源，所以可以在b.html里面修改a.html的hash值，这样a.html就可以通过获取自身的hash值得到数据

### 6. document.domain跨域
document.domain设置成自身或更高一级的父域，且主域必须相同。

### 7. 后端设置代理proxy跨域
因为JS同源策略是浏览器的安全策略，所以在浏览器客户端不能跨域访问，而服务器端调用HTTP接口只是使用HTTP协议，不会执行JS脚本，不需要同源策略，也就没有跨越问题。简单地说，就是浏览器不能跨域，后台服务器可以跨域。

- demo1
通过使用`http-proxy-middleware插件`设置后端的代理，在 http://localhost:3000 运行

- demo2
不使用插件配置代理，直接使用`http模块`发出请求， 在 http://localhost:3000 运行

### 8. WebSocket跨域
WebSocket是一种通信协议，使用ws://（非加密）和wss://（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。