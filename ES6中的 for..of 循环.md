#### ES6中的`for...of`循环aa

ES6中新增了一个循环的方法，那就是`for...of`循环。

##### 循环数组

```
const arr = ['dog', 'cat', 'apple']

for (let item of arr) {
  console.log(item)
}
// dog
// cat
// apple
```

如果想要拿到数组的`index`，需要为数组加上`entries()`方法

```
const arr = ['dog', 'cat', 'apple']

for (let [index, item] of arr.entries()) {
  console.log(`${index}: ${item}`)
}
// 0: dog
// 1: cat
// 2: apple
```

##### 循环字符串

```
const str = 'abcd'

for (let item of str) {
  console.log(item)
}
// a
// b
// c
// d
```

##### 循环对象

`for...of`不能直接循环对象，需要用`Object.keys()`获得对象的key，以此来循环对象

```
const obj = {
  a: 1,
  b: 2,
  c: 3
}

for (let key of Object.keys(obj)) {
  console.log(obj(key))
}
// 1
// 2
// 3
```
个人认为循环对象的时候用`for...in`循环就行了

##### 循环类数组

类数组要符合三个条件

* 有`length`属性
* 属性的`key`为非负整数
* 没有数组所具有的方法

```
let obj = {
  1: 'abc',
  3: 'ccc',
  length: 4
}
```

`obj`就是一个类数组。

`for...of`可以循环`DOM NodeList`、`arguments`这样有`Iterator`接口的类数组，对于没有`Iterator`接口的类数组，可以用`Array.from()`转化为数组，再进行循环。

`for...of`与数组的`forEach`方法相比，前者可以像for循环一样随时打断循环，后者则不可以。

```
const arr = [1, 2, 3, 4, 5]

for (let item of arr) {
  if (item > 3) {
    break
  }
  console.log(item)
}
```

##### 总结

`for...of`为我们提供了遍历所有数据结构的统一的方法，但是一般遍历对象我会用`for...in`,其他情况下能用`for...of`就用`for...of`。

###### 参考文章

* [http://es6.ruanyifeng.com/#docs/iterator#for---of-循环]('http://es6.ruanyifeng.com/#docs/iterator#for---of-循环')
* [https://segmentfault.com/a/1190000000415572]('https://segmentfault.com/a/1190000000415572')
