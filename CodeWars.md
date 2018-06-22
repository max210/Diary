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
