生成promise

1.
```
function a() {
  return new Promise((resolve, reject) => {
    reject(3)
  })
}
// 需要执行a的时候才会返回promise
```
2.
```
const b = new Promise((resolve, reject) => {
  resolve(4)
})
// b本身就是一个promise对象
```

3.
```
const b = Promise.resolve(5)
```

在async函数中,如果要并发执行多个promise，可以用到promise.all
```
function a() {
  return new Promise((resolve, reject) => {
    reject(3)
  }).catch(() => {
    return 'err'
  })  // 加入catch避免promise.all中其中一个reject而导致所有promise结果都不能拿到
}

const b = new Promise((resolve, reject) => {
  resolve(4)
}).catch(() => {
  return 'err'
})  // 加入catch避免promise.all中其中一个reject而导致所有promise结果都不能拿到

async function c() {
  const [d, e] = await Promise.all([a(), b])
  console.log(d)
  console.log(e)
}
c() // err 4
```
