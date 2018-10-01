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

### 3、`entries()`返回一个新的Array迭代器对象

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.entries()
console.log(result.next())   // {value: [0, 1], done: false}    value数组中第一个元素为索引，第二元素为索引对应的值
console.log(result.next())   // {value: [1, 2], done: false}
...
console.log(result.next())   // {value: [4, 5], done: false}
console.log(result.next())   // {value: undefined, done: true}
```

### 3、`every(fn)`判断数组中是否所有元素都满足fn函数中的条件

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
```

### 3、`copyWithin()`(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.
console.log(result)   // 
console.log(a)        // 
```

### 3、`copyWithin()`(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.
console.log(result)   // 
console.log(a)        // 
```

### 3、`copyWithin()`(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.
console.log(result)   // 
console.log(a)        // 
```

### 3、`copyWithin()`(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.
console.log(result)   // 
console.log(a)        // 
```

### 3、`copyWithin()`(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.
console.log(result)   // 
console.log(a)        // 
```

### 3、`copyWithin()`(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.
console.log(result)   // 
console.log(a)        // 
```

### 3、`copyWithin()`(改变原数组)

```javascript
let a = [1, 2, 3, 4, 5]
let result = a.
console.log(result)   // 
console.log(a)        // 
```
