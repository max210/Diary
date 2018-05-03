#### Ramda之常见的数组方法

学习优雅的函数式编程的库`ramda`，总结一下自己认为比较常用到的关于数组的方法

`any`

数组每一项都满足条件，返回`true`, 否则返回`false`

```
import R from 'ramda'

const arr = [1, 3, 4, 6]
cosnt f = item => item < 10

const newArr = R.any(f, arr)

console.log(newArr)  // true
```

`some`

数组其中一项都满足条件，返回`true`, 否则返回`false`

```
import R from 'ramda'

const arr = [1, 3, 4, 6]
cosnt f = item => item < 3

const newArr = R.some(f, arr)

console.log(newArr)  // true
```

`append`

在数组的尾部添加元素，返回新数组

```
import R from 'ramda'

const arr = [1, 3, 4, 6]

const newArr = R.append(7, arr)

console.log(newArr)  // [1, 3, 4, 6, 7]
```

`preppend`

在数组的头部添加元素，返回新数组

```
import R from 'ramda'

const arr = [1, 3, 4, 6]

const newArr = R.preppend(7, arr)

console.log(newArr)  // [7, 1, 3, 4, 6]
```

`init`

删除数组的尾部一个元素，返回剩余元素的数组(`tail`:删除数组的头部一个元素)

```
import R from 'ramda'

const arr = [1, 3, 4, 6]

const newArr = R.init(arr)

console.log(newArr)  // [1, 3, 4]
```

`drop`

删除数组从头部开始n个元素，返回剩余元素的数组(`dropLast`:删除数组从尾部开始n个元素)

```
import R from 'ramda'

const arr = [1, 3, 4, 6]

const newArr = R.drop(2, arr)

console.log(newArr)  // [4, 6]
```

`take`

取出数组从头部开始n个元素，返回取出元素的数组(`takeLast`:取出数组从尾部开始n个元素)

```
import R from 'ramda'

const arr = [1, 3, 4, 6]

const newArr = R.take(2, arr)

console.log(newArr)  // [1, 3]
```

`head`

返回数组头部第一个元素(`last`:返回数组尾部第一个元素)

```
import R from 'ramda'

const arr = [1, 3, 4, 6]

const newArr = R.head(arr)

console.log(newArr)  // [1]
```

`find`

返回与条件相符的首个元素(`findIndex`:返回与条件相符的首个元素的`index`)

```
import R from 'ramda'

const arr = [1, 3, 4, 6]
const f = item => item === 4

const newArr = R.find(f, arr)

console.log(newArr)  // [1]
```

`filter`

过滤出符合条件的元素，返回符合条件的元素组成的数组(`reject`:返回不符合条件的元素组成的数组)

```
import R from 'ramda'

const arr = [1, 3, 4, 6]
const f = item => item < 4

const newArr = R.filter(f, arr)

console.log(newArr)  // [1, 3]
```

`sort`

对元素进行排序

```
import R from 'ramda'

const arr = [1, 3, 4, 6]
const f = (a, b) => b - a

const newArr = R.sort(f, arr)

console.log(newArr)  // [6, 4, 3, 1]
```

`reverse`

翻转数组

```
import R from 'ramda'

const arr = [1, 3, 4, 6]

const newArr = R.reverse(arr)

console.log(newArr)  // [6, 4, 3, 1]
```

`map`

返回经过条件变换的数组

```
import R from 'ramda'

const arr = [1, 3, 4, 6]
const f = item => item * 2

const newArr = R.map(arr)

console.log(newArr)  // [2, 6, 8, 12]
```

`forEach`

对数组每一项做动作，返回原数组

```
import R from 'ramda'

const arr = [1, 3, 4, 6]
const f = item => console.log(item * 2)

const newArr = R.forEach(arr)

console.log(newArr)  // [1, 3, 4, 6]
// 2
// 6
// 8
// 12
```

`concat`

连接两个数组，返回新数组

```
import R from 'ramda'

const arr1 = [1, 3]
const arr12 = [4, 6]

const newArr = R.concat(arr1, arr2)

console.log(newArr)  // [1, 3, 4, 6]
```

`join`

把数组每个元素通过分隔符连起来，返回字符串

```
import R from 'ramda'

const arr = [1, 3, 4, 6]

const newArr = R.join(arr)

console.log(newArr)  // '1346'
```

`uniq`

返回没有重复元素的数组

```
import R from 'ramda'

const arr = [1, 3, 4, 4, 6]

const newArr = R.uniq(arr)

console.log(newArr)  // [1, 3, 4, 6]
```

`slice`

返回开始和结束元素的数组，包括开始，不包括结束

```
import R from 'ramda'

const arr1 = [1, 3, 4, 6]

const newArr = R.slice(2, 3, arr)

console.log(newArr)  // [4]
```
