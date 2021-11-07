# Write your own component

## Table with pagination

{% embed url="https://www.digitalocean.com/community/tutorials/how-to-build-custom-pagination-with-react" %}

{% embed url="https://www.freecodecamp.org/news/build-a-custom-pagination-component-in-react/" %}

{% embed url="https://medium.com/@SilentHackz/pagination-in-reactjs-a8c8acc027c7" %}





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







