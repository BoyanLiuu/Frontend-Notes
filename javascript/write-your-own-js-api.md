# Write your own JS API

{% embed url="https://juejin.cn/post/6873513007037546510" %}

{% embed url="https://juejin.cn/post/6946022649768181774" %}



## instanceof 

```text
function myInstanceof(left, right) {
    //基本数据类型直接返回false
    if(typeof left !== 'object' || left === null) return false;
    //getProtypeOf是Object对象自带的一个方法，能够拿到参数的原型对象
    let proto = Object.getPrototypeOf(left);
    while(true) {
        //查找到尽头，还没找到
        if(proto == null) return false;
        //找到相同的原型对象
        if(proto == right.prototype) return true;
        proto = Object.getPrototypeOf(proto);
    }
}

console.log(myInstanceof("111", String)); //false
console.log(myInstanceof(new String("111"), String));//true
```

## Object.is\(\)

```text
function is(x, y) {
  
  if (x === y) {
    //运行到1/x === 1/y的时候x和y都为0，但是1/+0 = +Infinity， 1/-0 = -Infinity, 是不一样的
    //需要排除x,y 为0的情况
    return x !== 0 || y !== 0 || 1 / x === 1 / y;
  } else {
    //NaN===NaN是false,这是不对的，我们在这里做一个拦截，x !== x，那么一定是 NaN, y 同理
    //两个都是NaN的时候返回true
    return x !== x && y !== y;
  }
}
```



## Write your own Promise

[https://juejin.cn/post/6844904004007247880\#heading-52](https://juejin.cn/post/6844904004007247880#heading-52)

## Array API:

### apply\(\)

### bind\(\)

* https://www.educative.io/courses/javascript-interview-handbook/qV0q64Z63Bk



### call\(\) 

### splice\(\)



### slice\(\)

### map\(\)

### reduce\(\)

[https://juejin.cn/post/6844903986479251464\#heading-26](https://juejin.cn/post/6844903986479251464#heading-26)

### forEach\(\)



### flat\(\)

### from\(\)



new\(\)





