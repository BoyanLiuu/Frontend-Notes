# JS 手写

{% embed url="https://juejin.cn/post/6873513007037546510" %}



{% embed url="https://juejin.cn/post/6946022649768181774" %}

{% embed url="https://juejin.cn/post/6844903809206976520" %}

## JSON.stringfy

## JSON.parse



## instanceof&#x20;

```
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

## Object.is()

```
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

{% embed url="https://juejin.cn/post/6844904004007247880#heading-52" %}

## JS new 操作符

1. 创建了一个全新的对象。
2. 这个对象会被执行`[[Prototype]]`（也就是`__proto__`）链接。
3. 生成的新对象会绑定到函数调用的`this`。
4. &#x20;通过`new`创建的每个对象将最终被`[[Prototype]]`链接到这个函数的`prototype`对象上
5. &#x20;如果函数没有返回对象类型`Object`(包含`Functoin`, `Array`, `Date`, `RegExg`, `Error`)，那么`new`表达式中的函数调用会自动返回这个新的对象

`student.constructor === Student;`&#x20;

`Student.prototype.constructor === Student;`

```
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

### apply()

func.apply(thisArg, \[argsArray])

**thisArg:** 可选的。在 `func` 函数运行时使用的 `this` 值。请注意，`this`可能不是该方法看到的实际值：如果这个函数处于**非严格模式**下，则指定为 `null` 或 `undefined` 时会自动替换为指向全局对象，原始值会被包装

**argsArray:**  可选的。一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 `func` 函数。如果该参数的值为 `null` 或 `undefined`，则表示不需要传入任何参数

我们只需要模拟实现apply，call可以根据参数个数都放在一个数组中，给到apply即可。

ES5规范

Function.prototype.apply (thisArg, argArray)

1. &#x20;如果 `IsCallable(func)` 是 `false`, 则抛出一个 `TypeError` 异常。
2. &#x20;如果 `argArray` 是 `null` 或 `undefined`, 则返回提供 `thisArg` 作为 `this` 值并以空参数列表调用 `func` 的 `[[Call]]` 内部方法的结果。
3. &#x20;返回提供 `thisArg` 作为 `this` 值并以空参数列表调用 `func` 的 `[[Call]]` 内部方法的结果。
4. &#x20;如果 `Type(argArray)` 不是 `Object`, 则抛出一个 `TypeError` 异常。
5. 略
6. 略
7. 略
8. 略
9. 提供 thisArg 作为 this 值并以 argList 作为参数列表，调用 func 的 \[\[Call]] 内部方法，返回结果。

&#x20;在外面传入的 `thisArg` 值会修改并成为 `this` 值。`thisArg` 是 `undefined` 或 `null` 时它会被替换成全局对象，所有其他值会被应用 `ToObject` 并将结果作为 `this` 值，这是第三版引入的更改。

返还数值：The result of calling the function with the specified this value and arguments.

```


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





### call()&#x20;

`fun.call(thisArg, arg1, arg2, ...)`

thisArg 值为原始值(数字，字符串，布尔值)的this会指向该原始值的自动包装对象

&#x20;返回值是你调用的方法的返回值，若该方法没有返回值，则返回`undefined`。

**第一步 更改 call 的this 指向**

```
var foo = {
    value: 1,
    bar: function() {
        console.log(this.value)
    }
};

foo.bar(); // 1
```

再对象上面额外添加一个属性，然后 执行这个函数 然后 删除该函数

```
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

```
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

```
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }

```

* 我们接下来要把这个参数数组 放到要执行的 funciton 参数里面

![](<../.gitbook/assets/image (96).png>)

因为数组和字符串相加时候 数组会调用 toString() 方法

```
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

```
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

```
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

&#x20;1： `fn` 同名覆盖问题，`thisArg`对象上有`fn`，那就被覆盖了然后被删除了。

* &#x20;解决方案一：采用ES6 Sybmol() 独一无二的。可以本来就是模拟ES3的方法。如果面试官不允许用呢
* 解决方案二：自己用Math.random()模拟实现独一无二的 UUID key，面试时可以直接用生成时间戳即可。

```
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

```
简单例子：
var sum = new Function('a', 'b', 'return a + b');
console.log(sum(2, 6));
```

### bind()

{% embed url="https://github.com/mqyqingfeng/Blog/issues/12" %}

{% embed url="https://juejin.cn/post/6844903718089916429#heading-6" %}

&#x20;The **`bind()`** method creates a new function that, when called, has its `this` keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

* `bind`是`Functoin`原型链中`Function.prototype`的一个属性，每个函数都可以调用它。
* `bind`本身是一个函数名为`bind`的函数，返回值也是函数，函数名是`bound`。（打出来就是`bound加上一个空格`）
* 调用`bind`的函数中的`this`指向`bind()`函数的第一个参数
* &#x20;传给`bind()`的其他参数接收处理了，`bind()`之后返回的函数的参数也接收处理了，也就是说合并处理了
* &#x20;并且`bind()`后的`name`为`bound + 空格 + 调用bind的函数名`。如果是匿名函数则是`bound + 空格`
* &#x20;`bind`后的返回值函数，执行后返回值是原函数（`original`）的返回值

\
&#x20;

&#x20;







_**第一版**_

```
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

#### 但我们知道函数是可以用new来实例化的。那么bind()返回值函数会是什么表现呢?

从例子种可以看出this指向了**new bound()生成的新对象。**

例子

```
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

```

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

```



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

### splice()



### slice()

### map()

### reduce()

[https://juejin.cn/post/6844903986479251464#heading-26](https://juejin.cn/post/6844903986479251464#heading-26)

### forEach()



### flat()

### from()



##

## 拷贝

### 浅拷贝

* 浅拷贝的意思就是只复制引用，而未复制真正的值。
* 其实就是遍历对象属性的问题

#### 浅拷贝实现

```
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

```
var arr = ['old', 1, true, null, undefined];

var new_arr = arr.concat();

new_arr[0] = 'new';

console.log(arr) // ["old", 1, true, null, undefined]
console.log(new_arr) // ["new", 1, true, null, undefined]

```

### 深拷贝

### 基础方法 1：

```
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


// 问题2： 引用同一个东西却返还不同的指向 引用丢失
var obj2 = {a: obj1, b: obj1};

var obj5 = JSON.parse(JSON.stringify(obj2));


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
  * **无法解决循环引用的问题**
  * **引用丢失**
  * **NaN 和 Infinity 的数值及 null 都会当做 null，**
  * 无法拷贝一写特殊的对象，诸如 RegExp, Date, Set, Map等。
  * **这些对象 Map、Set、WeakMap、WeakSet 仅会序列化可枚举的属性**
  * **无法拷贝函数(function)。**
  * `undefined`、`任意函数`、`Symbol 值`，在序列化过程有两种不同的情况。若出现在非数组对象的属性值中，会被忽略；若出现在数组中，会转换成 `null`

![](<../.gitbook/assets/image (138).png>)

### 基础方法 2，简易版本

* &#x20;就是对每一层的数据都实现一次 `创建对象->对象赋值` 的操作
* 接下来会给予 json.stringfy() 缺陷 一步一步改进
*



```
const deepCopy = source => {
  // 判断是否为数组
  const isArray = arr => Object.prototype.toString.call(arr) === '[object Array]'

  // 判断是否为引用类型
  const isObject = obj => obj !== null && (typeof obj === 'object' || typeof obj === 'function')

  // 拷贝（递归思路）
  const copy = input => {
    //如果是 
    if (typeof input === 'function' || !isObject(input)) return input

    const output = isArray(input) ? [] : {}
    for (let key in input) {
      if (input.hasOwnProperty(key)) {
        const value = input[key]
        output[key] = copy(value)
      }
    }

    return output
  }

  return copy(source)
}



```

###

### 完善版本1， 针对布尔值、数值、字符串的包装对象的处理

* &#x20;由于 [for...in](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FStatements%2Ffor...in) 无法遍历**不可枚举**的属性。例如，包装对象的 `[[PrimitiveValue]]` 内部属性，因此需要我们特殊处理一下。
* &#x20;包装对象的 `[[PrimitiveValue]]` 属性可通过 `valueOf()` 方法获取。
* 其中的 PrimitiveValue 就没法copy进去 我们

![](<../.gitbook/assets/image (135).png>)

![之前code的结果 没有 PrimitiveValue](<../.gitbook/assets/image (136).png>)

```
const deepCopy = source => {
    // 获取数据类型（本次新增）
    const getClass = x => Object.prototype.toString.call(x)

    // 判断是否为数组
    const isArray = arr => getClass(arr) === '[object Array]'

    // 判断是否为引用类型
    const isObject = obj => obj !== null && (typeof obj === 'object' || typeof obj === 'function')

    // 判断是否为包装对象（本次新增）
    const isWrapperObject = obj => {
        const theClass = getClass(obj)
        const isWrapper = ['[object Boolean]', '[object Number]', '[object String]', '[object Symbol]', '[object BigInt]']
        return isWrapper.includes(theClass)
    }



    // 处理包装对象（本次新增）
    const handleWrapperObject = obj => {
        const type = getClass(obj)
        switch (type) {
            case '[object Boolean]':
                return Object(Boolean.prototype.valueOf.call(obj))
            case '[object Number]':
                return Object(Number.prototype.valueOf.call(obj))
            case '[object String]':
                return Object(String.prototype.valueOf.call(obj))
            case '[object Symbol]':
                return Object(Symbol.prototype.valueOf.call(obj))
            case '[object BigInt]':
                return Object(BigInt.prototype.valueOf.call(obj))
            default:
                return undefined
        }
    }


    // 拷贝（递归思路）
    const copy = input => {
        //如果是 
        if (typeof input === 'function' || !isObject(input)) return input
        // 处理包装对象（本次新增）
        if (isWrapperObject(input)) {
            return handleWrapperObject(input)
        }
        const output = isArray(input) ? [] : {}
        for (let key in input) {
            if (input.hasOwnProperty(key)) {
                const value = input[key]
                output[key] = copy(value)
            }
        }

        return output
    }

    return copy(source)
}


let a = {
    bool: new Boolean(false),
    num: new Number(123),
    str: new String("xxx"),
    symbol: Object(Symbol("desc"))
}

let b = deepCopy(a);
console.log(b)
```



### 完善版本2，**针对函数的处理**

* 直接返回就好了 通常不需要管

****

```
const copy = input => {
  if (typeof input === 'function' || !isObject(input)) return input
}

```

### 完善版本3，**针对以 Symbol 值作为属性键的处理**

* &#x20;由于以上 `for...in` 方法无法遍历 `Symbol` 的属性键，因此：

```
const sym = Symbol('desc')
const obj = {
  [sym]: 'This is symbol value'
}
console.log(deepCopy(obj)) // {}，拷贝结果没有 [sym] 属性

```

* [Object.getOwnPropertySymbols()](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal\_Objects%2FObject%2FgetOwnPropertySymbols) 它返回一个对象自身的所有 Symbol 属性的数组，包括不可枚举的属性。
* [Object.prototype.propertyIsEnumerable()](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal\_Objects%2FObject%2FpropertyIsEnumerable) 它返回一个布尔值，表示指定的属性是否可枚举。

```
const deepCopy = source => {
    // 获取数据类型（本次新增）
    const getClass = x => Object.prototype.toString.call(x)

    // 判断是否为数组
    const isArray = arr => getClass(arr) === '[object Array]'

    // 判断是否为引用类型
    const isObject = obj => obj !== null && (typeof obj === 'object' || typeof obj === 'function')

    // 判断是否为包装对象（本次新增）
    const isWrapperObject = obj => {
        const theClass = getClass(obj)
        const isWrapper = ['[object Boolean]', '[object Number]', '[object String]', '[object Symbol]', '[object BigInt]']
        return isWrapper.includes(theClass)
    }



    // 处理包装对象（本次新增）
    const handleWrapperObject = obj => {
        const type = getClass(obj)
        switch (type) {
            case '[object Boolean]':
                return Object(Boolean.prototype.valueOf.call(obj))
            case '[object Number]':
                return Object(Number.prototype.valueOf.call(obj))
            case '[object String]':
                return Object(String.prototype.valueOf.call(obj))
            case '[object Symbol]':
                return Object(Symbol.prototype.valueOf.call(obj))
            case '[object BigInt]':
                return Object(BigInt.prototype.valueOf.call(obj))
            default:
                return undefined
        }
    }


    // 拷贝（递归思路）
    const copy = input => {
        //如果是 
        if (typeof input === 'function' || !isObject(input)) return input
        // 处理包装对象（本次新增）
        if (isWrapperObject(input)) {
            return handleWrapperObject(input)
        }
        const output = isArray(input) ? [] : {}
        for (let key in input) {
            if (input.hasOwnProperty(key)) {
                const value = input[key]
                output[key] = copy(value)
            }
        }

        return output
    }

    return copy(source)
}


const source = {}
const sym1 = Symbol('1')
const sym2 = Symbol('2')
Object.defineProperties(source,
  {
    [sym1]: {
      value: 'This is symbol value.',
      enumerable: true
    },
    [sym2]: {
      value: 'This is a non-enumerable property.',
      enumerable: false
    }
  }
)


console.log(deepCopy(source))
```

![Output](<../.gitbook/assets/image (137).png>)

### 完善版本**3**，循环引用， 引用丢失

1. 解决循环应用：
   1. 创建一个Map。记录下已经拷贝过的对象，如果说已经拷贝过，那直接返回它行了。
   2. **这里有一个潜在的坑**： 就是map 上的 key 和 map 构成了强引用关系，这是相当危险的
      1. 被弱引用的对象可以在任何时候被回收，而对于强引用来说，只要这个强引用还在，那么对象无法被回收。**假设我们使用的 Map，那么图中的 foo 对象和我们深拷贝内部的 const map = new Map() 创建的 map 对象一直都是强引用关系，那么在程序结束之前，foo 不会被回收，其占用的内存空间一直不会被释放。**
      2. **解决办法：**让 map 的 key 和 map 构成弱引用即可。ES6给我们提供了这样的数据结构，它的名字叫WeakMap，它是一种特殊的Map, 其中的键是弱引用的。其键必须是对象，而值可以是任意的。`const deepClone = (target, map = new WeakMap())`&#x20;
2. 解决引用丢失：
   1. 因为只要存储已拷贝过的对象就可以了

```
const deepCopy = source => {
    // 创建一个 WeakMap 对象，记录已拷贝过的对象（本次新增）
    const weakmap = new WeakMap()
    // 获取数据类型（本次新增）
    const getClass = x => Object.prototype.toString.call(x)

    // 判断是否为数组
    const isArray = arr => getClass(arr) === '[object Array]'

    // 判断是否为引用类型
    const isObject = obj => obj !== null && (typeof obj === 'object' || typeof obj === 'function')

    // 判断是否为包装对象（本次新增）
    const isWrapperObject = obj => {
        const theClass = getClass(obj)
        const isWrapper = ['[object Boolean]', '[object Number]', '[object String]', '[object Symbol]', '[object BigInt]']
        return isWrapper.includes(theClass)
    }



    // 处理包装对象（本次新增）
    const handleWrapperObject = obj => {
        const type = getClass(obj)
        switch (type) {
            case '[object Boolean]':
                return Object(Boolean.prototype.valueOf.call(obj))
            case '[object Number]':
                return Object(Number.prototype.valueOf.call(obj))
            case '[object String]':
                return Object(String.prototype.valueOf.call(obj))
            case '[object Symbol]':
                return Object(Symbol.prototype.valueOf.call(obj))
            case '[object BigInt]':
                return Object(BigInt.prototype.valueOf.call(obj))
            default:
                return undefined
        }
    }


    // 拷贝（递归思路）
    const copy = input => {
        //如果是 
        if (typeof input === 'function' || !isObject(input)) return input

        if (weakmap.has(input)) {
            return weakmap.get(input)
        }
        // 处理包装对象（本次新增）
        if (isWrapperObject(input)) {
            return handleWrapperObject(input)
        }
        const output = isArray(input) ? [] : {}
        // 记录每次拷贝的对象
        weakmap.set(input, output)
        for (let key in input) {
            if (input.hasOwnProperty(key)) {
                const value = input[key]
                output[key] = copy(value)
            }
        }

        return output
    }

    return copy(source)
}

// 循环引用
const foo ={name:"ssss"}
foo.bar =foo;
console.log(deepCopy(foo))


// 引用丢失
const obj ={obj1:foo,obj2:foo}
console.log(obj.obj1 ===obj.obj2)//true
const copyObj = deepCopy(obj);
console.log(copyObj.obj1 === copyObj.obj2);// 上一个版本 会返还false


```

{% embed url="https://juejin.cn/post/6975880204447121422/#heading-10" %}

{% embed url="https://juejin.cn/post/6844903692756336653#heading-3" %}

* 没有完全写完 还有一些细节 看上面这个link





## 防抖(debounce)

![](<../.gitbook/assets/image (103).png>)

* 输入框连续输入的请求次数控制
* 防止表单多次提交

### 最简易版本：

[https://jsfiddle.net/Boyanliuu/oznt71e4/25/](https://jsfiddle.net/Boyanliuu/oznt71e4/25/)

### 接受parameter：

[https://jsfiddle.net/h9cL5av4/15/](https://jsfiddle.net/h9cL5av4/15/)

###

### 没返回值版本

```
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

```
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

## 节流(throttle)

![](<../.gitbook/assets/image (104).png>)

* 如果我们需要统计用户滚动屏幕的行为来做出相应的网页反应我们就需要节流
* 因为如果用户不断进行滚动就会不断产生请求， 容易导致网络的堵塞
* 我们就可以在触发时间时候 马上处理任务 然后 设定时间间隔限制，在这段时间内不管用户如何进行滚动都忽视操作
* 在时间到了以后如果检测到用户有滚动行为再次执行任务 并且重新设置时间间隔

{% embed url="https://jsfiddle.net/Boyanliuu/ptg8xvej/13/" %}



例子： 更改页面尺寸大小时候 更改颜色

```
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





## 事件总线 | 发布订阅模式

## 柯里化

## es5 实现继承

异步并发数限制

异步串行 | 异步并行

## 图片懒加载

* 滚动到地方才加载出来所对应的图片 鼠标滚动到才会触发

{% embed url="https://jsfiddle.net/Boyanliuu/7395k1rv/14/" %}



第一种方法&#x20;

![](<../.gitbook/assets/image (105).png>)

```
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

![](<../.gitbook/assets/image (107).png>)

```
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

## Promises/A+规范的promise

{% embed url="https://juejin.cn/post/6844903625769091079#heading-0" %}



## 实现一个sleep函数

## EventEmitter实现事件发布、订阅



## 手写下拉刷新

## 手写上拉加载

## 手写预加载
