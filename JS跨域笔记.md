因项目本地开发时，调用API都是涉及到跨域的问题，而现在前端工程化，前端构建工具会集成跨域的功能，因此也没有深入地区探究跨域的问题，而自己本身也对跨域问题还有一些模糊之处，因此决定写下这篇文章，督促自己了解的同时，也是做个记录，方便以后回顾。

#### 本文目录结构：

1. 什么是跨域
2. 现阶段跨域的解决方案及案例
3. 最佳实践

#### 一、什么是跨域

##### 1. 跨域的两个误区

1. 动态请求就会有跨域问题
2. 跨域就是请求发不出去

对于误区1，跨域仅仅存在于浏览器端，不存在于其他环境；
  
对于误区2，只要网络没有问题，所有跨域的请求都是能正常发送出去，并且服务端也能收到请求并正常返回结果，只是由于跨域限制，被浏览器拦截了。
  
这也是为什么我们用`postman`等代理工具模拟请求时，可以获取到返回信息;
  
如果是非简单请求，(除`GET`,`POST`,`HEAD`之外，且http头信息不超出一下字段：`Accept、Accept-Language`、 `Content-Language`、 `Last-Event-ID`、 `Content-Type`(限于三个值：`application/x-www-form-urlencoded`、`multipart/form-data`、`text/plain`))，都会先发出预请求（`preflight`），预请求询问服务端该请求允许跨域否，接着服务端会返回只有`headers`不含`body`的信息，然后浏览器根据`headers`中的信息进行判断，若是允许跨域，则再次发送请求，否则抛出跨域限制的错误。

##### 2. 为什么跨域仅仅限制读取远端的数据

如果限制写入端（也就是发送请求端），那么服务器的资源仅仅只能同源请求，无法做到资源共享。

##### 3. 浏览器如何识别一个请求是否跨域

- 浏览器识别跨域是基于[同源策略][sameOrigin]
[sameOrigin]: https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy

同源策略限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的重要安全机制。
  
如果两个页面的协议（如：`http`,`https`）、域名或`ip`（如：`binnera.com.cn`）、端口（一般web网站都是默认80端口）都相同，则两个页面具有相同的源，而只要其中任意一个不同，则浏览器则会将这两个源之间的请求视为跨域。
  
我们以`http://www.binenar.com.cn`为例，进行具体说明

|链接|结果|原因|
| :-: | :-: | :-: |
| `http://www.binenar.com.cn`/blog		|是|同协议同域名同端口（默认80端口）|
| `http://www.binenar.com.cn`/blog		|是|同协议同域名同端口|
| `http://www.binenar.com.cn:81`/blog	|否|同协议同域名不同端口|
| `https://www.binenar.com.cn`			|否|协议不同|
| `http://binenar.com.cn`/blog			|否|域名不同（如果有做域名映射，那么两个域名可以指向同一个ip）|
| `http://b.binenar.com.cn`/blog		|否|域名不同|


##### 4. 浏览器跨域限制主要限制了什么

1. 不同的源无法读取对方的`Cookie`、`LocalStorage` 和 `IndexDB`；
2. 无法获取`DOM`,`BOM`；
3. JS无法获取`AJAX`以及Fetch请求的结果。

##### 5. 浏览器允许的跨域资源请求

浏览器允许嵌入跨域资源的请求
- `<script src="..."></script>`标签嵌入跨域脚本;
- `<link rel="stylesheet" href="...">` 标签嵌入CSS,CSS的跨域需要一个设置正确的Content-Type 消息头;
- `<img src="...">`嵌入图片；
- `<video>` 和 `<audio>`嵌入多媒体资源；
- `@font-face` 引入的字体；
- `<frame>` 和 `<iframe>` 载入的任何资源，可通过设置X-Frame-Options消息头来阻止iframe嵌入资源。


#### 二、现阶段跨域的解决方案及案例

##### 1. 跨域资源共享（CORS）

如果是简单请求，请求发送出去时，浏览器会在请求头添加Origin字段：

```http
Origin: http://binnear.com.cn
```

告诉服务端该请求是来自那个源。

  
接着服务端接受到请求后，并在响应头加上如下字段：

```http
Access-Control-Allow-Origin: http://binnear.com.cn
```

代表服务端允许的域，浏览器收到后，便会允许此次请求，以上字段若被设置为*，则表示可接受任意的源访问。
  
如果是非简单请求：

```javascript
const url = 'http://binnear.com.cn/data';
const xhr = new XMLHttpRequest();
xhr.open('PUT', url, true);
xhr.setRequestHeader('X-Custom-Header', 'value');
xhr.send();
```

浏览器则会发送预请求，在请求头会添加以下字段：

```http
OPTIONS /data HTTP/1.1
Origin: http://binnear.com.cn
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
```

OPTIONS是预请求的识别字段，`Access-Control-Request-Method`列出请求方法，`Access-Control-Request-Headers`指定发送的额外的头信息。
  
服务端收到预请求后，检查请求的字段后，确认允许跨域，便做出响应，

```http
Access-Control-Allow-Origin: http://binnear.com.cn
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: X-Custom-Header
```

响应头中会包含以上三个字段，正好对应我们请求头中添加的字段，表示允许的域，允许的请求方法，以及额外的请求头。浏览器接受到后，知道这次请求已被许可，接着发送真正的请求。

##### 2. JSONP跨域

相信每一个接触过跨域的程序员都或多或少了解过JSONP跨域，它的原理也很简单，就是利用上面我们提过的嵌入跨域资源请求的方法。

```javascript
<script type='text/javascript'>
    function localFn(data) {
        console.log('这是获取到的远程数据'：data)
    }
    const script = document.createElement('script');
    script.src = 'http://binnear.com.cn?callback=localFn';
    document.body.appendChild(script);
</script>
```

1. 首先我们定义了一个全局函数`localFn`;
2. 创建一个`script`标签，并将`src`属性指向我们需要跨域请求的API;
3. 将创建的`script`标签添加到页面

通过以上3步，我们发送上面所示的API请求，然后服务端会返回一段可执行的JS代码：

```javascript
localFn({remark: '我是远程数据对象里面的属性值'})
```

因为我们之前定义了全局的`localFn`，所以这段代码就会执行`localFn`这个函数，并将数据传递给形参`data`，在`localFn`内我们就通过`data`获取到服务端的数据了。
  
注意点：`crc`中的`localFn`可以为任意名，`callback`这个`key`是由接口提供者所定义。

##### 3. 基于iframe的跨域

1. 通过`window.name`传输跨域资源
2. 通过`window.postMessage`传输跨域资源

讲述以上方法之前我们先本地配置一下本地跨域模拟环境

![](https://user-gold-cdn.xitu.io/2018/9/8/165b714d9ee42046?w=309&h=127&f=png&s=7418)

`sever1.js`配置如下：

```javascript
const http = require('http');
const fs = require('fs');
const documentRoot = 'D:/code/sever1/';

const server = http.createServer(function (req, res) {
    const url = req.url;
    const file = documentRoot + url;
    fs.readFile(file, function (err, data) {
        if (err) {
            res.writeHeader(404, {
                'content-type': 'text/html;charset="utf-8"'
            });
            res.write('<h1>404错误</h1><p>你要找的页面不存在</p>');
            res.end();
        } else {
            res.write(data);
            res.end();
        }
    });
}).listen(8888);

console.log('服务器开启成功');
```

`sever2.js`的配置与`sever1.js`大致相同，只不过我们将文件路径更改为`domain2`的路径，监听的端口号改为了`8889`。

```javascript
const http = require('http');
const fs = require('fs');
const documentRoot = 'D:/code/sever2/';

const server = http.createServer(function (req, res) {
    const url = req.url;
    const file = documentRoot + url;
    fs.readFile(file, function (err, data) {
        if (err) {
            res.writeHeader(404, {
                'content-type': 'text/html;charset="utf-8"'
            });
            res.write('<h1>404错误</h1><p>你要找的页面不存在</p>');
            res.end();
        } else {
            res.write(data);
            res.end();
        }
    });

}).listen(8889);

console.log('服务器开启成功');
```

`domain1.html`配置

```xml
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>domain1</title>
</head>
<body>
  <div>this is domain 1</div>
</body>
</html>
```

`domain2.html`配置

```xml
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>domain1</title>
</head>
<body>
  <div>this is domain 2</div>
</body>
</html>
```

接着打开两个命令行工具，分别进入sever1和sever2文件夹，执行以下命令：

```bash
node ./sever1.js
node ./sever2.js
```

接着进入到浏览器中，输入如下链接

```xml
http://localhost:8888/domain1.html
http://localhost:8888/domain2.html
```

页面输出`this is domain 1`和`this is domain 2`则启动成功，到此我们前期的准备已经完成,接下来我们利用搭建好的环境来模拟`window.name`如何进行跨域传输数据。

##### window.name

在domain1中我们添加以下代码:

```javascript
<script>
	window.name = JSON.stringify({info: 'this is domain1\'s name'})
</script>
```

我们在domain1中，将一段json字符串赋值给了domain1中window的name属性。

接着我们在domain2中添加如下代码

```javascript
<script>
    const iframe = document.createElement('iframe');
    iframe.style.display = 'none';
    iframe.src = 'http://localhost:8888/domain1.html';
    document.body.appendChild(iframe);

    iframe.onload = () => {
      console.log(iframe.contentWindow.name)
    }
</script>
```

保存后，我们进入http://localhost:8889/domain2.html这个页面，然后刷新，打开控制台：

![](https://user-gold-cdn.xitu.io/2018/9/8/165b73ee6c27d527?w=778&h=36&f=png&s=8956)

咦，居然被跨域限制了，不是说好`window.name`传输跨域资源的吗？怎么还是被限制了呢？
  
不要急，猜想下，我们是不是忽略了什么事情，然后翻阅资源后，发现当前文件的所在的源与iframe的src指向的源不同，那么就无法操作iframe中的任何东西，自然window.name也就无法读取了。
  
原来是iframe的跨域限制了，那么问题来了，既然这样，window.name那不就是没有办法跨域传输数据了吗？
  
对于上面问题，window.name自身提供了的一个神奇功能，给了我们跨域传输的可能：那就是window.name的值在不同页面或域下，加载后依然存在。
  
结合这个功能我们再来优化一下我们在domain2.html中新加的代码：

```javascript
<script>
    const iframe = document.createElement('iframe');
    iframe.style.display = 'none';
    iframe.src = 'http://localhost:8888/domain1.html';
    document.body.appendChild(iframe);

    iframe.onload = () => {
      iframe.src = 'about:blank' // 新增代码
      console.log(iframe.contentWindow.name)
    }
</script>
```

我们新增了一行代码，就是在`iframe`加载完后，立马将`scr`指向`domain2`的源，这个时候`iframe`就与`domain2`的源一致了，我们就能读取到了`iframe`下的`window.name`属性了，而且因为`window.name`的神奇的功能，它的值依然是我们在`domain1`中设置的值。很好，成功似乎在向我们招手了，保存文件，浏览器打开domain2的链接，刷新。
  
![](https://user-gold-cdn.xitu.io/2018/9/8/165b75aa1aad6ddf?w=857&h=317&f=png&s=40725)

`window.name`的数据我们确实获取到了，但是控制却在不停地输出日志，仔细思考一下，发现是`iframe`的`scr`重新指向后，便触发了`onload`，导致进入死循环，再次优化，同时避免404的`error`，代码在次优化如下：

```javascript
<script>
    const iframe = document.createElement('iframe');
    iframe.style.display = 'none';
    iframe.src = 'http://localhost:8888/domain1.html';
    document.body.appendChild(iframe);
    let state = 0;
    iframe.onload = () => {
      if (state === 0) {
        state = 1
        iframe.src = ''
      }
      if (state === 1) {
        console.log(iframe.contentWindow.name)
        document.body.removeChild(iframe);
      }
    }
</script>
```

保存后在次刷新页面，我们终于如愿以偿地得到了我们希望的结果：

![](https://user-gold-cdn.xitu.io/2018/9/8/165b76b0592d201f?w=951&h=53&f=png&s=10905)

没有了死循环，数据正常获取，只是唯一不舒服的地方就是仍然有跨域限制的报错，怎么消除这个`error`，就留给爱探索的你了~~
  
小记：window.name可以携带的信息限制为2M。

##### window.postMessager
  
我们依旧用sever1和sever2文件夹的内容，删除关于window.name的相关代码，接着我们在`domain1.html`中添加如下代码：

```javascript
<script>
    window.addEventListener('message', function (e) {
      console.log('data from domain1 ---> ' + e.data);
      const data = { info: 'this is domain2' }
      window.parent.postMessage(JSON.stringify(data), 'http://localhost:8889');
    }, false);
</script>
```

在`domain2.html`中添加如下代码


```javascript
<script>
    const iframe = document.createElement('iframe');
    iframe.style.display = 'none';
    iframe.src = 'http://localhost:8888/domain1.html';
    document.body.appendChild(iframe);
    iframe.onload = function () {
      const data = { info: 'this is domain 1' };
      iframe.contentWindow.postMessage(JSON.stringify(data), 'http://localhost:8888/');
    };
    window.addEventListener('message', function (e) {
      console.log('data from domain2 ---> ' + e.data);
    }, false);
</script>
```

在`domain2.html`中，我们通过`iframe`将`domain1.html`引入进来，在`iframe`加载完成后，我们通过iframe中的postMessage方法将data数据发送给`domain1.html`所在的域，同时监听`domain2.html`中的message事件
  
接着我们在`domain1.html`中监听`domain1.html`中的`message`事件，获取到`domain2.html`传送过来的`data`，同时将零一份`data`通过`domain2.html`中的`postMessage`方法发送给`domain2.html`所在的域。
  
保存后，刷新`domain2.html`所在的页面，在控制台中我们可以看到如下信息。
  
![](https://user-gold-cdn.xitu.io/2018/9/8/165b7b38c68dfffd?w=587&h=41&f=png&s=4058)
  
这样我们就完成了在`domain2.html`中取`domain1.html`中的数据，并可以做到两者之间的交互。

##### 4. 服务端代理

前文我们有说过，跨域仅仅浏览器端限制了我们读取远程的数据，所以利用这一点，我们可以将跨域资源由服务端代理后，再将资源返回给我们，

##### 5. `WebSocket`协议跨域

WebSocket protocol是HTML5一种新的协议，它实现了浏览器端与服务端的双工通信，同时允许跨域通讯，这里不做叙述，有兴趣的可以去了解一下~
