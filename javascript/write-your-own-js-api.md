# Write your own JS API

{% embed url="https://juejin.cn/post/6873513007037546510" %}

{% embed url="https://juejin.cn/post/6946022649768181774" %}



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
    //借用外部传入的构造器给obj设置属性
    var ret = Constructor.apply(obj, arguments);
    //确保构造器总是返回一个对象
    return typeof ret === 'object' ? ret : obj;

};
```

## Apply & Call & Bind

### apply\(\)

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
    var args = [].slice.call(arguments, 1);
    var bound = function(){
        // bind返回的函数 的参数转成数组
        var boundArgs = [].slice.call(arguments);
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

#### 所以相当于new调用时，bind的返回值函数bound内部要模拟实现new实现的操作

_**第二版**_

```text
// 第二版 实现new调用
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

作者：若川
链接：https://juejin.cn/post/6844903718089916429
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
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



new\(\)





