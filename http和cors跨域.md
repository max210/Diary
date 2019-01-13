HTTP包括http请求和http响应

##### http请求包括：

* 请求行（`请求方法 URL HTTP/版本号`， 比如：`GET http://m.baidu.com/ HTTP/1.1`，`get`请求的参数会直接追加在url上，而post请求的参数是存在请求体中）
* 请求头
* 请求体

请求头中`Content-Type`的值常见的有`application/x-www-form-urlencoded`和`application/json`。

浏览器的原生`<form>`表单，如果不设置`enctype`属性，那么最终就会以`application/x-www-form-urlencoded`方式提交数据，提交的数据按照 `key1=val1&key2=val2`的方式进行编码，`key`和`val`都进行了`URL`转码：

```
POST http://www.example.com HTTP/1.1
Content-Type: application/x-www-form-urlencoded;charset=utf-8

title=test&sub%5B%5D=1&sub%5B%5D=2&sub%5B%5D=3
```

`application/json` post参数以json的形式传送

```
POST http://www.example.com HTTP/1.1
Content-Type: application/json;charset=utf-8

{"title":"test","sub":[1,2,3]}
```

##### http响应包括：

* 状态行（`HTTP/版本号 返回码 返回码描述`,  比如：`HTTP/1.1 200 OK`）
* 消息头（`Content-Encoding`用作媒体类型的修饰符，比如Content-Encoding：gzip。`Content-Type`指明发送给接收者的实体正文的媒体类型。Expires实体报头域给出响应过期的日期和时间。）
* 消息体（服务器返回的资源的内容，可能是html，也可能是json数据）

##### cors跨域

跨域的方案有很多种，看了阮一峰的关于`cors`文章，对`cors`有了理解，cors跨域主要是要在服务端进行设置，以koa为例

```
import Koa from 'koa'
import cors from 'koa2-cors'

cosnt app = new Koa()

app.use(cors({
    maxAge: 5,
    credentials: true, // 发送cookie
    origin: 'http://localhost:8080',
    allowMethods: ['GET', 'POST', 'DELETE'],
    allowHeaders: ['Content-Type', 'Authorization', 'Accept'],
    exposeHeaders: ['WWW-Authenticate', 'Server-Authorization']
}))

app.use((ctx, next) => {
  ctx.body = 'hello'
})

app.listen(3000)
```

前端用`axios`，需要设置一下`cookie`

```
axios.defaults.withCredentials = true  // axios请求携带cookie
```
这样就OK了。
