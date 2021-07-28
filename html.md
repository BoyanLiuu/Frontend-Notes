# HTML



## 1.&lt;script&gt;

```text
<script>
 function sayScript() {
 console.log("</script>");
 }
</script>
```

* You can not have  **"&lt;/script&gt;"**  anywhere in your code. The above code will throw error, the browser see that would be the closing script tag. We can use escape character  **"&lt;\/script&gt;"**



## 2.Describe the difference between &lt;script&gt;, &lt;script async&gt; and &lt;script defer&gt;

we use &lt;script&gt; to inserting Javascript into an HTML page 

* **&lt;script&gt;** - HTML parsing is blocked, the script is fetched and executed immediately, HTML parsing resumes after the script is executed.
* 
  **&lt;script async&gt;** - The script will be fetched in parallel to HTML parsing and executed as soon as it is available. Use async when the script is independent of any other scripts on the page, It  should not prevent other actions on the page such as downloading resources or   waiting for other scripts to load.

* 
  **&lt;script defer&gt;** - The script will be fetched in parallel to HTML parsing and executed when the page has finished parsing.If there are multiple of them, each deferred script is executed in the order they were encoun­tered in the document

## 3.Inline code vs external files:

*  it’s generally considered a best    practice to include as much JavaScript as possible using external files.
* **Maintainability**: —JavaScript code that is sprinkled throughout various HTML pages turns

  code maintenance into a problem. It is much easier to have a directory for all JavaScript files

  so that developers can edit JavaScript code independent of the markup in which it’s used.

* **caching**: Browsers cache all externally linked JavaScript files according to specific settings,

  meaning that if two pages are using the same file, the file is downloaded only once.

* future-proof

## 4. &lt;NOSCRIPT&gt; Element:

* It can contain any HTML elements aside from &lt;script&gt; that can be included in the document &lt;body&gt;. Any content contained in a &lt;noscript&gt; element will be displayed   under only the following _**two**_ circumstances:
  * The browser doesn’t support scripting
  * The browser’s scripting support is turned off.

## 5。 语义化标签

{% embed url="https://rainylog.com/post/ife-note-1/" %}



![](.gitbook/assets/image%20%2833%29.png)

* Semantic HTML elements are those that clearly describe their meaning in a human- and machine-readable way.
* The semantic elements added in HTML5 are:
  * `<article>`
  * `<aside>`
  * `<details>`
  * `<figcaption>`
  * `<figure>`
  * `<footer>`
  * `<header>`
  * `<main>`
  * `<mark>`
  * `<mark>`
  * `<section>`
  * `<summary>`
  * `<time>`

### Why use semantic elements?



![](.gitbook/assets/image%20%2832%29.png)

####  1. Easy to read

####  2. Greater accessibility**, better for SEO**

* Search engines and assistive technologies are also able to better understand the context and content of your website, meaning a better experience for your users

####  3. Lead to more consistent code.

* when using non-semantic elements, different programmers might write this differently. There are so many ways that you can create a header element,

### **&lt;section&gt; and &lt;article&gt; difference**

![](.gitbook/assets/image%20%2836%29.png)

*  The `<section>` and `<article>` elements are conceptually similar and interchangeable.
  * An article is intended to be independently distributable or reusable.
  * A section is a thematic grouping of content.

### **&lt;header&gt; and &lt;hgroup&gt;**

![](.gitbook/assets/image%20%2837%29.png)

*  The `<header>` element is generally found at the top of a document, a section, or an article and usually contains the main heading and some navigation and search tools

![](.gitbook/assets/image%20%2835%29.png)

*  The `<hgroup>` element should be used where you want a main heading with one or more subheadings.
* It can only contain other headers, that is &lt;h1&gt; to &lt;h6&gt;

### **&lt;aside&gt;**

```text
<aside>
  <p>This is a sidebar, for example a terminology definition or a short background to a historical figure.</p>
</aside>
```

*  The `<aside>` element is intended for content that is not part of the flow of the text in which it appears, however still related in some way. This of `<aside>` as a sidebar to your main content.

### **&lt;small&gt;**

![](.gitbook/assets/image%20%2834%29.png)

*  The `<small>` element often appears within a `<footer>` or `<aside>` element which would usually contain copyright information or legal disclaimers, and other such fine print. However, this is not intended to make the text smaller. It is just describing its content, not prescribing presentation.

### **&lt;time&gt;**

![](.gitbook/assets/image%20%2838%29.png)

*  The `<time>` element allows an unambiguous ISO 8601 date to be attached to a human-readable version of that date.

### **&lt;figure&gt; and &lt;figcaption&gt;**

*  `<figure>` is for wrapping your image content around it, and `<figcaption>` is to caption your image.



## 6. HTML 常用的 头部标签

### 基本标签

* DOCTYPE  该声明位于文档中最前面的位置，处于 html 标签之前，此标签告知浏览器文档使用哪种 HTML 或者 XHTML 规范。  声明以 &lt;!DOCTYPE&gt; 开始，不区分大小写，前面没有任何内容，
* 优先使用 IE 最新版本和 Chrome  `<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />`
* viewport：可以让布局在移动浏览器上显示的更好,如果网站不是响应式 就不要使用 initial-scale`<meta name="viewport" content="width=device-width, initial-scale=1.0">`
* Specifying your document's character encoding:    `<meta charset="utf-8">`
* Specifying language:  `<html lang="zh-cmn-Hans">` 

### SEO 优化标签

* 页面描述：  `<meta name="description" content="不超过150个字符">`
* 页面关键词：`<meta name="keywords" content="">`
* 定义页面标题: `<title>标题</title>`
* 定义网页作者：`<meta name="author" content="name, email@gmail.com">`

### Social Meta tag:

`<meta property="og:image" content="https://developer.mozilla.org/static/img/opengraph-logo.png">`

* og:title
* og:type
* og:image
* og:description



## HTML 中  href 和 src 的区别

![](.gitbook/assets/image%20%2847%29.png)

### href: hypertext reference 

* lina 和 a 元素
* href属性会建立一个通道， 通道联系一个资源
* 上面 那个图片的 a tag 点击之后才会显示图片

### src: source

* img, style, script, input, iframe 
* 应用的资源会直接成为当前文档的一部分
* 在引用时候会下载目标资源 嵌入到当前文档中，浏览器遇到这个属性时候 会暂停其他资源下载和处理， 直到这个属性下载编译执行完后才会进行下一步

## 

##  







