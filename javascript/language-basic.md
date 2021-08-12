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



//===== EG2  =====
console.log(x);//输出：function x(){}
var x=1;
function x(){}
//变成
var x;
function x(){}
console.log(x);
x=1;


// ==== =EG3 ======
console.log(foo);
var foo = 1;
console.log(foo);
function foo(){};
// 变成了

foo() 			  //函数提升
var foo			  //和函数重名了，被忽略
console.log(foo);	  //打印函数
foo = 1;		  //全局变量foo
console.log(foo);	  //打印1，事实上函数foo已经不存在了，变成了1
```

### 解析：

#### 1: 

* Output 是 undefined
* At compile time, the var temp is hoisted to the top of the display\(\) function.

![](../.gitbook/assets/image%20%283%29.png)

### Order of precedence

```text
// Variable assignment takes precedence over function declaration
var double = 22;

function double(num) {
  return (num*2);
}

console.log(typeof double); // Output: number


// Function declarations over variable declarations

function double(num) {
  return (num*2);
}
var double;

console.log(typeof double); // Output: function



// 例子 
console.log(foo);

function foo(){
    console.log("foo");
}

var foo = 1;
// 变成了
function foo(){// 函数提升
    console.log("foo");
}
var foo;// 变量提升
console.log(foo);
foo = 1;
```

具体原因看 32 小结的变量对象

* Variable assignment takes precedence over function declaration
* Function declarations take precedence over variable declarations









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

* It tell the type of a given variable, used them for **primitive type**  data 数据类型
* new 创建的都是 object

![](../.gitbook/assets/image%20%2899%29.png)

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
* 十进制0.1转换成二进制，**乘2取整过程**
* 从上面可以看出，0.1的二进制格式是：0.0001100011....。我们永远不会得到一个整数结果，这是一个二进制无限循环小数，但计算机内存有限，我们不能用储存所有的小数位数。那么在精度与内存间如何取舍呢？
* 在某个精度点直接舍弃。当然，代价就是，0.1在计算机内部根本就不是精确的0.1，而是一个有舍入误差的0.1。当代码被编译或解释后，0.1已经被四舍五入成一个与之很接近的计算机内部数字，以至于计算还没开始，一个很小的舍入错误就已经产生了。这也就是 0.1 + 0.2 不等于0.3 的原因。
* **不要直接比较两个浮点的大小**：
* **JS中如何进入浮点数运算 :**

![](../.gitbook/assets/image%20%28100%29.png)

#### 解决办法

```text

// 方法 1
{
  let x = new BigNumber(0.1);
  let y = new BigNumber(0.2)
  let z = new BigNumber(0.3)

  console.log(z.equals(x.add(y))) // 0.3 === 0.1 + 0.2, true
  console.log(z.minus(x).equals(y)) // true
  console.log(z.minus(y).equals(x)) // true
}
// 方法 2
parseFloat((0.1+ 0.2).toFixed(5))
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

###  Type conversion:

![https://juejin.cn/post/6844903974378668039\#heading-14](../.gitbook/assets/image%20%2822%29.png)

* toString\(\),转换成字符串
* parseInt,转换成数字
  * True -&gt; 1
  * False -&gt; 0
  * Undefined, 或者失误转换为 NaN
  * Null -&gt; 0
* new Boolean\(\),转换成布尔值
  * false value will return false;

### **Type concatenate:**

当计算 value1 + value2时：

1. lprim = ToPrimitive\(value1\)
2. rprim = ToPrimitive\(value2\)
3. 如果 lprim 是字符串或者 rprim 是字符串，那么返回 ToString\(lprim\) 和 ToString\(rprim\)的拼接结果
4. 返回 ToNumber\(lprim\) 和 ToNumber\(rprim\)的运算结果

* 如果+的 其中一个操作数时字符串， 则 concatenate， 否则 数字加法

```text
var a = '42';
var b ='0';
var c = 42;
var d = 0;
console.log(a+b); //420
```

![](../.gitbook/assets/image%20%28128%29.png)

![](../.gitbook/assets/image%20%28129%29.png)

![](../.gitbook/assets/image%20%28130%29.png)

 

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

### typeof 和 instanceof 区别

![](../.gitbook/assets/image%20%2898%29.png)

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

#### [https://github.com/mqyqingfeng/Blog/issues/164](https://github.com/mqyqingfeng/Blog/issues/164)

#### 



### 原始值转布尔

```text
console.log(Boolean()) // false

console.log(Boolean(false)) // false

console.log(Boolean(undefined)) // false
console.log(Boolean(null)) // false
console.log(Boolean(+0)) // false
console.log(Boolean(-0)) // false
console.log(Boolean(NaN)) // false
console.log(Boolean("")) // false
```

* 使用Boolean 函数将类型转换成boolean

### 原始值转数字

![](../.gitbook/assets/image%20%28125%29.png)

* 我们可以使用 Number 函数将类型转换成数字类型，如果参数无法被转换为数字，则返回 NaN。
* 调用了ToNumber\(\)

```text
console.log(Number()) // +0

console.log(Number(undefined)) // NaN
console.log(Number(null)) // +0

console.log(Number(false)) // +0
console.log(Number(true)) // 1

console.log(Number("123")) // 123
console.log(Number("-123")) // -123
console.log(Number("1.2")) // 1.2
console.log(Number("000123")) // 123
console.log(Number("-000123")) // -123

console.log(Number("0x11")) // 17

console.log(Number("")) // 0
console.log(Number(" ")) // 0

console.log(Number("123 123")) // NaN
console.log(Number("foo")) // NaN
console.log(Number("100a")) // NaN



console.log(parseInt("3 abc")) // 3
console.log(parseFloat("3.14 abc")) // 3.14
console.log(parseInt("-12.34")) // -12
console.log(parseInt("0xFF")) // 255
console.log(parseFloat(".1")) // 0.1
console.log(parseInt("0.1")) // 0
```

* 如果通过 Number 转换函数传入一个字符串，它会试图将其转换成一个整数或浮点数，而且会忽略所有前导的 0，如果有一个字符不是数字，结果都会返回 NaN，鉴于这种严格的判断，我们一般还会使用更加灵活的 parseInt 和 parseFloat 进行转换。
* parseInt 只解析整数，parseFloat 则可以解析整数和浮点数，如果字符串前缀是 "0x" 或者"0X"，parseInt 将其解释为十六进制数，parseInt 和 parseFloat 都会跳过任意数量的前导空格，尽可能解析更多数值字符，并忽略后面的内容。如果第一个非空格字符是非法的数字直接量，将最终返回 NaN：

### 原始值转字符

![](../.gitbook/assets/image%20%28126%29.png)

* 我们使用 String 函数将类型转换成字符串类型,调用了ToString\(）

```text
console.log(String()) // 空字符串

console.log(String(undefined)) // undefined
console.log(String(null)) // null

console.log(String(false)) // false
console.log(String(true)) // true

console.log(String(0)) // 0
console.log(String(-0)) // 0
console.log(String(NaN)) // NaN
console.log(String(Infinity)) // Infinity
console.log(String(-Infinity)) // -Infinity
console.log(String(1)) // 1
```

### 原始值转对象

* 原始值通过调用 String\(\)、Number\(\) 或者 Boolean\(\) 构造函数，转换为它们各自的包装对象。

### ToPrimitive

```text
ToPrimitive(input[, PreferredType])
```

第一个参数是 input，表示要处理的输入值。

第二个参数是 PreferredType，非必填，表示希望转换成的类型，有两个值可以选，Number 或者 String。

当不传入 PreferredType 时，如果 input 是日期类型，相当于传入 String，否则，都相当于传入 Number。

如果传入的 input 是 Undefined、Null、Boolean、Number、String 类型，直接返回该值。

![](../.gitbook/assets/image%20%28124%29.png)



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
* Inherits the value of **this** from enclosing scope, meaning since they don’t have their own context in which they execute, this gets inherited from the parent function. Hence, they don’t have their own this value.
* 没有 new 和 原型
* 也不能使用 call apply bind 修改 this

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


var obj = {
    name: '若川',
};
function original(a, b){
    console.log(this.name);
    console.log([a, b]);
    return false;
}
var bound = original.bind(obj, 1);
var boundResult = bound(2); // '若川', [1, 2]
console.log(boundResult); // false
console.log(original.bind.name); // 'bind'
console.log(original.bind.length); // 1
console.log(original.bind().length); // 2 返回original函数的形参个数
console.log(bound.name); // 'bound original'
console.log((function(){}).bind().name); // 'bound '
console.log((function(){}).bind().length); // 0



```

* Definded in the ECMAScript 5, It create a new function instance whose this value is bound to the value that was passed into bind\(\);
* a new function called objectSayColor\(\) is created from sayColor\(\) by calling bind\(\) and   passing in the object o. The objectSayColor\(\) function has a this value equivalent to o,
*  `bind`本身是一个函数名为`bind`的函数，返回值也是函数，函数名是`bound`。（打出来就是`bound加上一个空格`）
*  传给`bind()`的其他参数接收处理了，`bind()`之后返回的函数的参数也接收处理了，也就是说合并处理了
*  并且`bind()`后的`name`为`bound + 空格 + 调用bind的函数名`。如果是匿名函数则是`bound + 空格`。
*  `bind`后的返回值函数，执行后返回值是原函数（`original`）的返回值。

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

* The first argument is the **this** value, but the remaining arguments are passed directly into the   function. 

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

* apply只接收两个参数，第二个参数可以是数组也可以是类数组，其实也可以是对象，后续的参数忽略不计

### What is difference between call apply?

* apply requires an array as its second argument wheres call requires the parameters to be listed explicitly.
* **thisArg ,**如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装。



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

```text
//展示了 data privacy
function createStack() {
  const items = [];
  return {
    push(item) {
      items.push(item);
    },
    pop() {
      return items.pop();
    }
  };
}


```



### 闭包是什么时候被销毁的？

* 在 Javascript 中，局部变量会随着函数的执行完毕而被销毁，除非还有指向他们的引用。当闭包本身也被垃圾回收之后，这些闭包中的私有状态随后也会被垃圾回收。通常我们可以通过切断闭包的引用来达到这一目的

```text
let add = (function createAddClosure(){
    let arr = [];
    return function(obj){
       arr.push(obj);
    }
})();

function addALotOfObjects(){
    for(let i=1; i<=10000;i++) {
       add(new Todo(i));
    }
}
function clearAllObjects(){
    if(add){
       add = null; // 切断闭包引用，防止内存泄露
    }
}
$("#add").click(addALotOfObjects);
$("#clear").click(clearAllObjects);


```

### 

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

### 问题 1

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
}// i 被当作新的 argument 传进去了

// Solution 4 JS 中 基本类型是 pass by value
var output = function (i) {
    setTimeout(function() {
        console.log(i);
    }, 1000);
};

for (var i = 0; i < 5; i++) {
    output(i);  // 这里传过去的 i 值被复制了
}


// Method 4


var output = function (i) {
    setTimeout(function() {
        console.log(new Date, i);
    }, 1000);
};

for (var i = 0; i < 5; i++) {
    output(i);  // 这里传过去的 i 值被复制了
}

console.log(new Date, i);



```

#### 解析

1. Output:  Element: undefined, at index:4     乘4
   1. on each iteration, the setTimeout will be triggered,since it’s an asynchronous web API, the command enters the event queue, after which the next loop iteration occurs. Hence, the event queue waits for the loop commands to execute first and call stack to get empty, after which the four setTimeout commands move from the event queue to call stack and execute.
2.  There are 4  possible solution:
   1. Using Es6 featre, **let**   It  creates a new binding for each loop iteration, each i refers to the binding of one specific iteration and preserves the value that was current at that time
   2. **Using IFFE**: That function takes the parameter local\_i, that is the variable i. It calls another function in return, an anonymous function that displays the value of i stored in the variable local\_i
   3. 给定时器传入第三个参数, 作为timer函数的第一个函数参数
   4. JS 中 基本类型是 pass by value

### 问题2 ：每一秒输出数字题目

```text
// Method 1
for (var i = 0; i < 5; i++) {
    (function(j) {
        setTimeout(function() {
            console.log(new Date, j);
        }, 1000 * j);  // 这里修改 0~4 的定时器时间
    })(i);
}

setTimeout(function() { // 这里增加定时器，超时设置为 5 秒
    console.log(new Date, i);
}, 1000 * i);




// ==== Method 2 better 写法 ====
const tasks = []; // 这里存放异步操作的 Promise
const output = (i) => new Promise((resolve) => {
    setTimeout(() => {
        console.log(new Date, i);
        resolve();
    }, 1000 * i);
});

// 生成全部的异步操作
for (var i = 0; i < 5; i++) {
    tasks.push(output(i));
}

// 异步操作完成之后，输出最后的 i
Promise.all(tasks).then(() => {
    setTimeout(() => {
        console.log(new Date, i);
    }, 1000);
});


// ==== Method3 使用 asaync/await ====


const sleep = (timeountMS) => new Promise((resolve) => {
    setTimeout(resolve, timeountMS);
});

(async () => {  // 声明即执行的 async 函数表达式
    for (var i = 0; i < 5; i++) {
        if (i > 0) {
            await sleep(1000);
        }
        console.log(new Date, i);
    }

    await sleep(1000);
    console.log(new Date, i);
})();



```

### 问题 3

```text
// 3.1
let x = 1
function A(y) {
  let x = 2
  function B(z) {
    console.log(x + y + z)
  }
  return B
}
let C = A(2)
C(3)

// 3.1 ： 7
```

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



// 情况1: 全局上下文
console.log(window === this); // true
var a = 1;
this.b = 2;
window.c = 3;
console.log(a + b + c); // 6

// 情况2: 函数上下文
// 直接调用
function foo(){
  return this;
}
console.log(foo() === window); // true

// 箭头函数
function Person(name){
  this.name = name;
  this.say = () => {
    var name = "xb";
    return this.name;
  }
}
var person = new Person("axuebin");
console.log(person.say()); // axuebin

// 情况3:   this指向调用函数的对象
var person = {
  name: "axuebin",
  getName: function(){
    return this.name;
  }
}
console.log(person.getName()); // axuebin

```

**总共5种情况**

* **情况1: 全局上下文**
  * `var` === `this.` === `winodw.`
* **情况2: 函数上下文**  When a function is not   defined using the arrow syntax, the this object is bound at runtime based on the context in which   a function is executed: 
  * **直接调用** when used inside global functions, this is equal to **window in nonstrict** mode     and **undefined in strict mode**  
  * **call, apply, this 指向 绑定的对象上**
  * **箭头函数**
    * **箭头函数会捕获其所在上下文的this值，作为自己的this值。**
    * **箭头函数中没有this绑定，必须通过查找作用域链来决定其值。 如果箭头函数被非箭头函数包含，则this绑定的是最近一层非箭头函数的this，否则this的值则被设置为全局对象**
    * 其实就是相当于箭头函数外的this是缓存的该箭头函数上层的普通函数的this。如果没有普通函数，则是全局对象（浏览器中则是window）。

      也就是说**无法通过call、apply、bind绑定箭头函数的this\(它自身没有this\)**。而call、apply、bind可以绑定缓存箭头函数上层的普通函数的this

    * 箭头函数并不绑定 this，arguments，super\(ES6\)，抑或 new.target\(ES6\)。
* **情况3:   this指向调用函数的对象。**
  * this is equal to the object when called as an object method   `obj.a();`
* **情况4: this被绑定到正在构造的新对象。**
* **情况5: 作为一个DOM事件处理函数**
  * Dom 事件绑定, onclick和addEventerListener中 this 默认指向绑定事件的元素。
* **Ex1:**  getIdentityFunc\(\) returns a function, so calling this function immediately call the function that is returned.
  * Each function automatically gets two special variables as soon as the function is

    called: **this and arguments.** An inner function can never access these variables directly from an outer     function

  * Before     defining the anonymous function, a variable named that is assigned equal to the this object. When     the closure is defined, it has access to that because it is a uniquely named variable in the containing     function. Even after the function is returned, that is still bound to object, so calling object.getIdentityFunc\(\)  returns 'My Object'.
* Both **this and arguments** behave in this way. If you want access to a   containing scope’s arguments object, you’ll need to save a reference into another   variable that the closure can access.



### this 优先级

_**如果要判断一个运行中函数的 this 绑定， 就需要找到这个函数的直接调用位置**_。 找到之后 就可以顺序应用下面这四条规则来判断 this 的绑定对象

1.  `new` 调用：绑定到新创建的对象，注意：显示`return`函数或对象，返回值不是新创建的对象，而是显式返回的函数或对象
2. `call` 或者 `apply`（ 或者 `bind`） 调用：严格模式下，绑定到指定的第一个参数。非严格模式下，`null`和`undefined`，指向全局对象（浏览器中是`window`），其余值指向被`new Object()`包装的对象。
3. 对象上的函数调用：绑定到那个对象。
4. 普通函数调用： 在严格模式下绑定到 undefined，否则绑定到全局对象。

ES6 中的箭头函数：不会使用上文的四条标准的绑定规则，**而是箭头函数的this指向的是谁调用箭头函数的外层function，箭头函数的this就是指向该对象，如果箭头函数没有外层函数，则指向window**

DOM事件函数：一般指向绑定事件的DOM元素，但有些情况绑定到全局对象（比如IE6~IE8的attachEvent）

### 相关问题

```text
// 问题 1
var name = 'window';
var person = {
    name: 'person',
}
var doSth = function(){
    console.log(this.name);
    return function(){
        console.log('return:', this.name);
    }
}
var Student = {
    name: '若川',
    doSth: doSth,
}
// 普通函数调用
doSth(); // window
// 对象上的函数调用
Student.doSth(); // '若川'
// call、apply 调用
Student.doSth.call(person); // 'person'
```

{% embed url="https://www.cnblogs.com/xxcanghai/p/5189353.html" %}

{% embed url="https://segmentfault.com/a/1190000010981003" %}



```text
/**
 * Question 1
 */

var name = 'window'

var person1 = {
  name: 'person1',
  show1: function () {
    console.log(this.name)
  },
  show2: () => console.log(this.name),
  show3: function () {
    return function () {
      console.log(this.name)
    }
  },
  show4: function () {
    return () => console.log(this.name)
  }
}
var person2 = { name: 'person2' }

person1.show1() //person1
person1.show1.call(person2)// person2

person1.show2()//window
person1.show2.call(person2) //window

person1.show3()()//window
person1.show3().call(person2)//person2
person1.show3.call(person2)()//window

person1.show4()()// person1
person1.show4().call(person2)// person1
person1.show4.call(person2)()// person2



/**
 * Question 2
 */
var name = 'window'

function Person (name) {
  this.name = name;
  this.show1 = function () {
    console.log(this.name)
  }
  this.show2 = () => console.log(this.name)
  this.show3 = function () {
    return function () {
      console.log(this.name)
    }
  }
  this.show4 = function () {
    return () => console.log(this.name)
  }
}

var personA = new Person('personA')
var personB = new Person('personB')

personA.show1() // personA
personA.show1.call(personB) // personB

personA.show2() // personA
personA.show2.call(personB) // personA

personA.show3()() // window
personA.show3().call(personB) // personB
personA.show3.call(personB)() // window

personA.show4()() // personA
personA.show4().call(personB) // personA
personA.show4.call(personB)() // personB


```



## 15. Call Stack & Event Loop

![](../.gitbook/assets/image%20%2818%29.png)

* call stack
  * stores all your running JavaScript code. The interpreter reads the code line-by-line
* event table
  *  This table is responsible for moving the asynchronous code to the event queue after a specified time
* event loop:
  * Itt is responsible for keeping check of both the call stack and the event queue. It keeps checking if all the statements from the call stack have finished execution; that is, if the call stack is empty. If so, it pops the statement from the event queue \(if present\) to the call stack to execute.

### event loop 执行顺序

* 一开始整个脚本作为一个宏任务执行
* 执行过程中同步代码直接执行，宏任务进入宏任务队列，微任务进入微任务队列
* 当前宏任务执行完出队，检查微任务列表，有则依次执行，直到全部执行完
* 执行浏览器UI线程的渲染工作
* 检查是否有Web Worker任务，有则执行
  * Web Workers 可以与主线程交换消息，但是它们具有自己的变量和事件循环。
  * Web Workers are a simple means for web content to run scripts in background thread
* 执行完本轮的宏任务，回到2，依此循环，直到宏任务和微任务队列都为空

### 执行机制

![](../.gitbook/assets/image%20%2828%29.png)

![](../.gitbook/assets/image%20%28133%29.png)

* 微任务会在执行任何其他事件处理，或渲染，或执行任何其他宏任务之前完成。
* 对于每一个 宏任务而言， 其内部都有一个微任务队列，当该宏任务执行完成，会检查其中的微任务队列，如果为空则直接执行下一个宏任务，如果不为空，则依次执行微任务，执行完成才去执行下一个宏任务
* Macro task queue:宏任务
  * script，setTimeout，setImmediate，promise中的executor
* Micro Task queue\(Callback queue\):
  * MutationObserver，process.nextTick    、Promise.then\(或.reject\) 以及以 Promise 为基础开发的其他技术\(比如fetch API\)，
* **process.nextTick优先级高于Promise.then**

### 问题 1 ：

```text
//Q1
console.log("Before Function")
setTimeout(function(){
  console.log("Inside Function")
},3000)
console.log("After Function")
// Q1 answer: 
//Before function, After Function , Inside Function
```

### 问题 2 ：

```text
// Q2
console.log('start');
setTimeout(() => {
  console.log('timeout');
});
Promise.resolve().then(() => {
  console.log('resolve');
});
console.log('end');
```

1. 一开始整个脚本 都作为第一个宏任务执行先打印 start 和 end
2. setTimeout 作为一个宏任务放入宏任务队列（MacroTask）
3. Promise.then作为一个为微任务放入到微任务队列
4. 当本次宏任务执行完，检查微任务队列，发现一个Promise.then, 执行
5. 接下来进入到下一个宏任务——setTimeout, 执行

### 问题 3：

```text
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

// 答案
// start
// Promise1
// setTimeout1
// Promise2
// setTimeout2
```

第一轮

Print:  start

Micro queue: promise1

Macro queue:settimeout1

第二轮

Print:  start promise

Micro queue: 

Macro queue:settimeout1 settimeout2

第三轮

micro已经没了 们执行下一个 macro,并且 把 Promise2 放进micro

Print:  start promise1 settimeout1

Micro queue: promise2

Macro queue: settimeout2

第四轮

Print:  start promise1 settimeout1promise2

Micro queue: 

Macro queue: settimeout2

第五轮

执行settimout2

Print:  start promise1 settimeout1promise2 setTimeout2

Micro queue: 

Macro queue: 

### 问题 4：

```text
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
```

* **Promise的executor是一个同步函数，即非异步，立即执行的一个函数，因此他应该是和当前的任务一起执行的**

### 问题 5：

```text
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

// Solution 5 

// "promise1"
// "then11"
// "promise2"
// "then21"
// "then12"
// "then23"

```



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

###  问题 6：

```text
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

// Solution 6


// "promise1"
// "then11"
// "promise2"
// "then21"
// "then23"
// "then12"
```

* 这个考的重点在于Promise而非Eventloop了。这里就很好理解为何then12会在then23之后执行，这里Promise的第二个then相当于是挂在新Promise的最后一个then的返回值上

### 问题 7：

```text

//多个 promise
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

// solution

//[promise1,promise3,then11,promise2,then31,then21,then12,then23]
```

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

### 问题 8：

```text
console.log('1');
// settimout1
setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

// settimout2
setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})

//1，7，6，8，2，4，3，5，9，11，10，12
```

{% embed url="https://juejin.cn/post/6844903512845860872\#heading-4" %}

### 问题 9：

```text
const first = () => (new Promise((resolve,reject)=>{
    console.log(3);
    let p = new Promise((resolve, reject)=>{
         console.log(7);
        setTimeout(()=>{
           console.log(5);
           resolve(6); 
        },0)
        resolve(1);
    }); 
    resolve(2);
    p.then((arg)=>{
        console.log(arg);
    });

}));

first().then((arg)=>{
    console.log(arg);
});
console.log(4);

// 3-7-4-1-2-5
```

* **第一轮事件循环**
  * 先执行**宏任务**，主script ，new Promise立即执行，输出【3】，执行p这个new Promise 操作，输出【7】，发现setTimeout，将回调放入下一轮任务队列（Event Queue），p的then，姑且叫做then1，放入**微任务队列**，发现first的then，叫then2，放入**微任务队列**。执行console.log\(4\)，输出【4】,宏任务执行结束。再执行**微任务**，执行then1，输出【1】，执行then2，输出【2】。到此为止，第一轮事件循环结束。开始执行第二轮。
* **第二轮事件循环**
  * 先执行宏任务里面的，也就是setTimeout的回调，输出【5】。resovle不会生效，因为p这个Promise的状态一旦改变就不会在改变了。 所以最终的输出顺序是3、7、4、1、2、5。



```text

//8. 




// 9


```

### 问题 10:

```text
process.nextTick(() => {
  console.log('nextTick')
})
Promise.resolve()
  .then(() => {
    console.log('then')
  })
setImmediate(() => {
  console.log('setImmediate')
})
console.log('end')
// end
// nextTick
// then
// setImmediate
```

* `process.nextTick` 和 `promise.then` 都属于 microtask，而 `setImmediate` 属于 macrotask，在事件循环的 check 阶段执行。事件循环的每个阶段（macrotask）之间都会执行 microtask，事件循环的开始会先执行一次 microtask。



### 问题 11：

```text
const promise = new Promise((resolve, reject) => {
  console.log(1);
  resolve('success')
  console.log(2);
});
promise.then(() => {
  console.log(3);
});
console.log(4);

```

* 从上至下，先遇到`new Promise`，执行其中的同步代码`1`
* 再遇到`resolve('success')`， 将`promise`的状态改为了`resolved`并且将值保存下来
* 继续执行同步代码`2`
* 跳出`promise`，往下执行，碰到`promise.then`这个微任务，将其加入微任务队列
* 执行同步代码`4`
* 本轮宏任务全部执行完毕，检查微任务队列，发现`promise.then`这个微任务且状态为`resolved`，执行它。

### 题目 12：

```text
const promise = new Promise((resolve, reject) => {
  console.log(1);
  console.log(2);
});
promise.then(() => {
  console.log(3);
});
console.log(4);

// 1 2 4
```

* 只不过在`promise`中并没有`resolve`或者`reject`
* 因此`promise.then`并不会执行，它只有在被改变了状态之后才会执行。

### 题目· 13：

```text
const fn = () => (new Promise((resolve, reject) => {
  console.log(1);
  resolve('success')
}))
fn().then(res => {
  console.log(res)
})
console.log('start')

// 1
// 'start'
// 'success'
```

*  `fn`函数它是直接返回了一个`new Promise`的，而且`fn`函数的调用是在`start`之前，所以它里面的内容应该会先执行

###  题目 14:

```text
const fn = () =>
  new Promise((resolve, reject) => {
    console.log(1);
    resolve("success");
  });
console.log("start");
fn().then(res => {
  console.log(res);
});
```

* 是的，现在start就在1之前打印出来了，因为fn函数是之后执行的。

### 题目 15:

```text
const promise = new Promise((resolve, reject) => {
  console.log(1);
  setTimeout(() => {
    console.log("timerStart");
    resolve("success");
    console.log("timerEnd");
  }, 0);
  console.log(2);
});
promise.then((res) => {
  console.log(res);
});
console.log(4);

// 1
// 2
// 4
// "timerStart"
// "timerEnd"
// "success"
```

* 从上至下，先遇到`new Promise`，执行该构造函数中的代码`1`
* 然后碰到了定时器，将这个定时器中的函数放到下一个宏任务的延迟队列中等待执行
* 执行同步代码`2`
* 跳出`promise`函数，遇到`promise.then`，但其状态还是为`pending`，这里理解为先不执行
* 执行同步代码`4`
* 一轮循环过后，进入第二次宏任务，发现延迟队列中有`setTimeout`定时器，执行它
* 首先执行`timerStart`，然后遇到了`resolve`，将`promise`的状态改为`resolved`且保存结果并将之前的`promise.then`推入微任务队列
* 继续执行同步代码`timerEnd`
* 宏任务全部执行完毕，查找微任务队列，发现`promise.then`这个微任务，执行它。

### 题目 16：

```text
setTimeout(() => {
  console.log('timer1');
  setTimeout(() => {
    console.log('timer3')
  }, 0)
}, 0)
setTimeout(() => {
  console.log('timer2')
}, 0)
console.log('start')

// "start"
// "timer1"
// "timer2"
// "timer3"

setTimeout(() => {
  console.log('timer1');
  Promise.resolve().then(() => {
    console.log('promise')
  })
}, 0)
setTimeout(() => {
  console.log('timer2')
}, 0)
console.log('start')

// 'start'
// 'timer1'
// 'promise'
// 'timer2'
```

### 题目 17：

```text
Promise.resolve().then(() => {
  console.log('promise1');
  const timer2 = setTimeout(() => {
    console.log('timer2')
  }, 0)
});
const timer1 = setTimeout(() => {
  console.log('timer1')
  Promise.resolve().then(() => {
    console.log('promise2')
  })
}, 0)
console.log('start');

// "start"
// "promise1"
// "timer1"
// "promise2"
// "timer2"

```

* 刚开始整个脚本作为第一次宏任务来执行，我们将它标记为**宏1**，从上至下执行
* 遇到`Promise.resolve().then`这个微任务，将`then`中的内容加入第一次的微任务队列标记为**微1**
* 遇到定时器`timer1`，将它加入下一次宏任务的延迟列表，标记为**宏2**，等待执行\(先不管里面是什么内容\)
* 执行**宏1**中的同步代码`start`
* 第一次宏任务\(**宏1**\)执行完毕，检查第一次的微任务队列\(**微1**\)，发现有一个`promise.then`这个微任务需要执行
* 执行打印出**微1**中同步代码`promise1`，然后发现定时器`timer2`，将它加入**宏2**的后面，标记为**宏3**
* 第一次微任务队列\(**微1**\)执行完毕，执行第二次宏任务\(**宏2**\)，首先执行同步代码`timer1`
* 然后遇到了`promise2`这个微任务，将它加入此次循环的微任务队列，标记为**微2**
* **宏2**中没有同步代码可执行了，查找本次循环的微任务队列\(**微2**\)，发现了`promise2`，执行它
* 第二轮执行完毕，执行**宏3**，打印出`timer2`

  
  


![](../.gitbook/assets/image%20%28132%29.png)

### 题目 18：

```text
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('success')
  }, 1000)
})
const promise2 = promise1.then(() => {
  throw new Error('error!!!')
})
console.log('promise1', promise1)
console.log('promise2', promise2)
setTimeout(() => {
  console.log('promise1', promise1)
  console.log('promise2', promise2)
}, 2000)


// 'promise1' Promise{<pending>}
// 'promise2' Promise{<pending>}
// test5.html:102 Uncaught (in promise) Error: error!!! at test.html:102
// 'promise1' Promise{<resolved>: "success"}
//  'promise2' Promise{<rejected>: Error: error!!!}


```

* 从上至下，先执行第一个`new Promise`中的函数，碰到`setTimeout`将它加入下一个宏任务列表
* 跳出`new Promise`，碰到`promise1.then`这个微任务，但其状态还是为`pending`，这里理解为先不执行
* `promise2`是一个新的状态为`pending`的`Promise`
* 执行同步代码`console.log('promise1')`，且打印出的`promise1`的状态为`pending`
* 执行同步代码`console.log('promise2')`，且打印出的`promise2`的状态为`pending`
* 碰到第二个定时器，将其放入下一个宏任务列表
* 第一轮宏任务执行结束，并且没有微任务需要执行，因此执行第二轮宏任务
* 先执行第一个定时器里的内容，将`promise1`的状态改为`resolved`且保存结果并将之前的`promise1.then`推入微任务队列
* 该定时器中没有其它的同步代码可执行，因此执行本轮的微任务队列，也就是`promise1.then`，它抛出了一个错误，且将`promise2`的状态设置为了`rejected`
* 第一个定时器执行完毕，开始执行第二个定时器中的内容
* 打印出`'promise1'`，且此时`promise1`的状态为`resolved`
* 打印出`'promise2'`，且此时`promise2`的状态为`rejected`

###  题目 19：

```text
const promise = new Promise((resolve, reject) => {
  resolve("success1");
  reject("error");
  resolve("success2");
});
promise
.then(res => {
    console.log("then: ", res);
  }).catch(err => {
    console.log("catch: ", err);
  }

// "then: success1"

```

* 构造函数中的 resolve 或 reject 只有第一次执行有效，多次调用没有任何作用 

### 题目 20：

```text
const promise = new Promise((resolve, reject) => {
  reject("error");
  resolve("success2");
});
promise
.then(res => {
    console.log("then1: ", res);
  }).then(res => {
    console.log("then2: ", res);
  }).catch(err => {
    console.log("catch: ", err);
  }).then(res => {
    console.log("then3: ", res);
  })

// "catch: " "error"
// "then3: " undefined

```

* catch不管被连接到哪里，都能捕获上层未捕捉过的错误

### 题目 21：

```text
Promise.resolve(1)
  .then(res => {
    console.log(res);
    return 2;
  })
  .catch(err => {
    return 3;
  })
  .then(res => {
    console.log(res);
  });
// 1
// 2

```

* Promise可以链式调用，不过promise 每次调用 .then 或者 .catch 都会返回一个新的 promise

### 题目 22：

```text
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('timer')
    resolve('success')
  }, 1000)
})
const start = Date.now();
promise.then(res => {
  console.log(res, Date.now() - start)
})
promise.then(res => {
  console.log(res, Date.now() - start)
})

// 'timer'
// 'success' 1001
// 'success' 1002

```

* Promise 的 .then 或者 .catch 可以被调用多次，但这里 Promise 构造函数只执行一次。或者说 promise 内部状态一经改变，并且有了一个值，那么后续每次调用 .then 或者 .catch 都会直接拿到该值

### 题目 23：

```text
Promise.resolve().then(() => {
  return new Error('error!!!')
}).then(res => {
  console.log("then: ", res)
}).catch(err => {
  console.log("catch: ", err)
})
// "then: " "Error: error!!!"


```

*  返回任意一个非 `promise` 的值都会被包裹成 `promise` 对象，因此这里的`return new Error('error!!!')`也被包裹成了`return Promise.resolve(new Error('error!!!'))`

### 题目 24：

```text
Promise.resolve(1)
  .then(2)
  .then(Promise.resolve(3))
  .then(console.log)
// 1
```

* .then 或者 .catch 的参数期望是函数，传入非函数则会发生值透传。

### 题目 25：

```text
Promise.resolve('1')
  .then(res => {
    console.log(res)
  })
  .finally(() => {
    console.log('finally')
  })
Promise.resolve('2')
  .finally(() => {
    console.log('finally2')
  	return '我是finally2返回的值'
  })
  .then(res => {
    console.log('finally2后面的then函数', res)
  })

// "1"
// "finally2"
// "finally"
// "finally2后面的then函数"
// "2"

```

1. 它最终返回的默认会是一个**上一次的Promise对象值**，不过如果抛出的是一个异常则返回异常的`Promise`对象。
2. finally\(\)会等promise1\(\).then\(\)执行完才会将finally\(\)加入微任务队列，其实

### 题目 26：

```text
function promise1 () {
  let p = new Promise((resolve) => {
    console.log('promise1');
    resolve('1')
  })
  return p;
}
function promise2 () {
  return new Promise((resolve, reject) => {
    reject('error')
  })
}
promise1()
  .then(res => console.log(res))
  .catch(err => console.log(err))
  .finally(() => console.log('finally1'))

promise2()
  .then(res => console.log(res))
  .catch(err => console.log(err))
  .finally(() => console.log('finally2'))

// "promise1"
// "1"
// "error"
// "finally1"
// "finally2"
```

首先定义了两个函数`promise1`和`promise2`，先不管接着往下看。

`promise1`函数先被调用了，然后执行里面`new Promise`的同步代码打印出`promise1`

之后遇到了`resolve(1)`，将`p`的状态改为了`resolved`并将结果保存下来。

此时`promise1`内的函数内容已经执行完了，跳出该函数

碰到了`promise1().then()`，由于`promise1`的状态已经发生了改变且为`resolved`因此将`promise1().then()`这条微任务加入本轮的微任务列表\(**这是第一个微任务**\)

这时候要注意了，代码并不会接着往链式调用的下面走，也就是不会先将`.finally`加入微任务列表，那是因为`.then`本身就是一个微任务，它链式后面的内容必须得等当前这个微任务执行完才会执行，因此这里我们先不管`.finally()`

再往下走碰到了`promise2()`函数，其中返回的`new Promise`中并没有同步代码需要执行，所以执行`reject('error')`的时候将`promise2`函数中的`Promise`的状态变为了`rejected`

跳出`promise2`函数，遇到了`promise2().catch()`，将其加入当前的微任务队列\(**这是第二个微任务**\)，且链式调用后面的内容得等该任务执行完后才执行，和`.then()`一样。

OK， 本轮的宏任务全部执行完了，来看看微任务列表，存在`promise1().then()`，执行它，打印出`1`，然后遇到了`.finally()`这个微任务将它加入微任务列表\(**这是第三个微任务**\)等待执行

再执行`promise2().catch()`打印出`error`，执行完后将`finally2`加入微任务加入微任务列表\(**这是第四个微任务**\)

OK， 本轮又全部执行完了，但是微任务列表还有两个新的微任务没有执行完，因此依次执行`finally1`和`finally2`。



### 题目 27： 

```text
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}
async function async2() {
  console.log("async2");
}
async1();
console.log('start')

// 'async1 start'
// 'async2'
// 'start'
// 'async1 end'


//变成了

async function async1() {
  console.log("async1 start");
  // 原来代码
  // await async2();
  // console.log("async1 end");
  
  // 转换后代码
  new Promise(resolve => {
    console.log("async2")
    resolve()
  }).then(res => console.log("async1 end"))
}
async function async2() {
  console.log("async2");
}
async1();
console.log("start")


```

* 首先一进来是创建了两个函数的，我们先不看函数的创建位置，而是看它的调用位置发现`async1`函数被调用了，然后去看看调用的内容
* 执行函数中的同步代码`async1 start`，之后碰到了`await`，它会阻塞`async1`后面代码的执行，因此会先去执行`async2`中的同步代码`async2`，然后跳出`async1`
* 跳出`async1`函数后，执行同步代码`start`
* 在一轮宏任务全部执行完之后，再来执行刚刚`await`后面的内容`async1 end`
* 在这里，你可以理解为「紧跟着await后面的语句相当于放到了new Promise中，下一行及之后的语句相当于放在Promise.then中」

### 题目 28：

```text
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}
async function async2() {
  setTimeout(() => {
    console.log('timer')
  }, 0)
  console.log("async2");
}
async1();
console.log("start")
// "async1 start"
// "async2"
// "start"
// "async1 end"
// "timer"
```

* **定时器始终还是最后执行的，它被放到下一条宏任务的延迟队列中。**

### 题目 29：当时写错了

```text
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
  setTimeout(() => {
    console.log('timer1')
  }, 0)
}
async function async2() {
  setTimeout(() => {
    console.log('timer2')
  }, 0)
  console.log("async2");
}
async1();
setTimeout(() => {
  console.log('timer3')
}, 0)
console.log("start")

// "async1 start"
// "async2"
// "start"
// "async1 end"
// "timer2"
// "timer3"
// "timer1"
```

###  题目 30：

```text
async function async1 () {
  console.log('async1 start');
  await new Promise(resolve => {
    console.log('promise1')
    
  })
  console.log('async1 success');
  return 'async1 end'
}
console.log('srcipt start')
async1().then(res => console.log(res))
console.log('srcipt end')

// "srcipt start"
// "async1 start"
// "promise1"
// "srcipt end"


```

*  在`async1`中`await`后面的`Promise`是没有返回值的，也就是它的状态始终是`pending`状态，因此相当于一直在`await`，`await`，`await`却始终没有响应...

### 题目 31：

```text
async function async1 () {
  console.log('async1 start');
  await new Promise(resolve => {
    console.log('promise1')
    resolve('promise resolve')
  })
  console.log('async1 success');
  return 'async1 end'
}
console.log('srcipt start')
async1().then(res => {
  console.log(res)
})
new Promise(resolve => {
  console.log('promise2')
  setTimeout(() => {
    console.log('timer')
  })
})

// "srcipt start"
// "async1 start"
// "promise1"
// "promise2"
// "async1 success"
// "async1 end"
// "timer"
```

###  题目 32

```text
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}

async function async2() {
  console.log("async2");
}

console.log("script start");

setTimeout(function() {
  console.log("setTimeout");
}, 0);

async1();

new Promise(function(resolve) {
  console.log("promise1");
  resolve();
}).then(function() {
  console.log("promise2");
});
console.log('script end')

// "script start"
// "async1 start"
// "async2"
// "promise1"
// "script end"
// "async1 end"
// "promise2"
// "setTimeout"
```

### 题目 33：当时错了

```text
async function testSometing() {
  console.log("执行testSometing");
  return "testSometing";
}

async function testAsync() {
  console.log("执行testAsync");
  return Promise.resolve("hello async");
}

async function test() {
  console.log("test start...");
  const v1 = await testSometing();
  console.log(v1);
  const v2 = await testAsync();
  console.log(v2);
  console.log(v1, v2);
}

test();

var promise = new Promise(resolve => {
  console.log("promise start...");
  resolve("promise");
});
promise.then(val => console.log(val));

console.log("test end...");


// "test start..."
// "执行testSometing"
// "promise start..."
// "test end..."
// "testSometing"
// "执行testAsync"
// "promise"
// "hello async"
// "testSometing"
// "hello async"
```

### 题目 34：async处理错误

```text
async function async1 () {
  await async2();
  console.log('async1');
  return 'async1 success'
}
async function async2 () {
  return new Promise((resolve, reject) => {
    console.log('async2')
    reject('error')
  })
}
async1().then(res => console.log(res))

// 'async1'
// Uncaught (in promise) Error: error!!!
```

* 如果在async函数中抛出了错误，则终止错误结果，不会继续向下执行。

### 题目 35： 重点

```text
const first = () => (new Promise((resolve, reject) => {
    console.log(3);
    let p = new Promise((resolve, reject) => {
        console.log(7);
        setTimeout(() => {
            console.log(5);
            resolve(6);
            console.log(p)
        }, 0)
        resolve(1);
    });
    resolve(2);
    p.then((arg) => {
        console.log(arg);
    });
}));
first().then((arg) => {
    console.log(arg);
});
console.log(4);


// 解析
3
7
4
1
2
5
[object Promise] { ... }
```

第一段代码定义的是一个函数，所以我们得看看它是在哪执行的，发现它在`4`之前，所以可以来看看`first`函数里面的内容了。\(这一步有点类似于题目`1.5`\)

函数`first`返回的是一个`new Promise()`，因此先执行里面的同步代码`3`

接着又遇到了一个`new Promise()`，直接执行里面的同步代码`7`

执行完`7`之后，在`p`中，遇到了一个定时器，先将它放到下一个宏任务队列里不管它，接着向下走

碰到了`resolve(1)`，这里就把`p`的状态改为了`resolved`，且返回值为`1`，不过这里也先不执行

跳出`p`，碰到了`resolve(2)`，这里的`resolve(2)`，表示的是把`first`函数返回的那个`Promise`的状态改了，也先不管它。

然后碰到了`p.then`，将它加入本次循环的微任务列表，等待执行

跳出`first`函数，遇到了`first().then()`，将它加入本次循环的微任务列表\(`p.then`的后面执行\)

然后执行同步代码`4`

本轮的同步代码全部执行完毕，查找微任务列表，发现`p.then`和`first().then()`，依次执行，打印出`1和2`

本轮任务执行完毕了，发现还有一个定时器没有跑完，接着执行这个定时器里的内容，执行同步代码`5`

然后又遇到了一个`resolve(6)`，它是放在`p`里的，但是`p`的状态在之前已经发生过改变了，因此这里就不会再改变，也就是说`resolve(6)`相当于没任何用处，因此打印出来的`p`为`Promise{<resolved>: 1}`。\(这一步类似于题目`3.1`\)

### 题目 36：重点

```text
const async1 = async () => {
  console.log('async1');
  setTimeout(() => {
    console.log('timer1')
  }, 2000)
  await new Promise(resolve => {
    console.log('promise1')
  })
  console.log('async1 end')
  return 'async1 success'
} 

console.log('script start');
async1().then(res => console.log(res));
console.log('script end');
Promise.resolve(1)
  .then(2)
  .then(Promise.resolve(3))
  .catch(4)
  .then(res => console.log(res))
setTimeout(() => {
  console.log('timer2')
}, 1000)

// 结果
"script start"
"async1"
"promise1"
"script end"
1
"timer2"
"timer1"
```

### 题目 37： 重点

```text
const p1 = new Promise((resolve) => {
  setTimeout(() => {
    resolve('resolve3');
    console.log('timer1')
  }, 0)
  resolve('resovle1');
  resolve('resolve2');
}).then(res => {
  console.log(res)
  setTimeout(() => {
    console.log(p1)
  }, 1000)
}).finally(res => {
  
  console.log('finally', res)
})
// 解析
'resolve1'
'finally' undefined
'timer1'
Promise{<resolved>: undefined}

```

`Promise`的状态一旦改变就无法改变

`finally`不管`Promise`的状态是`resolved`还是`rejected`都会执行，且它的回调函数是接收不到`Promise`的结果的，所以`finally()`中的`res`是一个迷惑项\(类似`3.10`\)。

最后一个定时器打印出的`p1`其实是`.finally`的返回值，我们知道`.finally`的返回值如果在没有抛出错误的情况下默认会是上一个`Promise`的返回值\(`3.10`中也有提到\), 而这道题中`.finally`上一个`Promise`是`.then()`，但是这个`.then()`并没有返回值，所以`p1`打印出来的`Promise`的值会是`undefined`，如果你在定时器的**下面**加上一个`return 1`，则值就会变成`1`



###  

 

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



## 20. The client side storage

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

## 21 Client Detection

```text
 function isIOS() {
  return /iphone|ipad|itouch/i.test(navigator.userAgent);
}


 function isAndroid() {
  return /android/i.test(navigator.userAgent);
}
```



* The Navigator userAgent property is used for returning the user-agent header’s value sent to the server by the browser. It returns a string representing values such as the name, version, and platform of the browser.

## **22.** The Document Object Model

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







## 23.Pure functions

* Functions must be highly predictable, Given the same input, it should always return the same output.
* Functions must return exactly value which is determined solely by its input.
* Has no side effects.

## 24. Higher Order Functions

* Higher-Order functions accept functions as parameters or return a function as an output.
* setTimeout, map, reduce

## 25. Currying functions

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



## 26. Partial functions

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







## 27. forEach中return有效果吗？如何中断forEach循环？

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



## 28.JS判断数组中是否包含某个值

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



## 29.JS中flat---数组扁平化

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

## 30. JS中浅拷贝的手段有哪些?

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



## 31 Document.write\(\)和 innerHtml 区别

![](../.gitbook/assets/image%20%2897%29.png)



## 32. Asynchronous Callbacks

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

### 基础

```text
let promise = new Promise(resolve => {
  setTimeout(() => resolve("done!"), 1000);
});


const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});

//Promise 对象抛出的错误不会传递到外层代码
const someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2);
  });
};

someAsyncThing().then(function() {
  console.log('everything is great');
});

setTimeout(() => { console.log(123) }, 2000);


// .then 或 .catch 返回的值不能是 promise 本身，否则会造成死循环
const promise = Promise.resolve()
  .then(() => {
    return promise
  })
promise.catch(console.error)
// TypeError: Chaining cycle detected for promise #<Promise>



// .then 或者 .catch 的参数期望是函数，传入非函数则会发生值穿透。
Promise.resolve(1)
  .then(2)
  .then(Promise.resolve(3))
  .then(console.log)
  // 1
```

*  `Promise`对象代表一个异步操作， 有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）
* **一旦状态改变，就不会再变，任何时候都可以得到这个结果**， promise 内部状态一经改变，并且有了一个值，那么后续每次调用 .then 或者 .catch 都会直接拿到该值
*  `Promise`实例生成以后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函数,构造函数中的 resolve 或 reject 只有第一次执行有效，多次调用没有任何作用
* **then 和 catch 中 如果 return 处理**
  * 返回任意一个非 promise 的值都会被包裹成 promise 对象
  * 即 return new Error\('error!!!'\) 等价于 `return Promise.resolve(new Error('error!!!'))`
* 怎么在 catch 和 then 中 报错 并且被后面 catch 捕获
  * `return Promise.reject(new Error('error!!!'))`

    `throw new Error('error!!!')`
*  如果没有使用`catch()`方法指定错误处理的回调函数,Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。
*  `.then` 或 `.catch` 返回的值不能是 promise 本身，否则会造成死循环
*  `.then` 或者 `.catch` 的参数期望是函数，传入非函数则会发生值穿透。
* 异步行为应该始终返回一个 promise。这样就可以使得之后我们计划后续的行为成为可能。即使我们现在不打算对链进行扩展



```text
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(reject, ms, 'done');
  });
}

timeout(100).then((value) => {
  //success
  console.log('this is success');
  console.log(value);
},function(error) {
  // failure
  console.log(error);
  console.log('this is error')
});
//output: done, this is error
```

* **Promise 新建后就会立即执行**

```text
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('resolved.');
});

console.log('Hi!');

// Promise
// Hi!
// resolved

```

*  `p1`和`p2`都是 Promise 的实例，但是`p2`的`resolve`方法将`p1`作为参数，即一个异步操作的结果是返回另一个异步操作。 这时`p1`的状态就会传递给`p2`，也就是说，`p1`的状态决定了`p2`的状态。如果`p1`的状态是`pending`，那么`p2`的回调函数就会等待`p1`的状态改变；如果`p1`的状态已经是`resolved`或者`rejected`，那么`p2`的回调函数将会立刻执行。

```text
const p1 = new Promise(function (resolve, reject) {
  setTimeout(() => reject(new Error('fail')), 3000)
})

const p2 = new Promise(function (resolve, reject) {
  setTimeout(() => resolve(p1), 1000)
})

p2
  .then(result => console.log(result))
  .catch(error => console.log(error))
// Error: fail
```

 `p1`是一个 Promise，3 秒之后变为`rejected`。`p2`的状态在 1 秒之后改变，`resolve`方法返回的是`p1`。由于`p2`返回的是另一个 Promise，导致`p2`自己的状态无效了，由`p1`的状态决定`p2`的状态。所以，后面的`then`语句都变成针对后者（`p1`）。又过了 2 秒，`p1`变为`rejected`，导致触发`catch`方法指定的回调函数。

*  调用`resolve`或`reject`**并不会终结 Promise 的参数函数的执行。** 

```text
new Promise((resolve, reject) => {
  resolve(1);
  console.log(2);
}).then(r => {
  console.log(r);
});
// 2
// 1
```

### Promise.prototype.then\(\) <a id="Promise-prototype-then"></a>

```text
Promise.resolve()
  .then(function success (res) {
    throw new Error('error')
  }, function fail1 (e) {
    console.error('fail1: ', e)
  })
  .catch(function fail2 (e) {
    console.error('fail2: ', e)
  })




// 返回 promise
new Promise(function(resolve, reject) {

  setTimeout(() => resolve(1), 1000);

}).then(function(result) {

  alert(result); // 1

  return new Promise((resolve, reject) => { // (*)
    setTimeout(() => resolve(result * 2), 1000);
  });

}).then(function(result) { // (**)

  alert(result); // 2

  return new Promise((resolve, reject) => {
    setTimeout(() => resolve(result * 2), 1000);
  });

}).then(function(result) {

  alert(result); // 4

});
```

* 它的作用是为 Promise 实例添加状态改变时的回调函数。前面说过，`then`方法的第一个参数是`resolved`状态的回调函数，第二个参数是`rejected`状态的回调函数，它们都是可选的。 `then`方法返回的是一个新的`Promise`实例, 
* .then 的第二个处理错误的函数捕获不了第一个处理成功的函数抛出的错误，而后续的 .catch 可以捕获之前的错误\

![](../.gitbook/assets/image%20%28131%29.png)

* .then 也可以返回promise





```text
getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL);
}).then(function (comments) {
  console.log("resolved: ", comments);
}, function (err){
  console.log("rejected: ", err);
});
```

*  上面代码中，第一个`then`方法指定的回调函数，返回的是另一个`Promise`对象。这时，第二个`then`方法指定的回调函数，就会等待这个新的`Promise`对象状态发生变化。如果变为`resolved`，就调用第一个回调函数，如果状态变为`rejected`，就调用第二个回调函数。

### Promise.prototype.catch\(\)  <a id="Promise-prototype-catch"></a>

```text
window.addEventListener('unhandledrejection', function(event) {
  // 这个事件对象有两个特殊的属性：
  alert(event.promise); // [object Promise] - 生成该全局 error 的 promise
  alert(event.reason); // Error: Whoops! - 未处理的 error 对象
});

new Promise(function() {
  throw new Error("Whoops!");
}); // 没有用来处理 error 的 catch
```

* 如果我们没有 处理 promise 中的 rejection 可以用 unhandledrejection 捕获

```text
getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});


new Promise((resolve, reject) => {
  throw new Error("Whoops!");
}).catch(alert); // Error: Whoops!

// 跟这个相等
new Promise((resolve, reject) => {
  reject(new Error("Whoops!"));
}).catch(alert); // Error: Whoops!
```

* 如果异步操作抛出错误，状态就会变为`rejected`，就会调用`catch()`方法指定的回调函数，处理这个错误。另外，`then()`方法指定的回调函数，如果运行中抛出错误，也会被`catch()`方法捕获。
*  **如果 Promise 状态已经变成`resolved`，再抛出错误是无效的。**

```text
const promise = new Promise(function(resolve, reject) {
  resolve('ok');
  throw new Error('test');
});
promise
  .then(function(value) { console.log(value) })
  .catch(function(error) { console.log(error) });
// ok
```

* 函数代码周围有个“隐式的 try..catch”。所以，所有同步错误都会得到处理。, 但是这里的错误并不是在 executor 运行时生成的，而是在稍后生成的。因此，promise 无法处理它。

```text
new Promise(function(resolve, reject) {
  setTimeout(() => {
    throw new Error("Whoops!");
  }, 1000);
}).catch(alert);
```

###  <a id="Promise-prototype-finally"></a>

###  <a id="Promise-prototype-finally"></a>

### Promise.prototype.finally\(\) <a id="Promise-prototype-finally"></a>

```text
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```

* 不管 Promise 对象最后状态如何，都会执行的操作
*  它最终返回的默认会是一个**上一次的Promise对象值**，不过如果抛出的是一个异常则返回异常的`Promise`对象

### Promise.all\(\) <a id="Promise-all"></a>

```text
// 例子 1
const p = Promise.all([p1, p2, p3]);
const promise1 = Promise.resolve(3);
const promise2 = 42;
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo');
});

Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values);
});
```

* 方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。 `p1`、`p2`、`p3`都是 Promise 实例，如果不是，就会先调用下面讲到的`Promise.resolve`方法，将参数转为 Promise 实例，再进一步处理。另外，`Promise.all()`方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例
* **如果其中一个 promise 被 reject，Promise.all 就会立即被 reject，完全忽略列表中其他的 promise。它们的结果也被忽略**
*  `p`的状态由`p1`、`p2`、`p3`决定，分成两种情况。
  *  只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数
  *  只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函
*  如果作为参数的 Promise 实例，自己定义了`catch`方法，那么它一旦被`rejected`，并不会触发`Promise.all()`的`catch`方法。

```text
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result)
.catch(e => e);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result)
.catch(e => e);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// ["hello", Error: 报错了]


// 没有 自己的 catch
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// Error: 报错了
```

*  上面这个 例子 `p1`会`resolved`，`p2`首先会`rejected`，但是`p2`有自己的`catch`方法，该方法返回的是一个新的 Promise 实例，`p2`指向的实际上是这个实例。该实例执行完`catch`方法后，也会变成`resolved`，导致`Promise.all()`方法参数里面的两个实例都会`resolved`，因此会调用`then`方法指定的回调函数，而不会调用`catch`方法指定的回调函数。
*  **如果`p2`没有自己的`catch`方法，就会调用`Promise.all()`的`catch`方法。**

**Promise.all\(iterable\) 允许在 iterable 中使用 non-promise 的“常规”值**

```text
Promise.all([
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(1), 1000)
  }),
  2,
  3
]).then(alert); // 1, 2, 3


function runAsync (x) {
    const p = new Promise(r => setTimeout(() => r(x, console.log(x)), 1000))
    return p
}
Promise.all([runAsync(1), runAsync(2), runAsync(3)])
  .then(res => console.log(res))
  
  // 在间隔一秒后，控制台会同时打印出1, 2, 3，还有一个数组[1, 2, 3]。

```

* 通常，Promise.all\(...\) 接受含有 promise 项的可迭代对象（大多数情况下是数组）作为参数。但是，如果这些对象中的任何一个不是 promise，那么它将被“按原样”传递给结果数组



### Promise.race\(\) <a id="Promise-race"></a>

```text
const p = Promise.race([p1, p2, p3]);

// 例子 2
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
]);

p
.then(console.log)
.catch(console.error);





function runAsync (x) {
  const p = new Promise(r => setTimeout(() => r(x, console.log(x)), 1000))
  return p
}
Promise.race([runAsync(1), runAsync(2), runAsync(3)])
  .then(res => console.log('result: ', res))
  .catch(err => console.log(err))
// 1
// 'result: ' 1
// 2
// 3


```

*  它只会获取最先执行完成的那个结果，其它的异步任务虽然也会继续进行下去，不过race已经不管那些任务的结果了
* `Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。 只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数
* 上面的 例子2 如果指定时间内没有获得结果，就将 Promise 的状态变为`reject`，否则变为`resolve`。

### Promise.allSettled\(\) <a id="Promise-allSettled"></a>

```text
const resolved = Promise.resolve(42);
const rejected = Promise.reject(-1);

const allSettledPromise = Promise.allSettled([resolved, rejected]);

allSettledPromise.then(function (results) {
  console.log(results);
});
// [
//    { status: 'fulfilled', value: 42 },
//    { status: 'rejected', reason: -1 }
// ]
```

* 方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例。只有等到所有这些参数实例都返回结果，不管是fulfilled还是rejected
* 该方法返回的新的 Promise 实例，一旦结束，状态总是fulfilled，不会变成rejected
*  **只关心这些操作有没有结束。这时，`Promise.allSettled()`方法就很有用**

### Promise.any\(\) <a id="Promise-any"></a>

```text
Promise.any([
  fetch('https://v8.dev/').then(() => 'home'),
  fetch('https://v8.dev/blog').then(() => 'blog'),
  fetch('https://v8.dev/docs').then(() => 'docs')
]).then((first) => {  // 只要有一个 fetch() 请求成功
  console.log(first);
}).catch((error) => { // 所有三个 fetch() 全部请求失败
  console.log(error);
});
```

*  只要参数实例有一个变成`fulfilled`状态，包装实例就会变成`fulfilled`状态；如果所有参数实例都变成`rejected`状态，包装实例就会变成`rejected`状态。

### Promise.resolve\(\)**/reject\(\)** <a id="Promise-resolve"></a>

```text
let promise = new Promise(resolve => resolve(value));


Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))

//参数是一个thenable对象
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};

let p1 = Promise.resolve(thenable);
p1.then(function (value) {
  console.log(value);  // 42
});

// 参数不是具有then()方法的对象，或根本就不是对象
const p = Promise.resolve('Hello');

p.then(function (s) {
  console.log(s)
});

// 不带任何参数
```

*  `Promise.resolve(value)` **用结果 value 创建一个 resolved 的 promise。**
* `Promise.resolve()`方法的参数分成四种情况。

  *  **参数是一个 Promise 实例**
    * 如果参数是 Promise 实例，那么Promise.resolve将不做任何修改、原封不动地返回这个实例。
  *  **参数是一个`thenable`对象**
    * thenable对象指的是具有then方法的对象，比如下面这个对象。
    *  `Promise.resolve()`方法会将这个对象转为 Promise 对象，然后就立即执行`thenable`对象的`then()`方法。
  *  **参数不是具有`then()`方法的对象，或根本就不是对象**
    *  则`Promise.resolve()`方法返回一个新的 Promise 对象，状态为`resolved`。
    *  生成一个新的 Promise 对象的实例`p`。由于字符串`Hello`不属于异步操作（判断方法是字符串对象不具有 then 方法），返回 Promise 实例的状态从一生成就是`resolved`，所以回调函数会立即执行。`Promise.resolve()`方法的参数，会同时传给回调函数。
  *  **不带有任何参数**

    \*\*\*\*

  \*\*\*\*

### 

### 

### 

### 

### 

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

### 题目1

```text
var promise = func1();

promise

.then(function(result1) {
    console.log(result1);
    return func2();
})

.then(function(result2) {
    console.log(result2);
    return result2%10;
})

.then(function(result3) {
    console.log(result3);
});

function func1() {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            resolve("Hello");
        }, 1000);
    });
}

function func2() {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            resolve(50);
        }, 1000);
    });
}


// Output: "hello" 50 0
```

* 如果返还的不是 promise object， 他就会返还 auto resolved promise  with value 10, 所以就返还了 0

### 题目2

```text
function exam(arg) {
    return new Promise(function(resolve, reject) {
        if (arg>50) {
            resolve('Pass');
        } else {
            reject('Fail');
        }
    });
}

let promise = exam(60);

promise.then(function(result) {
    console.log(result);

    return exam(20);
})

.catch(function(error) {
    console.log(error);

    return 'Error';
})

.then(function(result) {
    console.log(result);

    return exam(80);
})

.catch(function(error) {
    console.log(error);
});

// 结果：Pass Fail Error
```

### 题目3

```text
function func1(){
  return new Promise(function(resolve,reject){
    setTimeout(function(){
      resolve("Func1")
    },1000)
  })
}

function func2(){
  return new Promise(function(resolve,reject){
    setTimeout(function(){
      resolve("Func2")
    },2000)
  })
}


func1()
  .then(func2())
  .then(function(result) {
    console.log(result);
  });
  
  // output: Func1
```

*  The `.then` function takes _two arguments_, the two callback functions that execute when a promise resolves or rejects, and it returns a _promise_. However, as you can see on **line 19**, the `.then`statement is missing both these callback functions. We can refer these functions as _handlers_. Instead, `func2()`function is being called. **For the promise returned from `func1`, `.then` has no handler.** Hence, the first `.then` is skipped and the second `.then` statement executes for this promise

### 题目4

```text
function func1(){
  return new Promise(function(resolve,reject){
    setTimeout(function(){
      resolve("Func1")
    },1000)
  })
}

function func2(){
  console.log("Func2")
}


func1()
  .then(function(result){
      console.log(result)
      func2()
  })
  .then(function(result){
      console.log(result)
  })
  
  // Func1,Func2,undefined
```

*  Since there is no return statement in the first `.then` handler, `undefined` gets returned which the second `.then`displays.

## 31： Async/await

```text
async function f() {
  return 1;
}

f().then(alert); // 1

//我们也可以显式地返回一个 promise，结果是一样的：
async function f() {
  return Promise.resolve(1);
}

f().then(alert); // 1


// await 接受 “thenables”
class Thenable {
  constructor(num) {
    this.num = num;
  }
  then(resolve, reject) {
    alert(resolve);
    // 1000ms 后使用 this.num*2 进行 resolve
    setTimeout(() => resolve(this.num * 2), 1000); // (*)
  }
}

async function f() {
  // 等待 1 秒，之后 result 变为 2
  let result = await new Thenable(1);
  alert(result);
}

f();

// error 处理例子
async function f() {
  await Promise.reject(new Error("Whoops!"));
}

async function f() {
  throw new Error("Whoops!");
}

// 捕获 error 错误 case1
async function f() {

  try {
    let response = await fetch('http://no-such-url');
  } catch(err) {
    alert(err); // TypeError: failed to fetch
  }
}

f()


// Error 处理
async function async1 () {
  // try {
  //   await Promise.reject('error!!!')
  // } catch(e) {
  //   console.log(e)
  // }
  await Promise.reject('error!!!')
    .catch(e => console.log(e))
  console.log('async1');
  return Promise.resolve('async1 success')
}
async1().then(res => console.log(res))
console.log('script start')







// case 2
async function f() {
  let response = await fetch('http://no-such-url');
}

// f() 变成了一个 rejected 的 promise
f().catch(alert); // TypeError: failed to fetch // (*)


// 在非 async 函数中调用 async 函数
async function wait() {
  await new Promise(resolve => setTimeout(resolve, 1000));

  return 10;
}

function f() {
  // 1 秒后显示 10
  wait().then(result => alert(result));
}

f();
```

* async 这个function 总是返回一个 promise,其他值将自动被包装在一个 `Promise.resolve()` 的 promise 中。
* await
  *  await 让 JavaScript 引擎等待直到 promise 完成（settle）并返回结果。
  * 相比于 promise.then，它只是获取 promise 的结果的一个更优雅的语法，同时也更易于读写。
  * await 接受 “thenables”,像 promise.then 那样，await 允许我们使用 thenable 对象（那些具有可调用的 then 方法的对象）。这里的想法是，第三方对象可能不是一个 promise，但却是 promise 兼容的：如果这些对象支持 .then，那么就可以对它们使用 await。
*  `async/await` 可以和 `Promise.all` 一起使用
* error 处理
  * 如果一个 promise 正常 resolve，await promise 返回的就是其结果。但是如果 promise 被 reject，它将 throw 这个 error，**接一个 catch**
  * **或者try catch捕获上面的错误**
* 
## 32： Execution Context & Execution stack

### Execution Context 

![](../.gitbook/assets/image%20%28121%29.png)

![](../.gitbook/assets/image%20%28122%29.png)

* an execution context is an abstract concept of an environment where the Javascript code is evaluated and executed. Whenever any code is run in JavaScript, it’s run inside an execution context.
* **Types of Execution Context:**
  * Global Execution Context 
    * This is the default or base execution context. The code that is not inside any function is in the global execution context. It performs two things: it creates a global object which is a window object \(in the case of browsers\) and sets the value of this to equal to the global object. There can only be one global execution context in a program
  * Functional Execution Context
    * Every time a function is invoked, a brand new execution context is created for that function
  * Eval Function Execution Context
    * Code executed inside an eval function also gets its own execution context

### 变量对象

* 变量对象是与执行上下文相关的数据作用域，存储了在上下文中定义的变量和函数声明。
* 全局上下文中的变量对象就是全局对象

#### 活动对象

* 在函数上下文中，我们用活动对象\(activation object, AO\)来表示变量对象。
* 不可在 JavaScript 环境中访问，只有到当进入一个执行上下文中，这个执行上下文的变量对象才会被激活，所以才叫 activation object 呐，而只有被激活的变量对象，也就是活动对象上的各种属性才能被访问。

![](../.gitbook/assets/image%20%28120%29.png)

```text
function bar(a) {
  console.log(a);// function
  function a(){
    console.log('????');
  }
}
bar(); 

//第二题
console.log(foo);

function foo(){
    console.log("foo");
}

var foo = 1;

// 变量提升完的样子
function foo(){// 函数提升
    console.log("foo");
}
var foo;// 变量提升
console.log(foo);
foo = 1;



```



* 因为在进入执行上下文时，首先会处理函数声明，其次会处理变量声明，如果变量名称跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性。在console log 时候 会 print function

```text
VO = {
    foo: reference to function foo(){},
}
```

### 执行上下文栈 Execution context stack

* 每一个函数执行时候都会创建一个 execution context 我们把他们放到 stack 管理

{% embed url="https://github.com/mqyqingfeng/Blog/issues/4" %}



```text
比较下面两段代码，试述两段代码的不同之处
// A--------------------------
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f();
}
checkscope();

// B---------------------------
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f;
}
checkscope()();
```

![](../.gitbook/assets/image%20%28123%29.png)

## 33 : Js Operator\_Precedence

{% embed url="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator\_Precedence" %}

```text
// eg1
new Foo.getName();
//变成了
new （Foo.getName）();

// eg2
 new Foo().getName();
//变成了
(new Foo()).getName();

// eg3
new new Foo().getName();
// 变成了
new ((new Foo()).getName)();
//这里确实是(new Foo()).getName()，但是跟括号优先级高于成员访问没关系，
实际上这里成员访问的优先级是最高的，因此先执行了 .getName，但是在进行左侧取值的时候， 
new Foo() 可以理解为两种运算：new 带参数（即 new Foo()）和函数调用（即 先 Foo() 
取值之后再 new），而 new 带参数的优先级是高于函数调用的，因此先执行了 new Foo()，
或得 Foo 类的实例对象，再进行了成员访问 .getName。
```

1. \(\) --&gt; 分组
2. obj.a --&gt; 成员访问
3. obj\["hello "\] --&gt; 需计算的成员访问
4. new（带参数列表）new … \( … \)  `const car1 = new Car('Eagle', 'Talon TSi', 1993);`
5. 函数调用\(\)   `myFunc(mycar);`
6. ？.
7. new（无参数列表）new …

## 34 正则表达式

$ 表示以什么结尾

g 代表全局模式找出所有的手机号

{}用大括号表示重复次数

\| 或者  
? 可有可无  
+ 一个到无限个

### 手机号

* `/^1[34578]\d/g    以1开头然后第二位是3，4，5，7，8， ，` 

### 颜色

\#48D1CC

```text
/#?([0-9a-fA-F]{6} | [0-9a-fA-F]{3})/g
```

### 邮箱

```text
/^([A-Za-z0-9_\-\.])+@([A-Za-z0-9_\-\.]+)\.([A-Za-z]{2,6})$/g
```

### url

```text
/^((https?|ftp|file):\/\/)?([\da-z\.\-]+)\.([a-z\.]{2,6})([\/\w\.\-]*)*\/?$/g
```



## 35: setTimeout 模拟setInterval\(\):

setInterval 函数 理论上是每隔 1秒执行 但是 函数里面的 需要花费3秒, 这里需要等到前面执行完才可以执行下一个回调函数，不会按照间隔时间来执行了， 也就是说 他不是真正的间隔一段时间来执行

![](../.gitbook/assets/image%20%28109%29.png)

![](../.gitbook/assets/image%20%28108%29.png)





## 36 .  参数传递

* 所有函数的参数都是按值传递的。就和把值从一个变量复制到另一个变量一样。
* 但是如果是 object 就是共享传递，传递对象的引用的副本，obj里面的 复杂类型就会是 引用传递 ，基本类型是 按置传递

```text
var obj = {
    value: 1
};
function foo(o) {
    o.value = 2;
    console.log(o.value); //2
}
foo(obj);
console.log(obj.value) // 2


var obj = {
    value: 1
};
function foo(o) {
    o = 2;
    console.log(o); //2
}
foo(obj);
console.log(obj.value) // 1
```

## 37.类数组

拥有一个 length 属性和若干索引属性的对象

### 将类数组转换成数组

```text
var arrayLike = {0: 'name', 1: 'age', 2: 'sex', length: 3 }
// 1. slice
Array.prototype.slice.call(arrayLike); // ["name", "age", "sex"] 
// 2. splice
Array.prototype.splice.call(arrayLike, 0); // ["name", "age", "sex"] 
// 3. ES6 Array.from
Array.from(arrayLike); // ["name", "age", "sex"] 
// 4. apply
Array.prototype.concat.apply([], arrayLike)
```

## 38. async/await 遇上 forEach

```text
// 有问题的 code 我们想每隔一秒 output一个数字
var getNumbers = () => {
  return Promise.resolve([1, 2, 3])
}
var multi = num => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (num) {
        resolve(num * num)
      } else {
        reject(new Error('num not specified'))
      }
    }, 1000)
  })
}
async function test () {
  var nums = await getNumbers()
  nums.forEach(async x => {
    var res = await multi(x)
    console.log(res)
  })
}
test()


// 解决办法
var getNumbers = () => {
  return Promise.resolve([1, 2, 3])
}
var multi = num => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (num) {
        resolve(num * num)
      } else {
        reject(new Error('num not specified'))
      }
    }, 1000)
  })
}
async function test () {
var nums = await getNumbers();

for(let i = 0; i < nums.length; i += 1) {
var res = await multi(nums[i]);

console.log(res)

}
}
test();

```

* 原因 在 foreach 里面we are not waiting for the callback to be done, so using a function that returns a promise won't wait for the promise to resolve everytime.但是实际却是并行执行了。
* **解决办法 使用 普通 for** 















\*\*\*\*





















