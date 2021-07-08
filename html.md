# HTML

## The following information comes from ch.2

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



##  







