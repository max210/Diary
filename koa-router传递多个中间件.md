#### kao-router如何传递多个中间件

最近也是在接触koa，试着用koa去写一些API服务，用到`koa-router`，由于收藏功能有权限的验证，想着把权限的验证抽离成一个中间件，通过这个中间件过滤之后，再进入收藏这个动作。

探究了一番，直接在权限验证中间件中`return next()` 即可继续流到下一个收藏动作。

```
const authRequired = async (ctx, next) => {
  //权限验证操作...

  //如果需要向下一个中间件传递参数，我直接把要传递的参数挂载到 ctx 对象上，下一个中间件即可通过 ctx 获取
  ctx.a = 1
  return next()
}
const removeCollection = async (ctx, next) => {
  //收藏操作...

  // 可获取上一个中间件传递的参数
  const a = ctx.a
}
router.get('/addCollection', authRequired, removeCollection)
```
