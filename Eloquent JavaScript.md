### Table of Contents
- [1. Values, Types and Operators](#1-values-types-and-operators)
- [4. Data Structures](#4-data-structures)
- [5. Higher-Order Functions](#5-higher-order-functions)
- [6. The Secret Life of Objects](#6-the-secret-life-of-objects)
- [7. Project: A Robot](#7-project-a-robot)
---
## 1. Values, Types and Operators

1. Uppercase letters are always less than lowercase ones
```js
console.log("Z" < "a")
// true
```

2. Only NaN isn't equal to itself
```js
console.log(NaN == NaN)
// false
console.log(NaN === NaN)
// false
```

3. The difference in meaning between `undefined` and `null` is an accident of JavaScript’s design

4. Automatic type conversion
```js
console.log(8 * null)
// → 0
console.log("5" - 1)
// → 4
console.log("5" + 1)
// → 51
console.log("5" * 2)
// → 10
console.log("five" * 2)
// → NaN
console.log(false == 0)
// → true
console.log(false === 0)
// → false
console.log(true == 1)
// → true
console.log(null == undefined);
// → true
console.log(null == 0);
// → false
console.log(false == "")
// → true
```

## 4. Data Structures

1. String Padding
```js
String(6).padStart(3, '0')
// "006"
String(6).padEnd(3, '0')
// "600"
```

2. Array Shifting
```js
let arr = [1, 2, 3]
arr.unshift() // 3
// [1, 2, 3]
arr.shift()   // 1
// [2, 3]
```

## 5. Higher-Order Functions

1. Unicode of Characters
```js
'🟨'.length
// 2

'🟨'.charCodeAt(0)
// 55357 (Code of half-character)
String.fromCharCode(55357)
// "\ud83d"
String.fromCodePoint(55357)
// "\ud83d"

'🟨'.codePointAt(0)
// 129000
String.fromCharCode(129000)
// "\uf7e8"
String.fromCodePoint(129000)
// "🟨"
```

2. Concatenation
```js
[1, 2].concat([3, 4])
// [1, 2, 3, 4]
```

## 6. The Secret Life of Objects

1. Methods
```js
function greet(to) {
  console.log(`${this.name}: Hi ${to}!`)
}

const ali = { name: 'Ali', greet }
ali.greet('Veli')
// Ali: Hi Veli!
greet.call(ali, 'Veli')
// Ali: Hi Veli!
```

2. Constructors
```js
function Car(brand) {
  this.brand = brand
}
Car.prototype.accelerate = function(speed) {
  console.log(`${this.brand} is accelerated ${speed}KMH`)
}

let mercedes = new Car('Mercedes')
mercedes.accelerate(15)
// Mercedes is accelerated 15KMH
```

3. Class Notation
```js
class Car {
  constructor(brand) {
    this.brand = brand
  }

  accelerate(speed) {
    console.log(`${this.brand} is accelerated ${speed}KMH`)
  }
}

let mercedes = new Car('Mercedes')
mercedes.accelerate(15)
// Mercedes is accelerated 15KMH
```

```js
(new class { 
  constructor() {
    console.log('constructor')
  };

  greet() {
    console.log('Hi!')
  }
}).greet()
// constructor
// Hi!
```

4. Overriding Derived Properties
```js
Object.prototype.toString.call([1, 2])
// "[object Array]"
Array.prototype.toString.call([1, 2])
// "1,2"
```

5. Create Objects with No Prototype
```js
Object.create(null)
// Object { }
Object.create({})
// Object { }
//    <prototype>: Object { }
```

6. Maps
```js
const ages = new Map()
ages.set('Ali', 18)
ages.set('Veli', 19)

ages.get('Ali')
// 18
ages.has('Veli')
// true

Object.keys(ages)
// []
Object.values(ages)
// []
```

7. `hasOwnProperty`
```js
const ali = { name: 'Ali' }
ali.name
// Ali
ali.toString()
// [object Object]

ali.hasOwnProperty('name')
// true
ali.hasOwnProperty('toString')
// false
```

8. Polymorphism
```js
class Person {
  constructor(name) {
    this.name = name
  }
}
Person.prototype.toString = function() {
  return `My name is ${this.name}.`
}

const ali = new Person('Ali')
ali.toString()
// My name is Ali.

const veli = new Person('Veli')
veli.constructor.prototype.toString = function() {
  return `They call me ${this.name}`
}
veli.toString()
// They call me Veli
```

```js
function Person(name) {
  this.name = name
}
Person.prototype.toString = function() {
  return `My name is ${this.name}.`
}

const hasan = new Person('Hasan')
hasan.toString()
// My name is Hasan.

const huseyin = new Person('Hüseyin')
huseyin.constructor.prototype.toString = function() {
  return `They call me ${this.name}`
}
huseyin.toString()
// They call me Hüseyin
```

9. Symbols

```js
const name = Symbol('name')
name == Symbol('name')
// false
```

```js
const toStringSymbol = Symbol('toString')
Array.prototype[toStringSymbol] = function() {
  return `${this.length} items.`
}
[1, 2].toString()
// 1, 2
[1, 2, 3][toStringSymbol]()
// 3 items
```

```js
const toStringSymbol = Symbol('toString')
const greet = {
  [toStringSymbol]() { return 'Hi!' }
}
greet[toStringSymbol]()
// Hi!
```

10. The Iterator Interface
```js
const hiIterator = "Hi"[Symbol.iterator]()
hiIterator.next()
// { value: "H", done: false }
hiIterator.next()
// { value: "i", done: false }
hiIterator.next()
// { value: undefined, done: true }
```

```js
class Matrix {
  constructor(width, height, element = (x, y) => undefined) {
    this.content = []
    this.width = width
    this.height = height

    for (let y = 0; y < height; y++) {
      for (let x = 0; x < width; x++) {
        this.content[y * width + x] = element(x, y)
      }
    }
  }

  get(x, y) {
    return this.content[y * this.width + x]
  }

  set(x, y) {
    this.content[y * this.width + x]
  }
}

class MatrixIterator {
  constructor(matrix) {
    this.x = 0
    this.y = 0
    this.matrix = matrix
  }

  next() {
    if (this.y === this.matrix.height) return { done: true }

    const value = {
      x: this.x,
      y: this.y,
      value: this.matrix.get(this.x, this.y)
    }

    this.x++
    if (this.x === this.matrix.width) {
      this.x = 0
      this.y++
    }

    return { value, done: false }
  }
}

Matrix.prototype[Symbol.iterator] = function() {
  return new MatrixIterator(this)
}

const matrix = new Matrix(2, 2, (x, y) => `value: ${x}, ${y}`)
for (let { x, y, value } of matrix) {
  console.log(x, y, value)
}
```

11. Getters, Setters and Statics

```js
const randomGenerator = {
  get number() {
    return Math.floor(Math.random() * 100)
  }
}
randomGenerator.number
// 58
randomGenerator.number
// 17
```

```js
class Temperature {
  constructor(celsius) {
    this.celsius = celsius
  }

  get fahrenheit() {
    return this.celsius * 1.8 + 32
  }

  set fahrenheit(value) {
    this.celsius = (value - 32) / 1.8
  }

  static fromFahrenheit(value) {
    return new Temperature((value - 32) / 1.8)
  }
}

const tempC = new Temperature(22)
console.log(tempC.fahrenheit)
// 71.6

temp.fahrenheit = 86
console.log(tempC.celsius)
// 30

const tempF = Temperature.fromFahrenheit(100)
console.log(tempF.celsius)
// 37.8
```

12. Inheritance

```js
class SymmetricMatrix extends Matrix {
  constructor(size, element = (x, y) => undefined) {
    super(size, size, (x, y) => {
      if (x < y) return element(y, x)
      else return element(x, y)
    })
  }

  set(x, y, value) {
    super.set(x, y, value)
    if (x != y) super.set(y, x, value)
  }
}

const matrix = new SymmetricMatrix(5, (x, y) => `${x}, ${y}`)
matrix.get(2, 3)
// 3, 2
```

13. The `instanceOf` Operator

```js
console.log(new SymmetricMatrix(2) instanceof SymmetricMatrix)
// true
console.log(new SymmetricMatrix(2) instanceof Matrix)
// true
console.log(new Matrix(2, 2) instanceof SymmetricMatrix)
// false
console.log([1] instanceof Array)
// true
```

14. `static` Methods: stored in a class’s constructor, rather than its prototype.

```js
class TestClass {
  constructor(value) {
    this.value = value
  }

  // Stored in prototype
  get() {
    return this.value
  }

  // Stored in constructor
  static fromStatic(value) {
    return new TestClass('static')
  }
}

console.log(TestClass.prototype)
// Object { ... }
//    constructor
//    get()
console.log(TestClass.prototype.constructor)
// class TestClass
//    fromStatic()
```

## 7. Project: A Robot

1. Persistent Data
```js
const pi = Object.freeze({ value: 3.14 })
pi.value = 5
pi.value
// 3.14
```

2. Static Method (written here by directly adding a property to the constructor)
```js
class Person { 
  constructor(name) {
    this.name = name
  }
}
Person.fromName = function(name) {
  return new Person(name)
}
```
