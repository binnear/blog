国庆7天假，6天加班，苦涩😂。

因为对数组的处理方法有些还是有点模糊，因此这里整理汇总一下，方便日后自己查阅。😜

### 1、`concat(value)`将数组和/或值连接成新数组(不改变原数组)

```javascript
let a = [1, 2], b = [3, 4], c = 5
let result = a.concat(b, c)
console.log(result)   // [1, 2, 3, 4, 5]
console.log(a)        // [1, 2]
```

### 2、`copyWithin(target, start, end)`将数组从start到end索引的元素复制到target开始的索引位置(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.copyWithin(0, 3, 5)  
console.log(result)   // [4, 5, 3, 4, 5]
console.log(a)        // [4, 5, 3, 4, 5]  索引3到5的元素为4和5，复制到从0开始的位置，替换掉了1和2
```

### 3、`entries()`返回一个新的Array迭代器对象,可用for...of遍历(不改变原数组)

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

### 3、`every(fn)`判断数组中是否所有元素都满足fn函数中的条件(不改变原数组)

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

### 3、`fill(value)`使用给定value填充数组(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.fill(0)
console.log(result)   // [0, 0, 0, 0, 0]
console.log(a)        // [0, 0, 0, 0, 0]
```

### 3、`filter(fn)`返回数组中满足fn函数中条件的集合(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.filter((currentValue)=>{
  return currentValue > 4
})
console.log(result)   // [5] 只有5满足条件
console.log(a)        // [1, 2, 3, 4, 5]
```

### 3、`find(fn)`返回数组中第一个匹配fn函数中条件的值没有则返回undefined(不改变原数组)

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

### 3、`findIndex(fn)`返回数组中第一个匹配fn函数中条件的索引没有则返回undefined(不改变原数组)

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

### 3、`flat()`将二维数组变为一维数组(不改变原数组)

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

### 3、`flatMap()`相当于map与flat的结合(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.flatMap((currentValue)=>{
  return [currentValue, currentValue * 2]
})
console.log(result)   // [1, 2, 2, 4, 3, 6, 4, 8, 5, 10]
console.log(a)        // [1, 2, 3, 4, 5]
```

### 3、`forEach()`遍历数组(不改变原数组)

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

### 3、`includes()`返回一个布尔值，表示某个数组是否包含给定的值(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.includes(2)
console.log(result)   // true
console.log(a)        // [1, 2, 3, 4, 5]

result = a.includes(6)
console.log(result)   // false
console.log(a)        // [1, 2, 3, 4, 5]
```

### 3、`indexOf(value)`从索引为0开始，检查数组是否包含value，有则返回匹配到的第一个索引，没有返回-1(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.indexOf(2)
console.log(result)   // 1
console.log(a)        // [1, 2, 3, 4, 5]

result = a.indexOf(6)
console.log(result)   // -1
console.log(a)        // [1, 2, 3, 4, 5]
```

### 3、`join(value)`将数组以value连接为字符串(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.join(0)
console.log(result)   // '102030405'
console.log(a)        // [1, 2, 3, 4, 5]

result = a.join(',')
console.log(result)   // '1,2,3,4,5'
console.log(a)        // [1, 2, 3, 4, 5]
```

### 3、`keys()`返回一个新的Array迭代器对象,可用for...of遍历(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.keys()
console.log(result.next())   // {value: 0, done: false}  value为索引  
...
console.log(result.next())   // {value: 3, done: false}
console.log(result.next())   // {value: 4, done: false}
console.log(result)          // Array Iterator {}
console.log(a)               // [1, 2, 3, 4, 5]

// 重新复制
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

### 3、`lastIndexOf()`(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.includes((v, i)=>{

})
console.log(result)   // 
console.log(a)        // [1, 2, 3, 4, 5]
```

### 3、`includes()`(不改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.includes((v, i)=>{

})
console.log(result)   // 
console.log(a)        // [1, 2, 3, 4, 5]
```
