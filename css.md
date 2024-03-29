# CSS

## Flex 布局

{% embed url="https://zhuanlan.zhihu.com/p/25303493" %}



* 有两个值： flex , inline-flex, 如果你使用块元素如 div，你就可以使用 flex，而如果你使用行内元素，你可以使用 inline-flex。
* 当设置为 flex 布局 后， **子元素的 float、clear、vertical-align 的属性将会失效。**
*   **下列 6种属性可以设置在 容器上**

    * flex-direction
    * flex-wrap
    * flex-flow
    * justify-content
    * align-items
    * align-content


*

    &#x20;**** flex-direction: 决定主轴的方向(即项目的排列方向)

    * &#x20;**** flex-direction: row | row-reverse | column | column-reverse;
* flex-wrap: 决定容器内项目是否可换行
  * &#x20; flex-wrap: nowrap | wrap | wrap-reverse;
* &#x20;flex-flow: flex-direction 和 flex-wrap 的简写形式
  * &#x20;flex-flow: \<flex-direction> || \<flex-wrap>;
  * 直接分开写 就不需要记这个属性了
* &#x20;justify-content：定义了项目在主轴的对齐方式。
* align-items: 定义了项目在交叉轴上的对齐方式
* align-content: 定义了多根轴线的对齐方式，如果项目只有一根轴线，那么该属性将不起作用
  * 当设置为 `flex-wrap: wrap`，   容器就会出现多条轴线
  * &#x20;_Note, this property has no effect when the flexbox has only a single line._

![](<.gitbook/assets/image (43).png>)

### Flex item 属性

* 有6种 属性可在 item 上面 使用
  * order
  * flex-basis
  * flex-grow
  * flex-shrink
  * flex
  * align-self
* order: 定义项目在容器中的排列顺序，数值越小，排列越靠前，默认值为 0
* **flex-grow**:默认值为 0，即如果存在剩余空间，也不放大,If all items have flex-grow set to 1, the remaining space in the container will be distributed equally to all children. If one of the children has a value of 2, the remaining space would take up twice as much space as the others&#x20;
* **flex-shrink**：**默认为1，即如果空间不足，该项目将缩小。** This defines the ability for a flex item to shrink if necessary.
* **flex-basis**:定义了在分配剩余空间之前元素的默认大小  can be set to any value that would apply to width, including values in px, ems, or percentages. Its initial value is auto, which means the browser will look to see if the element has a width declared. If so, the browser uses that size; if not, it determines the element’s size naturally by the contents   **默认值是`auto`，就是自动。有设置`width`则占据空间就是`width`，没有设置就按内容宽度来**
  * **This means that width will be ignored for elements that have any flex basis    &#x20;other than auto**
  * &#x20;当剩余空间不足的时候，flex子项的实际宽度并通常不是设置的`flex-basis`尺寸，因为flex布局剩余空间不足的时候默认会收缩
* &#x20;**flex**: flex-grow, flex-shrink 和 flex-basis的简写
  * &#x20;flex 的默认值是 0 1 auto。
  * &#x20;2个快捷键 `auto` (`1 1 auto`) 和 none (`0 0 auto`)。
* **align-self**:允许单个项目有与其他项目不一样的对齐方式

![](<.gitbook/assets/image (44).png>)

## Grid 布局

{% embed url="https://juejin.cn/post/6854573220306255880" %}



* Grid 算作二维布局， `display: grid | inline-grid;`
* **grid-template-columns** 和 **grid-template-rows** 属性来定义网格中的行和列
  * &#x20;`grid-template-columns` 属性设置列宽 `grid-template-rows` 属性设置行高
* **grid-row-gap** 属性、**grid-column-gap** 属性以及 grid-gap 属性
  * &#x20;`grid-row-gap` 属性、`grid-column-gap` 属性分别设置行间距和列间距。`grid-gap` 属性是两者的简写形式
  * &#x20;`grid-row-gap: 10px` 表示行间距是 10px，`grid-column-gap: 20px` 表示列间距是 20px。`grid-gap: 10px 20px` 实现的效果是一样的
* **grid-template-areas：** 属性用于定义区域，一个区域由一个或者多个单元格组成
* **grid-auto-flow ：默认值是 row**  `grid-auto-flow: row | column | row dense | column dense;`
* **grid-auto-columns:,grid-auto-rows:**Specifies the size of any auto-generated grid track, 多余的行会根据 grid-auto-rows 算，&#x20;
* &#x20;**justify-items** 属性设置单元格内容的水平位置
* &#x20;**align-items** 属性设置单元格的垂直位置（上中下）
* &#x20;**justify-content** 属性是整个内容区域在容器里面的水平位置
* &#x20;**align-content** 属性是整个内容区域的垂直位置（上中下）

### Grid 实战

* fr 实现 等分响应式: `grid-template-columns: 1fr 1fr 1fr;` 表示容器分为三等分
* repeat + auto-fit——固定列宽，改变列数量   `grid-template-columns: repeat(auto-fit, 200px);`
  * 我们的网格能够固定列宽，并根据容器的宽度来改变列的数量,表示固定列宽为 200px，数量是自适应的，只要容纳得下，就会往上排列

### Grid 兼容性

* 但在 IE 10 以下不支持。个人建议在公司的内部系统运用起来是没有问题的，但 TOC 的话，可能目前还是不太合适







## CSS box mode

![](<.gitbook/assets/image (41).png>)

* Default `box-sizing: content-box`   This means that any height or width you specify only sets the size of the content box

![](<.gitbook/assets/image (42).png>)

* If you set an element's width to 100 pixels, that 100 pixels will include any border or padding you added,设置元素的height/width属性指的是border + padding + content部分的高/宽

## CSS 那些属性是可以继承的

* 可继承的属性：font-size, font-family, color
* 不可继承的样式：border, padding, margin, width, height

## CSS Selectors

```
// eg 1
p, div, span {
  background-color: yellow;
}
// eg 2
h1[class] {color: silver;}

<h1 class="hoopla">Hello</h1>
<h1>Serenity</h1>
<h1 class="fancy">Fooling</h1>

// eg 3
planet[moons="1"] {font-weight: bold;}
<planet>Venus</planet>
<planet moons="1">Earth</planet>
<planet moons="2">Mars</planet>


// eg 4 This rule will select any element whose lang attribute is equal to en or begins with
en- , The first three would be selected
*[lang|="en"] {color: white;}

<h1 lang="en">Hello!</h1>
<p lang="en-us">Greetings!</p>
<div lang="en-au">G'day!</div>
<p lang="fr">Bonjour!</p>
<h4 lang="cy-en">Jrooana!</h4>

// eg 5
h1 em {color: gray;}



// eg 6 
h1 > strong {color: red;}

// eg 7
p + h1 {
  background-color: orange;
}

// eg 8

p ~ h1 {
  background-color: orange;
}
```

* Id selector has more specificity than the attribute, class, pseudo-class, and pseudo-element selectors. Only inline styles have move specificity than id selectors.
* We use , for grouping multiple selectors **(eg1)**
* **Attribute selectors:**
  * **Simple attribute selector** , select all h1 ele‐ ments that have a class attribute **(eg2)**
  * **With exact attribute value:   (eg 3)**
  * **Selection based on partial attribute values**
    * `[foo~="bar"]` Selects any element with an attribute foo whose value contains the word bar in a space-separated list of words
    * `[foo*="bar"]` Selects any element with an attribute foo whose value contains the substring bar
    * `[foo^="bar"]` Selects any element with an attribute foo whose value begins with bar
    * `[foo$="bar"]` Selects any element with an attribute foo whose value ends with bar
    * `[foo|="bar"]`] Selects any element with an attribute foo whose value starts with bar followed by a dash -  or whose value is exactly equal to bar   **(eg 4)**
* **Descendant Selector:  (eg 5),** Select **** em elements that are descended from h1 elements
* **Selecting children (eg 6),** select a strong element only if it is a direct child of an h1 element
* **Selecting Adjacent Sibling Elements (eg 7)**  ,  All the immediate h1 tags  after p tags changed their background color to orange. If there is any other element between `p` tag and `h1` tag then this rule is not applicable. **只有第一个 出现的  才会 apply**
* **General Sibling Selector (eg 8)**     All h1 tags after p tags have orange background color

### Chain pseudo-classes

```
a:link:hover {color: red;}
a:visited:hover {color: maroon;}
```

* The order doesn't really matter

### Pseduo-class 伪类

```
// EG 1
:root {border: 10px dotted gray;}

// EG 2,
// 	Selects every <p> element that has no children (including text nodes)
 p:empty {display: none;}
 
// EG 3 Selects every <p> element that is the only child of its parent
p:only-child
	
// EG 4
p:first-child

// EG 6

a:active

// EG 7

input:checked

// EG 8

input:default

// EG 9
p:first-of-type

// EG 12
p:nth-child(2)
```

A pseudo-class is a selector that selects elements that are in a specific state, e.g. they are the first element of their type, or they are being hovered over by the mouse pointer. They tend to act as if you had applied a class to some part of your document

* 伪类可以进行多个拼接  `input:out-of-range:focuse {background:gold;}`

伪类就是开头为冒号的关键字：

1. Selecting the root element   &#x20; `:root`
2. Selecting empty elements `:empty`
3. `:only-child`
4. `:first-child`Selects every  element that is the first child of its parent
5. `:last-child`
6. `:active`Selects the active link
7. `:checked` Selects every checked  element
8. `:default` Selects the default  element
9. `:first-of-type` Selects every \<p> element that is the first \<p> element of its parent
10. `:hover`
11. `:focus`
12. `:nth-child(n)`Selects every  p element that is the second child of its parent
    1. `:nth-child(2n)`  所有的 even element
13. `:nth-last-child(n)`
14. `:nth-last-of-type(n)`
15. `:last-of-type`

### &#x20;伪元素 pseudo-element

Pseudo-elements start with a double colon ::

* 伪元素只能放在最后面  所以 `input::after:checked{display:block}`   **不正确**

```
// eg 1, 就会在之后添加 这些东西
p::after { 
  content: " - Remember this";
}

// eg 3
p::first-line


// eg 4
p::first-line
```

1. `::after`
2. `::before`
3. `::first-letter`Selects the first letter of every  element，**only work for block-display elements**
4. `::first-line`Selects the first line of every \<p> element，**only work for block-display elements**

####



## CSS 优先级，specifity of each selector

![](<.gitbook/assets/image (39).png>)

![](<.gitbook/assets/image (40).png>)

```
// EG 1
html > body table tr[id="totals"] td ul > li {color: maroon;} 
```

* For every ID attribute value: 100
* For every class attribute value, attribute selection, or pseudo-class : 10
  * ayttribute selector : `[id="totals"]`
* For every element and pseudo-element: 1
* Combinators and the universal selector do not contribute anything to the specificity
  * combinators:&#x20;
    * descendant selector (space)
    * child selector (>)
    * adjacent sibling selector (+)
    * general sibling selector (\~)
* Inline-style: 1000
* !important: it always goes at the end of the declaration, just before the semicolon.where an important and a non-important declaration con‐flict, the important declaration always wins.
*   if two rules have exactly the same explicit weight, origin, and specificity, then

    the one that occurs later in the style sheet wins out
* 继承得到的样式的优先级最低。

### Order of link style:

```
a:link {color: blue;}
a:visited {color: purple;}
a:focus {color: green;}
a:hover {color: red;}
a:active {color: orange;}
```

* they all have same  explicit weight, origin, specificty, visited must come before hover, focus, active, since, every link is visited or unvisited.



## Margin:

![](<.gitbook/assets/image (49).png>)

* 底下方法可以使 div 包含 margin

```
<div class="wrapper"><h1>This is heading 1</h1></div>

// CSS

.wrapper{
background:red;
}
.wrapper::before,
.wrapper::after {
    display: table;
    content: " ";
}

```

### 外边框塌陷:（margin）

```
//case 1
<div class="box1 box">
</div>
<div class="box2 box">

//CSS 每个 box 都有 上下10px 理论上应该会有 20px间距 但是最终只有10px
.box{
  width:100px;
  height:100px;
  
}
// 方法 1：inline block的方法
.box1{
  background:lightblue;
  margin:10px;
}
.box2{
  background:red;
  margin:10px;
   display:inline-block; 
}

// 方法 2：相对定位
.box2{
  background:red;
   position:relative;
   top:10px;
}
```

![](<.gitbook/assets/image (54).png>)



![A元素包含 B元素，A元素和C元素是 兄弟关系](<.gitbook/assets/image (55).png>)

![错误的code 的效果， 这里 选取了 20px](<.gitbook/assets/image (56).png>)

```
// 怎么可以给A元素 margin top 10px， B 元素  margin top 20px
// 这里跟 A，B 同时设置外边距， 就会出现外边框塌陷

// 错误的 code
.divC{
height: 60px;
width:200px;
background:red;
}

.divA{
height: 80px;
width:200px;
background: lightgrey;
margin-top:10px;
}

.divB{
height: 30px;
width:200px;
background:lightblue;
margin-top:20px;
}



//绝对定位

.divC{
height: 60px;
width:200px;
background:red;
}

.divA{
height: 80px;
width:200px;
background: lightgrey;
margin-top:10px;
position: relative;
}

.divB{
height: 30px;
width:200px;
background:lightblue;
position: absolute;
margin-top:20px;
}


// 使用 inline block element

.divA{
height: 80px;
width:200px;
background: lightgrey;
margin-top:10px;
}

.divB{
height: 30px;
width:200px;
background:lightblue;
display:inline-block;
margin-top:20px;
}

// 使用 相对定位

.divA{
height: 80px;
width:200px;
background: lightgrey;
margin-top:10px;
}

.divB{
height: 30px;
width:200px;
background:lightblue;
position:relative;
top:20px;
}

// 在父类使用 BFC

.divA{
height: 80px;
width:200px;
background: lightgrey;
margin-top:10px;
overflow:hidden;
}

.divB{
height: 30px;
width:200px;
background:lightblue;
margin-top:20px;
}


// 使用 padding

.divA{
height: 80px;
width:200px;
background: lightgrey;
margin-top:10px;
padding-top:20px;
}

.divB{
height: 30px;
width:200px;
background:lightblue;
}
```

* 什么时候会发现这种事情？
  * 发生在垂直方向 而不是 水平方向
  * 只会发生在 block element， inline-block 和 inline 都不会发生
* 外边距计算
  * 两个都是正数 取 最大的数
  * 两个都是负数， 取 绝对值最大的
  * 一正 一负， 相加的和
*   解决办法

    * 绝对定位
    * &#x20;使用 inline block element
    * 使用 相对定位,就不可以使用 margin-top 定位了
    * 使用 BFC
    * 使用 padding







## float:

![Typical float example](<.gitbook/assets/image (45).png>)

*

### The great Collapse:浮动塌陷

![](<.gitbook/assets/image (47).png>)

* floated elements does not affect parent height

### **清除浮动**  How to fix above issue?

* Method 1: 使用BFC 在parent 这里 加入 `overflow:auto;`
* Method 2: 把父元素也变成float 就可以了， 但是这样子会影响布局， 比较局限
* Method 3: Adds an empty div with a clear at the end of parent div  ,  drawback: **It add unwanted markup to HTML**  `<div style="clear: left"></div>`
* Method 4:we can use pseudo-element, ::after   **最推荐**

![](<.gitbook/assets/image (48).png>)



### clear property:

![](<.gitbook/assets/image (46).png>)

* &#x20;When we use the `float` property, and we want the next element below (not on right or left), we will have to use the `clear` property.
* &#x20;The `clear` property specifies what should happen with the element that is next to a floating element.
* &#x20;The `clear` property can have one of the following values:
  * `none` - The element is not pushed below left or right floated elements. This is default
  * `left` - The element is pushed below left floated elements
  * `right` - The element is pushed below right floated elements
  * `both` - The element is pushed below both left and right floated elements
  * `inherit` - The element inherits the clear value from its parent

![](<.gitbook/assets/image (50).png>)



```
.media:nth-child(odd) {
  clear: left;
}
```

* In order to fix above issue, the third float needs to clear the floats above it.more generally, **the first element of each row needs to clear the float above it**

![img is floated to the left](<.gitbook/assets/image (51).png>)

![](<.gitbook/assets/image (52).png>)

![](<.gitbook/assets/image (53).png>)

```
<div class="media">
 <img class="media-image" src="shoes.png">
 <div class="media-body">
 <h4>Change it up</h4>
 <p>
 Don't run the same every time you hit the
 road. Vary your pace, and vary the distance
 of your runs.
 </p>
 </div>
</div>


//CSS create a new BFC
  .media-body {
      overflow: auto;
      margin-top: 0;
}
```

* When the text is long enough, it wraps around the floated element. This is the normal float behavior, but it’s not what we want in this case
* How to solve above issue?
  * we need to establish BFS for the media body. **So it will not overlap with floated elements outside the BFC.**

****

****

### ****



## BFC

```
//防止浮动导致父元素高度塌陷

  <div class="container">
    <div class="inner"></div>
  </div>
 .container {
    border: 10px solid red;
     overflow: hidden;// 父元素变成BFC 解决元素塌陷问题
  }

.inner {
    float: left;
    background: #08BDEB;
    height: 100px;
    width: 100px;
  }
  
  
  //阻止普通文档流元素被浮动元素覆盖
  
  <div class="demo">
    <div class="demo1">我是一个左侧浮动元素</div>
    <div class="demo2">我是一个没有设置浮动, 也没有触发BFC的元素</div>
</div>


    * {
        margin: 0;
        padding: 0;
    }
    .demo1 {
        width: 100px;
        height: 100px;
        float: left;
        background: pink
    }
    .demo2 {
        width: 200px;
        height: 200px;
        background: blue;
        overflow: hidden; // 创建BFC
    }
    
    
```

* A block formatting context itself is part of the surrounding document flow, **but it isolates its contents from  &#x20;the outside context,** This isolation does three things for the element that establishes the BFC(原理):
  * It contains the top and bottom margins of all elements within it. They won’t collapse with margins of elements outside of the block formatting context. **不会出现外边距塌陷**
  * **It contains internal floats**.
    * Make float content and alongside content the same height.
  * **It excludes external floats.**
    * because an element in the normal flow that establishes a new BFC must not overlap the margin box of any floats in the same bfc as the element itself. **The BFC area will not overlap with the float box.**
  * **When calculating the height of the BFC, floating elements also participate in the calculation**
* BFC是一个独立的容器，外面的元素不会影响里面的元素
* the contents inside a block formatting context will not overlap or interact with elements on the outside as you would normally expect
* How to establish a new BFC:
  * 根元素()
  * float: left or float: right—anything but none 浮动元素
  * overflow: hidden, auto, or scroll—anything but visible
  *   display: inline-block, table-cell, table-caption, flex, inline-flex,&#x20;

      grid, or inline-grid—these are called block containers
  * position: absolute or position: fixed 绝对定位元素
* 应用场景
  * **防止浮动导致父元素高度塌陷，** ![](<.gitbook/assets/image (62).png>)&#x20;
  * **外边框塌陷问题 （上面有解决办法）**
  * **阻止普通文档流元素被浮动元素覆盖** ![](<.gitbook/assets/image (63).png>) 创建了BFC 之后 ![](<.gitbook/assets/image (64).png>)&#x20;
  *

## 掌握一套完整的响应式布局方案



## Position:

* static（默认）：按照正常文档流进行排列；So if there is a left/right/top/bottom/z-index set then there will be no effect on that element.
* relative（相对定位）：不脱离文档流， But now left/right/top/bottom/z-index will work.
* absolute(绝对定位)：参考距其最近一个不为static的父级元素通过top, bottom, left, right 定位；
* fixed(固定定位)：the element is removed from the flow of the document like absolutely positioned elements. In fact they behave almost the same, only fixed positioned elements are always relative to the document, not any particular parent, and are unaffected by scrolling.
* sticky: sticky (experimental): the element is treated like a relative value until the scroll location of the viewport reaches a specified threshold, at which point the element takes a fixed position where it is told to stick.
  * 父类元素不能有 `overflow:visible`以外的overflow设置

```
.element {
  position: sticky; top: 50px;
}

//The element will be relatively positioned until the scroll location of the viewport reaches a point where the element will be 50px from the top of the viewport. At that point, 
//the element becomes sticky and remains at a fixed position 50px top of the screen
```

## CSS3 有哪些新特性

* RGBA和透明度
* word-wrap（对长的不可分割单词换行）word-wrap：break-word
* 圆角（边框半径）：border-radius 属性用于创建圆角
* 盒阴影：box-shadow: 10px 10px 5px #888888
* 媒体查询（media query）：定义两套css，当浏览器的尺寸变化时会采用不同的属性
*   边框图片：border-image: url(border.png) 30 30 round

    ```
    /* border-image: image-source image-height image-width image-repeat */

    ```

## **为什么要初始化CSS样式**

* 因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异

## &#x20; **CSS里的visibility属性有个collapse属性值？**

* visible**:** Default value. The element is visible
* hidden: The element is hidden (but still takes up space)
*   collapse: Only for table rows (\<tr>), row groups (\<tbody>), columns (\<col>), column groups (\<colgroup>). This value removes a row or column, but it does not affect the table layout. The space taken up by the row or column will be available for other content.

    **If collapse is used on other elements, it renders as "hidden"**
* initial: It is used to set a CSS property to its default value.

## &#x20;**display:none,visibility：hidden, opacity:0的区别？**

* display：none 不显示对应的元素，在文档布局中不再分配空间（回流+重绘）， 子元素即使设置了 display： block 也不会再出现
* visibility：hidden 隐藏对应元素，在文档布局中仍保留原来的空间（重绘）,transition 可以正常使用，子元素可以通过设置visibility 出现
* opacity: 跟visibility 很相似， opacity:0 **不会导致重绘，  子元素不能复原**

![](<.gitbook/assets/image (59).png>)





## CSS @media Rule

```
//Change the background color of the <body> 
//element to "lightblue" when the browser window is 600px wide or less:

@media only screen and (max-width: 600px) {
  body {
    background-color: lightblue;
  }
}


@media print{
 body{
 color:black;
 }
}

```

* all - for all media type device
* print - for printers
* speech - for screenreader that reads the page out loud
* screen for computer screens, tablets, phone





## Overflow 属性

* scroll : 必定出现滚动条
* auto: 子元素大于父元素时候 出现滚动条
* visible:  Default. The overflow is not clipped. The content renders outside the element's box
* hidden: The overflow is clipped, and the rest of the content will be invisible

## line-height

* 基线距离就是line height
* It accept both units and unitless values
* &#x20;CSS中起高度作用的应该就是`height`以及`line-height`了吧！如果一个标签没有定义`height`属性(包括百分比高度)，那么其最终表现的高度一定是由`line-height`起作用
* &#x20;`line-height`行高怎么就产生了高度呢?
  *   在`inline box`模型中，有个`line boxes`，这玩意是看不见的，这个玩意的工作就是包裹每行文字。一行文字一个`line boxes`。一个没有设置`height`属性的`div`的高度就是由一个一个`line boxes`的高度堆积而成的。

      其实`line boxes`不是直接的生产者，属于中层干部，真正的活儿都是它的手下 – `inline boxes`干的，这些手下就是文字啦，图片啊，`<span>`之类的`inline`属性的标签啦。`line boxes`只是个考察汇报人员，考察它的手下谁的实际`line-height`值最高，谁最高，它就要谁的值，然后向上汇报，形成高度。例如，`<span style="line-height:20px;">取手下line-height<span style="line-height:40px;">最高</span>的值</span>`。则line boxes的高度就是40像素了
* You should typically use unitless numbers because they’re inherited differently  `line-height: 1.2;`
  * the line    &#x20;height is calculated locally to 38.4 px (32 px × 1.2).
  * When you use a unitless number, that declared value is inherited, meaning its computed value is recalculated for each inheriting child element



## stacking context 层叠上下文

_Groups of elements with a common parent that move forward or backward together in the stacking order make up what is known as a stacking context_

* &#x20;`z-index`属性值并不是在任何元素上都有效果。它**仅在**定位元素（定义了`position`属性，且属性值为非`static`值的元素）上有效果。
* when you add a z-index to a positioned element that element becomes the root of a new stacking context
* 判断元素在`Z轴`上的堆叠顺序，不仅仅是直接比较两个元素的`z-index`值的大小，这个堆叠顺序实际由元素的**层叠上下文**、**层叠等级(stacking level)**共同决定。
* 普通元素的层叠等级优先由其所在的层叠上下文决定。
* 层叠等级的比较只有在当前层叠上下文元素中才有意义。不同层叠上下文中比较层叠等级是没有意义的
* 如何产生  stacking context
  * HTML中的根元素\<html>\</html>本身j就具有层叠上下文，称为“根层叠上下文
  * 普通元素设置position属性为非static值并设置z-index属性为具体数值，产生层叠上下文， **z-index: auto 不会产生 stacking context**
  *   CSS3中的新属性也可以产生层叠上下文, 只要满足以下条件， 就会产生 层叠 上下文

      * 父元素的display属性值为flex|inline-flex，子元素z-index属性值不为auto的时候，子元素为层叠上下文元素；
      * &#x20;元素的`opacity`属性值不是`1`
      * 元素的`transform`属性值不是`none`；
      * 元素`mix-blend-mode属性值不是`normal\`；
      * 元素的`filter`属性值不是`none`；
      * 元素的`isolation`属性值是`isolate`；
      * `will-change`指定的属性值为上面任意一个；
      * 元素的`-webkit-overflow-scrolling`属性值设置为`touch`。





### 例子 1

{% embed url="https://jsfiddle.net/Boyanliuu/yLr2150m/9/" %}



```
  <div>  
    <p class="a">a</p>  
    <p class="b">b</p>  
  </div> 

  <div>  
    <p class="c">c</p>  
  </div>  


// CSS
* {
     margin: 0;
     padding: 0;
}
 div {
     position: relative;
     width: 100px;
     height: 100px;
}
 p {
     position: absolute;
     font-size: 20px;
     width: 100px;
     height: 100px;
}
 .a {
     background-color: blue;
     z-index: 1;
}
 .b {
     background-color: green;
     z-index: 2;
     top: 20px;
     left: 20px;
}
 .c {
     background-color: red;
     z-index: 3;
     top: -20px;
     left: 40px;
}
```

![](<.gitbook/assets/image (80).png>)

* &#x20;因为p.a、p.b、p.c三个的父元素div都没有设置`z-index`，所以不会产生层叠上下文，所以.a、.b、.c都处于由`<html></html>`标签产生的“根层叠上下文”中，属于同一个层叠上下文，此时谁的`z-index`值大，谁在上面。

### 例子2

![](<.gitbook/assets/image (81).png>)

```
<body>
  <div class="box1">
    <p class="a">a</p>
    <p class="b">b</p>
  </div>

  <div class="box2">
    <p class="c">c</p>
  </div>
  
  
  // CSS
 div {
     width: 100px;
     height: 100px;
     position: relative;
}
 .box1 {
     z-index: 2;
}
 .box2 {
     z-index: 1;
}
 p {
     position: absolute;
     font-size: 20px;
     width: 100px;
     height: 100px;
}
 .a {
     background-color: blue;
     z-index: 100;
}
 .b {
     background-color: green;
     top: 20px;
     left: 20px;
     z-index: 200;
}
 .c {
     background-color: red;
     top: -20px;
     left: 40px;
     z-index: 9999;
}
```

* 我们发下，虽然`p.c`元素的`z-index`值为9999，远大于`p.a`和`p.b`的`z-index`值，但是由于`p.a`、`p.b`的父元素`div.box1`产生的层叠上下文的`z-index`的值为2，`p.c`的父元素`div.box2`所产生的层叠上下文的`z-index`值为1，所以`p.c`永远在`p.a`和`p.b`下面。
* &#x20;同时，如果我们只更改`p.a`和`p.b`的`z-index`值，由于这两个元素都在父元素`div.box1`产生的层叠上下文中，所以，谁的`z-index`值大，谁在上面。



### 层叠顺序

![](<.gitbook/assets/image (82).png>)



1. 首先先看要比较的两个元素是否处于同一个层叠上下文中：
   1. 如果是，谁的层叠等级大，谁在上面（怎么判断层叠等级大小呢？——看“层叠顺序”图）
   2. 如果两个元素不在统一层叠上下文中，请先比较他们所处的层叠上下文的层叠等级
2. 当两个元素层叠等级相同、层叠顺序相同时，在DOM结构中后面的元素层叠等级在前面元素之上。

### 例子3

![](<.gitbook/assets/image (83).png>)

```
  <div class="box1">
    <div class="child1"></div>
  </div>

  <div class="box2">
    <div class="child2"></div>
  </div>
  
  // css
    .box1, .box2 {
    position: relative;
   z-index: auto;
  }
  .child1 {
    width: 200px;
    height: 100px;
    background: #168bf5;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
  }
  .child2 {
    width: 100px;
    height: 200px;
    background: #32c292;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
  }

```

* `.box1/.box2`虽然设置了`position: relative`，但是`z-index: auto`的情况下，这两个`div`还是普通元素，并没有产生层叠上下文。所以，`child1/.child2`属于`<html></html>`元素的“根层叠上下文”中，此时，**谁的`z-index`值大，谁在上面**

### 例子4

![](<.gitbook/assets/image (84).png>)

* &#x20;上面例子 把 `z-index:auto` 改成 `z-index: 0;`
* 因为设置`z-index: 0`后，`.box1/.box2`产生了各自的层叠上下文，这时候要比较`.child1/.child2`的层叠关系完全由父元素`.box1/.box2`的层叠关系决定。但是`.box1/.box2`的`z-index`值都为`0`，都是块级元素（所以它们的层叠等级，层叠顺序是相同的），这种情况下，在`DOM`结构中**后面的覆盖前面的**，所以`.child2`就在上面。



### 例子5 display flex



![](<.gitbook/assets/image (85).png>)

{% embed url="https://jsfiddle.net/Boyanliuu/81mLhb79/12/" %}

```
  
   <div class="box">
    <div class="parent">
      parent
      <div class="child">child</div>
    </div>
  </div>
  
  
  //css
  .box {
    display: flex;
  }
  .parent {
    width: 200px;
    height: 100px;
    background: #168bf5;
    /* 虽然设置了z-index，但是没有设置position，z-index无效，.parent还是普通元素，没有产生层叠上下文 */
    z-index: 1;
  }
  .child {
    width: 100px;
    height: 200px;
    background: #32d19c;
    position: relative;
    z-index: -1;
  }
```

* 当给`.box`设置`display: flex`时，`.parent`就变成层叠上下文元素，根据层叠顺序规则，层叠上下文元素的`background/border`的层叠等级小于`z-index`值小于`0`的元素的层叠等级，所以`z-index`值为`-1`的`.child`在`.parent`上面\


### 例子6 opacity

![](<.gitbook/assets/image (87).png>)

```
<div class="box">
    <img src="mm1.jpg">
</div>

//css
.box { background-color: blue; opacity: 0.5; }
.box > img { 
  position: relative; z-index: -1; right: -150px;
}
```

* box 创建了一个 stacking context 后 它就变为 最低等级的 stacking order，



### &#x20;例子7

{% embed url="https://jsfiddle.net/Boyanliuu/tsovLxqr/5/" %}



* parent 创建了一个新的 stacking context， 然后 根据 上面的 stacking order 判断的话， 他的颜色是最低级的所以 它在 child2 后面，
* 同理 child2 也创建了一个新的 stacking order， 然后 它也是最低级的 在它的2个children 后面

##

\




\


\


## 等高布局

### 使用table

* 父元素:  `display:table;`  子元素 `display: table-cell;`

### 使用 flexbox

### 使用 grid:

* 父元素: `display:grid;   grid-auto-flow:column;`

### 使用 margin padding 互相抵消

* 这个方法 不需要考虑 兼容性问题 最好
* padding 会把背景颜色延伸出去， 正的 margin 会把 边框往外推，然后 创建 一个 BFC 就可以了

![](<.gitbook/assets/image (65).png>)

```
div.left{
padding-bottom: 99999px;
margin-bottom: -99999px;
}
section{
overflow:hidden;
}
```



## CSS 单行省略

![](<.gitbook/assets/image (71).png>)

```
p{
  height:20px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

*

## CSS 多行省略

```
  overflow: hidden;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 5;
```

![](<.gitbook/assets/image (66).png>)

### 私有属性

* 简洁明了， 缺点就是 私有属性了不是规范的 属性

### 使用 规范 css， 2个伪元素

* &#x20;优点完全符合css 规范 ， 缺点是 如果背景 不是纯色就不好办了， 或者是图片就不好办了

```
// 1 使用 伪元素 创建一个包含 ... 的文本 放在右下角

 p::before{
  content:"...";
  position:absolute;
  right:0;
  bottom:0;
}

 // 2 给省略号添加一些空间, 并且是 文字两端 对齐
 p{
  height:100px;
  overflow: hidden;
  position:relative;
  padding-right:10px;
  text-align:justify;
}
// 3 在不需要省略时候隐藏掉 。。。 这里我们用 跟背景一样的颜色 掩盖省略号, 因为我们只是设置了 right
// 所以他会一直跟在后面
p::after{
  content:"";
  position:absolute;
  width:1em;
  height:1em;
  background:white;
  
  right:0;
  margin-top:0.5em;
}
```

### 另外一种省略方法

![](<.gitbook/assets/image (67).png>)

![](<.gitbook/assets/image (68).png>)

* 创建一个渐变颜色方块 遮挡住最后的文字， 接着 把 这个方块放到右下角的位置
* 然后 把渐变颜色的后面部分改成和背景颜色一样即可
* 缺点也是 背景 如果是图片会比较麻烦







## Layout Idea:

### 静态布局（static layout）

* 即传统Web设计，网页上的所有元素的尺寸一律使用px作为单位。

#### 布局特点

不管浏览器尺寸具体是多少，网页布局始终按照最初写代码时的布局来显示。常规的pc的网站都是静态（定宽度）布局的，也就是设置了min-width，这样的话，如果小于这个宽度就会出现滚动条，如果大于这个宽度则内容居中外加背景，这种设计常见于pc端。

**优点**：这种布局方式对设计师和CSS编写者来说都是最简单的，亦没有兼容性问题。

**缺点**：显而易见，即不能根据用户的屏幕尺寸做出不同的表现。当前，大部分门户网站、大部分企业的PC宣传站点都采用了这种布局方式。固定像素尺寸的网页是匹配固定像素尺寸显示器的最简单办法。但这种方法不是一种完全兼容未来网页的制作方法，我们需要一些适应未知设备的方法。

### 流式布局（Liquid Layout）

* &#x20;网页中主要的划分区域的**尺寸使用百分数**（搭配min-\*、max-\*属性使用），例如，设置网页主体的宽度为80%，min-width为960px。图片也作类似处理（width:100%, max-width一般设定为图片本身的尺寸，防止被拉伸而失真）。

#### &#x20;布局特点

　　屏幕分辨率变化时，页面里元素的大小会变化而但布局不变。【这就导致如果屏幕太大或者太小都会导致元素无法正常显示。

&#x20;**这种布局方式在Web前端开发的早期历史上，用来应对不同尺寸的PC屏幕**（那时屏幕尺寸的差异不会太大），**在当今的移动端开发也是常用布局方式**，但**缺点明显**：**主要的问题**是如果屏幕尺度跨度太大，那么在相对其原始设计而言过小或过大的屏幕上不能正常显示。因为宽度使用%百分比定义，但是高度和文字大小等大都是用px来固定，所以在大屏幕的手机下显示效果会变成有些页面元素宽度被拉的很长，但是高度、文字大小还是和原来一样（即，这些东西无法变得“流式”），显示非常不协调

### 自适应布局（Adaptive Layout）

自适应布局的特点是分别为不同的屏幕分辨率定义布局，即创建多个静态布局，每个静态布局对应一个屏幕分辨率范围。改变屏幕分辨率可以切换不同的静态局部（页面元素位置发生改变），但在每个静态布局中，页面元素不随窗口大小的调整发生变化。可以把自适应布局看作是静态布局的一个系列。

### 响应式布局（Responsive Layout）

&#x20;　　随着CSS3出现了**媒体查询**技术，又出现了**响应式设计**的概念。响应式设计的目标是确保一个页面在所有终端上（各种尺寸的PC、手机、手表、冰箱的Web浏览器等等）都能显示出令人满意的效果，对CSS编写者而言，在实现上不拘泥于具体手法，但通常是糅合了流式布局+弹性布局，再搭配媒体查询技术使用。——分别为不同的屏幕分辨率定义布局，同时，在每个布局中，应用流式布局的理念，即页面元素宽度随着窗口调整而自动适配。即：创建多个流体式布局，分别对应一个屏幕分辨率范围。可以把响应式布局看作是流式布局和自适应布局设计理念的融合。

**优点**：适应pc和移动端，如果足够耐心，效果完美。

**缺点**：（1）媒体查询是有限的，也就是可以枚举出来的，只能适应主流的宽高。（2）要匹配足够多的屏幕大小，工作量不小，设计也需要多个版本。



### 弹性布局（rem/em布局）

**1. rem/em区别**：rem是相对于html元素的font-size大小而言的，而em是相对于其父元素。

2\. 使用 em 或 rem 单位进行相对布局，相对%百分比更加灵活，同时可以支持浏览器的字体大小调整和缩放等的正常显示，因为em是相对父级元素的原因没有得到推广。【中国站点制作网页的时候，习惯用CSS强制定义字体大小，保证每个人都看到一致的效果，包括网易、搜狐这些门户网站在内的大部分站点，用的都是绝对单位px（像素）。但是，如果从网站**易用性**方面考虑，字体大小应该是可变的，一些视力不是那么好的人需要放大字体才能看得清页面内容。然而，占据大部分浏览器市场的IE无法调整那些使用px作为单位的字体大小。国外人士非常重视网站的易用性，相当一部分外国站点已经使用em作为字体单位。

3\. 这类布局的特点是，**包裹文字的各元素的尺寸采用em/rem做单位，而页面的主要划分区域的尺寸仍使用百分数或px做单位（同「流式布局」或「静态/固定布局」）**。**早期浏览器不支持整个页面按比例缩放**，仅支持网页内文字尺寸的放大，这种情况下。使用em/rem做单位，可以使包裹文字的元素随着文字的缩放而缩放。

4\. 浏览器的默认字体高度一般为`16px`，即1em:16px，但是 1:16 的比例不方便计算，为了使单位em/rem更直观，CSS编写者常常将页面跟节点字体设为62.5%，比如选择用rem控制字体时，先需要设置根节点html的字体大小，因为浏览器默认字体大小16px\*62.5%=10px。这样1rem便是10px，方便了计算。

5\. 用em/rem定义尺寸的另一个好处是更能适应缩进/以字体单位padding或margin／浏览器设置字体尺寸等情况（因为em/rem相对于字体大小，会同步改变）。例如：p{ text-indent: 2em; }。

6\. **使用rem单位的弹性布局在移动端也很受欢迎**。

7\. **其实在移动端使用所谓的弹性布局，是比较勉强的**。移动端弹性布局流行起来的原因归根结底是rem单位对于（根据屏幕尺寸）调整页面的各元素的尺寸、文字大小时比较好用。其实，使用vw、vh等后起之秀的单位，可以实现完美的流式布局（高度和文字大小都可以变得“流式”），弹性布局就不再必要了。

#### 结论：

1.如果只做pc端，那么静态布局（定宽度）是最好的选择；&#x20;

2.如果做移动端，且设计对高度和元素间距要求不高，那么弹性布局（rem+js）是最好的选择，一份css+一份js调节font-size搞定；

3.如果pc，移动要兼容，而且要求很高那么响应式布局还是最好的选择，前提是设计根据不同的高宽做不同的设计，响应式根据媒体查询做不同的布局

## Explain how browser determines what elements match a css selctor?

* Browsers match selectors from rightmost (key selector) to left. Browsers filter out elements in the DOM according to the key selector and traverse up its parent elements to determine matches. The shorter the length of the selector chain, the faster the browser can determine if that element matches the selector.
* For example with this selector `p span`, browsers firstly find all the \<span> elements and traverse up its parent all the way up to the root to find the \<p> element.

## How would you approach fixing browser-specific styling issues?

* After identifying the issue and the offending browser, use a separate style sheet that only loads when that specific browser is being used. This technique requires server-side rendering though.
* Use libraries like Bootstrap that already handles these styling issues for you.
* Use **autoprefixer** to automatically add vendor prefixes to your code.
* Use  Normalize.css.

\
Writing efficient CSS
---------------------

* Firstly, understand that browsers match selectors from rightmost (key selector) to left. Browsers filter out elements in the DOM according to the key selector and traverse up its parent elements to determine matches. **The shorter the length of the selector chain, the faster the browser can determine if that element matches the selector**. Hence avoid key selectors that are tag and universal selectors. They match a large number of elements and browsers will have to do more work in determining if the parents do match
* **BEM(Block element modifier ) recommends that everything should has a single class.**
* Be aware of which CSS properties trigger reflow, repaint, and compositing. Avoid writing styles that change the layout (trigger reflow) where possible.



## What are the advantages/disadvantages of using CSS preprocessors?

* CSS is made more maintainable.
* Easy to write nested selectors.
* Variables for consistent theming. Can share theme files across different projects.
* Mixins to generate repeated CSS.
* Splitting your code into multiple files. CSS files can be split up too but doing so will require a HTTP request to download each CSS file.
* &#x20;**Disadvantages:** Requires tools for preprocessing. Re-compilation time can be slow.

## 怎么把下列图片宽度变成 300px

```
<img class="img" src="pic_trulli.jpg" alt="Trulli" style="width:480px!important;">

```

* `max-width: 300px`
* `transform: scale(0.625,0.625)`
*   `box-sizing: border-box;`

    `padding: 0 90px;`
* `document.getElementsByTagName("img")[0].setAttribute("style","width:300px!important;")`



## 水平垂直居中

{% embed url="https://boyanliu.hashnode.dev/how-many-ways-can-you-come-up-with-centering-horizontally-vertically-or-both" %}



## Is there any reason you’d want to use `translate()` instead of `absolute` positioning,

* &#x20;`translate()` is a value of CSS `transform`. Changing `transform` or `opacity` does not trigger browser reflow or repaint, only compositions, whereas changing the absolute positioning triggers `reflow`
* &#x20;Hence `translate()` is more efficient and will result in shorter paint times for smoother animations.

## 图片 与 图片之间产生间隙

![](<.gitbook/assets/image (60).png>)

* **inline-block元素放到一起会产生一段空白。**
* img 标签默认是 inline-block
* normal flow 的情况 下， 多个 空白行会合并成一行， 是因为 `white-space:normal` 换行也背当成一个空白符 ， 会合并多余空白 这就是为什么会有间隙的原因， 间隙就是空白符
* **解决办法**
  * 给其中一个元素添加 float：left/ right
  * img element 互相紧挨着， 没有换行， 但是 会导致代码不方便阅读
  * 因为空白符 是个 字符， 所以就会有字体大小 ， 所以我们可以把它 设置为 0， `font-size：0`, 不过增加了代码量, **只有这个方法可以把垂直和水平方向空隙都删除了**
  * `letter-spacing:-100px;   , 不过增加了`

![](<.gitbook/assets/image (61).png>)

* img 是 没有基线的， 所以 它的低端会和父元素的基线对齐， 所以这里即使没有字体， img 下面也会有间隙， 这样子 有字体出现的时候 字体就会有额外的位置了
* 如果把父元素 `font-size: 0;`  **它的底部就不会有间隙了， 但是这种情况是没有考虑父元素里面的字体的**
* 因为我们知道 字体是基线对齐才会导致底部有间隙， 所以我们可以让字体 不以 baseline 对齐
  * 我们更改 父元素 `vertical-align:bottom`

## Can you explain the difference between coding a website to be responsive versus using a mobile-first strategy?

* Making a website responsive means the some elements will respond by adapting its size or other functionality according to the device's screen size, typically the viewport width, through CSS media queries, for example, making the font size smaller on smaller devices.
* A mobile-first strategy is also responsive, however it agrees we should default and define all the styles for mobile devices, and only add specific responsive rules to other devices later. Following the previous example:
  * It's more performant on mobile devices, since all the rules applied for them don't have to be validated against any media queries.

## How browser renders the website

![](<.gitbook/assets/image (69).png>)

1. When the user enters the URL, It will fetch the HTML source code from the server
2. Browser Parse the HTML source code and convert into the Tokens <, TagName, Attribute, AttributeValue, >
3. The Tokens will convert into the nodes and will construct the DOM Tree
4. The CSSOM Tree will generate from the CSS rules
5. The DOM and CSSOM tree will combine into the RenderTree
6. The RenderTree are constructed as below:
   1. Start from the root of the dom tree and compute which elements are visible and their computed styles
   2. RenderTree will ignore the not visible elements like (meta, script, link) and display:none
   3. It will match the visible node to the appropriate CSSOM rules and apply them
7. Reflow(回流): Calculate the position and size of each visible node
8. Repaint(重绘):now, the browser will paint the renderTree on the screen



## 浏览器的回流与重绘 (Reflow & Repaint)

* 回流必将引起重绘，重绘不一定会引起回流。
* 回流比重绘的代价要更高。
* 有时即使仅仅回流一个单一的元素，它的父元素以及任何跟随它的元素也会产生回流。

### 回流 Reflow:

* Reflow means re-calculating the positions and geometries of elements in the document. The Reflow happens when changes are made to the elements, that affect the layout of the partial or whole page. The Reflow of the element will cause the subsequent reflow of all the child and ancestor elements in the DOM
  *   **Reflows are very expensive in terms of performance**, and is one of the main causes of slow DOM scripts, especially on devices with low

      processing power, such as phones. In many cases, they are equivalent to laying out the entire page again.
* &#x20;当`Render Tree`中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流。
* **会导致回流的操作：**
  * 页面首次渲染
  * Reflow will happen when Adding, Removing, Updating the DOM nodes
    * 元素内容变化（文字数量或图片大小等等
  * Hiding DOM Element with display: none will cause both reflow and repaint
  * Moving, animating a DOM node will trigger reflow and repaint
  * Resizing the window will trigger reflow
  * Changing font-style
  * 激活CSS伪类（例如：:hover）
  * Adding or removing Stylesheet will cause the reflow/repaint
  * 查询某些属性或调用某些方法
    * Script manipulating the DOM is the expensive operation because they have recalculated each time the document, or part of the document modified. As we have seen from all the many things that trigger a reflow, it can occur thousands and thousands of times per second

![](<.gitbook/assets/image (70).png>)

###

### 重绘 Repaint:

* 当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility等），浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘。The Repaint occurs when changes are made to the appearance of the elements that change the visibility, but doesn't affect the layout

### Minimizing repaints and reflows



**CSS：**

* 如果想设定元素的样式，通过改变元素的 class 名 (尽可能在 DOM 树的最末端)（Change classes on the element you wish to style (as low in the dom tree as possible)）
  * 回流可以自上而下，或自下而上的回流的信息传递给周围的节点。回流是不可避免的，但可以减少其影响。尽可能在DOM树的里面改变class，可以限制了回流的范围，使其影响尽可能少的节点。例如，你应该避免通过改变对包装元素类去影响子节点的显示。面向对象的CSS始终尝试获得它们影响的类对象（DOM节点或节点），但在这种情况下，它已尽可能的减少了回流的影响，增加性能优势
* 避免设置多项内联样式（Avoid setting multiple inline styles）
  * 因为每个都会造成回流，样式应该合并在一个外部类，这样当该元素的class属性可被操控时仅会产生一个reflow。
* 应用元素的动画，使用 position 属性的 fixed 值或 absolute 值（Apply animations to elements that are position fixed or absolute）
* 避免使用table布局（Avoid tables for layout）

```
// bad
var left = 10,
    top = 10;
el.style.left = left + "px";
el.style.top  = top  + "px";

// better 
el.className += " theclassname";

// or when top and left are calculated dynamically...

// better
el.style.cssText += "; left: " + left + "px; top: " + top + "px;";
```

**Don't change individual styles, one by one**. Best for sanity and maintainability is to change the class names, not the styles. If the styles are dynamic, edit the **cssText** property

**Batch DOM Changes**

1. 使元素脱离文档流
2. 对其进行多次修改
3. 将元素带回到文档中

该过程的第一步和第三步可能会引起回流，但是经过第一步之后，对DOM的所有修改都不会引起回流重绘，因为它已经不在渲染树了

有三种方式可以让DOM脱离文档流：

**一： 使用文档片段(document fragment)在当前DOM之外构建一个子树，再把它拷贝回文档。**

* Use a `documentFragment` to hold temp changes

```
function appendDataToElement(appendToElement, data) {
    let li;
    for (let i = 0; i < data.length; i++) {
    	li = document.createElement('li');
        li.textContent = 'text';
        appendToElement.appendChild(li);
    }
}
const ul = document.getElementById('list');
const fragment = document.createDocumentFragment();
appendDataToElement(fragment, data);
ul.appendChild(fragment);
```



二：**隐藏元素，应用修改，重新显示**

* 这个会在展示和隐藏节点的时候，产生两次回流
* Hide the element with display: none (1 reflow, 1 repaint), add 100 changes, restore the display (total 2 reflow, 2 repaint)

```
function appendDataToElement(appendToElement, data) {
    let li;
    for (let i = 0; i < data.length; i++) {
    	li = document.createElement('li');
        li.textContent = 'text';
        appendToElement.appendChild(li);
    }
}
const ul = document.getElementById('list');
ul.style.display = 'none';
appendDataToElement(ul, data);
ul.style.display = 'block';
```

**三**：**隐藏元素，应用修改，重新显示**

* Clone, update, replace the node

```
const ul = document.getElementById('list');
const clone = ul.cloneNode(true);
appendDataToElement(clone, data);
ul.parentNode.replaceChild(clone, ul);
```

## CSS 性能优化

* 建立公共样式类，把相同样式提取出来作为公共类使用。
* 减少通配符\*或者类似\[hidden="true"]这类选择器的使用
* 巧妙运用css的继承机制，如果父节点定义了，子节点就无需定义
* &#x20;去除无用CSS
  * 不同元素或者其他情况下的重复代码，
  * 整个页面内没有生效的CSS代码
* 使用normolize.css
* 减少css嵌套，最好不要嵌套三层以上





## 使用CSS实现常用布局

### 品字布局

![](<.gitbook/assets/image (78).png>)

{% embed url="https://jsfiddle.net/Boyanliuu/45adzu3q/10/" %}

* 使用float 把2，3 放在一行 然后 使用 margin-left + translateX()









### 圣杯布局

![](<.gitbook/assets/image (72).png>)

```
// 基础 结构
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>圣杯布局</title>
    </head>
    <body>
        <header> header</header>
        <div class="clearfix wrapper">
            <div class="center">
                Main content
            </div>
            <div class="left">
                Left content
            </div>
            <div class="right">
                right content
            </div>
        </div>
        <footer> footer</footer>
    </body>
</html>

// css

:root {
    box-sizing: border-box;
}

*,
::before,
::after {
    box-sizing: inherit;
    margin: 0;
    padding: 0;
}


.center{
  background:lightblue;
}

.left{
  background:gold;
}
.right{
  background:yellow;
}
footer , header{
  background:CornflowerBlue;
}
```

![](<.gitbook/assets/image (73).png>)

#### 一 利用 float 布局 + 负 margin&#x20;

{% embed url="https://jsfiddle.net/Boyanliuu/xLceghj3/32/" %}



1.中间需要优先加载， 所以会把 中间的 div 紧跟着header 放

2\. 为了让 中间的3个区域再同一行， 我们使用 float

![](<.gitbook/assets/image (74).png>)

```
.center,.left,.right{
  float:left;
}
```

3\. 页脚也跑了上来 因为 float产生了 塌陷， 所以 我们需要再中间区域 清除浮动

![](<.gitbook/assets/image (75).png>)

```
.clearfix::after{
  content: "";
  display:block;
  clear: both;
}
```

4\. 现在内容 不正确 ， 使用 negative margin 负外边距

![](<.gitbook/assets/image (76).png>)

```
.wrapper{
  padding: 0 100px; // 添加内边距 就不会产生被覆盖的情况了
}

.left,.right{
  width:100px;
}
.left{
  margin-left:-100%;
}
.center{
  width:100%;
}
.right{
  margin-left:-100px;
}
```

5\. 上面的问题是 左右区域会挡住 主区域的内容， 如果主区域是 1000 px 左右分别是100px的话， 我们就只能展示出来  800px， 我们可以使用 相对定位 解决.

![](<.gitbook/assets/image (77).png>)

```
.left{
  margin-left:-100%;
  position: relative;
 left:-100px;
}

.right{
  margin-left:-100px;
  position: relative;
  right: -100px;
}
```

#### &#x20;利用 flex 布局

{% embed url="https://jsfiddle.net/Boyanliuu/xe1wahLt/6/" %}

* code 最少

```
.wrapper{
  display:flex;
}
.left{
  order:-1;
}
.left, .right{
  width:100px;
}
.center{
   flex: 1;
}
```

**使用 grid 布局**

```
.wrapper{
 display:grid;
}
.left{
    grid-row:2;
    grid-column:1/2;
}


.right{
  grid-row:2;
  grid-column:4/5;
}

  .center{
   grid-row:2;
   grid-column:2/4;
}
```

{% embed url="https://jsfiddle.net/Boyanliuu/2xsztb7j/9/" %}

#### 使用 position absolute

{% embed url="https://jsfiddle.net/Boyanliuu/2bpwjh6f/13/" %}

* `.center { left: 200px; right: 200px; }`  这个可以让他的 宽度 起始点是 left 终点是right



### 双飞翼

{% embed url="https://jsfiddle.net/Boyanliuu/4bq2Losh/8/" %}

* 多加了一层 dom 树节点2， 增加了 渲染树生产的计算量， center 被 另外一个 div 包裹了
* 跟圣杯布局很类似
* 两种布局方式都是把主列放在文档流最前面，使主列优先加载。
* 两种布局方式在实现上也有相同之处，都是让三列浮动，然后通过负外边距形成三列布局。
* 两种布局方式的不同之处在于如何处理中间主列的位置： 圣杯布局是利用父容器的左、右内边距+两个从列相对定位； 双飞翼布局是把主列嵌套在一个新的父级块中利用主列的左、右外边距进行布局调整

### 两列布局

* 左列定宽， 右列自适应

#### float  + margin

{% embed url="https://jsfiddle.net/Boyanliuu/n4u7eg8c/10/" %}





```
 * {
     margin: 0;
     padding: 0;
}
 #left {
     background-color: #f00;
     float: left;
     width: 100px;
     height: 500px;
}
 #right {
     background-color: #0f0;
     height: 500px;
     margin-left: 100px;
    /*大于等于#left的宽度*/
}
```

#### float +  BFC

```
#left {
    background-color: #f00;
    float: left;
    width: 100px;
    height: 500px;
}

#right{
   overflow:hidden;
   background-color: #0f0;
   height: 500px;
}
```

#### flex 布局

```
body{
  display:flex;
}
#left {
    background-color: #f00;
    width: 100px;
    height: 500px;
}

#right{
   flex:1;
   background-color: #0f0;
   height: 500px;
}

```

#### grid 布局

```
body{
  display:grid;
  grid-template-columns:100px 1fr;
}
#left {
    background-color: #f00;
    height: 500px;
}

#right{
   background-color: #0f0;
   height: 500px;
}
```

### 粘连布局

![](<.gitbook/assets/image (79).png>)

* 有一块内容\<main>，当\<main>的高康足够长的时候，紧跟在\<main>后面的元素\<footer>会跟在\<main>元素的后面。
* 当\<main>元素比较短的时候(比如小于屏幕的高度),我们期望这个\<footer>元素能够“粘连”在屏幕的底部

{% embed url="https://jsfiddle.net/Boyanliuu/qhmgd45t/6/" %}

* footer必须是一个独立的结构，与wrap没有任何嵌套关系
* wrap区域的高度通过设置min-height，变为视口高度
* footer要使用margin为负来确定自己的位置
* 在main区域需要设置 padding-bottom。这也是为了防止负 margin 导致 footer 覆盖任何实际内容。





## 画CSS图形

### border radius 意思

![](<.gitbook/assets/image (89).png>)

* 水平半径 和垂直半径
* 顺序依次是： top-left， top-right， bottom-left， bottom-right

```
border-radius: 12px
//等于
border-top-left-radius: 12px 12px
border-top-right-radius: 12px 12px
border-bottom-left-radius: 12px 12px
border-bottom-right-radius: 12px 12px


border-radius: 10px / 5px 20px;
// 等于 
border-radius:10px 10px 10px 10px / 5px 20px
5px 20px
```

### 半椭圆

![](<.gitbook/assets/image (88).png>)

```
width:200px;
height:100px;
border-radius: 50% / 100% 100% 0 0;
```

*   这个形状是垂直对称的，这意味着左上角和右上角的半径值应该是

    相同的；与此类似，左下角和右下角的半径值也应该是相同的。
*   顶部边缘并没有平直的部分（也就是说，整个顶边都是曲线），这意

    味着左上角和右上角的半径之和应该等于整个形状的宽度。
*   基于前两条观察，我们可以推断出，左半径和右半径在水平方向上

    的值应该均为 50%
*   再看看垂直方向，似乎顶部的两个圆角占据了整个元素的高度，而

    且底部完全没有任何圆角。因此，在垂直方向上 border-radius 的

    合理值似乎就是 100% 100% 0 0。

### 四分之一椭圆

![](<.gitbook/assets/image (90).png>)

```
  border-radius:100% 0 0 0 / 100% 0 0 0;
```

### 平行四边形

![](<.gitbook/assets/image (91).png>)

```
  // Method 1
  width:200px;
  height:100px;
  background:lightblue;
  transform: skewX(-45deg);
  
  // Method 2
  .button{
  width: 100px;
  position: relative; 
}

.button::before {
 content: ''; /* 用伪元素来生成一个矩形 */
 position: absolute;
 top: 0; right: 0; bottom: 0; left: 0;
 z-index: -1;
 background: #58a;
 transform: skew(-45deg);
}
```

1.  Method 1 也会把 内容skew ， 我们可以给内容反向 skew 变形 从而抵消容器的变形

    效果，最终产生我们所期望的结果，不幸的是，这意味着我们将不得不使用

    一层额外的 HTML 元素来包裹内容，比如用一个 div： **额外的 HTML 元素**
2.  Method 2更好的方案 ，使用 伪元素,_**还适用于其他任何变形样式，**_

    _**当我们想变形一个元素而不想变形它的内容时就可以用到它**_

    1.  我们希望伪元素保持良好的灵活性，可以自动继承其宿主元素的尺寸，

        甚至当宿主元素的尺寸是由其内容来决定时仍然如此。一个简单的办法是

        给宿主元素应用 position: relative 样式，并为伪元素设置 position:

        absolute，然后再把所有偏移量设置为零，以便让它在水平和垂直方向上都

        被拉伸至宿主元素的尺寸

### 三角形

![](<.gitbook/assets/image (94).png>)

```
.div1{
  width:0;
  height:0;
  border-top: 100px solid #f6d365;
  border-left: 100px solid #a4d7e1;
  border-bottom: 100px solid #ed1250;
  border-right: 100px solid #414141;
}

// 变成三角形
.div1{
  width:0;
  height:0;
  border-top: 100px solid transparent;
  border-left: 100px solid transparent;
  border-bottom: 100px solid #ed1250;
  border-right: 100px solid transparent;
}
// 优化，缩小代码数量
.div1{
  width:0;
  height:0;
  border: 100px solid transparent;
  border-bottom: 100px solid #ed1250;
}
```

* 使用的就是对角切，所以我们隐藏其余的三角形就好了 使用 `transparent`

#### &#x20;      Talk Bubble（聊天框） <a href="#talk-bubble-ef-bc-88-e8-81-8a-e5-a4-a9-e6-a1-86-ef-bc-89" id="talk-bubble-ef-bc-88-e8-81-8a-e5-a4-a9-e6-a1-86-ef-bc-89"></a>

![](<.gitbook/assets/image (95).png>)

```
  #talkBubble{
      width: 120px;
      height: 80px;
      background: #81cfa2;
      position: relative;
      border-radius: 10px;

 }
#talkBubble:before{
     content: "";
     position: absolute;
     right: 100%;
     top: 26px;
     width: 0;
     height: 0;
     border-top: 13px solid transparent;
     border-right: 26px solid #81cfa2;
     border-bottom: 13px solid transparent;
 }
```

