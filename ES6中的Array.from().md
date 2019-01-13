#### ES6中的`Array.from()`

最近在看ES6，看到`Array.from()`还挺有意思的，它的作用就是把一个类数组转化成一个真正的数组，输入一个类数组的对象，返回一个真正的数组，挺符合函数式的思想，但是遗憾的是它可以接受第二个可选参数，没有curry化，不过可以只传一个参数。

平时我们见的比较多的类数组就是函数的`arguments`，通过JS选择DOM选择出来的`DOM NodeList`，字符串也可以算为类数组。

拿`arguments`举例，以前我们想要把它转化成真正的数组，一般这样转化

```
let arr = Array.prototype.slice.call(arguments)
```

或者这样

```
let arr = [].slice.call(arguments)
```

现在有了`Array.from()`，变得很方便

```
let arr = Array.from(arguments)
```

还有一个用法，就是生成一个特定数目的数组，类数组最重要的特性是length这个属性，所以可以这样生成一个特定数目的数组

```
//生成一个五个元素的数组

const arrLike = {length: 5}
const arr = Array.from(arrLike)
console.log(arr)

// [ undefined, undefined, undefined, undefined, undefined ]
```

这样有什么用呢，如果有一个需求是要生成一个这样顺序排列的数组`[ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]`，我们会用for循环

```
let arr = []
for (let i = 0; i < 10; i++) {
  arr.push(i + 1)
}
console.log(arr)
```

我们还可以用函数式编程的思路实现它

```
const R = require("ramda")

const f = (item, index) => index + 1
const arr = R.addIndex(R.map)(f, Array.from({length: 10}))
console.log(arr)
```
