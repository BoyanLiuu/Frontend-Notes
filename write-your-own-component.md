# Write your own component

## Table with pagination

{% embed url="https://www.freecodecamp.org/news/build-a-custom-pagination-component-in-react" %}

### Overall Layout:

* Table header
* Table elements , it will be a state value , changed based on pagination props
* A pagination component, from here we'll invoke the `usePagination` hook which will take in the following parameters to compute the page ranges: `totalCount` , `currentPage` , `pageSize` , `siblingCount` .







### Pagination props:

* **totalCount**: represents the total count of data available from the source.
* **currentPage**:** **represents the current active page. We'll use a **1-based index** instead of a traditional 0-based index for our `currentPage` value.
* **pageSize**:** **represents the maximum data that is visible in a single page.
* **onPageChange**:** **callback function invoked with the updated page value when the page is changed.
* **siblingCount **(optional):** **represents the min number of page buttons to be shown on each side of the current page button. Defaults to 1.

![](<.gitbook/assets/image (152).png>)





## Ant Design Steps

![](<.gitbook/assets/image (29).png>)



## Star Rating Widget:

{% embed url="https://codesandbox.io/s/react-starwidget-w7fl5?file=%2Fsrc%2Findex.js" %}

```
            onClick={() => setRating(index)}
            onMouseEnter={() => setHover(index)}
            onMouseLeave={() => setHover(rating)}

```

* 使用 className 给 element 不同的 style `className={index <= (hover || rating) ? "on" : "off"}`
* 使用setRating 判断 最后选择的 rate
* 使用 OnMouseEnter  和 onMouseLeave 判断更新我们hover上的 index
* OnMouseLeave 如果最后没有点击 任意一个 icon 那就是 不会更新rating index&#x20;







## Slider

## TreeSelect

## Carousel



## Collapse(Accordion)

Without JS

## Tabs



## Tag



## Timeline



## Progress

![](<.gitbook/assets/image (30).png>)



## Skeleton



## Tic-tac-toe

{% embed url="https://codepen.io/gaearon/pen/gWWZgR?editors=0010" %}

{% embed url="https://reactjs.org/tutorial/tutorial.html" %}

## Countdown&#x20;

{% embed url="https://codesandbox.io/s/react-countdown-example-bb98j?file=%2Fsrc%2Findex.js" %}

```
How we update  time
      let days = Math.floor(distanceToDate / (1000 * 60 * 60 * 24));

      let hours = Math.floor((distanceToDate / (1000 * 60 * 60)) % 24);

      let minutes = Math.floor((distanceToDate / 1000 / 60) % 60);
      let seconds = Math.floor((distanceToDate / 1000) % 60);
      
      
```







