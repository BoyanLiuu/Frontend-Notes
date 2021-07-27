# CSS

## Flex 布局

{% embed url="https://zhuanlan.zhihu.com/p/25303493" %}



* 有两个值： flex , inline-flex, 如果你使用块元素如 div，你就可以使用 flex，而如果你使用行内元素，你可以使用 inline-flex。
* 当设置为 flex 布局 后， **子元素的 float、clear、vertical-align 的属性将会失效。**
* **下列 6种属性可以设置在 容器上**

  * flex-direction
  * flex-wrap
  * flex-flow
  * justify-content
  * align-items
  * align-content

* 
   ****flex-direction: 决定主轴的方向\(即项目的排列方向\)

  *  **** flex-direction: row \| row-reverse \| column \| column-reverse;

* flex-wrap: 决定容器内项目是否可换行
  *   flex-wrap: nowrap \| wrap \| wrap-reverse;
*  flex-flow: flex-direction 和 flex-wrap 的简写形式
  *  flex-flow: &lt;flex-direction&gt; \|\| &lt;flex-wrap&gt;;
  * 直接分开写 就不需要记这个属性了
*  justify-content：定义了项目在主轴的对齐方式。
* align-items: 定义了项目在交叉轴上的对齐方式
* align-content: 定义了多根轴线的对齐方式，如果项目只有一根轴线，那么该属性将不起作用
  * 当设置为 `flex-wrap: wrap`，   容器就会出现多条轴线
  *  _Note, this property has no effect when the flexbox has only a single line._

![](.gitbook/assets/image%20%2839%29.png)

### Flex item 属性

* 有6种 属性可在 item 上面 使用
  * order
  * flex-basis
  * flex-grow
  * flex-shrink
  * flex
  * align-self
* order: 定义项目在容器中的排列顺序，数值越小，排列越靠前，默认值为 0
* **flex-grow**:默认值为 0，即如果存在剩余空间，也不放大,If all items have flex-grow set to 1, the remaining space in the container will be distributed equally to all children. If one of the children has a value of 2, the remaining space would take up twice as much space as the others 
* **flex-shrink**：**默认为1，即如果空间不足，该项目将缩小。** This defines the ability for a flex item to shrink if necessary.
* **flex-basis**:定义了在分配剩余空间之前元素的默认大小  can be set to any value that would apply to width, including values in px, ems, or percentages. Its initial value is auto, which means the browser will look to see if the element has a width declared. If so, the browser uses that size; if not, it determines the element’s size naturally by the contents   **默认值是`auto`，就是自动。有设置`width`则占据空间就是`width`，没有设置就按内容宽度来**
  * **This means that width will be ignored for elements that have any flex basis     other than auto**
  *  当剩余空间不足的时候，flex子项的实际宽度并通常不是设置的`flex-basis`尺寸，因为flex布局剩余空间不足的时候默认会收缩
*  **flex**: flex-grow, flex-shrink 和 flex-basis的简写
  *  flex 的默认值是 0 1 auto。
  *  2个快捷键 `auto` \(`1 1 auto`\) 和 none \(`0 0 auto`\)。
* **align-self**:允许单个项目有与其他项目不一样的对齐方式

![](.gitbook/assets/image%20%2840%29.png)

## Grid 布局



## CSS box mode

![](.gitbook/assets/image%20%2842%29.png)

* Default `box-sizing: content-box`   This means that any height or width you specify only sets the size of the content box

![](.gitbook/assets/image%20%2841%29.png)

* If you set an element's width to 100 pixels, that 100 pixels will include any border or padding you added,设置元素的height/width属性指的是border + padding + content部分的高/宽

## CSS 那些属性是可以继承的

* 可继承的属性：font-size, font-family, color
* 不可继承的样式：border, padding, margin, width, height

## CSS Selectors

```text
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
* We use , for grouping multiple selectors **\(eg1\)**
* **Attribute selectors:**
  * **Simple attribute selector** , select all h1 ele‐ ments that have a class attribute **\(eg2\)**
  * **With exact attribute value:   \(eg 3\)**
  * **Selection based on partial attribute values**
    * `[foo~="bar"]` Selects any element with an attribute foo whose value contains the word bar in a space-separated list of words
    * `[foo*="bar"]` Selects any element with an attribute foo whose value contains the substring bar
    * `[foo^="bar"]` Selects any element with an attribute foo whose value begins with bar
    * `[foo$="bar"]` Selects any element with an attribute foo whose value ends with bar
    * `[foo|="bar"]`\] Selects any element with an attribute foo whose value starts with bar followed by a dash -  or whose value is exactly equal to bar   **\(eg 4\)**
* **Descendant Selector:  \(eg 5\),** Select ****em elements that are descended from h1 elements
* **Selecting children \(eg 6\),** select a strong element only if it is a direct child of an h1 element
* **Selecting Adjacent Sibling Elements \(eg 7\)**  ,  All the immediate h1 tags  after p tags changed their background color to orange. If there is any other element between `p` tag and `h1` tag then this rule is not applicable. **只有第一个 出现的  才会 apply**
* **General Sibling Selector \(eg 8\)**     All h1 tags after p tags have orange background color

### Chain pseudo-classes

```text
a:link:hover {color: red;}
a:visited:hover {color: maroon;}
```

* The order doesn't really matter

### Pseduo-class 伪类

```text
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

伪类就是开头为冒号的关键字：

1. Selecting the root element     `:root`
2. Selecting empty elements  `:empty`
3. `:only-child`
4. `:first-child`Selects every  element that is the first child of its parent
5. `:last-child`
6. `:active`Selects the active link
7. `:checked` Selects every checked  element
8. `:default` Selects the default  element
9. `:first-of-type` Selects every &lt;p&gt; element that is the first &lt;p&gt; element of its parent
10. `:hover`
11. `:focus`
12. `:nth-child(n)`Selects every  p element that is the second child of its parent
    1. `:nth-child(2n)`  所有的 even element
13. `:nth-last-child(n)`
14. `:nth-last-of-type(n)`
15. `:last-of-type`

###  伪元素 pseudo-element

Pseudo-elements start with a double colon ::

```text
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
4. `::first-line`Selects the first line of every &lt;p&gt; element，**only work for block-display elements**

#### 



## CSS 优先级，specifity of each selector

![](.gitbook/assets/image%20%2844%29.png)

![](.gitbook/assets/image%20%2843%29.png)

```text
// EG 1
html > body table tr[id="totals"] td ul > li {color: maroon;} 
```

* For every ID attribute value: 100
* For every class attribute value, attribute selection, or pseudo-class : 10
  * ayttribute selector : `[id="totals"]`
* For every element and pseudo-element: 1
* Combinators and the universal selector do not contribute anything to the specificity
* Inline-style: 1000
* !important: it always goes at the end of the declaration, just before the semicolon.where an important and a non-important declaration con‐flict, the important declaration always wins.
* if two rules have exactly the same explicit weight, origin, and specificity, then

  the one that occurs later in the style sheet wins out

* 继承得到的样式的优先级最低。

### Order of link style:

```text
a:link {color: blue;}
a:visited {color: purple;}
a:focus {color: green;}
a:hover {color: red;}
a:active {color: orange;}
```

* they all have same  explicit weight, origin, specificty, visited must come before hover, focus, active, since, every link is visited or unvisited.









## BFC实现原理，可以解决的问题，如何创建BFC

## 掌握一套完整的响应式布局方案



## Position:

* static（默认）：按照正常文档流进行排列；So if there is a left/right/top/bottom/z-index set then there will be no effect on that element.
* relative（相对定位）：不脱离文档流， But now left/right/top/bottom/z-index will work.
* absolute\(绝对定位\)：参考距其最近一个不为static的父级元素通过top, bottom, left, right 定位；
* fixed\(固定定位\)：the element is removed from the flow of the document like absolutely positioned elements. In fact they behave almost the same, only fixed positioned elements are always relative to the document, not any particular parent, and are unaffected by scrolling.
* sticky: sticky \(experimental\): the element is treated like a relative value until the scroll location of the viewport reaches a specified threshold, at which point the element takes a fixed position where it is told to stick.
  * 父类元素不能有 `overflow:visible`以外的overflow设置

```text
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
* 盒阴影：box-shadow: 10px 10px 5px \#888888
* 媒体查询（media query）：定义两套css，当浏览器的尺寸变化时会采用不同的属性
* 边框图片：border-image: url\(border.png\) 30 30 round

  ```text
  /* border-image: image-source image-height image-width image-repeat */

  ```

## **为什么要初始化CSS样式**

* 因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异

##    **CSS里的visibility属性有个collapse属性值？**

* visible**:** Default value. The element is visible
* hidden: The element is hidden \(but still takes up space\)
* collapse: Only for table rows \(&lt;tr&gt;\), row groups \(&lt;tbody&gt;\), columns \(&lt;col&gt;\), column groups \(&lt;colgroup&gt;\). This value removes a row or column, but it does not affect the table layout. The space taken up by the row or column will be available for other content.

  **If collapse is used on other elements, it renders as "hidden"**

* initial: It is used to set a CSS property to its default value.

##  **display:none与visibility：hidden的区别？**

* display：none 不显示对应的元素，在文档布局中不再分配空间（回流+重绘）
* visibility：hidden 隐藏对应元素，在文档布局中仍保留原来的空间（重绘）

## CSS @media Rule

```text
//Change the background color of the <body> 
//element to "lightblue" when the browser window is 600px wide or less:

@media only screen and (max-width: 600px) {
  body {
    background-color: lightblue;
  }
}


```



## Overflow 属性

* scroll : 必定出现滚动条
* auto: 子元素大于父元素时候 出现滚动条
* visible:  Default. The overflow is not clipped. The content renders outside the element's box
* hidden: The overflow is clipped, and the rest of the content will be invisible



