# 第 8 章 数组的扩展
## 8.1 扩展运算符
### 8.1.1 含义
+ 扩展运算符 (spread) 是三个点 (...)，它如同 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列

### 8.1.2 替代数组的 apply 方法
+ 由于扩展运算符可以展开数组，所以不再需要使用 apply 方法将数组转为函数的参数
```javascript
// ES5 写法
function f(x, y ,z) {
    // ...
}
var args = [0, 1, 2]
f.apply(null, args)

// ES6 写法
function f(x, y, z) {
    // ...
}
var args = [0, 1, 2]
f(...args)

// 例子
// ES5 写法
Math.max.apply(null, [14, 3, 77])
// ES6 写法
Math.max(...[14, 3, 77])
// 等同于
Math.max(14, 3, 77)
```
### 8.1.3 扩展运算符的应用
+ 合并数组
+ 与解构赋值一起使用
  + > const [first, ...rest] = [1, 2, 3, 4] => first = 1, rest = [2, 3, 4]
  + 如果将扩展运算符用于数组赋值，则只能将其放在参数的最后一位，否则会报错
+ 扩展运算符还可以将字符串转为真正的数组
+ 任何 Iterator 接口的对象都可以用扩展运算符转为真正的数组
+ 扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，如 Map 结构
+ Generator 函数运行后会返回一个遍历器对象，因此也可以使用扩展运算符
+ 对于没有 Iterator 接口的对象，使用扩展运算符将会报错

## 8.2 Array.from()
+ Array.from 方法用于将两类对象转为真正的数组：
  + 类似数组的对象 (array-like object)
  + 可遍历 (Iterable) 对象 (包括 ES6 新增的数据结构 Set 和 Map)
  + 一般用作 NodeList 对象 和 arguments 对象
  + Array.from 还可以接受第二个参数，作用类似于数组的 map 方法，用来对每个元素进行处理，将处理后的值放入返回的数组
  + 如果 map 函数里面用到了 this 关键字，还可以传入 Array.from 第三个参数，用来绑定 this
  + 这意味着，只要有一个原始的数据结构，就可以先对他的值进行处理，然后转成规范的数组结构，进而可以使用数量众多的数组方法
```javascript
// ES5 写法
[].slice.call(arrayList)
// ES6 写法
Array.from(arrayLike)
```
## 8.3 Array.of()
+ Array.of 方法用于将一组值转换为数组
  + > Array.of(3, 11, 8) => [3, 11, 8]
  + 这个方法的主要目的就是为了弥补数组结构函数 Array() 的不足。因为参数个数的不同会导致 Array() 行为的差异
    + 只有当参数个数不少与 2 时， Array() 才会返回由参数组成的新数组。
    + 参数个数只有一个时，实际上是指定数组的长度

## 8.4 copyWithin()
+ 数组实例的 copyWithin 方法会在当前数组内部将指定位置的成员复制到其他位置 (会覆盖原有成员)，然后返回当前数组。也就是说，使用这个方法会修改当前数组
+ Array.prototype.copyWithin(target, start = 0, end = this.length)
  + target (必选): 从该位置开始替换数据
  + start (可选): 从该位置开始读取数据，默认为0。如果为负值，表示倒数
  + end (可选): 到该位置**前**停止读取数据，默认等于数组长度。如果为负值，表示倒数

## 8.5 find() 和 findIndex()
+ find 方法用于找出第一个符合条件的数组成员。他的参数是一个回调函数，所有数组成员依次执行该回调函数，知道找出第一个返回值为 true 的成员，然后返回该成员，如果没有，则返回 undefined
  + 可接受三个参数：当前的值、当前位置的索引、原数组
+ findIndex 的用法与 find 非常类似，返回第一个符合条件的数组成员的位置，如果都不符合条件，则返回 -1
+ 这两个方法都可以接受第二个参数，用来绑定回调函数的 this 对象
+ 这两个方法都可以发现 NaN，弥补了 indexOf 方法的不足

## 8.6 fill()
+ fill 方法使用给定值填充到一个数组
```javascript
[1, 2, 3].fill(7) // [7, 7, 7]

new Array(3).fill(1) // [1, 1, 1]
```
+ fill 方法用于空数组的初始化时非常方便。数组中已有的元素会被全部抹去
+ 可以接受第二个和第三个参数，分别是：填充的起始位置和结束位置

## 8.7 entries()、keys()、values()
+ 这三个方法都是用于遍历数组。他们都返回一个遍历器对象，可用 for...of 循环遍历，唯一的区别在于，keys()是对键名的遍历，values()是对键值的遍历，entries() 是对键值对的遍历
+ 如果不使用 for...of 循环，也可以手动调用遍历器对象的 next 方法进行遍历

## 8.8 includes()
+ 该方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的 includes 方法类似
+ 该方法的第二个参数表示搜索的起始位置，默认为 0。如果第二个参数为负数，则表示倒数的位置，如果这时他大于数组长度，则会重置为 0 开始
+ indexOf 方法的两个缺点：
  + 不够语义化，其含义是找到参数值的第一个出现位置，所以要比较是否不等于 -1，不够直观
  + 其内部使用严格相等运算符 (===) 进行判断，会导致对 NaN 的误判
+ 注意：Map 的 Set 数据结构有一个 has 方法，需要注意与 includes 区分
  + Map 结构的 has 方法是用来查找键名的
  + Set 结构的 has 方法是用来查找值的

## 8.9 数组的空位
+ 数组的的空位指数组的某一个位置没有任何值。比如，Array 构造函数返回的数组都是空位 
  + 注意！空位不是 undefined，一个位置的值等于 undefined 依然是有值的，空位是没有任何值的，in 运算符可以说明这一点
```javascript
0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
```
+ 上面的代码表示，第一个数组的 0 号位置是有值的，第二个数组的 0 号位置没有值
+ 在 ES5 中，大多数情况会忽略空位
  + forEach()、filter()、every()、some() 都会跳过空位
  + map() 会跳过空位，但会保留这个值
  + join() 和 toString() 会将空位视为 undefined，而 undefined 和 null 会被处理为空字符串
+ 在 ES6 中，明确将空位转为 undefined
  + Array.from 方法会将数组的空位转为 undefined
  + 扩展运算符也会将空位转为 undefined
  + copyWithin 会连空位一起赋值
  + fill 会将空位视为正常的数组位置
  + for...of 循环也会遍历空位
  + entries、keys、values、find、findIndex 会将空位处理成 undefined
+ 由于空位的处理规则对边，这边建议还是避免出现空位

# 第 9 章 对象的扩展
## 9.1 属性的简洁表示法
+ ES6 允许直接写入变量和函数作为对象的属性和方法。这样书写更加简洁
```javascript
var foo = 'bar'
var baz = { foo }
baz // { foo: 'bar' }
// 等同于
var baz = { foo: foo }
```
+ ES6 允许在对象中只写属性名，不写属性值。这时，属性值等于属性名所代表的变量。
```javascript
function f(x, y) {
    return {x, y}
}
// 等同于
function f(x, y) {
    return {x: x, y: y}
}

f(1, 2) // Object {x: 1, y: 2}
```
+ 除了属性简写，方法也可以简写
```javascript
var o = {
    method() {
        return 'Hello!'
    }
}
// 等同于
var o = {
    method: function() {
        return 'Hello!'
    }
}
// 实际的例子
var birth = '2000/01/01'
var Person = {
    name: '张三',
    // 等同于 birth: birth
    birth,
    // 等同于 hello: function() { ... }
    hello() {
        console.log(123)
    }
}
```
+ CommonJS 模块输出变量就非常适合使用简洁写法
```javascript
var ms = {}
function getItem(key) {
    return key in ms ? ms[key] : null
}
function setItem(key, value) {
    ms[key] = value
}
function clear() {
    ms = {}
}
module.exports = { getItem, setItem, clear }
// 等同于
module.exports = {
    getItem: getItem,
    setItem: setItem,
    clear: clear
}
```
+ 属性的赋值器 (setter) 和取值器 (getter) 事实上也采用了这种写法
```javascript
var cart = {
    _wheels: 4,
    get wheels() {
        return this._wheels
    },
    set wheels(value) {
        if (value < this._wheels) {
            throw new Error('数值太小了！')
        }
        this._wheels = value
    }
}
```
+ **注意**：简洁写法中属性名总是字符串，这会导致一些看上去比较奇怪的结果
```javascript
var obj = {
    class() {}
}
// 等同于
var obj = {
    'class': function() {}
}
// 上面的代码中，class 是字符串，所以不会因为它属于关键字而导致语法解析报错
```
+ 如果某个方法的值是一个 Generator 函数，则其前面需要加上星号
```javascript
var obj = {
    * m() {
        yield 'hello world'
    }
}
```
## 9.2 属性名表达式
+ JS 定义对象的属性有两种方法
  + > obj.foo = true
  + > obj['a' + 'bc'] = 123
+ 上面的方法 一是直接用标识符所谓属性名；二是用表达式作为属性名，这时要将表达式放在方括号里
+ 如果使用字面量方式定义对象 (使用大括号)，则在 ES5 中只能使用方法一 (标识符) 定义属性
+ ES6 中允许字面量定义对象时使用方法二 (表达式作为对象的属性名)，即把表达式放在方括号内
```javascript
let propKey = 'foo'

let obj = {
    [propKey]: true,
    ['a' + 'bc']: 123
}
```
+ 表达式还可以用于定义方法名
```javascript
let obj = {
    ['h' + 'ello']() {
        return 'hi'
    }
}
obj.hello() // 'hi'
```
+ 注意，属性名表达式与简洁表示法不能同时使用，否则会报错
+ 属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串[object Object]，这一点要特别小心

## 9.3 方法的 name 属性
+ 函数的 name 属性返回函数名。对象方法也是函数，因此也有 name 属性
```javascript
const person = {
    sayName() {
        console.log('hello')
    }
}
person.sayName.name // 'sayName'
```
+ name 属性返回函数名
+ 如果对象的方法使用了取值函数 (getter) 和 存值函数 (setter)，则 name 属性不是在该方法上面，而是在该方法属性的描述对象的 get 和 set 属性上面，返回值是方法名前加上 get 和 set
+ 有两种特殊情况：bind 方法创造的函数，name 属性返回 'bound' 加上原函数的名字；Function 构造函数创造的函数，name 属性返回 'anonymous'
```javascript
(new Function().name)   // 'anonymous'

var doSomething = function() {
    // ...
}
doSomething.bind().name // 'bound doSomething'
```
如果对象的方法是一个 Symbol 值，那么 name 属性返回的是这个 Symbol 值的描述
```javascript
const key1 = Symbol('description')
const key2 = Symbol()
let obj = {
    [key1]() {},
    [key2]() {}
}
obj[key1].name  // '[description]'
obj[key2].name  // ''
```

## 9.4 Object.is()
+ ES5 比较两个值是否相等，只有两个运算符：相等运算符 (==) 和严格相等运算符 (===)。他们都有缺点，前者会自动转换数据类型，后者的 NaN 不等于自身，以及 +0 等于 -0。JS 缺乏这样一种运算：在所有环境中，只要两个值是一样的，他们就应该相等
+ ES6 提出了 'Same-value equality' (同值相等) 算法用来解决这个问题。Object.is 就是部署这个算法的新方法。它用来比较两个值是否严格相等，与严格相等运算符 (===) 的行为基本一致
+ ES5 可以通过下面的代码部署 Object.is
```javascript
Object.defineProperty(Object, 'is', {
    value: function(x, y) {
        if (x === y) {
            // 针对 +0 不等于 -0 的情况
            return x !== 0 || 1 / x === 1 / y
        }
        // 针对 NaN 的情况
        return x !== x && y !== y
    },
    configurable: true,
    enumerable: false,
    writable: true
})
```

## 9.5 Object.assign()
### 9.5.1 基本用法
+ Object.assign 方法用于将源对象 (source) 的所有可枚举属性复制到目标对象 (target)。
```javascript
var target = { a: 1 }

var source1 = { b: 2 }
var source2 = { c: 3 }

Object.assign(target, source1, source2)
target  // { a: 1, b: 2, c: 3 }
```
+ Object.assign 方法的第一个参数是目标对象，后面的参数都是源对象
+ **注意**：如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性
```javascript
var target = { a: 1, b: 2 }

var source1 = { b: 2, c: 2 }
var source2 = { c: 3 }

Object.assign(target, source1, source2)
target  // { a: 1, b: 2, c: 3 }
```
+ 如果只有一个参数，Object.assign 会直接返回该参数
```javascript
var obj = { a: 1 }
Object.assign(obj) === obj  // true
```
+ 如果该参数不是对象，则会先转成对象，然后返回
```javascript
typeof Object.assign(2) // object
```
+ `undefined` 和 `null` 无法转成对象，所以如果将他们作为参数，就会报错
+ 如果非对象参数出现在源对象的位置 (即非首参数)，那么处理规则将有所不同
  + 首先，这些参数都会转成对象
  + 如果无法转成对象便会跳过
  + 这意味着，如果 `undefined` 和 `null` 不在首参数便不会报错
+ 其他参数的值 (即数值、字符串和布尔值) 不再首参数也不会报错。但是，除了**字符串会以数组形式复制到目标对象**，**其他值都不会产生效果**
```javascript
let v1 = 'abc'
let v2 = true
let v3 = 10
let obj = Object.assign({} , v1, v2, v3)
console.log(obj)    // { '0': 'a', '1': 'b', '2': 'c' }
```
```javascript
Object(true)    //  { [[PrimitiveValue]]: true } 
Object(10)      //  { [[PrimitiveValue]]: 10 }
Object('abc')   //  { 0: 'a', 1: 'b', 2: 'c', length: 3, [[ PrimitiveValue ]]: 'abc' }
```
+ 在上面的代码中，布尔值、数值、字符串分别转成对应的包装对象，可以看到他们的原始值都在包装对象的内部属性 `[[ PrimitiveValue ]]` 上面，这个属性是不会被 `Object.assign` 复制的。只有字符串的包装对象会产生可枚举的实义属性，那些属性则会被拷贝
+ `Object.assign` 复制的属性是有限制的，只复制源对象的自身属性 (不复制继承属性)，也不复制不可枚举的属性 (`enumerable: false`)
+ 属性名为 `Symbol` 值的属性也会被 `Object.assign` 复制
```javascript
Object.assign({a: 'b'}, { [Symbol('c'): 'd'] }) // { a: 'b', Symbol(c): 'd' }
```

### 9.5.2 注意点
+ `Object.assign` 方法实行的是浅复制，而不是深复制。也就是说，如果源对象某个属性的值是对象，那么目标对象复制得到的是这个对象的引用
```javascript
let obj1 = { a: {b: 1} }
let obj2 = Objetc.assign({}, obj1)

obj1.a.b = 2
obj2.a.b    // 2
// 这个对象的任何变化都会反映到目标对象上面
```
+ 对于这种嵌套的对象，一旦遇到同名属性，`Object.assign` 的处理方法是替换而不是添加。
```javascript
let target = { a: { b: 'c', d: 'e' } }
let source = { a: { b: 'hello' } }
Object.assign(target, source)   // { a: { b: 'hello' } }
```
+ 注意，`Object.assign()` 可以用来处理数组，但是会把数组视为对象来处理
```javascript
Object.assign([1, 2, 3], [4, 5])    // [4, 5, 3]
```
+ 上面的代码中，`Object.assign` 把数组视为属性名为 0、1、2 的对象，因此目标数组的 0 号属性 4 覆盖了原数组的 0 号属性 1。

### 9.5.3 常见用途
`Object.assign` 方法有很多用处
#### 为对象添加属性
```javascript
class Point {
    constructor(x, y) {
        Object.assign(this, {x, y})
    }
}
// 通过 assign 方法将 x 属性和 y 属性添加到了 Point 类的对象实例中
```
#### 为对象添加方法
```javascript
Object.assign(SomeClass.prototype, {
    someMethod(arg1, arg2) {
        // ...
    },
    anotherMethod() {
        // ...
    }
})
// 等同于
SomeClass.prototype.someMethod = function(arg1, arg2) {
    // ...
}
SomeClass.prototype.anotherMethod = function() {
    // ...
}
```
#### 克隆对象
```javascript
function clone(origin) {
    return Object.assign({}, origin)
}
```
+ 将原始对象复制到一个空对象中，就得到了原始对象的克隆
+ 不过，这种方法只能克隆原始对象自身的值，不能克隆他继承的值。如果想要保持继承链，可以采用下面的代码：
```javascript
function clone(origin) {
    let originProto = Object.getPrototypeOf(origin)
    return Object.assign(Object.create(originProto), origin)
}
```
#### 合并多个对象
```javascript
// 将多个对象合并到某个对象
const merge = (target, ...source) => Object.assign(target, ...sources)
// 合并并返回一个新对象
const merge = (...sources) => Object.assign({}, ...sources)
```
#### 为属性指定默认值
```javascript
const DEFAULTS = {
    logLevel: 0,
    outputFormat: 'html'
}

function processContent(options) {
    options = Object.assign({}, DEFAULTS, options)
    console.log(options)
}
```
+ 上面的代码中，DEFAULTS 对象是默认值，options 对象是用户提供的参数。`Object.assign` 方法将 DEFAULTS 和 options 合并成一个新对象，如果两者有同名属性，则 options 的属性将覆盖 DEFAULTS 的属性值
+ 由于存在深复制的问题，DEFAULTS 对象和 options 对象的所有属性的值都只能是简单类型，而不能指向另一个对象，否则将导致 DEFAULTS 对象的该属性不起作用
```javascript
const DEFAULTS = {
    url: {
        host: 'example.com',
        port: 7070
    }
}

processContent({
    url: {
        port: 8080
    }
})
// { url: { port: 8080 } }
```

## 属性的可枚举性
+ 对象的每一个属性都具有一个描述对象 (Descriptor)，用于控制该属性的行为
+ `Object.getOwnPropertyDescriptor` 方法可以获取该属性的描述对象
+ 描述对象的 `enumerable` 属性称为 ‘可枚举性’，如果该属性为 false，就表示某些操作会忽略当前属性
+ ES5 有 3 个操作会忽略 `enumerable` 为 false 的属性
  + `for...in` 循环：只遍历对象自身的和继承的可枚举属性
  + `Object.keys()`：返回对象自身的所有可枚举属性的键名
  + `JSON.stringify()`：只串行化对象自身的可枚举属性
  + ES6 新增了一个操作 `Object.assign()`，会忽略 `enumerable` 为 false 的属性，只复制对象自身的可枚举属性
+ 这四个操作之中，只有 `for...in` 会返回继承的属性。实际上，引入 `enumerable` 的最初目的就是让某些属性可以规避掉 `for...in` 操作
+ ES6 规定，所有 `Class` 的原型的方法都是不可枚举的
+ 总的来说，操作中引入继承的属性会让问题复杂化，大多时候，我们只关心对象自身的属性。所以尽量不要用 `for...in` 训话，而用 `Object.keys()` 代替

## 9.7 属性的遍历
+ ES6 一共有 5 种方法可以遍历对象的属性
+ `for..in`
  + 循环遍历对象自身的和继承的可枚举属性 (不含 Symbol 属性)
+ `Object.keys(obj)`
  + 返回一个数组，包括对象自身的 (不含继承的) 所有可枚举属性 (不含 Symbol 属性)
+ `Object.getOwnPropertyName(obj)`
  + 返回一个数组，包含对象自身的所有属性 (不含 Symbol 属性，但是包括不可枚举属性)
+ `Object.getOwnPropertySymbols(obj)`
  + 返回一个数组，包含对象自身的所有 Symbol 属性
+ `Reflect.ownKeys(obj)`
  + 返回一个数组，包含对象自身的所有属性，不管属性名是 Symbol 还是字符串，也不管是否可枚举

+ 以上五种方法遍历对象的属性时都遵循同样的属性遍历次序规则
  + 首先遍历所有属性名为数值的属性，按照数字排序
  + 其次遍历所有属性名为字符串的属性，按照生成时间排序
  + 最后遍历所有属性名为 Symbol 值的属性，按照生成时间排序

## 9.8 __proto__ 属性、Object.setPrototypeOf()、Object.getPrototypeOf()
### 9.8.1 __proto__ 属性
+ `__proto__` 属性用来读取设置当前对象的 `prototype` 对象
+ 该属性没有写入 ES6 正文，而是写入了附录，原因是 `__proto__` 前后的双下划线说明它本质上一个内部属性，而不是一个正式的对外的 API，只是由于浏览器广泛支持，才被加入了 ES6。标准明确规定，只有浏览器必须部署这个属性，其他运行环境不一定要部署，而且新的代码最好认为这个属性是不存在的。因此，无论从语义角度，还是从兼容性的角度，都不要使用这个属性，而是使用 `Object.setPrototypeOf()` (写操作)、`Object.getPrototypeOf()` (读操作) 或 `Object.create()` (生成操作) 代替
+ 在实现上，`__proto__` 调用的是 `Object.prototype.__proto__`，具体实现如下。
```javascript
Object.defineProperty(Object.prototype, '__proto__', {
    get() {
        let _thisObj = Object(this)
        return Object.getPrototypeOf(_thisObj)
    },
    set(proto) {
        if (this === undefined || this === null) {
            throw new TypeError()
        }
        if (!isObject(this)) {
            return undefined
        }
        if (!isObject(proto)) {
            return undefined
        }
        let status = Reflect.setPrototype(this, proto)
        if (!status) {
            throw new TypeError()
        }
    }
})
function isObject(value) {
    return Object(value) === value
}
```
+ 如果一个对象本身部署了 `__proto__` 属性，则该属性的值就是对象的原型

### 9.8.2 Object.setPrototypeOf()
+ `Object.setPrototypeOf` 方法的作用与 `__proto__` 相同，用来设置一个对象的 `prototype` 对象，返回参数对象本身。它是 ES6 正式推荐的设置原型对象的方法
```javascript
// 格式
Object.setPrototypeOf(object, prototype)
// 用法
var o = Object.setPrototypeOf({}, null)
// 等同于下面的函数
function (obj, proto) {
    obj.__proto__ = proto
    return obj
}
```
+ 如果第一个参数不是对象，则会自动转为对象。但是由于返回的还是第一个参数，所以这个操作不会产生任何效果
+ 由于 `undefined` 和 `null` 无法转为对象，所以如果第一个参数是 `undefined` 或 `null` 就会报错
### 9.8.3 Object.getPrototypeOf()
+ 用于读取一个对象的 `prototype` 对象
+ 与 `setPrototypeOf` 类似
## 9.9 Object.keys()、Object.values()、Object.entries()
### 9.9.1 Object.keys()
+ ES6 引入了 `Object.keys` 方法，返回一个数组，成员是参数对象自身的 (不含继承的) 所有可遍历 (enumerable) 属性的键名 (属性名)
+ `Object.keys(obj)`
### 9.9.2 Object.values()
+ 返回一个数组，成员是参数对象自身的 (不含继承的) 所有可遍历属性的键值 (属性值)
+ 顺序是：数字 => 字符串 => Symbol
+ 其中，数值大小 从小到大遍历
```javascript
var obj = Object.create({}, {p: {value: 42}})
Object.values(obj)  // [] 
```
+ `Object.create` 方法的第二个参数添加的对象属性如果不显示声明，默认是不可遍历的， 因为 p 的属性描述对象的 `enumerable` 默认是 false， `Object.values` 不会返回这个属性。
+ 如果 `Object.values` 方法的参数是一个字符串，则会返回各个字符串组成的一个数组
+ 如果参数不是对象，那么会先将其转换为对象。由于数值和布尔值的包装对象都不会为实例添加非继承的属性，所以 `Object.values` 会返回一个空数组
### 9.9.3 Object.entries()
+ 返回一个数组，成员是参数对象自身的 (不含继承的) 所有可遍历属性的键值对数组
```javascript
var obj = { foo: 'bar', baz: 42 }
Object.entries(obj) // [['foo', 'bar'], ['baz', 42]] 
```
+ 除了返回值不同，该方法的行为与 `Object.values` 基本一致
+ 如果原对象的属性名是一个 `Symbol` 值，该属性会被忽略
+ 基本用途是遍历对象的属性：
```javascript
let obj = { one: 1, two: 2 }
for (let [k, v] of Object.entries(obj)) {
    console.log(
        `${JSON.stringify(k)}: ${JSON.stringify(v)}`
    )
}
```
+ 另一个用处就是将对象转为真正的 Map 结构
```javascript
var obj = { foo: 'bar', baz: 42 }
var map = new Map(Object.entries(obj))
map // Map { foo: 'bar', baz: 42 }
```