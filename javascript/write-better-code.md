# Write better code

## &#x20;几个优雅的 JS 运算符号

{% embed url="https://mp.weixin.qq.com/s/FwF_awtPQAgXQYs1SWcDfw" %}

{% embed url="https://mp.weixin.qq.com/s/Xw7-6ZnuUKCq1_s_d-I98w" %}



nullish 代表 null 或者 undefined

### ??

```
null ?? 5 // => 5
3 ?? 5 // => 3
```

* **总结一下，?? 操作符允许我们分配默认值，同时忽略像 0 和空字符串这样的假值。**
* 在 JavaScript 中，?? 操作符被称为nullish 合并操作符。如果第一个参数不是 null/undefined，这个运算符将返回第一个参数，否则，它将返回第二个参数。

### 逻辑空分配（?? =）

```
var x = null
var y = 5

console.log(x ??= y) // => 5
console.log(x = (x ?? y)) // => 5

//例子2 
function gameSettingsWithNullish(options) {
  options.gameSpeed ??= 1
  options.gameDiff ??= 'easy'
  return options
}


function gameSettingsWithDefaultParams(gameSpeed=1, gameDiff='easy') {
  return {gameSpeed, gameDiff}
}

gameSettingsWithNullish({gameSpeed: null, gameDiff: null}) // => { gameSpeed: 1, gameDiff: 'easy' }
gameSettingsWithDefaultParams(null, null) // => { gameSpeed: null, gameDiff: null }
```

* 又称为逻辑 nullish 赋值操作符,只有当前值为 null 或 undefined 时，此赋值运算符才会分配新值。上面的例子强调了这个操作符如何实质上是 nullish 赋值的语法糖
* 例子2： 上面的函数处理空值的方式有一个显著的不同。默认参数将使用 **null 参数覆盖默认值**，**nullish 赋值操作符不会**。默认参数和 nullish 赋值都不会覆盖未定义的值。

### ?. 操作符

```
var travelPlans  = {
  destination: 'DC',
  monday: {
    location: 'National Mall',
    budget: 200
  }
};

// 查看 tuesday 是否为 null 为null return undefined
const tuesdayPlans = travelPlans.tuesday?.location;
console.log(tuesdayPlans) // => undefined
```

* 允许开发人员读取深度嵌套在一个对象链中的属性值，而不必沿途显式验证每个引用。当引用为 null 时，表达式停止计算并返回 undefined，让我们来看一个例子

### &#x20;? 操作符

```
function checkCharge(charge) {
return (charge > 0) ? 'Ready for use' : 'Needs to charge'
}
```

* 三元运算符

### 逻辑或分配（|| =）

```
const a = { duration: 50, title: '' };

a.duration ||= 10;
console.log(a.duration);
// expected output: 50

a.title ||= 'title is empty.';
console.log(a.title);
// expected output: "title is empty"

```

* The logical OR assignment (x ||= y) operator only assigns if x is falsy. 如果是 false 就会赋值

### 逻辑与分配（&& =）

```
const a = { duration: 50, title: '' };

a.duration &&= 10;
console.log(a.duration);
// output: 10

a.title &&= 'title is empty.';
console.log(a.title);
// output: ""
```

* 如果是 true 才会赋值



## CSS 规范

```
// 1
.content > .title {
  font-size: 2rem;
}


// 2. 不推荐

border-top-style: none;
font-family: palatino, georgia, serif;
font-size: 100%;
line-height: 1.6;
padding-bottom: 2em;
padding-left: 1em;
padding-right: 1em;
padding-top: 0;

// 推荐
border-top: 0;
font: 100%/1.6 palatino, georgia, serif;
padding: 0 1em 2em;


// 3. 不推荐：
div{
  padding-bottom: 0px;
  margin: 0em;
}

//推荐：
div{
  padding-bottom: 0;
  margin: 0;
}

```

1. 选择器
   1. **总是考虑直接子选择器，否则 会导致消耗性能**
2. 尽量使用缩写属性
3. 省略0后面的单位

## JS 规范：

```
// undefined 并非是一个关键字，而是全局对象的一个属性（全局变量）
// 在早期的 IE 中，undefined 是可以赋值的全局变量
var test;
undefined = 1;

typeof test === "undefined" // ture
test === undefined          // false
alert(test)                 // undefined
alert(undefined)            // 1
//函数的注释
/**
 * @description: 数据类型的检测的第二种方式
 * @param {any} data 要检测数据类型的变量
 * @return {string} type 返回具体的类型名称【小写】
 */
 //下载koroFileHeader 进行安装，可以一键生成注释格式


// 不满足条件尽早 return

function test(name, sex = 1) {
  if (!name){ // 不满足条件尽早抛出错误
      throw new Error('没有传递name参数');
  }
  // ...
}

// 函数返回多个值，推荐使用对象作为函数的返回值
// commonly
function processInput(input) {
  return [left, right, top, bottom];
}

// grace
function processInput(input) {
  return { left, right, top, bottom };
}
const { left, right } = processInput(input);

//如果有很多个 parameters
class User  {
  constructor(userData){
    this.name = userData.name;
    this.age = userData.age;
    this.email = userData.email;
  }
}

const user = new User({name:'Max',email:'max@test.com', age:31})

//functions should be small and do one thing

function renderContent(renderInformation){
  const element =  renderInformation.element;
  const rootElement =  renderInformation.root;
  
  validateElementType(element);
  const content =  createRenderableContent(renderInformation);
  renderOnRoot(rootElement,content);
}


//Extract code that works on the same functionality
user.setAge(31);
user.setName('Max');
// 可以变成
user.update({age:31,name:'max'});


// Extract code that requires more interpretation than the surrounding code

if(!email.includes('@')){
......
}
// 变成，function 名字解释了这在干啥
if(!isValid(email)){
....
}


// Use Guards & Fail Fast

if(!email.includes('@')){
    return;
}

//do stuff
```

* 小写驼峰命名 lowerCamelCase
* 其中 method 方法命名必须是 动词 或者 动词+名词 形式
  * `saveShopCarData /openShopCarInfoDialog`
* 常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。
  * `MAX_STOCK_COUNT`
* 对象声明, 不要使用 constructor 创建
* undefined 判断 使用 `typeof person === 'undefined'   or test === void 0`
* &#x20;this 的转换命名
  * 对上下文 this 的引用只能使用'self'来命名
* 函数
  * 函数的注释
  * 优先使用箭头函数
  * 接收参数，如果函数的的参数是对象，也要优先使用destructuring，
  * 不满足条件尽早 return
  * 函数返回多个值，推荐使用对象作为函数的返回值 ,因为对象作为返回值，更便于以后添加返回值，以及更改返回值的顺序，**相对于数组更加的灵活，更便于扩展**
  * 如果有很多个 parameters
    * 创建一个 class 接受 一个 object 或者是map 这样子 我们就不需要 一一对应 每个 field 是干什么的
  * 如果parameters 的数值 不是固定的， 可以把它变为 一个 array
  * functions should be small and do one thing
    * Extract code that works on the same functionality
    * Extract code that requires more interpretation than the surrounding code
* Keep your control structure clean:
  * avoid deep nesting
  * Using factory function & polymorphism
  * Prefer positive checks
  * Utilize errors
* Use Guards & Fail Fast







## React 规范

```
// 如果标签有多行属性，关闭标签要另起一行 。
<Component
  bar="bar"
  baz="baz" 
/>

//在自闭标签之前留一个空格。
<Foo />

// 组件的代码顺序
class Example extends Component {
    // 静态属性
    static defaultProps = {}

    // 构造函数
    constructor(props) {
        super(props);
        this.state={}
    }

    // 声明周期钩子函数
    // 按照它们执行的顺序
    // 1. componentWillMount
    // 2. componentWillReceiveProps
    // 3. shouldComponentUpdate
    // 4. componentDidMount
    // 5. componentDidUpdate
    // 6. componentWillUnmount
    componentDidMount() { ... }

    // 事件函数/普通函数
    handleClick = (e) => { ... }

    // 最后，render 方法
    render() { ... }
}



```

* 如果标签有多行属性，关闭标签要另起一行 。
* 在自闭标签之前留一个空格。
* 组件的代码顺序,有利于代码 维护



## Clean code

### name

```

// Don't ❌
const foo = "JDoe@example.com";
const bar = "John";
const age = 23;
const qux = true;

// Do ✅
const email = "John@example.com";
const firstName = "John";
const age = 23;
const isActive = true
```

* Use intention revealing names,  Names of variables should be descriptive.
* Avoid Disinformation, We should  &#x20;avoid words whose entrenched meanings vary from our intended meaning
* Use Pronounceable names
* Method should have verb or verb phrase name
* Pick one word for one abstract concept and stick with it.
* Avoid using the same word for two purposes

```
// Function ,should be small.
public static String renderPageWithSetupsAndTeardowns(
 PageData pageData, boolean isSuite) throws Exception {
 if (isTestPage(pageData))
 includeSetupAndTeardownPages(pageData, isSuite);
 return pageData.getHtml();
 }
 
 // Prefer exceptions to returning error codes
 try {
deletePage(page);
registry.deleteReference(page.name);
configKeys.deleteKey(page.name.makeKey());
}
catch (Exception e) {
logger.log(e.getMessage());
}
```

### function

* It should be small.
* Do one thing
* One level of abstraction per function
* Use Descriptive names
* have no side effect
* Prefer exceptions to returning error codes
* Extract Try Catch blocks into functions of their own

### Comments

* Dont use a comment when you can use a function or a variable
* Don not comment closing brace comments, try to shorten your functions instead





### Best Practice

{% embed url="https://medium.com/geekculture/writing-clean-javascript-es6-edition-834e83abc746" %}

**1.通过条件判断给变量赋值布尔值的正确姿势**

```
// bad
if (a === 'a') {
    b = true
} else {
    b = false
}

// good
b = a === 'a'

```

**2.在if中判断数组长度不为零的正确姿势**

```
// bad
if (arr.length !== 0) {
    // todo
}

// good
if (arr.length) {
    // todo
}

```

**3.简单的if判断使用三元表达式**

```
// bad
if (a === 'a') {
    b = a
} else {
    b = c
}

// good
b = a === 'a' ? a : c

```

**4.使用includes简化if判断**

```
// bad
if (a === 1 || a === 2 || a === 3 || a === 4) {
    // todo
}

// good
let arr = [1, 2, 3, 4]
if (arr.includes(a)) {
    // todo
}

```

**5.使用some方法判断是否有满足条件的元素**

```
// bad
let arr = [1, 3, 5, 7]
function isHasNum (n) {
    for (let i = 0; i < arr.length; i ++) {
        if (arr[i] === n) {
            return true
        }
    }
    return false
}

// good
let arr = [1, 3, 5, 7]
let isHasNum = n => arr.some(num => num === n)

// best
let arr = [1, 3, 5, 7]
let isHasNum = (n, arr) => arr.some(num => num === n)
```

**6.使用forEach方法遍历数组，不形成新数组**

```
// bad
for (let i = 0; i < arr.length; i ++) {
    // todo
    arr[i].key = balabala
}

// good
arr.forEach(item => {
    // todo
    item.key = balabala
})

```

**7.使用filter方法过滤原数组，形成新数组**

```
// bad
let arr = [1, 3, 5, 7],
    newArr = []
for (let i = 0; i < arr.length; i ++) {
    if (arr[i] > 4) {
        newArr.push(arr[i])
    }
}

// good
let arr = [1, 3, 5, 7]
let newArr = arr.filter(n => n > 4) // [5, 7]

```

**8.使用map对数组中所有元素批量处理，形成新数组**

```
// bad
let arr = [1, 3, 5, 7],
    newArr = []
for (let i = 0; i < arr.length; i ++) {
    newArr.push(arr[i] + 1)
}

// good
let arr = [1, 3, 5, 7]
let newArr = arr.map(n => n + 1) // [2, 4, 6, 8]

```

**9.使用Object.values快速获取对象键值**

```
let obj = {
    a: 1,
    b: 2
}
// bad
let values = []
for (key in obj) {
    values.push(obj[key])
}

// good
let values = Object.values(obj) // [1, 2]

```

**10. 使用Object.keys快速获取对象键名**

```
let obj = {
    a: 1,
    b: 2
}
// bad
let keys = []
for (value in obj) {
    keys.push(value)
}

// good
let keys = Object.keys(obj) // ['a', 'b']

```

**11.解构数组进行变量值的替换**

```
// bad
let a = 1,
    b = 2
let temp = a
a = b
b = temp

// good
let a = 1,
    b = 2
[b, a] = [a, b]

```

**12.解构对象**

```
// bad
setForm (person) {
    this.name = person.name
    this.age = person.age 
}

// good
setForm ({name, age}) {
    this.name = name
    this.age = age 
}

```

**13.解构时重命名简化命名**

```
// bad
setForm (data) {
    this.one = data.aaa_bbb_ccc_ddd
    this.two = data.eee_fff_ggg
}
// good
setForm ({aaa_bbb_ccc_ddd, eee_fff_ggg}) {
    this.one = aaa_bbb_ccc_ddd
    this.two = eee_fff_ggg
}

// best
setForm ({aaa_bbb_ccc_ddd: one, eee_fff_ggg: two}) {
    this.one = one
    this.two = two
}

```

**14.解构时设置默认值**

```
// bad
setForm ({name, age}) {
    if (!age) age = 16
    this.name = name
    this.age = age 
}

// good
setForm ({name, age = 16}) {
    this.name = name
    this.age = age 
}

```

**15.||短路符设置默认值**

```
let person = {
    name: '张三',
    age: 38
}

let name = person.name || '佚名'

```

**16.字符串拼接使用`${}`**

```
let person = {
    name: 'LiMing',
    age: 18
}
// bad
function sayHi (obj) {
    console.log('大家好，我叫' + person.name = '，我今年' + person.age + '了')
}

// good
function sayHi (person) {
    console.log(`大家好，我叫${person.name}，我今年${person.age}了`)
}

// best
function sayHi ({name, age}) {
    console.log(`大家好，我叫${name}，我今年${age}了`)
}


```

**17.函数使用箭头函数**

```
let arr [18, 19, 20, 21, 22]
// bad
function findStudentByAge (arr, age) {
    return arr.filter(function (num) {
        return num === age
    })
}

// good
let findStudentByAge = (arr, age)=> arr.filter(num => num === age)

```

**18.函数参数校验**

```
// bad
let findStudentByAge = (arr, age) => {
    if (!age) throw new Error('参数不能为空')
    return arr.filter(num => num === age)
}

// good
let checkoutType = () => {
    throw new Error('参数不能为空')
}
let findStudentByAge = (arr, age = checkoutType()) =>
    arr.filter(num => num === age)

```

**19： use sprad operation:**

* spreading an array or object into a new array or object, and joining multiple parameters into an array

```
let a = [3, 4, 5];let b = [1, 2, ...a, 6];console.log(b);  // [1, 2, 3, 4, 5, 6]
```

**20: Avoid hardcoded values**

Instead of plugging in constant values, make sure to declare meaningful and searchable constants. Notice that global constants can be stylized in Screaming Snake Case (_SCREAMING\_SNAKE\_CASE_).

```
// Don't ❌
setTimeout(clearSessionData, 900000);

// Do ✅
const SESSION_DURATION_MS = 15 * 60 * 1000;

setTimeout(clearSessionData, SESSION_DURATION_MS);
```

#### 21: Avoid executing multiple actions in a function

A function should do one thing at a time. This rule helps reduce the function’s size and complexity, which results in easier testing, debugging, and refactoring. The number of lines in a function is a strong indicator that should raise a flag on whether the function is doing many actions. Generally, try aiming for something less than 20–30 lines of code.

```
// Don't ❌
function pingUsers(users) {
  users.forEach((user) => {
    const userRecord = database.lookup(user);
    if (!userRecord.isActive()) {
      ping(user);
    }
  });
}

// Do ✅
function pingInactiveUsers(users) {
  users.filter(!isUserActive).forEach(ping);
}

function isUserActive(user) {
  const userRecord = database.lookup(user);
  return userRecord.isActive();
}
```

#### 22: Avoid using flags as arguments <a href="2c9d" id="2c9d"></a>

A flag in one of the arguments effectively means the function can still be simplified.

```
// Don't ❌
function createFile(name, isPublic) {
  if (isPublic) {
    fs.create(`./public/${name}`);
  } else {
    fs.create(name);
  }
}

// Do ✅
function createFile(name) {
  fs.create(name);
}

function createPublicFile(name) {
  createFile(`./public/${name}`);
}
```

#### 23:Avoid side effects <a href="d93c" id="d93c"></a>

keep functions pure unless needed otherwise. Side effects can modify shared states and resources, resulting in undesired behaviors. All side effects should be centralized; if you need to mutate a global value or modify a file, dedicate one and only one service for that.

```
// Don't ❌
let date = "21-8-2021";

function splitIntoDayMonthYear() {
  date = date.split("-");
}

splitIntoDayMonthYear();

// Another function could be expecting date as a string
console.log(date); // ['21', '8', '2021'];

// Do ✅
function splitIntoDayMonthYear(date) {
  return date.split("-");
}

const date = "21-8-2021";
const newDate = splitIntoDayMonthYear(date);

// Original vlaue is intact
console.log(date); // '21-8-2021';
console.log(newDate); // ['21', '8', '2021'];
```

#### 24:Avoid branching and return soon <a href="d997" id="d997"></a>

Returning early will make your code linear, more readable, and less complex.

```
// Don't ❌
function addUserService(db, user) {
  if (!db) {
    if (!db.isConnected()) {
      if (!user) {
        return db.insert("users", user);
      } else {
        throw new Error("No user");
      }
    } else {
      throw new Error("No database connection");
    }
  } else {
    throw new Error("No database");
  }
}

// Do ✅
function addUserService(db, user) {
  if (!db) throw new Error("No database");
  if (!db.isConnected()) throw new Error("No database connection");
  if (!user) throw new Error("No user");

  return db.insert("users", user);
}
```

#### 25:Favor object literals or maps over switch statements

Whenever this applies, indexing using objects or maps will reduce code and improve performance.

```
// Don't ❌
const getColorByStatus = (status) => {
  switch (status) {
    case "success":
      return "green";
    case "failure":
      return "red";
    case "warning":
      return "yellow";
    case "loading":
    default:
      return "blue";
  }
};

// Do ✅
const statusColors = {
  success: "green",
  failure: "red",
  warning: "yellow",
  loading: "blue",
};

const getColorByStatus = (status) => statusColors[status] || "blue";
```

#### 26:Handle thrown errors and rejected promises

No need to mention why this is an extremely important rule. Spending time now on handling errors correctly will reduce the likelihood of having to hunt down bugs later, especially when your code reaches production.

```
// Don't ❌
try {
  // Possible erronous code
} catch (e) {
  console.log(e);
}

// Do ✅
try {
  // Possible erronous code
} catch (e) {
  // Follow the most applicable (or all):
  // 1- More suitable than console.log
  console.error(e);

  // 2- Notify user if applicable
  alertUserOfError(e);

  // 3- Report to server
  reportErrorToServer(e);

  // 4- Use a custom error handler
  throw new CustomError(e);
}
view raw
```
