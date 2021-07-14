# Javascript

## 1.Strict Mode

* Using _**strict**_ enforces strict mode, which means stricter parsing and error handling of the JavaScript code. It allows you to catch errors in an erroneous code, whereas in a normal case.
* to enable strict mode for an entire script, include `"use strict";`   at top.
* **adhering to strict mode makes your code generally more optimizable by the engine**

### 题目：

```text
//Question 1
"use strict"
var obj1 = {};
Object.defineProperty(obj1, 'x', { value: 42, writable: false });
obj1.x = 9;

//Question 2, what is the output

(function test() {
    'use strict';
	var fn = function () {
		return this * 2;
	};

	console.log(fn.apply(undefined));
	console.log(fn.apply(null));
	console.log(fn.apply(1));
})();
```

### 解析：

#### Question 1:

* It is readonly property, we would  throw an error since it is not allow to write



#### Question 2:

* use strict can either be used for an entire script or functions. It cannot be used for a block of code enclosed in {} alone. When used inside functions, _**it changes the value of this. It changes from window object to undefined**_
  * When we call a function with apply, the value of this is set to some custom value that is the first parameter to the apply method.
* undefined \* 2  return NaN since, result is not a number
* null &gt;= 0 , so null \*0  return 0
* 1\*2  = 2





## 2. var const let

### var:

* It can be hosited
* It is function scoped, or the global scope if at the top level outside of any function. Wherever a var appears inside a scope, that declaration is taken to belong to the entire scope and accessible everywhere throughout.

![](../.gitbook/assets/image%20%2812%29.png)

* If you try to access a variable's value in a scope where it's not available, you'll get a ReferenceError thrown

```text
function setWidth(){
  var width = 100;

}  
//ReferenceError
con.log(width);
```

### let

* **It is blocked scoped**,it scope does not extend outside the block, 
* **block scope**:It is defined as the nearest set of enclosing curly braces

```text
if (true) {
 var name = 'Matt';
 console.log(name); // Matt
}
console.log(name); // Matt
if (true) {
 let age = 26;
 console.log(age); // 26
}
console.log(age); // ReferenceError: age is not defined 
```

* **It is not allowed hoisted**, Let declaration also does not allow any redundant declarations within a block scope

![](../.gitbook/assets/image%20%2810%29.png)

* **It will not attach to the window** object as they do with var

![](../.gitbook/assets/image%20%281%29.png)

![](../.gitbook/assets/image.png)

* **let Declaration in for Loops**  
  * It will declare a new iterator variable for each loop iteration.

### const:

* It behaves identically to that of let but with one important difference—it must be initialized   with a value, and that value cannot be redefined after declaration.



## 3.Hoisting

* A variable can be declared after it has been used. This is because variable declarations using var hoisted to the top of their functional scope at compile time.
* **Only the declarations get hoisted to the top, not the initializations.**
* **function declaration** are also hoisted, but function expression are not   function declaration

![](../.gitbook/assets/image%20%2813%29.png)

![Subsequent function declaration do override previous ones:](../.gitbook/assets/image%20%287%29.png)

### 题目

```text
//=====EG1===== 
text ='123';
console.log(text);
var text;
//declarations get hoisted to top,
var text;
text ='123';
console.log(text);


//=====Q1===== 
var temp= 'hi';
function display(){
    console.log(temp);
    var temp = 'bye';
};
display();

```

### 解析：

#### 1: 

* Output 是 undefined
* At compile time, the var temp is hoisted to the top of the display\(\) function.

![](../.gitbook/assets/image%20%283%29.png)



## 4.Data Type

* There are **6 simple** data type\(primitive types\)
  * String, number, boolean,BigInt, Undefined, symbol
* 1 complex data type, **Object**
  * **3 wrapper Object**
    * Boolean  Number, String
  * null:  null is considered to be      an empty object reference 
  * **4 subtype of Object**

    * Function, Array,Date,RegExp

    ****

![](../.gitbook/assets/image%20%286%29.png)

![typeof can not deter](../.gitbook/assets/image%20%284%29.png)

### 如何理解bigint：

```text
// 第一种创建 方法
console.log( 9007199254740995n );    // → 9007199254740995n	
console.log( 9007199254740995 );     // → 9007199254740996

// 第二种创建 方法
BigInt("9007199254740995");    // → 9007199254740995n

// 简单的 使用方法
10n + 20n;    // → 30n	
10n - 20n;    // → -10n	
+10n;         // → TypeError: Cannot convert a BigInt value to a number	
-10n;         // → -10n	
10n * 20n;    // → 200n	
20n / 10n;    // → 2n	
23n % 10n;    // → 3n	
10n ** 3n;    // → 1000n	

const x = 10n;	
++x;          // → 11n	
--x;          // → 9n
console.log(typeof x);   //"bigint"

作者：神三元
链接：https://juejin.cn/post/6844903974378668039
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

* 在JS中，所有的数字都以双精度64位浮点格式表示，那这会带来什么问题呢？
* 这导致JS中的Number无法精确表示非常大的整数，它会将非常大的整数四舍五入，确切地说，JS中的Number类型只能安全地表示-9007199254740991\(-\(2^53-1\)\)和9007199254740991（\(2^53-1\)），任何超出此范围的整数值都可能失去精度。
* `console.log(9999999999999999);  //=>10000000000000000`
* 有一定安全性问题： `9007199254740992 === 9007199254740993;    // → true 居然是true!`
* 如何创建并使用BigInt？
  * 要创建BigInt，只需要在数字末尾追加n即可。
  * 另一种创建BigInt的方法是用BigInt\(\)构造函数
* 值得警惕的点：
  * 因为隐式类型转换可能丢失信息，所以不允许在bigint和 Number 之间进行混合操作。当混合使用大整数和浮点数时，结果值可能无法由BigInt或Number精确表示。 
    * `10 + 10n;    // → TypeError`
  * 不能将BigInt传递给Web api和内置的 JS 函数，这些函数需要一个 Number 类型的数字。尝试这样做会报TypeError错误。
    * Math.max\(2n, 4n, 6n\);    // → TypeError
  * 元素都为BigInt的数组可以进行sort。
  * BigInt可以正常地进行位运算，如\|、&、&lt;&lt;、&gt;&gt;和^

### The typeof Operator:

* It tell the type of a given variable, used them for **primitive type**  data

```text
let message = "some string";
let str = new String('hello world);
console.log(typeof message); // "string"
console.log(typeof str); // "Object" 
console.log(typeof 95); // "number"


typeof null ===> object
```

### What values converted to true?

![](../.gitbook/assets/image%20%285%29.png)

### \[\] == !\[\]结果是什么？为什么？

* == 中，左右两边都需要转换为数字然后进行比较。
* \[\]转换为数字为0。  !\[\] 首先是转换为布尔值，由于\[\]作为一个引用类型转换为布尔值为true,



  因此!\[\]为false，进而在转换成数字，变为0。0 == 0 ， 结果为true





### null是对象吗？为什么？

* 虽然 typeof null 会输出 object，但是这只是 JS 存在的一个悠久 Bug。在 JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象然而 null 表示为全零，所以将它错误的判断为 object 。

### 0.1+0.2为什么不等于0.3？

* `0.1 + 0.2 = 0.30000000000000004`
* 十进制0.1转换成二进制，乘2取整过程

![](../.gitbook/assets/image%20%2821%29.png)

* 从上面可以看出，0.1的二进制格式是：0.0001100011....。这是一个二进制无限循环小数，但计算机内存有限，我们不能用储存所有的小数位数。那么在精度与内存间如何取舍呢？
* 在某个精度点直接舍弃。当然，代价就是，0.1在计算机内部根本就不是精确的0.1，而是一个有舍入误差的0.1。当代码被编译或解释后，0.1已经被四舍五入成一个与之很接近的计算机内部数字，以至于计算还没开始，一个很小的舍入错误就已经产生了。这也就是 0.1 + 0.2 不等于0.3 的原因。
* **不要直接比较两个浮点的大小**：
* **JS中如何进入浮点数运算 :**

```text
{
  let x = new BigNumber(0.1);
  let y = new BigNumber(0.2)
  let z = new BigNumber(0.3)

  console.log(z.equals(x.add(y))) // 0.3 === 0.1 + 0.2, true
  console.log(z.minus(x).equals(y)) // true
  console.log(z.minus(y).equals(x)) // true
}
```

\*\*\*\*



### What is NaN

* Not a number
* Any operation involving with NaN  always return NaN
* NaN is not equal to any value, use isNaN\(\) to check
* `console.log(NaN == NaN)   // false`
* **isNaN\(\)**: when passed a value, it will try to convert it to a number

```text
console.log(isNaN(NaN)); // true
console.log(isNaN(10)); // false - 10 is a number
console.log(isNaN("10")); // false - can be converted to number 10
console.log(isNaN("blue")); // true - cannot be converted to a number
console.log(isNaN(true)); // false - can be converted to number 1
```

###  Type Conversion/ Concatenate:

![https://juejin.cn/post/6844903974378668039\#heading-14](../.gitbook/assets/image%20%2822%29.png)

* toString\(\),转换成字符串
* parseInt,转换成数字
  * True -&gt; 1
  * False -&gt; 0
  * Undefined, 或者失误转换为 NaN
  * Null -&gt; 0
* new Boolean\(\),转换成布尔值
  * false value will return false;
* **concatenate:**
  * 如果+的 其中一个操作数时字符串， 则 concatenate， 否则 数字加法

```text
var a = '42';
var b ='0';
var c = 42;
var d = 0;
console.log(a+b); //420
```

 

### 对象转原始类型是根据什么流程运行的？

* 对象转原始类型，会调用内置的\[ToPrimitive\]函数，对于该函数而言，其逻辑如下：
  * 如果Symbol.toPrimitive\(\)方法，优先调用再返回
  * 调用valueOf\(\)，如果转换为原始类型，则返回
  * 调用toString\(\)，如果转换为原始类型，则返回
  * 如果都没有返回原始类型，会报错

```text
var obj = {
  value: 3,
  valueOf() {
    return 4;
  },
  toString() {
    return '5'
  },
  [Symbol.toPrimitive]() {
    return 6
  }
}
console.log(obj + 1); // 输出7

作者：神三元
链接：https://juejin.cn/post/6844903974378668039
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 如何让if\(a == 1 && a == 2\)条件成立？

* 其实就是上个知识点的应用

```text
var a = {
  value: 0,
  valueOf: function() {
    this.value++;
    return this.value;
  }
};
console.log(a == 1 && a == 2);//true

作者：神三元
链接：https://juejin.cn/post/6844903974378668039
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### Instance of:

* It checks whether the type of the left object belong to the right object in its prototype chain.
* f instanceof foo 
  * f 的 \_\_proto\_\_ 一层一层往上， 能否对应到 Foo.prototype

#### 题目：

```text
// #####Q1#####
var names = ["Tom","Anna",2,true]
console.log(names instanceof String) // false
console.log(names instanceof Number) // false
console.log(names instanceof Object) // true
console.log(names instanceof Array) //true

//#####Q2#####
var str1 = 'This is a string'
var str2 = new String("String using new")
console.log(str1 instanceof String) // false
console.log(str2 instanceof String) //true
console.log(str2 instanceof Object) //true
console.log(str1 instanceof Object) //false
```

#### 解析

2：

* str1 is string literal, and is not created using the **String** object, Hence even though its type is string , it is not an instance of String

### Object.is和===的区别？

![](../.gitbook/assets/image%20%2823%29.png)

* Object在严格等于的基础上修复了一些特殊情况下的失误，具体来说就是+0和-0，NaN和NaN。 源码如下：
* The Object.is\(\) method determines whether two values are the same value.

```text
// Case 2: Signed zero
Object.is(0, -0);                 // false
Object.is(+0, -0);                // false
Object.is(-0, -0);                // true
Object.is(0n, -0n);               // true

// 0 ===-0 // true


// Case 3: NaN
Object.is(NaN, 0/0);              // true
Object.is(NaN, Number.NaN)        // true
Object.is(NaN,NaN)                //  true
NaN === NaN                      //false
```



### ==  and ===

* == check for value equality with coercion allowed,
* === checks for both value equality without coercion allowed, This is often called strict equality

## 5.equal and not equal

* Convert equal to 0 or 1 and then compare
* If one is string and another one is number, convert string to number first
* 
  If one is object another one is not, then it will use valueOf\(\)

  * If both are object, they will check if they are the same object,

* Values of null and undefined are equal.
* Values of null and undefined cannot be converted into any other values for equality checking.



![](../.gitbook/assets/image%20%289%29.png)





## 6.Detecting array

```text
//Method 1
const a = [];
console.log(a instanceof Array);//true

//Method 2
const a = [];
console.log(a.constructor == Array);//true

//Method 3 例子
const a = ['Hello','Howard'];
const b = {0:'Hello',1:'Howard'};
const c = 'Hello Howard';

console.log(a.toString())//"Hello,Howard"
console.log(b.toString())//"[object Object]"
console.log(c.toString())//"Hello,Howard"

//Method 3
const a = ['Hello','Howard'];
const b = {0:'Hello',1:'Howard'};
const c = 'Hello Howard';
Object.prototype.toString.call(a);//"[object Array]"
Object.prototype.toString.call(b);//"[object Object]"
Object.prototype.toString.call(c);//"[object String]"

const isArray = (something)=>{
    return Object.prototype.toString.call(something) === '[object Array]';
}

//重写了toString方法, method 3前提条件是不能更改 toString()
Object.prototype.toString = () => {
    alert('你吃过了么？');
}
//调用String方法
const a = [];
Object.prototype.toString.call(a);//弹框问你吃过饭没有



//Method 4
const a = [];
const b = {};
Array.isArray(a);//true
Array.isArray(b);//false
```

1.  使用 instanceof 判断
2. 使用 constructor 判断
   1. 但是这个 方法 前提条件是 **constructor 没有被改写**
3. 用Object的toString方法判断， 每一个 继承自 Object的对象都拥有 toString\(\) 方法
   1. 前提条件是 toString\(\) 没有被改写  
   2. 如果一个对象的toString方法没有被重写过的话，那么toString方法将会返回"\[object type\]"，其中的type代表的是对象的类型，根据type的值，我们就可以判断这个疑似数组的对象到底是不是数组了。 **我们不能直接调用 数组的toString 方法**
   3. 从Method3 例子当中，除了对象之外，其他的数据类型的toString返回的都是内容的字符创，只有对象的toString方法会返回对象的类型。所以要判断除了对象之外的数据的数据类型，我们需要“借用”对象的toString方法，所以我们需要使用call或者apply方法来改变toString方法的执行上下文
4. 用Array对象的isArray方法判断, 最靠谱的方法了



## 7.toFixed / toPrecision:

* toFixed: 是从小数点后开始判断
* toPrecision: 判断是从最开始判断

```text
var a = 46.39
console.log(a.toFixed(0))//"46"
console.log(a.toFixed(1))//"46.4"
console.log(a.toPrecision(2))//"46"
console.log(a.toPrecision(3))//"46.4"
```

## 8.How to empty an array

```text
//Method 1
var ary = [1,2,3];

ary =[];
console.log(ary);

//Method 2
var ary = [1,2,3];

ary.length =0;
console.log(ary);

//Method 3 
var ary = [1,2,3];

while(ary.length){
  ary.pop();
  
}
console.log(ary);


// Method 4
var ary = [1,2,3];


ary.splice(0,ary.length); //splice(start, deleteCount) 

console.log(ary);
```

## 9. Arrow function

### Syntax

```text
//Function with no parameter
const identifierName = () => {codeForFunction}
//Function with single parameter
const identifierName = singleParameter => {codeForFunction}
//function with multiple parameters
const identifierName = (functionParameters) => {codeForFunction}
//Single-line function
const identifierName = (functionParameters) => oneLineCode
let double = (x) => { return 2 * x; };
let triple = (x) => 3 * x;
```



### How does the arrow function differ from other functions?

* Implicitly returns values, the return keyword can be avoided
* Is anonymous,There is no need for a name or the function keyword in an arrow function
* Inherits the value of this from enclosing scope, meaning since they don’t have their own context in which they execute, this gets inherited from the parent function. Hence, they don’t have their own this value.

### Which of the following are an arrow function's uses cases?

* Managing asynchronous code
  * Using arrow functions in codes using promises or asynchronous callbacks makes the code easier to read and more concise.
* Array manipulation
  * One of the common operations you might need to perform on an array is to map or reduce them. Doing this using arrow functions makes the code more concise and easier to read.

### What will be the result of clicking on the button?

```text
const button = document.querySelector('#pushy');
button.addEventListener('click', () => {
    this.classList.toggle('on');
});
```

* TypeError, cannot read property 'toggle' of undefined
* In JavaScript, arrow functions do not bind their own this, meaning they inherit the one from the parent scope; this is also known as lexical scoping.Hence, in the code above, if you display the value of this you’ll see that the result will be **window**:





## 10. Understanding function arguments:

```text
//Basic
function sayHi() {
 console.log("Hello " + arguments[0] + ", " + arguments[1]);
}

sayHi('OO','XXX') //"Hello OO, XXX"
```

* when a function is defined using the **function** keyword \(meaning a non-arrow function\), there actually is an **arguments object** that can be accessed while inside a function to retrieve the values of each

  argument that was passed in.

* The arguments object acts like an array \(though it isn’t an instance of Array\)
* can use `arguments.length`



## 11. Rest parameter and spread syntax

### Rest parameter:

```text
function getSum(...values) {
 // Sequentially sum all elements in 'values'
 // Initial total = 0
 return values.reduce((x, y) => x + y, 0);
}
```

*  It allows us to represent an indefinite number of arguments as an array, It has to be the last formal parameter
* **values becomes an array**





### Spread parameter:

```text
let values = [1, 2, 3, 4];
function getSum() {
 let sum = 0;
 for (let i = 0; i < arguments.length; ++i) {
 sum += arguments[i];
 }
 return sum;
}

console.log(getSum(-1, ...values)); // 9
console.log(getSum(...values, 5)); // 15
console.log(getSum(-1, ...values, 5)); // 14
console.log(getSum(...values, ...[5,6,7])); // 28
```

* It break array into  individually and pass each value as a separate argument
* there are no restrictions on other parameters appearing before   or after the spread operator

### How is rest parameter different with spread syntax?

* It collects values in the form of an array
* Where spread is used to create copies of arrays/objects, rest is used to collect all the remaining values into an array.
* 
  Works in function argument

### What are the benefits of using spread syntax?

* Easy to create copies of arrays
* Easy to create copies of objects
* Beneficial when working with data that should not be mutated



## 12 .Functions

### new.target in function

```text
function King() {
 if (!new.target) {
 throw 'King must be instantiated using "new"'
 }
 console.log('King instantiated using "new"';
}
new King(); // King instantiated using "new"
King(); // Error: King must be instantiated using "new"
```

* Functions have always been able to behave as both a constructor to instantiate a new object, and

  as a normal callable function

*  New in ECMAScript 6 is the ability to determine if a function was   invoked with the new keyword using _**new.target**_ If a function is called normally, new.target will   be undefined. 

### Function properties

```text
function sayName(name) {
 console.log(name);
}

function sum(num1, num2) {
 return num1 + num2;
}

function sayHi() {
 console.log("hi");
}

console.log(sayName.length); // 1
console.log(sum.length); // 2
console.log(sayHi.length); // 0
console.log(sayHi.name);// "sayHi"
```

* Each function has 3 properties: **length and prototype, name**. The **length property** indicates   the number of named arguments that the function expects, **name give its name**
* In strict mode, the this value of a function called without a context   object is not coerced to window. Instead, this becomes undefined unless explicitly set by either attaching the function to an object or using apply\(\) or call\(\).

### Function method :bind,call & apply 

* Both call the function with a specific **this** value,  effective setting the value of this object inside the function body.

### bind:

```text
window.color = 'red';
var o = {
 color: 'blue'
};

function sayColor() {
 console.log(this.color);
}
let objectSayColor = sayColor.bind(o);
objectSayColor(); // blue
```

* Definded in the ECMAScript 5, It create a new function instance whose this value is bound to the value that was passed into bind\(\);
* a new function called objectSayColor\(\) is created from sayColor\(\) by calling bind\(\) and   passing in the object o. The objectSayColor\(\) function has a this value equivalent to o,

### call

```text
function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  //we call function by using new operator
  // so we are attaching it to the Food object,
  //so this == Food object
  Product.call(this, name, price);
  this.name = name;
  this.price = price;

}

console.log(new Food('cheese', 5).price);//5


window.color = 'red';
let o = {
 color: 'blue'
};

function sayColor() {
 console.log(this.color);
}

sayColor(); // red

sayColor.call(this); // red
sayColor.call(window); // red
sayColor.call(o); // blue
```

* The first argument is the this value, but the remaining arguments are passed directly into the   function. 

### apply

```text
function sum(num1, num2) {
 return num1 + num2;
}

function callSum1(num1, num2) {
 return sum.apply(this, arguments); // passing in arguments object
}

function callSum2(num1, num2) {
 return sum.apply(this, [num1, num2]); // passing in array
}

console.log(callSum1(10, 10)); // 20
console.log(callSum2(10, 10)); // 20
```

* It accepts two arguments: the value of this inside the function and an

  array of arguments, the second argument can be an array or arguments

### What is difference between call apply?

* apply requires an array as its second argument wheres call requires the parameters to be listed explicitly.



### Private Variables:

```text
function MyObject() {
 // private variables and functions
 let privateVariable = 10;

 function privateFunction() {
 return false;
 }

 // privileged methods
 this.publicMethod = function() {
 privateVariable++;
 return privateFunction();
 };
}

let a = new MyObject();
console.log(a.privateVariable);// undefined
```

* publicMethod: This is one way to create privileged methods on object\(inside a constructor\) 
* 
### StaticPrivateVariables:

* Here is another way to create privileged methods , using private scope to define the private variables or   functions.
* In this pattern, a private scope is created to **enclose the constructor and its methods**. The private   variables and functions are defined first, followed by the constructor and the public methods. Public methods are defined on the prototype, as in the typical **prototype pattern**
* This would create static private variables for better code reuse through prototype., although each instance doesn't have its own private variable.

```text
(function() {
 // private variables and functions
 let privateVariable = 10;

 function privateFunction() {
 return false;
 }

 // constructor
 MyObject = function() {};

 // public and privileged methods
 MyObject.prototype.publicMethod = function() {
 privateVariable++;
 return privateFunction();
 };
})();


let a =  new MyObject ();
console.log(a.publicMethod());//false
console.log(a.privateVariable);//undefined
```

* publicMethod is defined on its prototype,
* Initializaing an undeclared variable always creates a global variable, so MyObject becomes global and available   outside the private scope
* **publicMethod**: being a closure, always holds a reference to the containing scope. so that is why he can access privateVariable 

```text
(function() {
 let name = '';

 Person = function(value) {
 name = value;
 };

 Person.prototype.getName = function() {
 return name;
 };

 Person.prototype.setName = function(value) {
 name = value;
 };
})();

let person1 = new Person('Nicholas');
console.log(person1.getName()); // 'Nicholas'
person1.setName('Matt');
console.log(person1.getName()); // 'Matt'

let person2 = new Person('Michael');
console.log(person1.getName()); // 'Michael'
console.log(person2.getName()); // 'Michael'

```



## 13.Closure

* It is when an inner function has access to variables in the outer scope That all functions are closures in javascript
* Common use of closure
  * For object Data Privacy， have private variable, private method
  * In event Handlers
  * In callback Functions
  * Currying

### Immediately invoked function expression

```text
// IIFE
(function () {
 for (var i = 0; i < count; i++) {
 console.log(i);
 }
})();
```

* An anonymous function that is called immediately

### 问题

```text
//Q1
const array = [5, 11, 18, 25];
for (var i = 0; i < array.length; i++) {
  setTimeout(function() {
    console.log('Element: ' + array[i] + ', at index: ' + i);
  }, 3000);
}

//Q2 how to fix about question?

// Solution 1
const array = [5, 11, 18, 25];
for (let i = 0; i < array.length; i++) {
  setTimeout(function() {
    console.log('Element: ' + array[i] + ', at index: ' + i);
    }, 3000);
}


// Solution 2
const array = [5, 11, 18, 25];
for (var i = 0; i < array.length; i++) {
  setTimeout((function(local_i) {
  return function(){
  console.log('Element: ' + array[local_i] + ', at index: ' + local_i);
  }
 })(i), 3000);
}
// 这个跟 solution2 一样
for(var i = 1;i <= array.length;i++){
  (function(local_i){
    setTimeout(function timer(){
       console.log('Element: ' + array[local_i] + ', at index: ' + local_i);
    }, 0)
  })(i)
}

// end of solution for Q2
// Solution 3

for(var i=1;i<= array.length;i++){
  setTimeout(function timer(local_i){
     console.log('Element: ' + array[local_i] + ', at index: ' + local_i);
  }, 0, i)
}
```

### 解析

1. Output:  Element: undefined, at index:4     乘4
   1. on each iteration, the setTimeout will be triggered,since it’s an asynchronous web API, the command enters the event queue, after which the next loop iteration occurs. Hence, the event queue waits for the loop commands to execute first and call stack to get empty, after which the four setTimeout commands move from the event queue to call stack and execute.
2.  There are 3 possible solution:
   1. Using Es6 featre, **let**   It  creates a new binding for each loop iteration, each i refers to the binding of one specific iteration and preserves the value that was current at that time
   2. **Using IFFE**: That function takes the parameter local\_i, that is the variable i. It calls another function in return, an anonymous function that displays the value of i stored in the variable local\_i
   3. 给定时器传入第三个参数, 作为timer函数的第一个函数参数
      1. 

## 14.This object

```text
//Ex1
window.identity = 'The Window';
                   
let object = {
  identity: 'My Object',           
  getIdentityFunc() {
    return function() {
      return this.identity;
    };
  }
};
                   
console.log(object.getIdentityFunc()());  // 'The Window'


//Solution 1
window.identity = 'The Window';

let object = {
 identity: 'My Object',
 getIdentityFunc() {
 let that = this;
 return function() {
 return that.identity;
 };
 }
};

console.log(object.getIdentityFunc()()); // 'My Object'

// Ex 2
let obj = {
  a: function() {
    console.log(this);
  }
}
let func = obj.a;
func();  // this is window
obj.a(); // this represent object

```

* When a function is not   defined using the arrow syntax, the this object is bound at runtime based on the context in which   a function is executed: 
  * when used inside global functionsm, this is equal to **window in nonstrict** mode     and **undefined in strict mode**
  * this is equal to the object when called as an object method   `obj.a();`
  * Dom 事件绑定, onclick和addEventerListener中 this 默认指向绑定事件的元素。
* **Ex1:**  getIdentityFunc\(\) returns a function, so calling this function immediately call the function that is returned.
  * Each function automatically gets two special variables as soon as the function is

    called: **this and arguments.** An inner function can never access these variables directly from an outer     function

  * Before     defining the anonymous function, a variable named that is assigned equal to the this object. When     the closure is defined, it has access to that because it is a uniquely named variable in the containing     function. Even after the function is returned, that is still bound to object, so calling object.getIdentityFunc\(\)  returns 'My Object'.
* Both **this and arguments** behave in this way. If you want access to a   containing scope’s arguments object, you’ll need to save a reference into another   variable that the closure can access.



## 15. Call Stack & Event Loop

![](../.gitbook/assets/image%20%2818%29.png)

* call stack
  * stores all your running JavaScript code. The interpreter reads the code line-by-line
* event table
  *  This table is responsible for moving the asynchronous code to the event queue after a specified time
* event loop:
  * Itt is responsible for keeping check of both the call stack and the event queue. It keeps checking if all the statements from the call stack have finished execution; that is, if the call stack is empty. If so, it pops the statement from the event queue \(if present\) to the call stack to execute.
* Macro task queue:
  * * 宏任务
  * script，setTimeout，setImmediate，promise中的executor
* Micro Task queue\(Callback queue\):
  * 对于每一个 宏任务而言， 其内部都有一个微任务队列，当该宏任务执行完成，会检查其中的微任务队列，如果为空则直接执行下一个宏任务，如果不为空，则依次执行微任务，执行完成才去执行下一个宏任务
  * MutationObserver、Promise.then\(或.reject\) 以及以 Promise 为基础开发的其他技术\(比如fetch API\)
* **process.nextTick优先级高于Promise.then**

### 问题：

```text
//Q1
console.log("Before Function")
setTimeout(function(){
  console.log("Inside Function")
},3000)
console.log("After Function")
// Q1 answer: 
//Before function, After Function , Inside Function


// Q2
console.log('start');
setTimeout(() => {
  console.log('timeout');
});
Promise.resolve().then(() => {
  console.log('resolve');
});
console.log('end');



// Question 3:

Promise.resolve().then(()=>{
  console.log('Promise1')  
  setTimeout(()=>{
    console.log('setTimeout2')
  },0)
});
setTimeout(()=>{
  console.log('setTimeout1')
  Promise.resolve().then(()=>{
    console.log('Promise2')    
  })
},0);
console.log('start');

// start
// Promise1
// setTimeout1
// Promise2
// setTimeout2

// Question 4
setTimeout(()=>{
   console.log(1) 
},0)
let a=new Promise((resolve)=>{
    console.log(2)
    resolve()
}).then(()=>{
   console.log(3) 
}).then(()=>{
   console.log(4) 
})
console.log(5)

//2 -> 5 -> 3 -> 4 -> 1

// Solution 5 

new Promise((resolve,reject)=>{
    console.log("promise1")
    resolve()
}).then(()=>{
    console.log("then11")
    new Promise((resolve,reject)=>{
        console.log("promise2")
        resolve()
    }).then(()=>{
        console.log("then21")
    }).then(()=>{
        console.log("then23")
    })
}).then(()=>{
    console.log("then12")
})

// "promise1"
// "then11"
// "promise2"
// "then21"
// "then12"
// "then23"



// Solution 6
new Promise((resolve,reject)=>{
    console.log("promise1")
    resolve()
}).then(()=>{
    console.log("then11")
    return new Promise((resolve,reject)=>{
        console.log("promise2")
        resolve()
    }).then(()=>{
        console.log("then21")
    }).then(()=>{
        console.log("then23")
    })
}).then(()=>{
    console.log("then12")
})

// "promise1"
// "then11"
// "promise2"
// "then21"
// "then23"
// "then12"


//7. 多个 promise
new Promise((resolve,reject)=>{
    console.log("promise1")
    resolve()
}).then(()=>{
    console.log("then11")
    new Promise((resolve,reject)=>{
        console.log("promise2")
        resolve()
    }).then(()=>{
        console.log("then21")
    }).then(()=>{
        console.log("then23")
    })
}).then(()=>{
    console.log("then12")
})
new Promise((resolve,reject)=>{
    console.log("promise3")
    resolve()
}).then(()=>{
    console.log("then31")
})

//[promise1,promise3,then11,promise2,then31,then21,then12,then23]

```

### 解析：

1. 不需要解释什么
2. 解释

   1. 先打印 start 和 end
   2. setTimeout 作为一个宏任务放入宏任务队列（MacroTask）
   3. Promise.then作为一个为微任务放入到微任务队列
   4. 当本次宏任务执行完，检查微任务队列，发现一个Promise.then, 执行
   5. 接下来进入到下一个宏任务——setTimeout, 执行

![](../.gitbook/assets/image%20%2826%29.png)

4. 解释

* Promise的executor是一个同步函数，即非异步，立即执行的一个函数，因此他应该是和当前的任务一起执行的。而Promise的链式调用then，每次都会在内部生成一个新的Promise，然后执行then，在执行的过程中不断向微任务\(microtask\)推入新的函数，因此直至微任务\(microtask\)的队列清空后才会执行下一波的macrotask。

5. 解释 **then中return一个promise的掌握**

第一轮:

* current task: promise1是当之无愧的立即执行的一个函数，参考上一章节的executor，立即执行输出`[promise1]`
* micro task queue: \[promise1的第一个then\]

第二轮

* current task: then1执行中，立即输出了**then11**以及新promise2的**promise2**
* micro task queue: \[新promise2的then函数,以及promise1的第二个then函数\]

第三轮

* current task: 新promise2的then函数输出then21和promise1的第二个then函数输出then12
* micro task queue: \[新promise2的第二then函数\]



第四轮

* current task: 新promise2的第二then函数输出then23
* micro task queue: \[\]

6. 解释 then中返还一个promise的掌握

* 这个考的重点在于Promise而非Eventloop了。这里就很好理解为何then12会在then23之后执行，这里Promise的第二个then相当于是挂在新Promise的最后一个then的返回值上

7. 如果有多个 promise

第一轮:

* current task: promise1，promise3
* micro task queue: \[promise1的第一个then，promise3的第一个then\]

第二轮

* current task: then11，promise2，then31
* micro task queue: \[promise2的第一个then，promise1的第二个then\]

第三轮

* current task: then21，then12
* micro task queue: \[promise2的第二个then\]

第四轮

* current task: then23
* micro task queue: \[\]





## 16.Array splice\(\) & slice\(\):

### slice\(\)

```text
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]

console.log(animals.slice(1, 5));
// expected output: Array ["bison", "camel", "duck", "elephant"]

console.log(animals.slice(-2));
// expected output: Array ["duck", "elephant"]

console.log(animals.slice(2, -1));
// expected output: Array ["camel", "duck"]
```

* It is used to return all the values from the given start index \(first parameter\) to the given end index \(second parameter\)

### splice\(\)

```text
//splice(start)
//splice(start, deleteCount)
//splice(start, deleteCount, item1)
//splice(start, deleteCount, item1, item2, itemN)


const months = ['Jan', 'March', 'April', 'June'];
// deletion:
months.splice(2, 1); ==== months.splice(2);
console.log(months);
// expected output: Array ["Jan", "March", "June"]

// insert
months.splice(1,0,'Feb');
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "June"]

//replace
months.splice(2,1,'Feb','July');
console.log(months);
// expected output: Array ["Jan", "March", "Feb", "July", "June"]
```

* It is method changes the contents of an array by removing or replacing existing elements and/or adding new elements in place
* 






## 17. Destructuring

![](../.gitbook/assets/image%20%2817%29.png)

```text
// Q1 Get name of second object 
const exampleObject = {
   collection : [
                  {name: "kelly",},
                  {name: "Anna",}
                ],
}

//Q1 solution: 
const {collection: [,{name:secondObject,}]} = exampleObject


//Q2 list returned with the first two elements removed
function removeFirstTwo(list) {
  return list
} 

//Q2 solution
function removeFirstTwo(list) {
  const [, , ...arr] = list; 
  return arr;
} 


// Q3  return nth cat
function returnNthCat(n){
const state = {
   cats : [
      {catId : 1 , name : "tom"},
      {catId : 2 , name : "tiggy"},
      {catId : 3 , name : "leo"},
      {catId : 4 , name : "nixie"}
   ],
   curpage : 3
}
}

//Q3 solution
function returnNthCat(n){
const state = {
   cats : [
      {catId : 1 , name : "tom"},
      {catId : 2 , name : "tiggy"},
      {catId : 3 , name : "leo"},
      {catId : 4 , name : "tommy"}
   ],
   curpage : 3
}

const { cats: { [n]:{name:thisCatName}}} = state;

return thisCatName
}
console.log(returnNthCat(1))


//Q4  implement the destructuring of undefined
function pointValues(point){
    const {name:n,age:a} = point 
    console.log(n)
    console.log(a)
}
//Solution 1
function pointValues(point){
    const {name:n,age:a} = {...point} 
    console.log(n)
    console.log(a)
}
pointValues({name:"jerry", age:2})
pointValues(undefined)

// solution 2
function pointValues(point){
    const {name:n,age:a} = point || {}
    console.log(n)
    console.log(a)
}
pointValues(undefined)
pointValues({name: "jerry", age:2})
```

Object destructuring follows a syntax similar to creating an object literal but on the left-hand side

1. we use **,** to skip first index element
2.  we use rest operator to collect all the remaining values into an array
3.  steps
   1. We first access the cats property.
   2. After accessing it, we need to decide which cat object to access given the value of n.
   3. Since cats is an array, we access the specific cat object in the array using indexing \[n\] and store it in the thisCat property.
4. Two solutions
   1. using spread operator, we create a clone of object, The values undefined and null get ignored when passed in this case, and we get an empty new object.
   2. using circus shortcut



## 18. Event Handling

```text
let btn = document.getElementById("myBtn");
btn.addEventListener("click", () => {
 console.log(this.id);
}, false);
btn.addEventListener("click", () => {
 console.log("Hello world!");
}, false);

let btn = document.getElementById("myBtn");
let handler = function() {
 console.log(this.id);
};
btn.addEventListener("click", handler, false);

// other code here

btn.removeEventListener("click", handler, false); // works!
```

* we can add multiple eventListener , The event handlers fire in the order in which

  they were added

* `btn.removeEventListener()`   remove event listener or `btn.onclick = null` 

### Internet Explorer Event Handlers

* using attachEvent\(\) and detachEvent\(\)



### Event bubbling

* When an event happens on an element, it first runs the handlers on it, then on its parent, then all the way up on other ancestors.  **Goes all the way to html**
* How to stop bubbling?
  * **event.stopPropagation\(\)**
  * **event.cancelBubble\(\)**

\*\*\*\*

### Event Capturing

```text
parent.addEventListener(
  "click",
  function() {
    console.log("Box is clicked");
  },
  true
);
```

* when handler as true, we will have event capture, **it runs from top to bottom**
* 
### Event Delegation

![](../.gitbook/assets/image%20%2815%29.png)

* The solution to the too many event handlers issue
* Event delegation takes   advantage of event bubbling to assign a single event handler to manage all events of a particular type
* we add eventlistener on the parent div



## 19. Security

### Same origin Policy

```text
//Q1
url1 = http://company.com/dir/page.html

url2 = http://company.com/dir/inner/another.html

url3 = https://company.com/dir/page.html

url4 = http://sub.company.com/dir/page.html

url5 = http://xyz.company.com/dir/page.html
// in the code on xyz.company.com the following change is made
document.domain = "company.com"
// in the code on company.com the following change is made
document.domain = "company.com"
```

* It allows the script in one page to access data in another page only if they have the same origin
* It is a security protocol to prevent attacks like cross site scripting
* How to checks that the same origin policy makes for two URLS?
  * Same host name
  * Same port number
  * Same Protocol
* Which of the urls have same origin comparison with url1?
  * **Url2:** the hostname, port \(HTTP is port 80 by default\), and domain are the same for both URLs; only the path dir/inner/another.html is different
  * Url3:It has a different protocol than url1, https instead of http;
  * Url4: we can see that it has a different hostname; hence, it is not of the same origin.
  * **URL5**: We set document.domain to company.com for url5; this means we are allowing the subdomain xyz.company.com to access its parent’s company.com's resources.
    * However, that means we need to do the same for url1; this step will indicate that url1 wishes to allow url5 to access its resources.Now, url5 can pass the same-origin check with url1. Implementing the above-mentioned steps allows url5 to pass the port number check as well. How is that?
    * The port number is checked separately; any call to `document.domain` overwrites the port number to null. Calling this statement for url5 sets its port number to null. Since both url1 and url5 need to have the same port number, we need to execute this statement for url1 as well. In the example above, we do exactly that; now, url5 will be of the same origin as url1.



## 19. The client side storage

### cookies

```text
document.cookie = encodeURIComponent("name") + "=" +
 encodeURIComponent("Nicholas");
 
 document.cookie = encodeURIComponent("name") + "=" +
 encodeURIComponent("Nicholas") + "; domain=.wrox.com; path=/";
 
 class CookieUtil {
 static get(name) {
 let cookieName = `${encodeURIComponent(name)}=`,
 cookieStart = document.cookie.indexOf(cookieName),
 cookieValue = null;

 if (cookieStart > -1){
 let cookieEnd = document.cookie.indexOf(";", cookieStart);
 if (cookieEnd == -1){
 cookieEnd = document.cookie.length;
 }
 cookieValue = decodeURIComponent(document.cookie.substring(cookieStart
 + cookieName.length, cookieEnd));
 }

 return cookieValue;
 }

 static set(name, value, expires, path, domain, secure) {
 let cookieText =
 `${encodeURIComponent(name)}=${encodeURIComponent(value)}`

 if (expires instanceof Date) {
 cookieText += `; expires=${expires.toGMTString()}`;
 }

 if (path) {
 cookieText += `; path=${path}`;
 }

 if (domain) {
 cookieText += `; domain=${domain}`;
 }

 if (secure) {
 cookieText += "; secure";
 }

 document.cookie = cookieText;
 }
 static unset(name, path, domain, secure) {
 CookieUtil.set(name, "", new Date(0), path, domain, secure);
 }
}; 
 
 
```

* This HTTP response sets a cookie with the name of "name" and a value of "value". Both the name   and the value are **URL-encoded** when sent
* To specify additional information about the created cookie, just append it to string in the same format as the Set-Cookie header
* Cookie get method:
  * We first use indexOf\(\) to find its occurrence,
  * Then the next indexOf is used to find the next semicolon after that location.
  * If the semicolon isn’t found, this means that the cookie is the last one in the string.

### session storage

```text
// Save data to sessionStorage
sessionStorage.setItem('key', 'value');

// Get saved data from sessionStorage
let data = sessionStorage.getItem('key');

// Remove saved data from sessionStorage
sessionStorage.removeItem('key');

// Remove all saved data from sessionStorage
sessionStorage.clear();
```

* Around 5MB
* Do not store sensitive information because the data   cache isn’t encrypted
* The data is stored **until the browser is closed,**
* 
  **When to use:** It should only be used primary for small pieces of data that are valid only for a session.

### local storage

* Around 5MB
* Do not store sensitive information because the data   cache isn’t encrypted
* Permanent storage
* In order to access the same localStorage object, pages must   be served  followed same origin policy

## 19 Client Detection

```text
 function isIOS() {
  return /iphone|ipad|itouch/i.test(navigator.userAgent);
}


 function isAndroid() {
  return /android/i.test(navigator.userAgent);
}
```



* The Navigator userAgent property is used for returning the user-agent header’s value sent to the server by the browser. It returns a string representing values such as the name, version, and platform of the browser.

## **20.** The Document Object Model

* Api that treats HTML and XML documents as tree structures with nodes
* The DOM is constructed in the browser as the page loads

### **Window VS document:**

* window is that global object that holds global variables, global functions, location, history everything is under it. Besides, setTimeout, ajax call \(XMLHttpRequest\), console or localStorage are part of window
* the document is also under the window. document is a property of the window object. document represents the DOM and DOM is the object oriented representation of the html markup you have written. All the nodes are part of document

### **W**indow.onload vs document.onload

* window.onload is fired when DOM is ready and all the contents including images, css, scripts, sub-frames, etc. finished loaded. This means everything is loaded.
* document.onload is fired when DOM \(DOM tree built from markup code within the document\)is ready which can be prior to images and other external content is loaded.Think about the differences between window and document, this would be easier for you.

### Node 

#### Node name

* `xx.nodeName == tag name`

#### Node type

![](../.gitbook/assets/image%20%2816%29.png)

* There are 12 numeric constants on the Node type

#### Node manipulation

```text
 someNode.firstNode
 someNode.lastNode
 someNode.parentNode
 someNode.childNodes[0];
 
someNode.nextSibling
// adds a node to the end of the childNodes
 someNode.appendChild(newNode);
 
// insert as last child
returnedNode = someNode.insertBefore(newNode, null);
alert(newNode == someNode.lastChild); // true

// insert as the new first child
returnedNode = someNode.insertBefore(newNode, someNode.firstChild);
alert(returnedNode == newNode); // true
alert(newNode == someNode.firstChild); // true

// insert before last child
returnedNode = someNode.insertBefore(newNode, someNode.lastChild);
alert(newNode == someNode.childNodes[someNode.childNodes.length - 2]); // true

// replace first child
someNode.replaceChild(newNode, someNode.firstChild);

//remove child
 this.removeChild(e.target);   
```

### Get elements  from dom:

* getElementById
* getElementsByName
* getElementsByTagName
* getElementsByTagNameNS
  * with the given tag name belonging to the given namespace
* getElementsByClassName
* querySelector
* querySelectorAll

###  I can't use forEach or similar array methods on a NodeList?

* Both array and nodeList have length and you can loop through elements but they are not same object.

* Both are inherited from Object. However array has different prototype object than nodeList. forEach, map, etc are on array.prototype which doesn't exist in the NodeList.prototype object. Hence, you don't have forEach

### How to solve previous question?

```text
//Solution 1
let nodesArray = Array.prototype.slice.call(myNodeList);

//Solution 2
let nodesArray = Array.from(myNodeList);

//Solution 3
function sum(a, b) {
  let args = [...arguments];
  console.log(args.reduce((sum, cur) => sum + cur));//args可以调用数组原生的方法啦
}
sum(1, 2);//3

//Solution 4
function sum(a, b) {
  let args = Array.prototype.concat.apply([], arguments);//apply方法会把第二个参数展开
  console.log(args.reduce((sum, cur) => sum + cur));//args可以调用数组原生的方法啦
}
sum(1, 2);//3


```

1.  It create an empty array, then iterate through the object it's running on \(originally an array, now a NodeList\) and keep appending the elements of that object to the empty array it created, which is eventually returned
2. **Array.from\(\)**
3.  **ES6展开运算符**
4. **利用concat+apply**

### How could you make sure to run some javaScript when DOM is ready like $\(document\).ready?

1. Put your script in the last tag of html body element. DOM would be ready by the time browser hits the script tag.
2.  `document.addEventListener('DOMContentLoaded')`
3. Watch changes in the readyState of the document. And the last state is "complete" state, you can put your code there  , either loading, or complete ![](../.gitbook/assets/image%20%2819%29.png) 

### The classList property

```text
div.classList.remove('dsiabled');
div.classList.add('dsiabled');
div.classList.toggle('dsiabled');
```







## 21.Pure functions

* Functions must be highly predictable, Given the same input, it should always return the same output.
* Functions must return exactly value which is determined solely by its input.
* Has no side effects.

## 22. Higher Order Functions

* Higher-Order functions accept functions as parameters or return a function as an output.
* setTimeout, map, reduce

## 23. Currying functions

```text
// Basic example 1
function multiply(a,b,c){
  return a*b*c
}
console.log(multiply(2,3,4))

//curry function for example 1
function multiply(a) {
    return (b) => {
        return (c) => {
            return a * b * c
        }
    }
}
```

* currying transforms a function into a sequence of nesting functions. Basically, it converts a function from `f(a,b,c)`  to this: `f(a)(b)(c)`
* It involves taking a function with multiple arguments and returning a sequence of nested functions, each taking a single argument, eventually resolving to a value.

### 问题:

```text
// Question 1
function currying(func) {
}
function multiply(a, b, c) {
    return a*b*c;
}
// Q1 要求 that takes any function and converts it into a currying function.
let curried = currying(multiply);
curried(2)(3)(4) //24
curried(2,3)(4) //24

// Q1 答案
function currying(func) {
    function curriedfunc(...args) {
        if(args.length >= func.length) {
            return func(...args);
        } else {
            return function(...next) {
                return curriedfunc(...args,...next);
            }
        }
    }
    return curriedfunc;
}
```

### 解析:

* First, we are passing the function, func, to our function, currying \(line 1\).
* The next step is to think about the output. Re-stating the problem, we want to return a currying function. Hence, we define the function curriedfunc , which we will return as an output.
* Then inside curriedfunc, we will check for the length of the arguments \(args\) passed
* If args.length &gt;= func.length, meaning all the arguments are passed, we call func \(the function multiply\)
* However, if all the arguments are not passed, we return a function again to get the remaining arguments.



## 24. Partial functions

* Partial functions allow taking a function as an argument and along with it takes arguments of other types too. It then uses some of the arguments passed and returns a function that will take the remaining arguments. The function returned when invoked will call the parent function with the original and its own set of arguments.

![](../.gitbook/assets/image%20%2820%29.png)

```text
const filter = func => arr => arr.filter(func);
const map = func => arr => arr.map(func);

const funcCompose = (...funcs) => args => funcs.reduceRight((arg, fn) => fn(arg), args);

function test(customers){
  const ans = funcCompose(
  map(x => x.name),
  filter(x => x.age >= 18)
  )(customers)
  return ans
}
```







## 25. forEach中return有效果吗？如何中断forEach循环？

```text
let nums = [1, 2, 3];
nums.forEach((item, index) => {
  return;//无效
})


//Solution 1
let nums = [1, 2, 3];
try{
  nums.forEach((item, index) => {
        throw 'Parameter is not a number!';
})
}catch(e){
    console.log(e);
 }
 
 // solution 2
 
Array.prototype.some()
const array = [1, 2, 3, 4, 5];

// checks whether an element is even
const even = (element) => element % 2 === 0;

console.log(array.some(even));
// expected output: true
```

For each 用 r二turn 不会返回, 函数会继续执行

1. 使用try监视代码块，在需要中断的地方抛出异常。
2. 官方推荐方法（替换方法）：用every和some替代forEach函数。every在碰到return false的时候，中止循环。some在碰到return true的时候，中止循环



## 26.JS判断数组中是否包含某个值

```text
//SOLUTION 1
var arr=[1,2,3,4];
var index=arr.indexOf(3);
console.log(index);

//SOLUTION 2
var arr=[1,2,3,4];
if(arr.includes(3))
    console.log("存在");
else
    console.log("不存在");
    
    
//SOLUTION 3
var arr=[1,2,3,4];
var result = arr.find(item =>{
    return item > 3
});
console.log(result);


// SOLUTION 4

var arr=[1,2,3,4];
var result = arr.findIndex(item =>{
    return item > 3
});
console.log(result);

```

1. **array.indexOf**
2. **array.includes\(searcElement\[,fromIndex\]\)**
3. **array.find\(callback\[,thisArg\]\)  ,** 返回数组中满足条件的第一个元素的值，如果没有，返回undefined
4. **array.findeIndex\(callback\[,thisArg\]\)**    返回数组中满足条件的第一个元素的下标，如果没有找到，返回-1\]



## 27.JS中flat---数组扁平化

```text
需求:多维数组=>一维数组
//SOLUTION 1
ary = ary.flat(Infinity);

//SOLUTION 2
ary = str.replace(/(\[|\])/g, '').split(',')

//SOLUTION 3
str = str.replace(/(\[|\])/g, '');
str = '[' + str + ']';
ary = JSON.parse(str);

//SOLUTION 4
let result = [];
let fn = function(ary) {
  for(let i = 0; i < ary.length; i++) {
    let item = ary[i];
    if (Array.isArray(ary[i])){
      fn(item);
    } else {
      result.push(item);
    }
  }
}



//SOLUTION 5
function flatten(ary) {
    return ary.reduce((pre, cur) => {
        return pre.concat(Array.isArray(cur) ? flatten(cur) : cur);
    }, []);
}
let ary = [1, 2, [3, 4], [5, [6, 7]]]
console.log(flatten(ary))



//SOLUTION 6
ary = ary.flat(Infinity);
```

1. 调用ES6中的flat方法
2. replace + split
3.  replace + JSON.parse
4. 普通递归
5. 利用reduce函数迭代
6. 扩展运算符

## 28. JS中浅拷贝的手段有哪些?

```text
// Solution 1
const shallowClone = (target) => {
  if (typeof target === 'object' && target !== null) {
    const cloneTarget = Array.isArray(target) ? []: {};
    for (let prop in target) {
      if (target.hasOwnProperty(prop)) {
          cloneTarget[prop] = target[prop];// 只是copy reference
      }
    }
    return cloneTarget;
  } else {
    return target;
  }
}


//Solution 2
//但是需要注意的是，Object.assgin() 拷贝的是对象的属性的引用，而不是对象本身。
let obj = { name: 'sy', age: 18 };
const obj2 = Object.assign({}, obj, {name: 'sss'});
console.log(obj2);//{ name: 'sss', age: 18 }
console.log(obj);//{ name: 'sy', age: 18 }


//Solution 3

let arr = [1, 2, 3];
let newArr = arr.concat();//如果第一个parameter 忽略了, 我们就返还一个shallow copy
newArr[1] = 100;
console.log(arr);//[ 1, 2, 3 ]

// Solution 4
let arr = [1, 2, {val: 4}];
let newArr = arr.slice();
newArr[2].val = 1000;

console.log(arr);//[ 1, 2, { val: 1000 } ]

// Solution 5

let arr = [1, 2, 3];
let newArr = [...arr];//跟arr.slice()是一样的效果
```

1.  手动实现
2. Object.assign
3. concat浅拷贝数组
4. slice浅拷贝
5. ...展开运算符

## 29.实现一个 深拷贝

```text
//Solution 1
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
// ======================================

// Solution 2 简易版本

const deepClone = (target) => {
  if (typeof target === 'object' && target !== null) {
    const cloneTarget = Array.isArray(target) ? []: {};
    for (let prop in target) {
      if (target.hasOwnProperty(prop)) {
          //recursive call to copy every object 
          cloneTarget[prop] = deepClone(target[prop]);
      }
    }
    return cloneTarget;
  } else {
    return target;
  }
}

//Solution 3 解决了 循环应用问题
const isObject = (target) => (typeof target === 'object' || typeof target === 'function') && target !== null;
//const deepClone = (target, map = new WeakMap()) 
const deepClone = (target, map = new Map()) => { 
  if(map.get(target))  
    return target; 
 
 
  if (isObject(target)) { 
    map.set(target, true); 
    const cloneTarget = Array.isArray(target) ? []: {}; 
    for (let prop in target) { 
      if (target.hasOwnProperty(prop)) { 
          cloneTarget[prop] = deepClone(target[prop],map); 
      } 
    } 
    return cloneTarget; 
  } else { 
    return target; 
  } 
  
}
const a = {val:2};
a.target = a;
let newA = deepClone(a);
console.log(newA)//{ val: 2, target: { val: 2, target: [Circular] } }


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

1.  `JSON.parse()`
   1. 问题: 无法解决循环引用的问题。举个例子：
   2. 无法拷贝一写特殊的对象，诸如 RegExp, Date, Set, Map等。
   3. 无法拷贝**函数\(function\)**。
2. 简易版本
3. 解决循环应用：
   1. 创建一个Map。记录下已经拷贝过的对象，如果说已经拷贝过，那直接返回它行了。
   2. **这里有一个潜在的坑**： 就是map 上的 key 和 map 构成了强引用关系，这是相当危险的
      1. 被弱引用的对象可以在任何时候被回收，而对于强引用来说，只要这个强引用还在，那么对象无法被回收。拿上面的例子说，map 和 a一直是强引用的关系， 在程序结束之前，a 所占的内存空间一直**不会被释放**
      2. **解决办法：**让 map 的 key 和 map 构成弱引用即可。ES6给我们提供了这样的数据结构，它的名字叫WeakMap，它是一种特殊的Map, 其中的键是弱引用的。其键必须是对象，而值可以是任意的。`const deepClone = (target, map = new WeakMap())` 
4. 解决特殊对象：
   1. 对于特殊的对象，我们使用以下方式来鉴别:`Object.prototype.toString.call(obj);`
   2. 然后 分别处理 可继续遍历的， 和 不可遍历的对象
   3. 不可遍历对象 ， 不同对象有不同的处理
5. 解决拷贝函数问题
   1. 一种是普通函数，另一种是箭头函数。每个普通函数都是 Function的实例，而箭头函数不是任何类的实例，每次调用都是不一样的引用。那我们只需要 处理普通函数的情况，箭头函数直接返回它本身就好了。
   2. 那么如何来区分两者呢？
      1. 利用原型。箭头函数是不存在原型的。
6. [别人的 post](https://juejin.cn/post/6975880204447121422/#heading-15)



## 30. Asynchronous Callbacks

### 异步编程 有哪些方案

```text
// 回调函数时代
fs.readFile('1.json', (err, data) => {
    fs.readFile('2.json', (err, data) => {
        fs.readFile('3.json', (err, data) => {
            fs.readFile('4.json', (err, data) => {

            });
        });
    });
});


// Promise 时代
readFilePromise('1.json').then(data => {
    return readFilePromise('2.json')
}).then(data => {
    return readFilePromise('3.json')
}).then(data => {
    return readFilePromise('4.json')
});

// async + await方式
const readFileAsync = async function () {
  const f1 = await readFilePromise('1.json')
  const f2 = await readFilePromise('2.json')
  const f3 = await readFilePromise('3.json')
  const f4 = await readFilePromise('4.json')
}

```

1. 回调函数时代: 回调当中嵌套回调，也称回调地狱。这种代码的可读性和可维护性都是非常差的，因为嵌套的层级太多
2. Promise 时代
3. async + await方式

###  解释一下async/await的运行机制

```text

// await 200 变成 下面这个 function了

function resolveAfter0Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('200');
    }, 0);
  });
}

async function asyncCall() {
  console.log('calling');
  const result = await resolveAfter2Seconds();
  console.log(result);
  console.log(300)
}
console.log(1);
asyncCall();
console.log(100)
// 1
// "calling"
// 100
// "200"
// 300
```

* [https://juejin.cn/post/6844904004007247880\#heading-67](https://juejin.cn/post/6844904004007247880#heading-67)

###   forEach 中用 await 会产生什么问题?怎么解决这个问题？, 

```text
async function test() {
	let arr = [4, 2, 1]
	arr.forEach(async item => {
		const res = await handle(item)
		console.log(res)
	})
	console.log('结束')
}

function handle(x) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			resolve(x)
		}, 1000 * x)
	})
}

test()


```

* 问题:对于异步代码，forEach 并不能保证按顺序执行。
* * 我们期望 4，2，1 但其实得到 1，2，4
* 问题原因
  * foreach 底层 是拿来 直接执行了   ![](../.gitbook/assets/image%20%2827%29.png) 
* 解决办法： 使用 for loop 就好了
* 解决原理：for...of并不像forEach那么简单粗暴的方式去遍历执行，而是采用一种特别的手段——迭代器去遍历。

```text
async function test() {
  let arr = [4, 2, 1]
  let iterator = arr[Symbol.iterator]();
  let res = iterator.next();
  while(!res.done) {
    let value = res.value;
    console.log(value);
    await handle(value);
    res = iterater.next();
  }
	console.log('结束')
}
// 4
// 2
// 1
// 结束


```





## 30. Promise:

### Promise 凭借什么消灭了回调地狱（callback hell）？

```text
//Solution 1
let readFilePromise = (filename) => {
  fs.readFile(filename, (err, data) => {
    if(err) {
      reject(err);
    }else {
      resolve(data);
    }
  })
}
readFilePromise('1.json').then(data => {
  return readFilePromise('2.json')
});


//Solution 2
let x = readFilePromise('1.json').then(data => {
  return readFilePromise('2.json')//这是返回的Promise
});
x.then(/* 内部逻辑省略 */)


// Solution 3
readFilePromise('1.json').then(data => {
    return readFilePromise('2.json');
}).then(data => {
    return readFilePromise('3.json');
}).then(data => {
    return readFilePromise('4.json');
}).catch(err => {
  // xxx
})


```

* 多层嵌套的问题。complex nested callbacks
* 每种任务的处理结果存在两种可能性（成功或失败），那么需要在每种任务执行结束后分别处理这两种可能性。
* each and every callback takes an argument that is a result of the previous callbacks.
* 使用 3大手段 解决 callback hell
  * 回调函数延迟绑定 use `.then()`  回调函数不是直接声明的，而是在通过后面的 then 方法传入的，即延迟传入。这就是回调函数延迟绑定。
  * 返回值穿透: 我们会根据 then 中回调函数的传入值创建不同类型的Promise, 然后把返回的 Promise 穿透到外层, 以供后续的调用。这里的 x 指的就是内部返回的 Promise，然后在 x 后面可以依次完成链式调用
  * 错误冒泡  use .catch\(\)





























\*\*\*\*





















