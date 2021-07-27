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

{% embed url="https://juejin.cn/post/6854573220306255880" %}



* Grid 算作二维布局，  `display: grid | inline-grid;`
* **grid-template-columns** 和 **grid-template-rows** 属性来定义网格中的行和列
  *  `grid-template-columns` 属性设置列宽 `grid-template-rows` 属性设置行高
* **grid-row-gap** 属性、**grid-column-gap** 属性以及 grid-gap 属性
  *  `grid-row-gap` 属性、`grid-column-gap` 属性分别设置行间距和列间距。`grid-gap` 属性是两者的简写形式
  *  `grid-row-gap: 10px` 表示行间距是 10px，`grid-column-gap: 20px` 表示列间距是 20px。`grid-gap: 10px 20px` 实现的效果是一样的
* **grid-template-areas：** 属性用于定义区域，一个区域由一个或者多个单元格组成
* **grid-auto-flow ：默认值是 row**  `grid-auto-flow: row | column | row dense | column dense;`
* **grid-auto-columns:,grid-auto-rows:**Specifies the size of any auto-generated grid track, 多余的行会根据 grid-auto-rows 算， 
*  **justify-items** 属性设置单元格内容的水平位置
*  **align-items** 属性设置单元格的垂直位置（上中下）
*  **justify-content** 属性是整个内容区域在容器里面的水平位置
*  **align-content** 属性是整个内容区域的垂直位置（上中下）

### Grid 实战

* fr 实现 等分响应式: `grid-template-columns: 1fr 1fr 1fr;` 表示容器分为三等分
* repeat + auto-fit——固定列宽，改变列数量   `grid-template-columns: repeat(auto-fit, 200px);`
  * 我们的网格能够固定列宽，并根据容器的宽度来改变列的数量,表示固定列宽为 200px，数量是自适应的，只要容纳得下，就会往上排列

### Grid 兼容性

* 但在 IE 10 以下不支持。个人建议在公司的内部系统运用起来是没有问题的，但 TOC 的话，可能目前还是不太合适







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

## line-height

* It accept both units and unitless values
*  CSS中起高度作用的应该就是`height`以及`line-height`了吧！如果一个标签没有定义`height`属性\(包括百分比高度\)，那么其最终表现的高度一定是由`line-height`起作用
*  `line-height`行高怎么就产生了高度呢?
  * 在`inline box`模型中，有个`line boxes`，这玩意是看不见的，这个玩意的工作就是包裹每行文字。一行文字一个`line boxes`。一个没有设置`height`属性的`div`的高度就是由一个一个`line boxes`的高度堆积而成的。

    其实`line boxes`不是直接的生产者，属于中层干部，真正的活儿都是它的手下 – `inline boxes`干的，这些手下就是文字啦，图片啊，`<span>`之类的`inline`属性的标签啦。`line boxes`只是个考察汇报人员，考察它的手下谁的实际`line-height`值最高，谁最高，它就要谁的值，然后向上汇报，形成高度。例如，`<span style="line-height:20px;">取手下line-height<span style="line-height:40px;">最高</span>的值</span>`。则line boxes的高度就是40像素了
* You should typically use unitless numbers because they’re inherited differently  `line-height: 1.2;`
  * the line     height is calculated locally to 38.4 px \(32 px × 1.2\).
  * When you use a unitless number, that declared value is inherited, meaning its computed value is recalculated for each inheriting child element





## Layout Idea:

### 静态布局（static layout）

* 即传统Web设计，网页上的所有元素的尺寸一律使用px作为单位。

#### 布局特点

不管浏览器尺寸具体是多少，网页布局始终按照最初写代码时的布局来显示。常规的pc的网站都是静态（定宽度）布局的，也就是设置了min-width，这样的话，如果小于这个宽度就会出现滚动条，如果大于这个宽度则内容居中外加背景，这种设计常见于pc端。

**优点**：这种布局方式对设计师和CSS编写者来说都是最简单的，亦没有兼容性问题。

**缺点**：显而易见，即不能根据用户的屏幕尺寸做出不同的表现。当前，大部分门户网站、大部分企业的PC宣传站点都采用了这种布局方式。固定像素尺寸的网页是匹配固定像素尺寸显示器的最简单办法。但这种方法不是一种完全兼容未来网页的制作方法，我们需要一些适应未知设备的方法。

### 流式布局（Liquid Layout）

*  网页中主要的划分区域的**尺寸使用百分数**（搭配min-\*、max-\*属性使用），例如，设置网页主体的宽度为80%，min-width为960px。图片也作类似处理（width:100%, max-width一般设定为图片本身的尺寸，防止被拉伸而失真）。

####  布局特点

　　屏幕分辨率变化时，页面里元素的大小会变化而但布局不变。【这就导致如果屏幕太大或者太小都会导致元素无法正常显示。

 **这种布局方式在Web前端开发的早期历史上，用来应对不同尺寸的PC屏幕**（那时屏幕尺寸的差异不会太大），**在当今的移动端开发也是常用布局方式**，但**缺点明显**：**主要的问题**是如果屏幕尺度跨度太大，那么在相对其原始设计而言过小或过大的屏幕上不能正常显示。因为宽度使用%百分比定义，但是高度和文字大小等大都是用px来固定，所以在大屏幕的手机下显示效果会变成有些页面元素宽度被拉的很长，但是高度、文字大小还是和原来一样（即，这些东西无法变得“流式”），显示非常不协调

### 自适应布局（Adaptive Layout）

自适应布局的特点是分别为不同的屏幕分辨率定义布局，即创建多个静态布局，每个静态布局对应一个屏幕分辨率范围。改变屏幕分辨率可以切换不同的静态局部（页面元素位置发生改变），但在每个静态布局中，页面元素不随窗口大小的调整发生变化。可以把自适应布局看作是静态布局的一个系列。

### 响应式布局（Responsive Layout）

 　　随着CSS3出现了**媒体查询**技术，又出现了**响应式设计**的概念。响应式设计的目标是确保一个页面在所有终端上（各种尺寸的PC、手机、手表、冰箱的Web浏览器等等）都能显示出令人满意的效果，对CSS编写者而言，在实现上不拘泥于具体手法，但通常是糅合了流式布局+弹性布局，再搭配媒体查询技术使用。——分别为不同的屏幕分辨率定义布局，同时，在每个布局中，应用流式布局的理念，即页面元素宽度随着窗口调整而自动适配。即：创建多个流体式布局，分别对应一个屏幕分辨率范围。可以把响应式布局看作是流式布局和自适应布局设计理念的融合。

**优点**：适应pc和移动端，如果足够耐心，效果完美。

**缺点**：（1）媒体查询是有限的，也就是可以枚举出来的，只能适应主流的宽高。（2）要匹配足够多的屏幕大小，工作量不小，设计也需要多个版本。



### 弹性布局（rem/em布局）

**1. rem/em区别**：rem是相对于html元素的font-size大小而言的，而em是相对于其父元素。

2. 使用 em 或 rem 单位进行相对布局，相对%百分比更加灵活，同时可以支持浏览器的字体大小调整和缩放等的正常显示，因为em是相对父级元素的原因没有得到推广。【中国站点制作网页的时候，习惯用CSS强制定义字体大小，保证每个人都看到一致的效果，包括网易、搜狐这些门户网站在内的大部分站点，用的都是绝对单位px（像素）。但是，如果从网站**易用性**方面考虑，字体大小应该是可变的，一些视力不是那么好的人需要放大字体才能看得清页面内容。然而，占据大部分浏览器市场的IE无法调整那些使用px作为单位的字体大小。国外人士非常重视网站的易用性，相当一部分外国站点已经使用em作为字体单位。

3. 这类布局的特点是，**包裹文字的各元素的尺寸采用em/rem做单位，而页面的主要划分区域的尺寸仍使用百分数或px做单位（同「流式布局」或「静态/固定布局」）**。**早期浏览器不支持整个页面按比例缩放**，仅支持网页内文字尺寸的放大，这种情况下。使用em/rem做单位，可以使包裹文字的元素随着文字的缩放而缩放。

4. 浏览器的默认字体高度一般为`16px`，即1em:16px，但是 1:16 的比例不方便计算，为了使单位em/rem更直观，CSS编写者常常将页面跟节点字体设为62.5%，比如选择用rem控制字体时，先需要设置根节点html的字体大小，因为浏览器默认字体大小16px\*62.5%=10px。这样1rem便是10px，方便了计算。

5. 用em/rem定义尺寸的另一个好处是更能适应缩进/以字体单位padding或margin／浏览器设置字体尺寸等情况（因为em/rem相对于字体大小，会同步改变）。例如：p{ text-indent: 2em; }。

6. **使用rem单位的弹性布局在移动端也很受欢迎**。

7. **其实在移动端使用所谓的弹性布局，是比较勉强的**。移动端弹性布局流行起来的原因归根结底是rem单位对于（根据屏幕尺寸）调整页面的各元素的尺寸、文字大小时比较好用。其实，使用vw、vh等后起之秀的单位，可以实现完美的流式布局（高度和文字大小都可以变得“流式”），弹性布局就不再必要了。

#### 结论：

1.如果只做pc端，那么静态布局（定宽度）是最好的选择； 

2.如果做移动端，且设计对高度和元素间距要求不高，那么弹性布局（rem+js）是最好的选择，一份css+一份js调节font-size搞定；

3.如果pc，移动要兼容，而且要求很高那么响应式布局还是最好的选择，前提是设计根据不同的高宽做不同的设计，响应式根据媒体查询做不同的布局

##  What are some ways for writing efficient CSS

* Firstly, understand that browsers match selectors from rightmost \(key selector\) to left. Browsers filter out elements in the DOM according to the key selector and traverse up its parent elements to determine matches. The shorter the length of the selector chain, the faster the browser can determine if that element matches the selector. Hence avoid key selectors that are tag and universal selectors. They match a large number of elements and browsers will have to do more work in determining if the parents do match
* **Everything should has a single class.**
* Be aware of which CSS properties trigger reflow, repaint, and compositing. Avoid writing styles that change the layout \(trigger reflow\) where possible.





  
  
  






### 



