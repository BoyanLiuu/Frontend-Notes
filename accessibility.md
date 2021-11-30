# Accessibility

### How people use the web: <a href="#semantic-html" id="semantic-html"></a>

* keyboard only
* Head wand
* Mouse  Stick
* SIngle switch
* Screen Reader

### Using Semantic HTML <a href="#semantic-html" id="semantic-html"></a>

* Using the various HTML elements to reinforce the meaning of information in our websites will often give us accessibility for free
* Or label region with  `aria-label,aria-labelledby1,aria-describedby`
  * aria-describedby: it provides extended information that the user might need

![](<.gitbook/assets/image (149).png>)

## **Using alt for image**

## **Labels tag:**

```
        
          <form>
            <label>
              First Name
              <input id="first" type="text" />
            </label>
            <label>
              Last Name
              <input id="last" type="text" />
            </label>
            <label>
              Password
              <input id="password" type="password" />
            </label>
            <input type="submit" value="Submit" />
          </form>
```

* wrap input with label tag , this is called implicit labelling
* The label tag can only works with "labelable" elements. Those include:
  * \<button>
  * \<input>
  * \<keygen>
  * \<meter>
  * \<output>
  * \<progress>
  * \<select>
  * \<textarea>
* If we want to give lable to other HTML use **aria-label**

## **Focus Management**

* As users navigate around using only the keyboard, focus rings provide a necessary clue as to the currently active item

### tab navigation:

```
<div tabindex="0">I'm focusable</div>
```

* You can use the tab key to navigate to the next tabbable item and shift + tab to navigate to the previous item.
* Tabbable elements include:
  * \<a>
  * \<button>
  * \<input>
  * \<select>
  * \<textarea>
  * \<iframe>
* **Tabindex values**

![](<.gitbook/assets/image (150).png>)

## **Table:**

* Accessible tables need HTML markup that indicates header cells and data cells and defines their relationship

## **Form:**

```
<label for="firstname">First name:</label>
<input type="text" name="firstname" id="firstname"><br>

<input type="checkbox" name="subscribe" id="subscribe">
<label for="subscribe">Subscribe to newsletter</label>
```

* Whenever possible, use the label element to associate text with form elements explicitly. The for attribute of the label must exactly match the id of the form control.  **react 里面 使用的是 htmlFor**
* &#x20;**** The label can be hidden visually, though it still needs to be provided within the code to support other forms of presentation and interaction, such as for screen reader and speech input users.
  * Approach 1: Hide it by using css
  * Approach 2: use `aria-label="Search"`

### Grouping Controls

* Associating related controls with fieldset

```
<fieldset>
<legend>Output format</legend>
  <div>
    <input type="radio" name="format" id="txt" value="txt" checked>
    <label for="txt">Text file</label>
  </div>
  <div>
    <input type="radio" name="format" id="csv" value="csv">
    <label for="csv">CSV file</label>
  </div>
  […]
</fieldset>
```





## What tool we need to used for accessibility?

* Google's lighthouse
* Eslint-plugin-jsx- a11y

