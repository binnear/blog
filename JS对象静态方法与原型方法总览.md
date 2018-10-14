上一篇总结了数组的方法，这里总结一下对象的方法，因主要是为了自己以后简单回顾，不详尽之处望理解，如要了解更多，建议移步MDN，缺点是访问比较慢。😝

## 静态方法

### 01、`Object.assign(a, b)`将对象b合并到a，并返回a。

```javascript
// Base
const a = { a: 1 }, b = { b: 2 }
const result = Object.assign(a, b)
console.log(result)          // {a: 1, b: 2}
console.log(a)               // {a: 1, b: 2}

// More
```

### 01、`Object.create(a)`创建一个以a为原型的新对象

```javascript
// Base
const a = { a: 1 }
const result = Object.create(a)
console.log(result.__proto__ === a)      // true

```

### 01、`Object.defineProperties(obj, props)`在obj上定义props配置的属性，并返回obj

```javascript
// Base
const a = { a: 1 }
const result = Object.defineProperty(a, 'b', {
  configurable: true,
  enumerable: true,
  get: function () { return 3; }
})
console.log(result)           //
console.log(a)                //
console.log(a.b)

```

### 01、``

```javascript
// Base
const a = { a: 1 }
const result = Object.create(a)
console.log(result)           //
console.log(a)                //

```

### 01、``

```javascript
// Base
const a = { a: 1 }
const result = Object.create(a)
console.log(result)           //
console.log(a)                //

```

### 01、``

```javascript
// Base
const a = { a: 1 }
const result = Object.create(a)
console.log(result)           //
console.log(a)                //

```

### 01、``

```javascript
// Base
const a = { a: 1 }
const result = Object.create(a)
console.log(result)           //
console.log(a)                //

```

### 01、``

```javascript
// Base
const a = { a: 1 }
const result = Object.create(a)
console.log(result)           //
console.log(a)                //

```

### 01、``

```javascript
// Base
const a = { a: 1 }
const result = Object.create(a)
console.log(result)           //
console.log(a)                //

```

### 01、``

```javascript
// Base
const a = { a: 1 }
const result = Object.create(a)
console.log(result)           //
console.log(a)                //

```

### 01、``

```javascript
// Base
const a = { a: 1 }
const result = Object.create(a)
console.log(result)           //
console.log(a)                //

```
