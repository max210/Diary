* Find the unique number
```
function findUniq(arr) {
  let itemArr = []
  let indexArr = []
  for (let i = 0, len = arr.length; i < len; i++) {
    if (itemArr.indexOf(arr[i]) === -1) {
      itemArr.push(arr[i])
      indexArr.push(1)
    } else {
      indexArr[itemArr.indexOf(arr[i])] += 1
    }
  }
  const index = indexArr.indexOf(1)
  return itemArr[index]
}
```

* Simple Pig Latin
```
function pigIt(str){
  let arr = str.split(' ')
  if ((/[a-z]/).test(arr[arr.length -1])) {
    return `${arr.map(x => `${x.slice(1)}${x.slice(0, 1)}ay`).join(' ')}`
  } else {
    const end = arr.pop()
    return `${arr.map(x => `${x.slice(1)}${x.slice(0, 1)}ay`).join(' ')} ${end}`
  }
}
```

* A Chain adding function
```
function add(n) {
  let tmp = n
  const inner = n2 => {
    tmp += n2
    return inner
  }
  inner.valueOf = () => tmp
  return inner
}
```

* First non-repeating character
```
function firstNonRepeatingLetter(s) {
  let numArr = []
  let noRepeatArr = []
  let upperArr = s.split('').map(i => i.toUpperCase())
  upperArr.map((item, index) => {
    if (noRepeatArr.indexOf(item) < 0) {
      noRepeatArr.push(item)
      numArr.push(1)
    } else {
      numArr[noRepeatArr.indexOf(item)] += 1
    }
  })

  return numArr.indexOf(1) > -1 ? s[upperArr.indexOf(noRepeatArr[numArr.indexOf(1)])] : ''
}
console.log(firstNonRepeatingLetter('abaaCbc'))
```

* Pete, the baker
```
function cakes(recipe, available) {
  let numArr = []
  for (let recipeKey in recipe) {
    if (available.hasOwnProperty(recipeKey)) {
      numArr.push(parseInt(available[recipeKey] / recipe[recipeKey]))
    } else {
      return 0
    }
  }
  return Math.min(...numArr)
}
```
