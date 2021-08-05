# Objects, Classes, and Object-Oriented Programming

## Understanding Objects:

### Object Destructuing:

```text
// EG 1
let person = {
 name: 'Matt',
 age: 27
};
let { name, job } = person;
console.log(name); // Matt
console.log(job); // undefined

// EG 2
let person = {
 name: 'Matt',
 age: 27
};
let { name, job='Software engineer' } = person;
console.log(name); // Matt
console.log(job); // Software engineer



// EG 3
let person = {
 name: 'Matt',
 age: 27,
 job: {
 title: 'Software engineer'
 }
};
// Declares 'title' variable and assigns person.job.title as its value
let { job: { title }} = person;
console.log(title); // Software engineer 
```

1. 如果没有match 就会得到 undefined
2. It can have default value
3. Nested Destructuring
   1. 

### Object 方法：

```text
//1. 对象本身的方法
Object.print = function (o) { console.log(o) };

//2. Object 实例方法
Object.prototype.print = function () {
  console.log(this);
};

var obj = new Object();
obj.print() // Object
```

1. 对象本身的方法
   * _**直接定义在Object 对象的方法**_
2. Object 实例方法
   1. _**定义在 Object.prototype**_ 也可以被 Object 实例直接使用
   2. 凡是定义在Object.prototype对象上面的属性和方法，将被所有实例对象共享

### Object\(\):

```text
// EG 1
var obj = Object();
// 等同于
var obj = Object(undefined);
var obj = Object(null);

obj instanceof Object // true

// EG 2
var obj = Object(1);
obj instanceof Object // true
obj instanceof Number // true

var obj = Object('foo');
obj instanceof Object // true
obj instanceof String // true

var obj = Object(true);
obj instanceof Object // true
obj instanceof Boolean // true

// EG 3
var arr = [];
var obj = Object(arr); // 返回原数组
obj === arr // true

var value = {};
var obj = Object(value) // 返回原对象
obj === value // true

var fn = function () {};
var obj = Object(fn); // 返回原函数
obj === fn // true
function isObject(value) {
  return value === Object(value);
}

isObject([]) // true
isObject(true) // false
```

 `Object`本身是一个函数，可以当作工具函数使用，将任意值转为对象。Object\(\)

1.  如果参数为空（或者为`undefined`和`null`），`Object()`返回一个空对象。
2.  如果参数是原始类型的值，`Object`方法将其转为对应的包装对象的实例,  `Object`函数的参数是各种原始类型的值，转换成对象就是原始类型值对应的包装对象。
3.  如果`Object`方法的参数是一个对象，**它总是返回该对象，即不用转换。** 利用这一点，可以写一个判断变量是否为对象的函数。

### Object 构造函数

```text
var obj = new Object();

var o1 = {a: 1};
var o2 = new Object(o1);
o1 === o2 // true

var obj = new Object(123);
obj instanceof Number // true
```

 `Object`构造函数的首要用途，是直接通过它来生成新对象.  注意，通过`var obj = new Object()`的写法生成新对象，与字面量的写法`var obj = {}`是等价的。或者说，后者只是前者的一种简便写法。

1.  `Object`构造函数的用法与工具方法很相似，几乎一模一样



### Object 的静态方法\(static method\) <a id="object-&#x7684;&#x9759;&#x6001;&#x65B9;&#x6CD5;"></a>

```text
// ===== EG 1 =====
var obj = {
  p1: 123,
  p2: 456
};

Object.keys(obj) // ["p1", "p2"]

var a = ['Hello', 'World'];

Object.keys(a) // ["0", "1"]
Object.getOwnPropertyNames(a) // ["0", "1", "length"]


// ===== EG 3.1 Object.preventExtensions() =====
var obj = new Object();
Object.preventExtensions(obj);
obj.age = 10;
Object.defineProperty(obj, 'p', {
  value: 'hello'
});
// TypeError: Cannot define property:p, object is not extensible.

obj.age // undefined
obj.p = 1;
obj.p // undefined


// ===== EG 3.2 Object.isExtensible() =====
var obj = new Object();

Object.isExtensible(obj) // true
Object.preventExtensions(obj);
Object.isExtensible(obj) // false

// ===== EG 3.3 Object.seal() =====
var obj = { p: 'hello' };
Object.seal(obj);

delete obj.p;
obj.p // "hello"

obj.x = 'world';
obj.x // undefined

// ===== EG 3.5 Object.freeze() =====

var obj = {
  p: 'hello'
};

Object.freeze(obj);

obj.p = 'world';
obj.p // "hello"

obj.t = 'hello';
obj.t // undefined

delete obj.p // false
obj.p // "hello"


// ===== EG 3.7 漏洞 和 局限性 =====

var obj = new Object();
Object.preventExtensions(obj);
obj.t = 10;
console.log(obj.t )//undefined
var proto = Object.getPrototypeOf(obj);
proto.t = 'hello';

console.log(obj.t); // hello

//解决办法 把obj的原型也冻结住。
var obj = new Object();
Object.preventExtensions(obj);

var proto = Object.getPrototypeOf(obj);
Object.preventExtensions(proto);

proto.t = 'hello';
obj.t // undefined

// 局限
var obj = {
  foo: 1,
  bar: ['a', 'b']
};
Object.freeze(obj);

obj.bar.push('c');
obj.bar // ["a", "b", "c"]


//4.1 ===== Object.create() =====


var A = {
  print: function () {
    console.log('hello');
  }
};

var B = Object.create(A);

Object.getPrototypeOf(B) === A // true
B.print() // hello
B.print === A.print // true

Object.create()
// TypeError: Object prototype may only be an Object or null
Object.create(123)
// TypeError: Object prototype may only be an Object or null

// 在原型上添加或修改任何方法，会立刻反映在新对象之上。
var obj1 = { p: 1 };
var obj2 = Object.create(obj1);

obj1.p = 2;
obj2.p // 2


// 第二个参数

var obj = Object.create({}, {
  p1: {
    value: 123,
    enumerable: true,
    configurable: true,
    writable: true,
  },
  p2: {
    value: 'abc',
    enumerable: true,
    configurable: true,
    writable: true,
  }
});

// 等同于
var obj = Object.create({});
obj.p1 = 123;
obj.p2 = 'abc';


// 4.2 ===== 原型链相关方法 =====
var F = function () {};
var f = new F();
Object.getPrototypeOf(f) === F.prototype // true

// 空对象的原型是 Object.prototype
Object.getPrototypeOf({}) === Object.prototype // true

// Object.prototype 的原型是 null
Object.getPrototypeOf(Object.prototype) === null // true

// 函数的原型是 Function.prototype
function f() {}
Object.getPrototypeOf(f) === Function.prototype // true

// 4.3 ===== Object.setPrototypeOf() =====
var a = {
  name:'test'
};
var b = {
  age: 10
};
Object.setPrototypeOf(a, b);

Object.getPrototypeOf(a) === b // true
console.log(a.name); //test
console.log(a.age); // 10



// 5 ===== getOwnPropertyNames =====
Object.getOwnPropertyNames(Date)
// ["parse", "arguments", "UTC", "caller", "name", "prototype", "now", "length"]



// 6 ===== hasOwnProperty =====
Date.hasOwnProperty('length') // true
Date.hasOwnProperty('toString') // false
```

_**是指Object 对象自身的方法**_

**Object.prototype.keys\(obj\) // \["p1", "p2"\]   // this is not a function**

1. **`Object.keys()，Object.getOwnPropertyNames() , for ...in`**  都用来遍历对象的属性 
   1.  这两个方法的参数是一个对象，返回一个数组。该数组的成员都是该对象自身的（而不是继承的）所有属性名。
   2.  只有涉及不可枚举属性时，才会有不一样的结果。`Object.keys`方法只返回可枚举的属性，`Object.getOwnPropertyNames`方法还返回不可枚举的属性名
   3.  一般情况下，几乎总是使用`Object.keys`方法，遍历对象的属性。
2. 对象属性模型的相关方法
   1. `Object.getOwnPropertyDescriptor()`  在 **Reading Property Attributes** 那个 section 解释了
   2. `Object.defineProperty(）`在 **Data properties** 那个 section 解释了
   3. `Object.defineProperties()` 在 **Defining multiple properties** 那个 section 解释了
3. 控制对象状态的方法
   1. `Object.preventExtensions()` 方法可以使得一个对象无法再添加新的属性。
   2. `Object.isExtensible()`  方法用于检查一个对象是否使用了`Object.preventExtensions`方法。也就是说，检查是否可以为一个对象添加属性。
   3. `Object.seal()` 方法使得一个对象既无法添加新属性，也无法删除旧属性。 `Object.seal`实质是把属性描述对象的`configurable`属性设为`false`，因此属性描述对象不再能改变了。
   4. `Object.isSealed()`  方法用于检查一个对象是否使用了`Object.seal`方法。
   5. `Object.freeze()` 方法可以使得一个对象无法添加新属性、无法删除旧属性、也无法改变属性的值，使得这个对象实际上变成了常量。
   6. `Object.isFrozen()`  方法用于检查一个对象是否使用了`Object.freeze`方法。
   7. 上述的 3个 方法锁定对象都有一个可能的漏洞和 局限性，
      1. 漏洞： 可以通过改变原型对象，来为对象增加属性。 上面代码中，对象obj本身不能新增属性，但是可以在它的原型对象上新增属性，就依然能够在obj上读到。
      2. 另外一个局限是，如果属性值是对象，上面这些方法只能冻结属性指向的对象，而不能冻结对象本身的内容。 上面代码中，`obj.bar`属性指向一个数组，`obj`对象被冻结以后，这个指向无法改变，即无法指向其他值，但是所指向的数组是可以改变的。
4. 原型链相关方法
   1. `Object.create()`：该方法接受一个对象作为参数，然后以它为原型，返回一个实例对象。该实例完全继承原型对象的属性
      1.  使用Object.create\(\)方法的时候，必须提供对象原型，即参数不能为空，或者不是对象，否则会报错
      2.  Object.create\(\)方法生成的新对象，动态继承了原型。在原型上添加或修改任何方法，会立刻反映在新对象之上
      3.  `Object.create()`方法还可以接受第二个参数。该参数是一个属性描述对象，它所描述的对象属性，会添加到实例对象，作为该对象自身的属性
   2. `Object.getPrototypeOf()`：获取对象的`Prototype`对象。
   3.  `Object.setPrototypeOf()`  方法为参数对象设置原型，返回该参数对象。它接受两个参数，第一个是现有对象，第二个是原型对象。`Object.setPrototypeOf(a, b);`  将a的原型设置到b上面
5. `getOwnPropertyNames`    返回一个数组，成员是参数对象本身的所有属性的键名，不包含继承的属性键名。
6. `Object.prototype.hasOwnProperty()`用于判断某个属性定义在对象自身，还是定义在原型链上。 `Date.length`（构造函数`Date`可以接受多少个参数）是`Date`自身的属性，`Date.toString`是继承的属性。

### 

###  获取实例对象`obj`的原型对象 

```text
var obj = new Object();

obj.__proto__ === Object.prototype
// true
obj.__proto__ === obj.constructor.prototype

// 三种方法
obj.__proto__
obj.constructor.prototype
Object.getPrototypeOf(obj)

// 手动更改 原型对象时候 也需要更改 constructor 属性

var P = function () {};
var p = new P();

var C = function () {};
C.prototype = p;
var c = new C();

c.constructor.prototype === p // false
// 正确修改方法
C.prototype = p;
C.prototype.constructor = C;

var c = new C();
c.constructor.prototype === p // true
```

*  上面三种方法之中，前两种都不是很可靠。`__proto__`属性只有浏览器才需要部署，其他环境可以不部署。而`obj.constructor.prototype`在手动改变原型对象时，可能会失效。
*  上面例子中 构造函数`C`的原型对象被改成了`p`，但是实例对象的`c.constructor.prototype`却没有指向`p`。所以，在改变原型对象时，一般要同时设置`constructor`属性。
* 关于 constructor 的解释情况 在 原型链 section看

###  <a id="object-&#x7684;&#x5B9E;&#x4F8B;&#x65B9;&#x6CD5;"></a>

### Object 的实例方法\(instance method） <a id="object-&#x7684;&#x5B9E;&#x4F8B;&#x65B9;&#x6CD5;"></a>

 定义在`Object.prototype`对象。它们称为实例方法  所有`Object`的实例对象都继承了这些方法。

```text
// ===== EG 1 =====
var obj = new Object();
obj.valueOf() === obj // true

var obj = new Object();
1 + obj // "1[object Object]"
// 自动转换 例子
var obj = new Object();
obj.valueOf = function () {
  return 2;
};

1 + obj // 3

// ===== EG 2 =====
var o1 = new Object();
o1.toString() // "[object Object]"

var o2 = {a:1};
o2.toString() // "[object Object]"

// 2.2
var obj = new Object();

obj.toString = function () {
  return 'hello';
};

obj + ' ' + 'world' // "hello world"

// 2.3
[1, 2, 3].toString() // "1,2,3"

'123'.toString() // "123"

(function () {
  return 123;
}).toString()
// "function () {
//   return 123;
// }"

(new Date()).toString()
// "Tue May 10 2016 09:11:31 GMT+0800 (CST)"

// 2.4
Object.prototype.toString.call(2) // "[object Number]"
Object.prototype.toString.call('') // "[object String]"
Object.prototype.toString.call(true) // "[object Boolean]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(null) // "[object Null]"
Object.prototype.toString.call(Math) // "[object Math]"
Object.prototype.toString.call({}) // "[object Object]"
Object.prototype.toString.call([]) // "[object Array]"

// ===== EG 3 =====

var person = {
  toString: function () {
    return 'Henry Norman Bethune';
  },
  toLocaleString: function () {
    return '白求恩';
  }
};

person.toString() // Henry Norman Bethune
person.toLocaleString() // 白求恩





// ===== EG 6 =====
function F() {
    this.a = 1;
    this.b = 2;
}
var obj = new F();
F.prototype.c = 3;
console.log(obj.propertyIsEnumerable("a"));//true
console.log(obj.propertyIsEnumerable("b")); //true
console.log(obj.propertyIsEnumerable("c")); //false
obj.d = 4;
console.log(obj.propertyIsEnumerable("d"));//true
for (var I in obj) {
    console.log(I);
}
//a,b,d,c

console.log(obj.hasOwnProperty("a"));// true
console.log(obj.hasOwnProperty("b"));// true
console.log(obj.hasOwnProperty("c"));// false
console.log(obj.hasOwnProperty("d"));// true

var o = new Object();               // Create an object
o.x = 3.14;                         // Define a property
o.propertyIsEnumerable("x");        // true: property x is local and enumerable
o.propertyIsEnumerable("y");        // false: o doesn't have a property y
o.propertyIsEnumerable("toString"); // false: toString property is inherited
Object.prototype.propertyIsEnumerable("toString");  // false: nonenumerable

// ===== EG 7 =====
var obj = {};
var p = {};

obj.__proto__ = p;
Object.getPrototypeOf(obj) === p // true
```

1. `Object.prototype.valueOf()`：返回当前对象对应的值。默认情况下返回对象本身。
   1.  `valueOf`方法的主要用途是，JavaScript 自动类型转换时会默认调用这个方法
   2.  上面代码将对象`obj`与数字`1`相加，这时 JavaScript 就会默认调用`valueOf()`方法，求出`obj`的值再与`1`相加。所以，如果自定义`valueOf`方法，就可以得到想要的结果。
2. `Object.prototype.toString()`：返回当前对象对应的字符串形式。
   1. 默认情况下返回类型字符串
   2.  字符串`[object Object]`本身没有太大的用处，但是通过自定义`toString`方法，可以让对象在自动类型转换时，得到想要的字符串形式。
   3.  数组、字符串、函数、Date 对象都分别部署了自定义的`toString`方法，覆盖了`Object.prototype.toString`方法。
3. `Object.prototype.toLocaleString()`：返回当前对象对应的本地字符串形式。
   1.  这个方法的主要作用是留出一个接口，让各种不同的对象实现自己版本的`toLocaleString`，用来返回针对某些地域的特定的值
4.  `Object.prototype.hasOwnProperty()`：判断某个属性是否为当前对象自身的属性，还是继承自原型对象的属性
5. `Object.prototype.isPrototypeOf()`：判断当前对象是否为另一个对象的原型。
6. `Object.prototype.propertyIsEnumerable()`：判断某个属性是否可枚举。
   1. 这个方法只能用于判断对象自身的属性，对于继承的属性一律返回false
7. `Object.prototype.__proto__`   返回该对象的原型。该属性可读写。

   1. `Object.getPrototypeOf(a) === a.proto;`   等同于Object.getPrototypeof\(\);
   2.  根据语言标准，`__proto__`属性只有浏览器才需要部署，其他环境可以没有这个属性。它前后的两根下划线，表明它本质是一个内部属性，不应该对使用者暴露。因此，应该尽量少用这个属性，而是用`Object.getPrototypeOf()`和`Object.setPrototypeOf()`，进行原型对象的读写操作。

### \*\*\*\*

### **D**ata properties:

```text

// ===== Configurable =====
var obj = Object.defineProperty({}, 'p', {
  value: 1,
  writable: false,
  enumerable: false,
  configurable: false
});

Object.defineProperty(obj, 'p', {writable: true})
// TypeError: Cannot redefine property: p

Object.defineProperty(obj, 'p', {enumerable: true})
// TypeError: Cannot redefine property: p

Object.defineProperty(obj, 'p', {configurable: true})
// TypeError: Cannot redefine property: p

Object.defineProperty(obj, 'p', {value: 2})
// TypeError: Cannot redefine property: p

// Configurable = false but writable = true
var obj = Object.defineProperty({}, 'p', {
  writable: true,
  configurable: false
});

Object.defineProperty(obj, 'p', {writable: false})
// 修改成功

// writable为false时，直接对目标属性赋值，不报错，但不会成功。
var obj = Object.defineProperty({}, 'p', {
  value: 1,
  writable: false,
  configurable: false
});

obj.p = 2;
obj.p // 1

// 可配置性决定了目标属性是否可以被删除（delete）。
var obj = Object.defineProperties({}, {
  p1: { value: 1, configurable: true },
  p2: { value: 2, configurable: false }
});

delete obj.p1 // true
delete obj.p2 // false

obj.p1 // undefined
obj.p2 // 2





// ===== Enumerable =====
var obj = {};

Object.defineProperty(obj, 'x', {
  value: 123,
  enumerable: false
});

obj.x // 123

for (var key in obj) {
  console.log(key);
}
// undefined

Object.keys(obj)  // []
JSON.stringify(obj) // "{}"
```

* Configurable
  * **Indicates if the property may be redefined by removing the property**

    **via delete, changing the property’s attributes,** or changing the property into an accessor

    property. By default, this is true for all properties defined directly on an object

  * Calling **delete on** the property has no effect in nonstrict mode and throws an error in strict mode.
  * **决定 是否可以修改 属性描述对象** 如果为 false  他的  `writable`、`enumerable`和`configurable`都不能被修改了。
  *  `value`属性的情况比较特殊。只要`writable`和`configurable`有一个为`true`，就允许改动`value`。
  *  `writable`为`false`时，直接对目标属性赋值，不报错，但不会成功。
* Enumerable
  * **Indicates if the property will be returned in a for-in loop**. By default,

    this is true for all properties defined directly on an object

  * 如果是 false， 下面3个操作 都不会取到该属性
    * for..in循环
    * Object.keys方法
    * JSON.stringify方法
* Writable
  * **Indicates if the property’s value can be changed**. By default, this is true for

    all properties defined directly on an object
* Value
  * **Contains the actual data value for the property**. This is the location from which

    the property’s value is read and the location to which new values are saved

```text
let person = {
 name: "Nicholas"
};

```

* Here, the property called name is created and a value of "Nicholas" is assigned. That means

  \[\[Value\]\] is set to "Nicholas", and any changes to that value are stored in this location.

To change any of the default property attributes you must use the **`Object.defineProperty()`**

```text
let person = {
  name:'boyan'
};
console.log(person);//boyan
Object.defineProperty(person, "name", {
 writable: false,
 value: "Nicholas"
});
console.log(person.name); // "Nicholas"
person.name = "Greg";
console.log(person.name); // "Nicholas"
```

* . This method accepts three arguments: the object on which the property should be added or

  modified, the name of the property, and a descriptor object. The properties on the descriptor object

  match the attribute names: configurable, enumerable, writable, and value. You can set one or all

  of these values to change the corresponding attribute values

* **Once setting configurable to false means that the property cannot be removed from the object. It can not changed configurable any more**

### **Accessor properties**

* Accessor properties do not contain a data value. Instead, they contain a combination of a getter function and a setter function \(though both are not necessary\)。
* Accessor properties have four attributes:
  * Configurable
  * Enumerable
  * Get—The function to call when the property is read from. The default value is undefined
  * Set—The function to call when the property is written to. The default value is **undefined**

```text
// Define object with pseudo-private member 'year_'
// and public member 'edition'
let book = {
 year_: 2017,
 edition: 1
};
console.log(book.year);  // undefined
Object.defineProperty(book, "year", {
 get() {
 return this.year_;
 },
 set(newValue) {
 if (newValue > 2017) {
 this.year_ = newValue;
 this.edition += newValue - 2017;
 }
 }
});
book.year = 2018;
console.log(book.edition); // 2
console.log(book.year); // 2018


// EG 2

var obj = Object.defineProperty({}, 'p', {
  get: function () {
    return 'getter';
  },
  set: function (value) {
    console.log('setter: ' + value);
  }
});

obj.p // "getter"
obj.p = 123 // "setter: 123"

```

### Defining multiple properties

```text
let book = {};
Object.defineProperties(book, {
 year_: {
 value: 2017
 },

 edition: {
 value: 1
 },

 year: {
 get() {
 return this.year_;
 },

 set(newValue) {
 if (newValue > 2017) {
 this.year_ = newValue;
 this.edition += newValue - 2017;
 }
 }
 }
});
```

### Reading Property Attributes

```text
let book = {};
Object.defineProperties(book, {
 year_: {
 value: 2017
 },

 edition: {
 value: 1
 },

 year: {
 get: function() {
 return this.year_;
 },

 set: function(newValue){
 if (newValue > 2017) {
 this.year_ = newValue;
 this.edition += newValue - 2017;
 }
 }
 }
});

let descriptor = Object.getOwnPropertyDescriptor(book, "year_");
console.log(descriptor.value); // 2017
console.log(descriptor.configurable); // false
console.log(typeof descriptor.get); // "undefined"
let descriptor = Object.getOwnPropertyDescriptor(book, "year");
console.log(descriptor.value); // undefined
console.log(descriptor.enumerable); // false
console.log(typeof descriptor.get); // "function"




//New in ES2017
let book = {};
Object.defineProperties(book, {
 year_: {
 value: 2017
 },

 edition: {
 value: 1
 },

 year: {
 get: function() {
 return this.year_;
 },

 set: function(newValue){
 if (newValue > 2017) {
 this.year_ = newValue;
 this.edition += newValue - 2017;
 }
 }
 }
});
console.log(Object.getOwnPropertyDescriptors(book));
// {
// edition: {
// configurable: false,
// enumerable: false,
// value: 1,
// writable: false
// },
// year: {
// configurable: false,
// enumerable: false,
// get: f(),
// set: f(newValue),
// },
// year_: {
// configurable: false,
// enumerable: false,
// value: 2019,
// writable: false
// }
// }
```

* Use  `Object.  getOwnPropertyDescriptor()`
* This method accepts two arguments: the object on which   the property resides and the name of the property whose descriptor should be retrieved.
* In ES2017, This   method effectively performs on Object.getOwnPropertyDescriptor\(\) on all own properties and   returns them in a new object

### 对象的拷贝 <a id="&#x5BF9;&#x8C61;&#x7684;&#x62F7;&#x8D1D;"></a>

```text
// 有问题的方法 
var extend = function (to, from) {
  for (var property in from) {
    to[property] = from[property];
  }

  return to;
}

extend({}, {
  a: 1
})
// {a: 1}

// 问题 复现
extend({}, {
  get a() { return 1 }
})
// {a: 1}

// 正确 的对象拷贝方法 方法1

var extend = function (to, from) {
  for (var property in from) {
    if (!from.hasOwnProperty(property)) continue;
    Object.defineProperty(
      to,
      property,
      Object.getOwnPropertyDescriptor(from, property)
    );
  }

  return to;
}

let a = {
  name:'boyan',
  age:10,
  hello : function(){
   console.log('hello');
  }
}


a.hello();

let b = extend({},  a)
console.log(b);
// [object Object] {
//   age: 10,
//   hello: function(){
//    window.runnerWindow.proxyConsole.log('hello');
//   },
//   name: "boyan"
// }


// 方法二
function copyObject(orig) {
  return Object.create(
    Object.getPrototypeOf(orig),
    Object.getOwnPropertyDescriptors(orig)
  );
}


```

* 上面这个方法问题在于， 遇到 accessor 属性时候 ， 只会拷贝数值
* 方法二

### Merging Objects:

```text
let dest, src, result;

dest = {};
src = { id: 'src' };
result = Object.assign(dest, src);
// Object.assign mutates the destination object
// and also returns that object after exiting.
console.log(dest === result); // true
console.log(dest !== src); // true
dest.id ='heyya';

console.log(result); // { id: heyya }
console.log(dest); // { id: heya }
console.log(src); // { id: src}



/**
 * Object references
 */
dest = {};
src = { a: {} };
Object.assign(dest, src);
// Shallow property copies means only object references copied.
console.log(dest); // { a :{} }
console.log(dest.a === src.a); // true 
```

* **`Object.assign()`** method.
* `Object.assign(target, ...sources)`
* It take one destination , and one or many sources, If we modify destination, result would change as well, but it would not affect source object if we do not modify object, since **Object.assign\(\) only perform shallow copy  from each source object.**
* If multiple source objects have the same property defined, the **last one to b**e copied will be the ultimate value
* 已经做过的copy出错后也不会 重置，所以就会有 partially copy
* 
### 创建Object的多种方法

```text
// Method 1 
let mlt =  new Object();
mlt.meat = [];
mlt.vegetable = [];


// Method 2
let mlt ={
    meat: [],
    vegetable:[]
}

// Method 3 factory pattern
// 这种方式  instaceof 没链接
function createMlt (meat,vegetable,ingredient){
    let obj =  new Object();
    obj.meat = meat;
    obj.vegetable = vegetable;
    obj.ingredient = ingredient;
    return obj;
}

let mlt1 = createMlt(meat1,vegetable1,ingredient1);

// Method 4 构造函数

function Mlt (meat,vegetable,ingredient){
    
    this.meat = meat;
    this.vegetable = vegetable;
    this.ingredient = ingredient;
}
let mlt1 = new Mlt(meat1,vegetable1,ingredient1);


// Method 5 Object create
let mlt1 =  Object.create(obj);

var obj1 = Object.create({});
var obj2 = Object.create(Object.prototype);
var obj3 = new Object();

var obj = Object.create(null);

obj.valueOf()
// TypeError: Object [object Object] has no method 'valueOf'


// class 
class MLT {
    constructor(meat,vegetable,ingredient){
        this.meat = meat;
        this.vegetable = vegetable;
        this.ingredient = ingredient;
    }
}
```

* Object.creat\(\)
  * 这些都是等价的
  *  如果想要生成一个不继承任何属性（比如没有`toString()`和`valueOf()`方法）的对象，可以将`Object.create()`的参数设为`null`。
  * 你修改对象属性也会修改原型的属性

## Object creation

Although using the Object constructor or an object literal are convenient ways to create single objects, there is an obvious downside: Creating multiple objects with the same interface requires a lot of code duplication.

### The factory pattern:

```text
function createPerson(name, age, job) {
 let o = new Object();
 o.name = name;
 o.age = age;
 o.job = job;
 o.sayName = function() {
 console.log(this.name);
 };
 return o;
}

let person1 = createPerson("Nicholas", 29, "Software Engineer");
let person2 = createPerson("Greg", 27, "Doctor");
```

* The function can be called any number of times with different arguments and will still return an object that has three properties and one method. Though this solved the problem of creating multiple similar objects, the **factory pattern didn’t address the issue of object identification** \(what type of object an object is\)

### The Function Constructor Pattern:

```text
function Person(name, age, job){
 this.name = name;
 this.age = age;
 this.job = job;
 this.sayName = function() {
 console.log(this.name);
 };
}

let person1 = new Person("Nicholas", 29, "Software Engineer");
let person2 = new Person("Greg", 27, "Doctor");
person1.sayName(); // Nicholas
person2.sayName(); // Greg

//  Or this way
let Person = function(name, age, job) {
 this.name = name;
 this.age = age;
 this.job = job;
 this.sayName = function() {
 console.log(this.name);
 };
}
```

* define custom constructors, in the form   of a function, that define properties and methods for your own type of object.
* `person1 instanceof Person`
* The major downside to constructors is that methods are created once for each instance.
* To solve the downside, we use prototype, , 在底下解释了

### Prototype pattern 

## New 命令

### new 命令的原理

英文：

* A new object is created in memory
* The new object’s internal \[\[Prototype\]\] pointer is assigned to the constructor’s prototype property
* The this value of the constructor is assigned to the new object \(so this points to the

  new object\).

* The code inside the constructor is executed \(adds properties to the new object\).
* If the constructor function returns a non-null object, that object is returned. Otherwise, the

  new object that was just created is returned.

中文:

* 创建一个空对象，作为将要返回的对象实例。
* 将这个空对象的原型，指向构造函数的prototype属性。
* 将这个空对象赋值给函数内部的this关键字
* 开始执行构造函数内部的代码。

也就是说，构造函数内部，this指的是一个新生成的空对象，所有针对this的操作，都会发生在这个空对象上。构造函数之所以叫“构造函数”，就是说这个函数的目的，就是操作一个空对象（即this对象），将其“构造”为需要的样子

```text
var Vehicle = function () {
  this.price = 1000;
  return 1000;
};

(new Vehicle()) === 1000
// false

var Vehicle = function (){
  this.price = 1000;
  return { price: 2000 };
};

(new Vehicle()).price
// 2000,因为不是返还 this返还的是别的 object


//如果对普通函数（内部没有this关键字的函数）使用new
function getMessage() {
  return 'this is a message';
}

var msg = new getMessage();

msg // {}
typeof msg // "object"
```

* **如果构造函数内部有return语句，而且return后面跟着一个对象，new命令会返回return语句指定的对象；否则，就会不管return语句，返回this对象**。
*  上面代码中，构造函数`Vehicle`的`return`语句返回一个数值。这时，`new`命令就会忽略这个`return`语句，返回“构造”后的`this`对象。
*  但是，如果`return`语句返回的是一个跟`this`无关的新对象，`new`命令会返回这个新对象，而不是`this`对象。这一点需要特别引起注意。
*  另一方面，如果对普通函数（内部没有`this`关键字的函数）使用`new`命令，则会返回一个空对象。

### new 命令简化的内部流程

```text
function _new(/* 构造函数 */ constructor, /* 构造函数参数 */ params) {
  // 将 arguments 对象转为数组
  var args = [].slice.call(arguments);
  // 取出构造函数
  var constructor = args.shift();
  // 创建一个空对象，继承构造函数的 prototype 属性
  var context = Object.create(constructor.prototype);
  // 执行构造函数
  var result = constructor.apply(context, args);
  // 如果返回结果是对象，就直接返回，否则返回 context 对象
  return (typeof result === 'object' && result != null) ? result : context;
}

// 实例
var actor = _new(Person, '张三', 28);
```

## 原型&原型链（prototype & prototype chain）

![](../.gitbook/assets/image%20%28115%29.png)

![](../.gitbook/assets/image%20%28111%29.png)



![](../.gitbook/assets/image%20%2831%29.png)

```text
function Person() {}
/**
 * Upon declaration, the constructor function already
 * has a prototype object associated with it:
 */
console.log(typeof Person.prototype); //Object
console.log(Person.prototype);
// {
// constructor: f Person(),
// __proto__: Object
// }

console.log(Person.prototype.constructor === Person); // true


/**
 * An instance is linked to the prototype through __proto__, which
 * is the literal manifestation of the [[Prototype]] hidden property.
 *
 * A constructor is linked to the prototype through the constructor property.
 *
 * An instance has no direct link to the constructor, only through the prototype.
 */
console.log(person1.__proto__ === Person.prototype); // true
conosle.log(person1.__proto__.constructor === Person); // true
```

`person1.__proto__ === Person.prototype  
person1.__proto__.constructor === Person.prototype.constructor === Person  
person1.constructor === Person`

 1️⃣`__proto__`和`constructor`是**对象**独有的。2️⃣`prototype`属性是**函数**独有的；

 `__proto__`属性既不能被 `for in` 遍历出来，也不能被 `Object.keys(obj)` 查找出来。

![](../.gitbook/assets/image%20%28118%29.png)

### prototype原型

![](../.gitbook/assets/image%20%28117%29.png)

```text
function f() {}
typeof f.prototype // "object"

// EG 1
function Animal(name) {
  this.name = name;
}
Animal.prototype.color = 'white';

var cat1 = new Animal('大毛');
var cat2 = new Animal('二毛');

cat1.color // 'white'
cat2.color // 'white'

//只要修改原型对象，变动就立刻会体现在所有实例对象上。
Animal.prototype.color = 'yellow';

cat1.color // "yellow"
cat2.color // "yellow"
```

* **每一个对象都会从原型"继承"属性。** ``原型对象的所有属性和方法，都能被实例对象共享。也就是说，如果属性和方法定义在原型上，那么所有实例对象就能共享，不仅节省了内存，还体现了实例对象之间的联系。
*  每个函数都有一个`prototype`属性，指向一个对象。
* EG 1 
  * 生成实例的时候，该属性会自动成为实例对象的原型
  *  只要修改原型对象，变动就立刻会体现在**所有**实例对象上。
  * 当实例对象本身没有某个属性或方法的时候，它会到原型对象去寻找该属性或方法

### \_\__proto\_\__

_**所有函数对象的 \_\_proto\_\_ 都指向 Function.prototype，它是一个空函数（Empty function）**_

_**对象\_\_proto\_\_属性的值就是它所对应的原型对象：**_

![](../.gitbook/assets/image%20%28116%29.png)

### 原型链

它的作用就是当你在访问一个对象属性的时候，如果该对象内部不存在这个属性，那么就回去它的\_\_proto\_\_属性所指向的对象（父类对象）上查找

```text
Object.getPrototypeOf(Object.prototype)
// NULL

// ===== EG1 =====
var MyArray = function () {};

MyArray.prototype = new Array();
MyArray.prototype.constructor = MyArray;

var mine = new MyArray();
mine.push(1, 2, 3);
mine.length // 3
mine instanceof Array // true
```

* 所有对象都有自己的原型对象（prototype）。一方面，任何一个对象，都可以充当其他对象的原型；另一方面，由于原型对象也是对象，所以它也有自己的原型。因此，就会形成一个“原型链”（prototype chain）：对象到原型，再到原型的原型……
*  所有对象的原型最终都可以上溯到`Object.prototype`
*  即`Object`构造函数的`prototype`属性。也就是说，所有对象都继承了`Object.prototype`的属性
*  `object.prototype`的原型是`null  ,` **原型链的尽头就是`null`。**

### constructor 属性

**`prototype`对象有一个`constructor`属性**，默认指向`prototype`对象所在的构造函数\(function\)

当获取 person.constructor 时，其实 person 中并没有 constructor 属性,当不能读取到constructor 属性时，会从 person 的原型也就是 Person.prototype 中读取，正好原型中有该属性，所以：

![](../.gitbook/assets/image%20%28119%29.png)

```text
function P() {}
P.prototype.constructor === P // true
var p = new P();
p.constructor === P // true
p.constructor === P.prototype.constructor // true
p.hasOwnProperty('constructor') // false


//  查看出是哪一个构造函数产生的
function F() {};
var f = new F();

f.constructor === F // true
f.constructor === RegExp // false

// 如果修改了原型对象，一般会同时修改constructor属性
function Person(name) {
  this.name = name;
}

Person.prototype.constructor === Person // true
// 修改了 原型对象 但是没有更改 constructor
Person.prototype = {
  method: function () {}
};

Person.prototype.constructor === Person // false
Person.prototype.constructor === Object // true
```

*  **`f.constructor.name`**  可以得到 构造函数的名称。
*  由于`constructor`属性定义在`prototype`对象上面，意味着可以被所有实例对象继承。
*  `constructor`属性的作用是，可以得知某个实例对象，到底是哪一个构造函数产生的。
* `constructor`属性表示**原型对象与构造函数之间的关联关系**，如果修改了原型对象，一般会同时修改`constructor`属性，防止引用的时候出错。 **修改原型对象时，一般要同时修改`constructor`属性的指向。**
  *  上面代码中，构造函数`Person`的原型对象改掉了，但是没有修改`constructor`属性，导致这个属性不再指向`Person`。由于`Person`的新原型是一个普通对象，而普通对象的`constructor`属性指向`Object`构造函数，导致`Person.prototype.constructor`变成了`Object`



## 

### Prototypes 继承

**1 显示原型继承**

* 就是指我们亲自将某个对象设置为另一个对象的原型
* 如下，通过调用 Object.setPrototypeOf 方法，我们将 obj\_a 设置为 obj\_b 的原型

```text
const obj_a = {a:1}
const obj_b = {b:2}

Object.setPrototypeOf(obj_b,obj_a);
```

* 还有一种是 Object.create\(\);直接继承另一个对象,Object.create，给我一个对象，它将作为我创建的新对象的原型

#### 2 隐式原型继承

* 创建空对象
* 设置该空对象的原型为另一个对象或者 null
* 填充该对象，增加属性或方法。

![](../.gitbook/assets/image%20%28114%29.png)



 3 .constructor 构造函数，在使用 new 关键字实例化时，会自动继承 constructor 的 prototype 对象，作为实例的原型。

4. 在 ES2015 中提供了 class 的风格，背后跟 constructor 工作方式一样，写起来更内聚一些。

## 继承

### _Prototype Chain 继承_

```text
function SuperType() {
 this.property = true;
}

SuperType.prototype.getSuperValue = function() {
 return this.property;
};

function SubType() {
 this.subproperty = false;
}

// inherit from SuperType
SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function () {
 return this.subproperty;
};

let instance = new SubType();
console.log(instance.getSuperValue()); // true
```

**缺点**

* that all instances get the same property values by default. 
* The main problem comes with their shared nature. The real problem occurs when a property contains a reference value.， 你更改其中一个 field 所有的 instance都会得到更改过后的值

### 借用构造函数\(constructor stealing\)

```text
function SuperType(name){
 this.name = name;
 this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function() {
 console.log(this.name);
};

function SubType(name, age){
 // inherit properties
 SuperType.call(this, name);

 this.age = age;
}

// inherit methods
SubType.prototype = new SuperType();

SubType.prototype.sayAge = function() {
 console.log(this.age);
};

let instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
console.log(instance1.colors); // "red,blue,green,black"
instance1.sayName(); // "Nicholas";
instance1.sayAge(); // 29

let instance2 = new SubType("Greg", 27);
console.log(instance2.colors); // "red,blue,green"
instance2.sayName(); // "Greg";
instance2.sayAge(); // 

```

**优点**

* 可以pass argument

#### 缺点

```text
function Cat(name, color) {
  this.name = name;
  this.color = color;
  this.meow = function () {
    console.log('喵喵');
  };
}

var cat1 = new Cat('大毛', '白色');
var cat2 = new Cat('二毛', '黑色');

cat1.meow === cat2.meow
// false
```

* 同一个构造函数的多个实例之间，无法共享属性，从而造成对系统资源的浪费
*  上面代码中，`cat1`和`cat2`是同一个构造函数的两个实例，它们都具有`meow`方法。由于`meow`方法是生成在每个实例对象上面，所以两个实例就生成了两次。也就是说，**每新建一个实例，就会新建一个`meow`方法**
* 解决办法 JavaScript 的原型对象（prototype）
* `SuperClass.call(this,id)`当然就是构造函数继承的核心语句了.由于父类中给this绑定属性，因此子类自然也就继承父类的共有属性。由于这种类型的继承没有涉及到原型`prototype`，所以父类的原型方法自然不会被子类继承，而如果想被子类继承，就必须放到构造函数中，这样创建出来的每一个实例都会单独的拥有一份而不能共用，这样就违背了代码复用的原则，所以综合上述两种，我们提出了组合式继承方法

### combination inheritance（原型链继承和经典继承双剑合璧。） <a id="&#x6784;&#x9020;&#x51FD;&#x6570;&#x7684;&#x7EE7;&#x627F;"></a>

```text
function Parent (name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Parent.prototype.getName = function () {
    console.log(this.name)
}

function Child (name, age) {

    Parent.call(this, name);//
    
    this.age = age;

}

Child.prototype = new Parent();

var child1 = new Child('kevin', '18');

child1.colors.push('black');

console.log(child1.name); // kevin
console.log(child1.age); // 18
console.log(child1.colors); // ["red", "blue", "green", "black"]

var child2 = new Child('daisy', '20');

console.log(child2.name); // daisy
console.log(child2.age); // 20
console.log(child2.colors); // ["red", "blue", "green"]
```

#### 缺点:

* 调用了 两次 父类constructor `Child.prototype = new Parent();   var child1 = new Child('kevin', '18');`

### 原型式继承Prototypal Inheritance

```text
function object(o) {
 function F() {}
 F.prototype = o;
 return new F();
}

let person = {
 name: "Nicholas",
 friends: ["Shelby", "Court", "Van"]
};

let anotherPerson = object(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

let yetAnotherPerson = object(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");

console.log(person.friends); // "Shelby,Court,Van,Rob,Barbie"

```

#### 缺点

包含引用类型的属性值始终都会共享相应的值，这点跟原型链继承一样。

* 注意：修改`person1.name`的值，`person2.name`的值并未发生改变，并不是因为`person1`和`person2`有独立的 name 值，而是因为`person1.name = 'person1'`，给`person1`添加了 name 值，并非修改了原型上的 name 值。

### 寄生式继承Parasitic Inheritance

创建一个仅用于封装继承过程的函数，该函数在内部以某种形式来做增强对象，最后返回对象。

```text
function createObj (o) {
    var clone = Object.create(o);
    clone.sayName = function () {
        console.log('hi');
    }
    return clone;
}
```

#### 缺点

* 跟借用构造函数模式一样，每次创建对象都会创建一遍方法。

### 寄生组合式继承

```text
function object(o) {
 function F() {}
 F.prototype = o;
 return new F();
}



function inheritPrototype(subType, superType) {
 let prototype = object(superType.prototype); // create object
 prototype.constructor = subType; // augment object
 subType.prototype = prototype; // assign object
}

function SuperType(name) {
 this.name = name;
 this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function() {
 console.log(this.name);
};

function SubType(name, age) {
 SuperType.call(this, name);

 this.age = age;
}

inheritPrototype(SubType, SuperType);

SubType.prototype.sayAge = function() {
 console.log(this.age);
}; 
```

**开发人员普遍认为寄生组合式继承是引用类型最理想的继承范式。**



## Classes

```text
//ES 5的 class 写法
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);

// ES 6 的class

class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}


// ===== class 就是一个 function type=====
class Point {
  // ...
}

typeof Point // "function"
Point === Point.prototype.constructor // true


// ===== EG 3 =====
class Point {
  constructor(){
    // ...
  }
}

Object.assign(Point.prototype, {
  toString(){},
  toValue(){}
});


// ===== EG 5 =====
var p1 = new Point(2,3);
var p2 = new Point(3,2);

p1.__proto__ === p2.__proto__
```

1. ES6 的class可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到
2. class 的所有 method 都再 prototype 上面 Point.prototype
3.  由于类的方法都定义在`prototype`对象上面，所以类的新方法可以添加在`prototype`对象上面。`Object.assign()`方法可以很方便地一次向类添加多个方法。
4. 类的内部所有定义的方法，都是不可枚举的（non-enumerable）
5. 与 ES5 一样，类的所有实例共享一个原型对象。
6. 类 不存在 hoist

### 静态方法 ， 静态 属性

```text
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function

class Foo {
  static bar() {
    this.baz();
  }
  static baz() {
    console.log('hello');
  }
  baz() {
    console.log('world');
  }
}

Foo.bar() // hello



// 静态属性
class MyClass {
  static myStaticProp = 42;

  constructor() {
    console.log(MyClass.myStaticProp); // 42
  }
}
```



* 该方法不会被实例继承，而是直接通过类来调用， static 里的 this 指的 是 lei 而不是 实例



### 私有方法 & 私有属性

```text
// ===== 方法 1 =====
class Widget {

  // 公有方法
  foo (baz) {
    this._bar(baz);
  }

  // 私有方法
  _bar(baz) {
    return this.snaf = baz;
  }

  // ...
}

// ===== 方法 2 =====
const bar = Symbol('bar');
const snaf = Symbol('snaf');

export default class myClass{

  // 公有方法
  foo(baz) {
    this[bar](baz);
  }

  // 私有方法
  [bar](baz) {
    return this[snaf] = baz;
  }

  // ...
};
```

1. 一种做法是在命名上加以区别，这种命名是不保险的，在类的外部，还是可以调用到这个方法
2.  还有一种方法是利用`Symbol`值的唯一性，将私有方法的名字命名为一个`Symbol`值。 一般情况下无法获取到它们，因此达到了私有方法和私有属性的效果。但是也不是绝对不行，`Reflect.ownKeys()`依然可以拿到它们。

### Class 的继承

```text
// ===== EG 1 =====
class Point {
}

class ColorPoint extends Point {
}

// ===== EG 2 =====
class Point { /* ... */ }

class ColorPoint extends Point {
  constructor() {
  }
}

let cp = new ColorPoint(); // ReferenceError
```

1.  Class 可以通过`extends`关键字实现继承
2.  子类必须在`constructor`方法中调用`super`方法，否则新建实例时会报错。这是因为子类自己的`this`对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用`super`方法，子类就得不到`this`对象。

### Class 的 prototype 属性和\_\_proto\_\_属性 <a id="&#x7C7B;&#x7684;-prototype-&#x5C5E;&#x6027;&#x548C;__proto__&#x5C5E;&#x6027;"></a>

```text
class A {
}

class B extends A {
}

B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true




Object.setPrototypeOf(B.prototype, A.prototype);
// 等同于
B.prototype.__proto__ = A.prototype;

Object.setPrototypeOf(B, A);
// 等同于
B.__proto__ = A;
```

*  子类的`__proto__`属性，表示构造函数的继承，总是指向父类
*  子类`prototype`属性的`__proto__`属性，表示方法的继承，总是指向父类的`prototype`属性
*  这两条继承链，可以这样理解：作为一个对象，子类（`B`）的原型（`__proto__`属性）是父类（`A`）；作为一个构造函数，子类（`B`）的原型对象（`prototype`属性）是父类的原型对象（`prototype`属性）的实例。



### 实例的 \_\_proto\_\_ 属性

```text
var p1 = new Point(2, 3);
var p2 = new ColorPoint(2, 3, 'red');

p2.__proto__ === p1.__proto__ // false
p2.__proto__.__proto__ === p1.__proto__ // true
```

*  子类实例的`__proto__`属性的`__proto__`属性，指向父类实例的`__proto__`属性。也就是说，子类的原型的原型，是父类的原型。

### Mixin 模式

```text
// 最基本的实现
const a = {
  a: 'a'
};
const b = {
  b: 'b'
};
const c = {...a, ...b}; // {a: 'a', b: 'b'}

// 完整实现
class Vehicle {}

let FooMixin = (Superclass) => class extends Superclass {
  foo() {
    console.log('foo');
  }
};
let BarMixin = (Superclass) => class extends Superclass {
  bar() {
    console.log('bar');
  }
};
let BazMixin = (Superclass) => class extends Superclass {
  baz() {
    console.log('baz');
  }
};

function mix(BaseClass, ...Mixins) {
  return Mixins.reduce((accumulator, current) => current(accumulator), BaseClass);
}

class Bus extends mix(Vehicle, FooMixin, BarMixin, BazMixin) {}

let b = new Bus();
b.foo();  // foo
b.bar();  // bar
b.baz();  // baz 
```

* Mixin 指的是多个对象合成一个新的对象，新对象具有各个组成成员的接口。它的最简单实现如下。
*  One strategy is to define “nestable” functions that accept a superclass as a parameter, define the mixin class as a subclass of the parameter, and return that class. These mixins can be chained inside each other and provided as the superclass expression



