徐老师说过要自己长期维护一个自己的项目，慢慢迭代，这样会发现自己的小项目会越来愈大，功能越来越丰富。在做其他项目的时候，遇到有和自己项目的类似功能时，可以快速实现功能。

我自己也是写了一个项目，后端`express`提供`API`接口，前端用vue，虽然功能比较单一，但是也都用到了`vue-router`，`vuex`等，但当时想着先把项目跑起来再说，代码写的有点粗糙，最近想着好好整理一下自己的代码，以后就维护这个项目。自己要了解一些`koa`，就想着把接口用`koa`再写一遍，前端的请求换用`axios`（之前用的`vue-resource`），跨域用`CORS`来代替之前用的代理的方式，把自己代码整理了一番。整理后的前后端代码都放到了`github`[https://github.com/max210]('https://github.com/max210')上，有兴趣的同学可以看一下，欢迎提建议～

这周末想增加一个功能，`GitHub`第三方登录，看了老师的`community`项目，网上查了资料，终于实现了。

大致流程就是用户同意授权`github`以后，github会给到一个`code`，拿到这个`code`去请求`github`的接口，然后获得`access_token`，然后利用`access_token`再请求`GitHub`的一个接口就可以获得用户的信息了。

首先去`GitHub`上面注册`Oauth application`，李鹏同学已经有了[教程]('http://xugaoyang.com/post/5ae3258a90919f7d042209c2')，注册以后会得倒`Client ID`和`Client Secret   `
至于`Authorization callback URL` 的意思是用户同意`GitHub`授权以后要跳转到的页面。我是在`vue`项目中新写了一个`callback`页面，所以我填入的地址是`http://localhost:8080/callback`，进行本地调试。

首先在我的注册的页面增加一个`GitHub`的图标，用一个`a`链接包裹，`a`链接的地址为`https://github.com/login/oauth/authorize?client_id=xxx`，`client_id`就是注册得到的`Client ID`，用户点击这个链接会跳转到`GitHub`提供的同意授权页面，当用户同意授权以后，会跳转到我们填写的`Authorization callback URL`地址，我个人写了名字为callback页面，跳转的`url`中会携带一个`code`参数，接下里我在`callback.vue`文件中，钩子函数`mounted`中拿到`code`，然后利用`axios`发送请求给我的后端。代码如下

```
import * as R from 'ramda'

async mounted () {
  const url = window.location.href
  const code = R.pipe(R.split('?'), R.last, R.split('='), R.last)(url)

  try {
    const res = await this.axios.post(`${this.globalData.host}/user/github/third-login`, { code })
    if (res.data.status === 0) {                // 成功
      this.username(res.data.name)              // 设置拿到的username
      this.$router.push({ name: 'GoodIndex' })  // 跳转到首页
    } else {
      //出错了
    }
  } catch (e) {
    //出错了
  }
}
```

后端通过接口拿到`code`，然后`POST`请求`https://github.com/login/oauth/access_token`这个接口，以获得access_token，请求参数有`client_id`,`client_secret`,`code`,成功以后，就可以拿到`access_token`，然后`GET`请求`https://api.github.com/user`，参数为`access_token`，成功回调后，就可以获得用户的信息了。代码如下

```
//第三方登录（GitHub）
export const thirdLogin = async (ctx, next) => {
  // 获得前端请求携带的code参数
  const code = ctx.request.body.code

  try {
    const params = {
      client_id: 'xxx',
      client_secret: 'xxx',
      code
    }
    const res = await axios.post(`https://github.com/login/oauth/access_token`, params) // 获取access_token
    const access_token = R.pipe(R.split('&'), R.head, R.split('='), R.last)(res.data)
    const userData = await axios.get(`https://api.github.com/user`, { params: { access_token } }) // 获取GitHub用户信息

    // 存入数据库
    const name = userData.data.login
    const email = userData.data.email

    const user = new User({ name, email })
    await user.save()

    // 获取_id
    const findUser = await User.findOne({ name }).exec()

    const payload = {
      _id: findUser._id,
      name: findUser.name
    }
    // 用jwt生成token
    const token = jwt.encode(payload, config.jwtSecret)

    const opts = {
      path: '/',
      maxAge: 1000 * 60 * 60 * 24 * 30,
      signed: true,
      httpOnly: true
    }

     ctx.cookies.set(config.cookieName, token, opts)  //发送cookie
     ctx.body = { status: 0, msg: '登录成功', name: findUser.name, token }   //返回token 以备客户端需要
  } catch (e) {
    ctx.body = { status: 1, msg: '登录失败'}
  }
}

```
