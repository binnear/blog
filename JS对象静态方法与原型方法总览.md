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

### 01、`Object.defineProperty(obj, prop, descriptor)`为对象创建一个自定义配置的属性

```javascript
// Base
const a = { a: 1 }
const result = Object.defineProperty(a, 'b', {
  // 必定存在的配置项
  configurable: false,   // 决定属性能否从对象中被删除
  enumerable: false,     // 决定该属性能否被遍历出来
  // 1 互斥配置项，与2互斥
  writable: false,       // 决定该属性的值能否被改变
  value: 2,              // 设置属性的值
  // 2 与1互斥
  get() {},              // 获取属性值都会执行这个方法，方法的返回值就是我们将获取到的值
  set(value) {},         // 每次对属性的赋值都会作为value传递到set方法，并执行该方法
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
