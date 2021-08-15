# Write your own JS API

{% embed url="https://juejin.cn/post/6873513007037546510" %}

```text

}
```

{% embed url="https://juejin.cn/post/6946022649768181774" %}

{% embed url="https://juejin.cn/post/6844903809206976520" %}

## JSON.stringfy

## JSON.parse



## instanceof 

```text
function myInstanceof(left, right) {
    //基本数据类型直接返回false
    if(typeof left !== 'object' || left === null) return false;
    //getProtypeOf是Object对象自带的一个方法，能够拿到参数的原型对象
    let proto = Object.getPrototypeOf(left);
    while(true) {
        //查找到尽头，还没找到
        if(proto == null) return false;
        //找到相同的原型对象
        if(proto == right.prototype) return true;
        proto = Object.getPrototypeOf(proto);
    }
}

console.log(myInstanceof("111", String)); //false
console.log(myInstanceof(new String("111"), String));//true
```

## Object.is\(\)

```text
function is(x, y) {
  
  if (x === y) {
    //运行到1/x === 1/y的时候x和y都为0，但是1/+0 = +Infinity， 1/-0 = -Infinity, 是不一样的
    //需要排除x,y 为0的情况
    return x !== 0 || y !== 0 || 1 / x === 1 / y;
  } else {
    //NaN===NaN是false,这是不对的，我们在这里做一个拦截，x !== x，那么一定是 NaN, y 同理
    //两个都是NaN的时候返回true
    return x !== x && y !== y;
  }
}
```



## Write your own Promise

{% embed url="https://juejin.cn/post/6844904004007247880\#heading-52" %}

## JS new 操作符

1. 创建了一个全新的对象。
2. 这个对象会被执行`[[Prototype]]`（也就是`__proto__`）链接。
3. 生成的新对象会绑定到函数调用的`this`。
4.  通过`new`创建的每个对象将最终被`[[Prototype]]`链接到这个函数的`prototype`对象上
5.  如果函数没有返回对象类型`Object`\(包含`Functoin`, `Array`, `Date`, `RegExg`, `Error`\)，那么`new`表达式中的函数调用会自动返回这个新的对象

`student.constructor === Student;` 

`Student.prototype.constructor === Student;`

```text
/**
 * 模拟实现 new 操作符
 * @param  {Function} ctor [构造函数]
 * @return {Object|Function|Regex|Date|Error}      [返回结果]
 */
function newOperator(ctor){
    if(typeof ctor !== 'function'){
      throw 'newOperator function the first param must be a function';
    }
    // ES6 new.target 是指向构造函数
    newOperator.target = ctor;
    // 1.创建一个全新的对象，
    // 2.并且执行[[Prototype]]链接
    // 4.通过`new`创建的每个对象将最终被`[[Prototype]]`链接到这个函数的`prototype`对象上。
    var newObj = Object.create(ctor.prototype);
    // ES5 arguments转成数组 当然也可以用ES6 [...arguments], Aarry.from(arguments);
    // 除去ctor构造函数的其余参数
    var argsArr = [].slice.call(arguments, 1);
    // 3.生成的新对象会绑定到函数调用的`this`。
    // 获取到ctor函数返回结果
    var ctorReturnResult = ctor.apply(newObj, argsArr);
    // 小结4 中这些类型中合并起来只有Object和Function两种类型 typeof null 也是'object'所以要不等于null，排除null
    var isObject = typeof ctorReturnResult === 'object' && ctorReturnResult !== null;
    var isFunction = typeof ctorReturnResult === 'function';
    if(isObject || isFunction){
        return ctorReturnResult;
    }
    // 5.如果函数没有返回对象类型`Object`(包含`Functoin`, `Array`, `Date`, `RegExg`, `Error`)，那么`new`表达式中的函数调用会自动返回这个新的对象。
    return newObj;
}



// 另外一种
function objectFactory() {
    //创建一个新的 object
    var obj = new Object(),
    //得到 constructor 
    Constructor = [].shift.call(arguments);
    //这个对象会被执行[[Prototype]]（也就是__proto__）链接。
    obj.__proto__ = Constructor.prototype;
    //使用 apply，改变构造函数 this 的指向到新建的对象，
    //这样 obj 就可以访问到构造函数中的属性
    var ret = Constructor.apply(obj, arguments);
    //确保构造器总是返回一个对象
    return typeof ret === 'object' ? ret : obj;

};
```

## Apply & Call & Bind

### apply\(\)

func.apply\(thisArg, \[argsArray\]\)

**thisArg:**  可选的。在 `func` 函数运行时使用的 `this` 值。请注意，`this`可能不是该方法看到的实际值：如果这个函数处于**非严格模式**下，则指定为 `null` 或 `undefined` 时会自动替换为指向全局对象，原始值会被包装

**argsArray:**  可选的。一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 `func` 函数。如果该参数的值为 `null` 或 `undefined`，则表示不需要传入任何参数

我们只需要模拟实现apply，call可以根据参数个数都放在一个数组中，给到apply即可。

ES5规范

Function.prototype.apply \(thisArg, argArray\)

1.  如果 `IsCallable(func)` 是 `false`, 则抛出一个 `TypeError` 异常。
2.  如果 `argArray` 是 `null` 或 `undefined`, 则返回提供 `thisArg` 作为 `this` 值并以空参数列表调用 `func` 的 `[[Call]]` 内部方法的结果。
3.  返回提供 `thisArg` 作为 `this` 值并以空参数列表调用 `func` 的 `[[Call]]` 内部方法的结果。
4.  如果 `Type(argArray)` 不是 `Object`, 则抛出一个 `TypeError` 异常。
5. 略
6. 略
7. 略
8. 略
9. 提供 thisArg 作为 this 值并以 argList 作为参数列表，调用 func 的 \[\[Call\]\] 内部方法，返回结果。

 在外面传入的 `thisArg` 值会修改并成为 `this` 值。`thisArg` 是 `undefined` 或 `null` 时它会被替换成全局对象，所有其他值会被应用 `ToObject` 并将结果作为 `this` 值，这是第三版引入的更改。

返还数值：The result of calling the function with the specified this value and arguments.

```text


Function.prototype.apply2 = function (context, arr) {


    // 1.如果 `IsCallable(func)` 是 `false`, 则抛出一个 `TypeError` 异常。
    if(typeof this !== 'function'){
        throw new TypeError(this + ' is not a function');
    }
    // 2.如果 argArray 是 null 或 undefined, 则
    // 返回提供 thisArg 作为 this 值并以空参数列表调用 func 的 [[Call]] 内部方法的结果。
    if(typeof arr === 'undefined' || arr === null){
        arr = [];
    }
    // 3.如果 Type(argArray) 不是 Object, 则抛出一个 TypeError 异常 .
    if(arr !== new Object(arr)){
        throw new TypeError('CreateListFromArrayLike called on non-object');
    }

    var context = Object(context) || window;
    context.fn = this;

    var result;
        var args = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            args.push('arr[' + i + ']');
        }
        result = eval('context.fn(' + args + ')')


    delete context.fn
    return result;
}
```





### call\(\) 

`fun.call(thisArg, arg1, arg2, ...)`

thisArg 值为原始值\(数字，字符串，布尔值\)的this会指向该原始值的自动包装对象

 返回值是你调用的方法的返回值，若该方法没有返回值，则返回`undefined`。

**第一步 更改 call 的this 指向**

```text
var foo = {
    value: 1,
    bar: function() {
        console.log(this.value)
    }
};

foo.bar(); // 1
```

再对象上面额外添加一个属性，然后 执行这个函数 然后 删除该函数

```text
// 第一版
Function.prototype.call2 = function(context) {
    // 首先要获取调用call的函数，用this可以获取
    context.fn = this;
    // 这个时候 执行function 内容， 这个时候 this 就更改了 
    // 变成了 foo
    context.fn();
    delete context.fn;
}

// 测试一下
var foo = {
    value: 1
};

function bar() {
    console.log(this.value);
}

bar.call2(foo); // 1
```

**第二步给定参数**

```text
// 需要达成的
var foo = {
    value: 1
};

function bar(name, age) {
    console.log(name)
    console.log(age)
    console.log(this.value);
}

bar.call(foo, 'kevin', 18);
// kevin
// 18
// 1

```

* 从arguments 对象中取值， 取出第二个到最后一个字1参数 然后 放到一个数组里面

```text
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }

```

* 我们接下来要把这个参数数组 放到要执行的 funciton 参数里面

![](../.gitbook/assets/image%20%2896%29.png)

因为数组和字符串相加时候 数组会调用 toString\(\) 方法

```text
  // 字符串拼接
  var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }
    // args: ["arguments[1]", "arguments[2]"]
    eval('context.fn(' + args +')');
    
    
    
    
    // 或者是 es6 的spread operator
  var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push(arguments[i]);
    }
  
  context.fn(...args);
    
```

```text
// 第二版
Function.prototype.call2 = function(context) {
    context.fn = this;
    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }
    eval('context.fn(' + args +')');
    delete context.fn;
}

// 测试一下
var foo = {
    value: 1
};

function bar(name, age) {
    console.log(name)
    console.log(age)
    console.log(this.value);
}

bar.call2(foo, 'kevin', 18); 
// kevin
// 18
// 1
```

#### 第三步

* this 参数可以传 null，当为 null 的时候，视为指向 window
* 函数是可以有返回值的！

```text
// 最终版
Function.prototype.call2 = function (context) {

    var context = context || window;
    context.fn = this;

    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }

    var result = eval('context.fn(' + args +')');

    delete context.fn
    return result;
}

// 测试一下
var value = 2;

var obj = {
    value: 1
}

function bar(name, age) {
    console.log(this.value);
    return {
        value: this.value,
        name: name,
        age: age
    }
}

bar.call(null); // 2

console.log(bar.call2(obj, 'kevin', 18));
// 1
// Object {
//    value: 1,
//    name: 'kevin',
//    age: 18
// }


```

#### 目前版本中出现的问题

 1： `fn` 同名覆盖问题，`thisArg`对象上有`fn`，那就被覆盖了然后被删除了。

*  解决方案一：采用ES6 Sybmol\(\) 独一无二的。可以本来就是模拟ES3的方法。如果面试官不允许用呢
* 解决方案二：自己用Math.random\(\)模拟实现独一无二的 UUID key，面试时可以直接用生成时间戳即可。

```text
function generateGuid() {
  var result, i, j;
  result = '';
  for(j=0; j<32; j++) {
    if( j == 8 || j == 12 || j == 16 || j == 20)
      result = result + '-';
    i = Math.floor(Math.random()*16).toString(16).toUpperCase();
    result = result + i;
  }
  return result;
}
//"CB30BDD1-5985-C0EC-AB8E-11010D39A983"
console.log(generateGuid());
```

2： 万一不让使用 eval的话， 我们可以使用 new Function 来生产执行函数

```text
简单例子：
var sum = new Function('a', 'b', 'return a + b');
console.log(sum(2, 6));
```

### bind\(\)

{% embed url="https://github.com/mqyqingfeng/Blog/issues/12" %}

{% embed url="https://juejin.cn/post/6844903718089916429\#heading-6" %}

 The **`bind()`** method creates a new function that, when called, has its `this` keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

* `bind`是`Functoin`原型链中`Function.prototype`的一个属性，每个函数都可以调用它。
* `bind`本身是一个函数名为`bind`的函数，返回值也是函数，函数名是`bound`。（打出来就是`bound加上一个空格`）
* 调用`bind`的函数中的`this`指向`bind()`函数的第一个参数
*  传给`bind()`的其他参数接收处理了，`bind()`之后返回的函数的参数也接收处理了，也就是说合并处理了
*  并且`bind()`后的`name`为`bound + 空格 + 调用bind的函数名`。如果是匿名函数则是`bound + 空格`
*  `bind`后的返回值函数，执行后返回值是原函数（`original`）的返回值

  
 

 







_**第一版**_

```text
// 第一版 修改this指向，合并参数
Function.prototype.bindFn = function bind(thisArg){
    if(typeof this !== 'function'){
        throw new TypeError(this + 'must be a function');
    }
    // 存储函数本身
    var self = this;
    // 去除thisArg的其他参数 转成数组
    var args = Array.prototype.slice.call(arguments, 1);
    var bound = function(){
        // bind返回的函数 的参数转成数组
        var boundArgs = Array.prototype.slice.call(arguments);
        // apply修改this指向，把两个函数的参数合并传给self函数，并执行self函数，返回执行结果
        return self.apply(thisArg, args.concat(boundArgs));
    }
    return bound;
}
// 测试
var obj = {
    name: '若川',
};
function original(a, b){
    console.log(this.name);
    console.log([a, b]);
}
var bound = original.bindFn(obj, 1);
bound(2); // '若川', [1, 2]


```

#### 但我们知道函数是可以用new来实例化的。那么bind\(\)返回值函数会是什么表现呢?

从例子种可以看出this指向了**new bound\(\)生成的新对象。**

例子

```text
var obj = {
    name: '若川',
};
function original(a, b){
    console.log('this', this); // original {}
    console.log('typeof this', typeof this); // object
    this.name = b;
    console.log('name', this.name); // 2
    console.log('this', this);  // original {name: 2}
    console.log([a, b]); // 1, 2
}
var bound = original.bind(obj, 1);
var newBoundResult = new bound(2);
console.log(newBoundResult, 'newBoundResult'); // original {name: 2}






```

#### 我们目前的code 实例和原型对象 没有联系

#### 所以相当于new调用时，bind的返回值函数bound内部要模拟实现new实现的操作

**最终版 方案一**

```text

Function.prototype.bindFn = function bind(thisArg){
    if(typeof this !== 'function'){
        throw new TypeError(this + ' must be a function');
    }
    // 存储调用bind的函数本身
    var self = this;
    // 去除thisArg的其他参数 转成数组
    var args = [].slice.call(arguments, 1);
    var bound = function(){
        // bind返回的函数 的参数转成数组
        var boundArgs = [].slice.call(arguments);
        var finalArgs = args.concat(boundArgs);
        // new 调用时，其实this instanceof bound判断也不是很准确。es6 new.target就是解决这一问题的。
        if(this instanceof bound){
            // 这里是实现上文描述的 new 的第 1, 2, 4 步
            // 1.创建一个全新的对象
            // 2.并且执行[[Prototype]]链接
            // 4.通过`new`创建的每个对象将最终被`[[Prototype]]`链接到这个函数的`prototype`对象上。
            // self可能是ES6的箭头函数，没有prototype，所以就没必要再指向做prototype操作。
            if(self.prototype){
                // ES5 提供的方案 Object.create()
                // bound.prototype = Object.create(self.prototype);
                // 但 既然是模拟ES5的bind，那浏览器也基本没有实现Object.create()
                // 所以采用 MDN ployfill方案 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create
                function Empty(){}
                Empty.prototype = self.prototype;
                bound.prototype = new Empty();
            }
            // 这里是实现上文描述的 new 的第 3 步
            // 3.生成的新对象会绑定到函数调用的`this`。
            var result = self.apply(this, finalArgs);
            // 这里是实现上文描述的 new 的第 5 步
            // 5.如果函数没有返回对象类型`Object`(包含`Functoin`, `Array`, `Date`, `RegExg`, `Error`)，
            // 那么`new`表达式中的函数调用会自动返回这个新的对象。
            var isObject = typeof result === 'object' && result !== null;
            var isFunction = typeof result === 'function';
            if(isObject || isFunction){
                return result;
            }
            return this;
        }
        else{
            // apply修改this指向，把两个函数的参数合并传给self函数，并执行self函数，返回执行结果
            return self.apply(thisArg, finalArgs);
        }
    };
    return bound;
}


```

#### 另外一个版本的最终版（看这个）

* `this instanceof fNOP`  用来判断是否使用了 new 构造函数，如果是 我们就需要把 this绑定到新实例上面
* 把fNop的 原型对象修改为this（person）的原型对象
* 再把 返还的函数对象 作为空函数fNop的实例进行串联

```text



Function.prototype.bind2 = function (context) {

    if (typeof this !== "function") {
      throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);

    var fNOP = function () {};

    var fBound = function () {
        var bindArgs = Array.prototype.slice.call(arguments);
        return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs));
    }

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();
    return fBound;
}
```

## Array API:

### splice\(\)



### slice\(\)

### map\(\)

### reduce\(\)

[https://juejin.cn/post/6844903986479251464\#heading-26](https://juejin.cn/post/6844903986479251464#heading-26)

### forEach\(\)



### flat\(\)

### from\(\)



## 

## 拷贝

### 浅拷贝

* 浅拷贝的意思就是只复制引用，而未复制真正的值。
* 其实就是遍历对象属性的问题

#### 浅拷贝实现

```text
// 1
function shallowClone(source) {
    var target = {};
    for(var i in source) {
        if (source.hasOwnProperty(i)) {
            target[i] = source[i];
        }
    }

    return target;
}

// 2 Object.assign()
var x = {
  a: 1,
  b: { f: { g: 1 } },
  c: [ 1, 2, 3 ]
};
var y = Object.assign({}, x);
console.log(y.b.f === x.b.f);     // true

```

#### 数组的浅拷贝

```text
var arr = ['old', 1, true, null, undefined];

var new_arr = arr.concat();

new_arr[0] = 'new';

console.log(arr) // ["old", 1, true, null, undefined]
console.log(new_arr) // ["new", 1, true, null, undefined]

```

### 深拷贝

### 基础方法 1：

```text
JSON.parse(JSON.stringify());
//问题 1
//拷贝a会出现系统栈溢出，因为出现了无限递归的情况。
let obj = {
  name:'boyan',
  age:15
}

obj.name = obj;
//"TypeError: Converting circular structure to JSON
JSON.parse(JSON.stringify(obj));

// 问题 3
let obj = {
  name:'boyan',
  age:15,
  getName: function() {
    return this.name;
  }
}

let objB = JSON.parse(JSON.stringify(obj));
console.log(objB.getName());
//"TypeError: objB.getName is not a function
```

* 这种方法使用较为简单，可以满足基本日常的深拷贝需求，而且能够处理JSON格式能表示的所有数据类型，但是有以下几个缺点：
  * 问题: 无法解决循环引用的问题。举个例子：
  * 无法拷贝一写特殊的对象，诸如 RegExp, Date, Set, Map等。
  * 无法拷贝**函数\(function\)**。
  * `undefined`、`任意函数`、`Symbol 值`，在序列化过程有两种不同的情况。若出现在非数组对象的属性值中，会被忽略；若出现在数组中，会转换成 `null`

### 基础方法 2，简易版本

*  就是对每一层的数据都实现一次 `创建对象->对象赋值` 的操作

一个简单的深拷贝就完成了，但是这个实现还存在很多问题。



*  没有对传入参数进行校验，传入 `null` 时应该返回 `null` 而不是 `{}`
* 对于对象的判断逻辑不严谨，因为 typeof null === 'object'
* 没有考虑数组的兼容

```text

// 木易杨
function cloneDeep1(source) {
    var target = {};
    for(var key in source) {
        if (Object.prototype.hasOwnProperty.call(source, key)) {
            if (typeof source[key] === 'object') {
                target[key] = cloneDeep1(source[key]); // 注意这里
            } else {
                target[key] = source[key];
            }
        }
    }
    return target;
}

// 使用上面测试用例测试一下
var b = cloneDeep1(a);
console.log(b);
// { 
//   name: 'muyiy', 
//   book: { title: 'You Don\'t Know JS', price: '45' }, 
//   a1: undefined,
//   a2: {},
//   a3: 123
// }




```

### 完善版本1， 拷贝数组

* 解决了数组兼容 并且 完成了 判断 object 的情况

```text
// 木易杨
typeof null //"object"
typeof {} //"object"
typeof [] //"object"
typeof function foo(){} //"function" (特殊情况)

function isObject(obj) {
	return typeof obj === 'object' && obj != null;
}


// 木易杨
function cloneDeep2(source) {

    if (!isObject(source)) return source; // 非对象返回自身
      
    var target = Array.isArray(source) ? [] : {};
    for(var key in source) {
        if (Object.prototype.hasOwnProperty.call(source, key)) {
            if (isObject(source[key])) {
                target[key] = cloneDeep2(source[key]); // 注意这里
            } else {
                target[key] = source[key];
            }
        }
    }
    return target;
}

// 使用上面测试用例测试一下
var b = cloneDeep2(a);
console.log(b);
// { 
//   name: 'muyiy', 
//   book: { title: 'You Don\'t Know JS', price: '45' },
//   a1: undefined,
//   a2: null,
//   a3: 123
// }
```



### 完善版本2，循环引用， 引用丢失

1. 解决循环应用：
   1. 创建一个Map。记录下已经拷贝过的对象，如果说已经拷贝过，那直接返回它行了。
   2. **这里有一个潜在的坑**： 就是map 上的 key 和 map 构成了强引用关系，这是相当危险的
      1. 被弱引用的对象可以在任何时候被回收，而对于强引用来说，只要这个强引用还在，那么对象无法被回收。拿上面的例子说，map 和 a一直是强引用的关系， 在程序结束之前，a 所占的内存空间一直**不会被释放**
      2. **解决办法：**让 map 的 key 和 map 构成弱引用即可。ES6给我们提供了这样的数据结构，它的名字叫WeakMap，它是一种特殊的Map, 其中的键是弱引用的。其键必须是对象，而值可以是任意的。`const deepClone = (target, map = new WeakMap())` 

```text
// 引用丢失问题

var obj1 = {};
var obj2 = {a: obj1, b: obj1};

obj2.a === obj2.b; 
// true

var obj3 = cloneDeep2(obj2);
obj3.a === obj3.b; 
// false
//上面的对象 obj2，obj2 的键值 a 和 b 同时引用了同一个对象 obj1
//，使用 cloneDeep2 进行深拷贝后就丢失了引用关系变成了两个不同的对象




function isObject(obj) {
	return typeof obj === 'object' && obj != null;
}

function cloneDeep3(source, hash = new WeakMap()) {

    if (!isObject(source)) return source; 
    if (hash.has(source)) return hash.get(source); // 新增代码，查哈希表
      
    var target = Array.isArray(source) ? [] : {};
    hash.set(source, target); // 新增代码，哈希表设值
    
    for(var key in source) {
        if (Object.prototype.hasOwnProperty.call(source, key)) {
            if (isObject(source[key])) {
                target[key] = cloneDeep3(source[key], hash); // 新增代码，传入哈希表
            } else {
                target[key] = source[key];
            }
        }
    }
    return target;
}


a.circleRef = a;

var b = cloneDeep3(a);
console.log(b);
// {
// 	name: "muyiy",
// 	a1: undefined,
//	a2: null,
// 	a3: 123,
// 	book: {title: "You Don't Know JS", price: "45"},
// 	circleRef: {name: "muyiy", book: {…}, a1: undefined, a2: null, a3: 123, …}
// }
```

### 完善版本3特殊对象



```text


// Solution 4
// 解决 特殊对象

const getType = obj => Object.prototype.toString.call(obj);

const isObject = (target) => (typeof target === 'object' || typeof target === 'function') && target !== null;

const canTraverse = {
  '[object Map]': true,
  '[object Set]': true,
  '[object Array]': true,
  '[object Object]': true,
  '[object Arguments]': true,
};
// 不可遍历的对象
const mapTag = '[object Map]';
const setTag = '[object Set]';
const boolTag = '[object Boolean]';
const numberTag = '[object Number]';
const stringTag = '[object String]';
const symbolTag = '[object Symbol]';
const dateTag = '[object Date]';
const errorTag = '[object Error]';
const regexpTag = '[object RegExp]';
const funcTag = '[object Function]';

const handleRegExp = (target) => {
  const { source, flags } = target;
  return new target.constructor(source, flags);
}

const handleFunc = (func) => {
  // 箭头函数直接返回自身
  if(!func.prototype) return func;
  const bodyReg = /(?<={)(.|\n)+(?=})/m;
  const paramReg = /(?<=\().+(?=\)\s+{)/;
  const funcString = func.toString();
  // 分别匹配 函数参数 和 函数体
  const param = paramReg.exec(funcString);
  const body = bodyReg.exec(funcString);
  if(!body) return null;
  if (param) {
    const paramArr = param[0].split(',');
    return new Function(...paramArr, body[0]);
  } else {
    return new Function(body[0]);
  }
}

const handleNotTraverse = (target, tag) => {
  const Ctor = target.constructor;
  switch(tag) {
    case boolTag:
      return new Object(Boolean.prototype.valueOf.call(target));
    case numberTag:
      return new Object(Number.prototype.valueOf.call(target));
    case stringTag:
      return new Object(String.prototype.valueOf.call(target));
    case symbolTag:
      return new Object(Symbol.prototype.valueOf.call(target));
    case errorTag: 
    case dateTag:
      return new Ctor(target);
    case regexpTag:
      return handleRegExp(target);
    case funcTag:
      return handleFunc(target);
    default:
      return new Ctor(target);
  }
}

const deepClone = (target, map = new WeakMap()) => {
  if(!isObject(target)) 
    return target;
  let type = getType(target);
  let cloneTarget;
  if(!canTraverse[type]) {
    // 处理不能遍历的对象
    return handleNotTraverse(target, type);
  }else {
    // 这波操作相当关键，可以保证对象的原型不丢失！
    let ctor = target.constructor;
    cloneTarget = new ctor();
  }

  if(map.get(target)) 
    return target;
  map.set(target, true);

  if(type === mapTag) {
    //处理Map
    target.forEach((item, key) => {
      cloneTarget.set(deepClone(key, map), deepClone(item, map));
    })
  }
  
  if(type === setTag) {
    //处理Set
    target.forEach(item => {
      cloneTarget.add(deepClone(item, map));
    })
  }

  // 处理数组和对象
  for (let prop in target) {
    if (target.hasOwnProperty(prop)) {
        cloneTarget[prop] = deepClone(target[prop], map);
    }
  }
  return cloneTarget;
}
```

1. 解决特殊对象：
   1. 对于特殊的对象，我们使用以下方式来鉴别:`Object.prototype.toString.call(obj);`
   2. 然后 分别处理 可继续遍历的， 和 不可遍历的对象
   3. 不可遍历对象 ， 不同对象有不同的处理





## 防抖\(debounce\)

![](../.gitbook/assets/image%20%28103%29.png)

* 输入框连续输入的请求次数控制
* 防止表单多次提交

{% embed url="https://jsfiddle.net/Boyanliuu/oznt71e4/25/" %}

### 没返回值版本

```text
const button  = document.querySelector('button');

function PayMoney(){
console.log('Paid!')
}


function debounce(func,delay){
	// 使用 closure 
	let timer;
  // 使用 高阶函数， high order function， 这样子 就只会在点击时候 
  // 才执行
	return function(){

  		let context = this;
      // 清楚上一个定义的延时
  		clearTimeout(timer);
      let args = arguments;
  		timer = setTimeout(function(){
      		//使用 apply 解决参数问题
          //保证this 指向正确
      		func.apply(context,arguments);
      }, delay);
  }
}


button.addEventListener('click',debounce(PayMoney,1000));


// 立刻执行
// 意思是触发事件后函数会立即执行，然后 n 秒内不触发事件才能继续执行函数的效果。
// 我不希望非要等到事件停止触发后才执行，我希望立刻执行函数，
// 然后等到停止触发 n 秒后，才可以重新触发执行。

function debounce(func, wait, immediate) {

    var timeout;

    return function () {
        var context = this;
        var args = arguments;

        if (timeout) clearTimeout(timeout);
        if (immediate) {
            // 如果已经执行过，不再执行
            var callNow = !timeout;
            timeout = setTimeout(function(){
                timeout = null;
            }, wait)
            if (callNow) func.apply(context, args)
        }
        else {
            timeout = setTimeout(function(){
                func.apply(context, args)
            }, wait);
        }
    }
}

// 取消
// 我希望能取消 debounce 函数，比如说我 debounce 的时间间隔是 10 秒钟，
// immediate 为 true，这样的话，我只有等 10 秒后才能重新触发事件，现在我希望有一个按钮，
// 点击后，取消防抖，这样我再去触发，就可以又立刻执行


function debounce(func, wait, immediate) {

    var timeout, result;

    var debounced = function () {
        var context = this;
        var args = arguments;

        if (timeout) clearTimeout(timeout);
        if (immediate) {
            // 如果已经执行过，不再执行
            var callNow = !timeout;
            timeout = setTimeout(function(){
                timeout = null;
            }, wait)
            if (callNow) result = func.apply(context, args)
        }
        else {
            timeout = setTimeout(function(){
                func.apply(context, args)
            }, wait);
        }
        return result;
    };

    debounced.cancel = function() {
        clearTimeout(timeout);
        timeout = null;
    };

    return debounced;
}


```

### 有返回值的版本

```text
// 有返回值的版本， 使用 promise 处理
function debounce(method, wait, immediate) {
  let timeout, result
  let debounced = function(...args) {
    let context = this
    // 返回一个Promise，以便可以使用then或者Async/Await语法拿到原函数返回值
    return new Promise(resolve => {
      if (timeout) {
        clearTimeout(timeout)
      }
      if (immediate) {
        let callNow = !timeout
        timeout = setTimeout(() => {
          timeout = null
        }, wait)
        if (callNow) {
          result = method.apply(context, args)
          // 将原函数的返回值传给resolve
          resolve(result)
        }
      } else {
        timeout = setTimeout(() => {
          result = method.apply(context, args)
          // 将原函数的返回值传给resolve
          resolve(result)
        }, wait)
      }
    })
  }

  debounced.cancel = function() {
    clearTimeout(timeout)
    timeout = null
  }

  return debounced
}
// 使用方法一：在调用防抖后的函数时，使用then拿到原函数的返回值
function square(num) {
  return Math.pow(num, 2)
}

let debouncedFn = debounce(square, 1000, false)

window.addEventListener('resize', () => {
  debouncedFn(4).then(val => {
    console.log(`原函数的返回值为：${val}`)
  })
}, false)

// 使用方法二：调用防抖后的函数的外层函数使用Async/Await语法等待执行结果返回

function square(num) {
  return Math.pow(num, 2)
}

let debouncedFn = debounce(square, 1000, false)

window.addEventListener('resize', async () => {
  let val
  try {
    val = await debouncedFn(4)
  } catch (err) {
    console.error(err)
  }
  console.log(`原函数返回值为${val}`)
}, false)
```

## 节流\(throttle\)

![](../.gitbook/assets/image%20%28105%29.png)

* 如果我们需要统计用户滚动屏幕的行为来做出相应的网页反应我们就需要节流
* 因为如果用户不断进行滚动就会不断产生请求， 容易导致网络的堵塞
* 我们就可以在触发时间时候 马上处理任务 然后 设定时间间隔限制，在这段时间内不管用户如何进行滚动都忽视操作
* 在时间到了以后如果检测到用户有滚动行为再次执行任务 并且重新设置时间间隔

{% embed url="https://jsfiddle.net/Boyanliuu/ptg8xvej/13/" %}



例子： 更改页面尺寸大小时候 更改颜色

```text
function coloring(){
		let r = Math.floor(Math.random() * 255);
    let g = Math.floor(Math.random() * 255);
        
    let b = Math.floor(Math.random() * 255);
    
    document.body.style.background = `rgb(${r},${g},${b})`;
}

// 第一种方法 使用 set time out
function throttle(func,delay){
let timer;
	return function(){
  	let context = this;
    let args =  arguments;
  	// 如果 timer 被赋值了， 那就是在等待时间间隔内， 就不执行
  	if(timer){
    	return;
    }
  	timer = setTimeout(function(){
    	func.apply(context,args);
      timer = null;
    },delay);
  
  }
}


//第二种方法 使用 date 相减
function throttle2(func,delay){
let prev = 0;
	return function(){
  	let context = this;
    let args =  arguments;
    let now = new Date();
  	if(now - prev > delay){
			func.apply(context,args);
      prev = now;
    }
      
  }
}

window.addEventListener('resize',throttle2(coloring,2000));
```





## 事件总线 \| 发布订阅模式

## 柯里化

### es5 实现继承

异步并发数限制

异步串行 \| 异步并行

## 图片懒加载

* 滚动到地方才加载出来所对应的图片 鼠标滚动到才会触发

{% embed url="https://jsfiddle.net/Boyanliuu/7395k1rv/14/" %}



第一种方法 

![](../.gitbook/assets/image%20%28104%29.png)

```text
// 这个情况下 滚动会触发很多次
//即使图片已经加载了 还是回不断触发事件
//    <img data-src ="https://cdn.pixabay.com/photo/2018/01/05/02/47/fishing-3062034_960_720.jpg">
window.addEventListener('scroll', (e) =>{
		images.forEach(image =>{
    	const imageTop =  image.getBoundingClientRect().top;
      //图片可以开始加载了, html 使用了 data-src 这样子他就不会提前加载图片了
      if(imageTop < window.innerHeight){
      	const data_src = image.getAttribute('data-src');
        image.setAttribute('src',data_src);
      }
      console.log('触发')
      
    });
})
```



更好的 方法 使用 IntersectionObserver

* 前提条件是浏览器需要支持， 目标元素与可视窗口会产生交叉区域

![](../.gitbook/assets/image%20%28107%29.png)

```text
// 回调函数接受一个参数
const callback = (entries) =>{
		entries.forEach(entry =>{
    		//为true 证明 正在观察这个 element我们可以把它展示出来了
    		if(entry.isIntersecting){
        	const image = entry.target;
          const data_src = image.getAttribute('data-src');
        	image.setAttribute('src',data_src);
          // 如果已经加载了  就不再触发 观察了
          observer.unobserve(image);
          console.log('触发')
        
        }
    });

};

const observer =  new IntersectionObserver(callback);

// 每一个元素都需要被观察着
images.forEach(image =>{
	observer.observe(image);
});
```

### Promises/A+规范的promise

{% embed url="https://juejin.cn/post/6844903625769091079\#heading-0" %}



### 

### 







