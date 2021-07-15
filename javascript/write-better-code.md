# Write better code

##  几个优雅的 JS 运算符号

{% embed url="https://mp.weixin.qq.com/s/FwF\_awtPQAgXQYs1SWcDfw" %}

{% embed url="https://mp.weixin.qq.com/s/Xw7-6ZnuUKCq1\_s\_d-I98w" %}



nullish 代表 null 或者 undefined

### ??

```text
null ?? 5 // => 5
3 ?? 5 // => 3
```

* **总结一下，?? 操作符允许我们分配默认值，同时忽略像 0 和空字符串这样的假值。**
* 在 JavaScript 中，?? 操作符被称为nullish 合并操作符。如果第一个参数不是 null/undefined，这个运算符将返回第一个参数，否则，它将返回第二个参数。

### 逻辑空分配（?? =）

```text
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

```text
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

###  ? 操作符

```text
function checkCharge(charge) {
return (charge > 0) ? 'Ready for use' : 'Needs to charge'
}
```

* 三元运算符

### 逻辑或分配（\|\| =）

```text
const a = { duration: 50, title: '' };

a.duration ||= 10;
console.log(a.duration);
// expected output: 50

a.title ||= 'title is empty.';
console.log(a.title);
// expected output: "title is empty"

```

* The logical OR assignment \(x \|\|= y\) operator only assigns if x is falsy. 如果是 false 就会赋值

### 逻辑与分配（&& =）

```text
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

```text
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

* 小写驼峰命名 lowerCamelCase













