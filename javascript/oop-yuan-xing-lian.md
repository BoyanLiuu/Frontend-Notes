# Objects, Classes, and Object-Oriented Programming

## Understanding Objects:

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



### Object 的静态方法 <a id="object-&#x7684;&#x9759;&#x6001;&#x65B9;&#x6CD5;"></a>

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
```

_**是指Object 对象自身的方法**_

**Object.prototype.keys\(obj\) // \["p1", "p2"\]   // this is not a function**

1. **`Object.keys()，Object.getOwnPropertyNames()`**   都用来遍历对象的属性 
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
   1. `Object.create()`：该方法可以指定原型对象和属性，返回一个新的对象。
   2. `Object.getPrototypeOf()`：获取对象的`Prototype`对象。



### Object 的实例方法 <a id="object-&#x7684;&#x5B9E;&#x4F8B;&#x65B9;&#x6CD5;"></a>

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

// 正确 的对象拷贝方法

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

```

* 上面这个方法问题在于， 遇到 accessor 属性时候 ， 只会拷贝数值

### Merging Objects:

* **`Object.assign()`** method.
* `Object.assign(target, ...sources)`
* It take one destination , and one or many so









