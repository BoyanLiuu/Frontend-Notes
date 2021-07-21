# CSS

## Flex 布局

## Grid 布局

## CSS box mode

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





## BFC实现原理，可以解决的问题，如何创建BFC

## 掌握一套完整的响应式布局方案

