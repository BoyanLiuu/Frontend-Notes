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

[https://juejin.cn/post/6844904004007247880\#heading-52](https://juejin.cn/post/6844904004007247880#heading-52)

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

* 


### 

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





