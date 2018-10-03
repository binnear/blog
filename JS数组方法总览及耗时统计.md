国庆7天假，6天加班，苦涩😂。

因为对数组的处理方法有些还是有点模糊，因此这里整理汇总一下，方便日后自己查阅。😜

### 01、`push(value)`将value添加到数组的最后，返回数组长度(改变原数组)

```javascript
Base
let a = [1, 2, 3, 4, 5]
let result = a.push(1)
console.log(result)   // 6
console.log(a)        // [1, 2, 3, 4, 5, 1] 原数组被改变

Advance
a = [1, 2, 3, 4, 5]
result = a.push('a', 'b') // 可一次添加多个值
console.log(result)   // 7
console.log(a)        // [1, 2, 3, 4, 5, 'a', 'b']
```

### 02、`unshift()`添加元素到数组的开头，返回数组的长度(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.unshift(1)
console.log(result)   // 6
console.log(a)        // [1, 1, 2, 3, 4, 5]
```

### 03、`pop()`删除数组中最后一个元素，返回被删除元素(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.pop()
console.log(result)   // 5
console.log(a)        // [1, 2, 3, 4]
```

### 04、`shift()`删除数组第一个元素，返回删除的元素(改变原数组)

```javascript
let a = [5, 4, 3, 2, 1]
let result = a.shift()
console.log(result)   // 5
console.log(a)        // [4, 3, 2, 1]
```

### 05、`join(value)`将数组用value连接为字符串(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.join(0)
console.log(result)   // '102030405'
console.log(a)        // [1, 2, 3, 4, 5]

result = a.join(',')
console.log(result)   // '1,2,3,4,5'
console.log(a)        // [1, 2, 3, 4, 5]
```

### 06、`reverse()`反转数组(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.reverse()
console.log(result)   // [5, 4, 3, 2, 1]
console.log(a)        // [5, 4, 3, 2, 1]
```

### 07、`slice(start, end)`返回新数组，包含原数组索引start的值到索引end的值，不包含end(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.slice(2, 4)
console.log(result)   // [3, 4]
console.log(a)        // [1, 2, 3, 4, 5]
```

### 08、`splice(index, count, value)`从索引为index处删除count个元素，插入value(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.splice(1, 2, 0)
console.log(result)   // [2, 3]
console.log(a)        // [1, 0, 4, 5]
```

### 09、`sort()`对数组元素进行排序(改变原数组)

```javascript
let a = [3, 2, 7, 1, 9]
let result = a.sort()
console.log(result)   // [1, 2, 3, 7, 9]
console.log(a)        // [1, 2, 3, 7, 9]
```

### 10、`toString()`将数组中的元素用逗号拼接成字符串(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.toString()
console.log(result)   // 1,2,3,4,5
console.log(a)        // [1, 2, 3, 4, 5]
```

### 11、`indexOf(value)`从索引为0开始，检查数组是否包含value，有则返回匹配到的第一个索引，没有返回-1(不改变原数组)

```javascript
let a = [1, 2, 3, 2, 5]
let result = a.indexOf(2)
console.log(result)   // 1
console.log(a)        // [1, 2, 3, 2, 5]

result = a.indexOf(6)
console.log(result)   // -1
console.log(a)        // [1, 2, 3, 2, 5]
```

### 12、`lastIndexOf(value)`从最后的索引开始，检查数组是否包含value，有则返回匹配到的第一个索引，没有返回-1(不改变原数组)

```javascript
let a = [1, 2, 3, 2, 5]
let result = a.lastIndexOf(2)
console.log(result)   // 3
console.log(a)        // [1, 2, 3, 2, 5]

result = a.lastIndexOf(6)
console.log(result)   // -1
console.log(a)        // [1, 2, 3, 2, 5]
```

### 13、`concat(value)`将数组和/或值连接成新数组(不改变原数组)

```javascript
Use
let a = [1, 2], b = [3, 4], c = 5
let result = a.concat(b, c)
console.log(result)   // [1, 2, 3, 4, 5]
console.log(a)        // [1, 2]
```

### 14、`fill(value)`使用给定value填充数组(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.fill(0)
console.log(result)   // [0, 0, 0, 0, 0]
console.log(a)        // [0, 0, 0, 0, 0]
```

### 15、`flat()`将二维数组变为一维数组(不改变原数组)

```javascript
let a = [1, 2, 3, [4, 5]]
let result = a.flat()
console.log(result)   // [1, 2, 3, 4, 5]
console.log(a)        // [1, 2, 3, [4, 5]]

let a = [1, 2, 3, [4, 5, [6, 7, [8, 9]]]]
let result = a.flat()
console.log(result)   // [1, 2, 3, 4, 5, [6, 7, [8, 9]]] 很显然只能将第二层嵌套数组“拉平”
console.log(a)        // [1, 2, 3, [4, 5, [6, 7, [8, 9]]]]
```

### 16、`flatMap()`相当于map与flat的结合(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.flatMap((currentValue)=>{
  return [currentValue, currentValue * 2]
})
console.log(result)   // [1, 2, 2, 4, 3, 6, 4, 8, 5, 10]
console.log(a)        // [1, 2, 3, 4, 5]
```

### 17、`copyWithin(target, start, end)`将数组从start到end索引的元素复制到target开始的索引位置(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.copyWithin(0, 3, 5)  
console.log(result)   // [4, 5, 3, 4, 5]
console.log(a)        // [4, 5, 3, 4, 5]  索引3到5的元素为4和5，复制到从0开始的位置，替换掉了1和2
```

### 18、`entries()`返回一个新的Array迭代器对象,可用for...of遍历(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.entries()
console.log(result.next())   // {value: [0, 1], done: false}    value数组中第一个元素为索引，第二元素为索引对应的值
...
console.log(result.next())   // {value: [4, 5], done: false}
console.log(result.next())   // {value: undefined, done: true}
console.log(result)          // Array Iterator {}
console.log(a)               // [1, 2, 3, 4, 5]

// 重新赋值
result = a.entries()
for(let value of result) {
  console.log(value)
}
// [0, 1]
// [1, 2]
// [2, 3]
// [3, 4]
// [4, 5]
```

### 19、`keys()`返回一个新的Array迭代器对象,可用for...of遍历(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.keys()
console.log(result.next())   // {value: 0, done: false}  value为索引  
...
console.log(result.next())   // {value: 3, done: false}
console.log(result.next())   // {value: 4, done: false}
console.log(result)          // Array Iterator {}
console.log(a)               // [1, 2, 3, 4, 5]

// 重新赋值
result = a.keys()
for(let value of result) {
  console.log(value)
}
// 0
// 1
// 2
// 3
// 4
```

### 20、`values()`返回一个新的迭代器(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.values()
console.log(result.next())   // {value: 1, done: false}  value为索引  
...
console.log(result.next())   // {value: 4, done: false}
console.log(result.next())   // {value: 5, done: false}
console.log(result)          // Array Iterator {}
console.log(a)               // [1, 2, 3, 4, 5]

// 重新赋值
result = a.values()
for(let value of result) {
  console.log(value)
}
// 1
// 2
// 3
// 4
// 5
```

### 21、`forEach()`遍历数组(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.forEach((v, i)=>{
  console.log(v, i)  
  // 1 0
  // 2 1
  // 3 2
  // 4 3
  // 5 4
})
console.log(result)   // undefined
console.log(a)        // [1, 2, 3, 4, 5]
```

### 22、`every(fn)`判断数组中是否所有元素都满足fn函数中的条件(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.every((currentValue)=>{
  return currentValue > 0
})
console.log(result)   // true  显然所有元素都大于0

result = a.every((currentValue)=>{
  return currentValue > 1
})
console.log(result)   // false  1并不大于1
console.log(a)        // [1, 2, 3, 4, 5]
```


### 23、`filter(fn)`返回数组中满足fn函数中条件的集合(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.filter((currentValue)=>{
  return currentValue > 4
})
console.log(result)   // [5] 只有5满足条件
console.log(a)        // [1, 2, 3, 4, 5]
```

### 24、`find(fn)`返回数组中第一个匹配fn函数中条件的值没有则返回undefined(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.find((currentValue)=>{
  return currentValue > 3
})
console.log(result)   // 4
console.log(a)        // [1, 2, 3, 4, 5]

let result = a.find((currentValue)=>{
  return currentValue > 5
})
console.log(result)   // undefined
console.log(a)        // [1, 2, 3, 4, 5]
```

### 25、`findIndex(fn)`返回数组中第一个匹配fn函数中条件的索引没有则返回undefined(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.findIndex((currentValue)=>{
  return currentValue > 3
})
console.log(result)   // 3
console.log(a)        // [1, 2, 3, 4, 5]

let result = a.findIndex((currentValue)=>{
  return currentValue > 5
})
console.log(result)   // -1
console.log(a)        // [1, 2, 3, 4, 5]
```

### 26、`includes()`返回一个布尔值，表示某个数组是否包含给定的值(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.includes(2)
console.log(result)   // true
console.log(a)        // [1, 2, 3, 4, 5]

result = a.includes(6)
console.log(result)   // false
console.log(a)        // [1, 2, 3, 4, 5]
```

### 27、`map(fn)`以fn函数中返回值组成新的数组返回(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.map((v, i)=>{
  return 9
})
console.log(result)   // [9, 9, 9, 9, 9]
console.log(a)        // [1, 2, 3, 4, 5]
```

### 28、`reduce()`累计器(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.reduce((accumulator, currentValue, currentIndex, array)=>{
  console.log(accumulator, currentValue, currentIndex, array)
  return accumulator + currentValue
  // 5  1 0 [1, 2, 3, 4, 5]  第一次accumulator的值为reduce第二个参数5, currentValue为数组第一个元素
  // 6  2 1 [1, 2, 3, 4, 5]  第二次accumulator的值为5加上数组a中的第一个值，即是第一次循环时return的值
  // 8  3 2 [1, 2, 3, 4, 5]  同上
  // 11 4 3 [1, 2, 3, 4, 5]  同上 
  // 15 5 4 [1, 2, 3, 4, 5]  同上
}, 5)
console.log(result)   // 20 为最终累计的和
console.log(a)        // [1, 2, 3, 4, 5]

// 无初始值时，accumulator的初始值为数组的第一个元素，currentValue为数组第二个元素
result = a.reduce((accumulator, currentValue, currentIndex, array)=>{
  console.log(accumulator, currentValue, currentIndex, array)
  return accumulator + currentValue
  // 1  2 1 [1, 2, 3, 4, 5]
  // 3  3 2 [1, 2, 3, 4, 5]
  // 6  4 3 [1, 2, 3, 4, 5]
  // 10 5 4 [1, 2, 3, 4, 5]
})
console.log(result)   // 15 为最终累计的和
console.log(a)        // [1, 2, 3, 4, 5]
```

### 29、`reduceRight()`与reduce功能一样，只是从数组末尾开始进行累计(不改变原数组)

```javascript
略...
```

### 30、`some(fn)`检查数组中是否含有满足fn函数条件的值(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.some((v)=>{
  return v > 2
})
console.log(result)   // true
console.log(a)        // [1, 2, 3, 4, 5]

result = a.some((v)=>{
  return v > 6
})
console.log(result)   // false
console.log(a)        // [1, 2, 3, 4, 5]
```

### 31、`toLocaleString()`将数组中的每个元素使用各自的toLocaleString()转换后用`,`拼接(不改变原数组)

```javascript
let a = [1, new Date(), 'a', {m: 1}]
let result = a.toLocaleString()
console.log(result)   // '1,2018/10/3 下午9:23:59,a,[object Object]'
console.log(a)        // [1, Wed Oct 03 2018 21:23:59 GMT+0800 (中国标准时间), "a", {m: 1}]
```

### 32、`[@@iterator]()`数组自带的迭代器方法(不改变原数组)

```javascript
使得数组原生可以使用for...of进行遍历
```
