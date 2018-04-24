#### vue-cli中使用axios

目前vue官方已经宣布vue-resource官方已经停止维护，推荐使用axios，我就把之前的项目用更换为axios。遇到了一些问题，通过查找资料，总结一下。

##### 如何在vue中全局使用axios

两种方法，一种直接把axios挂载到vue构造函数的prototype属性上，组件中通过this.axios来拿到axios
第二种方法就是通过vue-axios来全局使用axios，组件中也是通过this.axios来拿到axios

```
// 第一种方法
// main.js
import Vue from 'vue'
import axios from 'axios'

Vue.prototype.axios = axios


//第二种方法
// main.js
import Vue from 'vue'
import axios from 'axios'
import Vueaxios from 'vue-axios'

Vue.use(Vueaxios, axios)
```

##### axios的get与post方法

axios的get方法params传递参数

```
// 组件中使用get方法
this.axios.get(url, parmas: {a: 1, b: 2})
  .then(res => {
    // 成功回调
  }, res => {
    // 错误回调
  })


// 组件中使用post方法
this.axios.post(url, {a: 1, b: 2})
  .then(res => {
    // 成功回调
  }, res => {
    // 错误回调
  })
```

对于post方式，提交的数据必须放在消息主体中，服务器通过消息头中的Content-Type字段来获知请求中的消息主体是用何种方式编码，再对主体进行解析，Content-Type比较常见的有application/x-www-form-urlencoded和application/json等，axios的post的方法的参数默认是发送json格式，有的后端不能解析json格式，我们可以用qs库中的stringify来转化参数，使后端可以解析出post参数

```
// 组件中使用post方法
import qs form 'qs'

this.axios.post(url, qs.stringify({a: 1, b: 2}))
  .then(res => {
    // 成功回调
  }, res => {
    // 错误回调
  })
```

##### 后端获取参数

以koa为例，koa中通过body-parse插件处理后再获取get和post参数

```
import Koa from 'koa'
import bodyparser from 'koa-bodyparser'

const app = new Koa()

app.use(bodyparser({enableTypes: ['json', 'form', 'text']}))

app.use((ctx, next) => {
  // get请求
  if (ctx.method === 'get') {
    const { a, b } = ctx.query  // 用ctx.query获取get请求参数
    ctx.body = { a, b }         // 返回参数
  }

   // post请求
   if ((ctx, next)) {
     const { a, b } = ctx.request.body  // 用ctx.request.body获取post请求参数
     ctx.body = { a, b }                // 返回参数
   }
})

app.listen(8088)
```
