# React

react技术栈,推荐阅读的源码是react,react-router,redux,react-redux,axios。

[卡颂的react技术揭秘](https://react.iamkasong.com/)



[若川的源码系列](https://juejin.cn/user/1415826704971918/posts)

## JSX

###  `createElement`  

```text
<div>
   <TextComponent />
   <div>hello,world</div>
   let us learn React!
</div>

//变成
 React.createElement("div", null,
        React.createElement(TextComponent, null),
        React.createElement("div", null, "hello,world"),
        "let us learn React!"
    )
```

* 第一个参数：如果是组件类型，会传入组件对应的类或函数；如果是 dom 元素类型，传入 div 或者 span 之类的字符串。
* 第二个参数：一个对象，在 dom 类型中为标签属性，在组件类型中为 props 。
* 其他参数：依次为 children，根据顺序排列。

```text
          <div style={{ marginTop:'100px' }} className="container"  >   
                 { /* element 元素类型 */ }
                <div>hello,world</div>  
                { /* fragment 类型 */ }
                <React.Fragment>      
                    <div> 👽👽 </div>
                </React.Fragment>
                { /* text 文本类型 */ }
                my name is alien       
                { /* 数组节点类型 */ }
                { toLearn.map(item=> <div key={item} >let us learn { item } </div> ) } 
                { /* 组件类型 */ }
                <TextComponent/>  
                { /* 三元运算 */  }
                { this.status ? <TextComponent /> :  <div>三元运算</div> }  
                { /* 函数执行 */ } 
                { this.renderFoot() }  
                <button onClick={ ()=> console.log( this.render() ) } >打印render后的内容</button>
            </div>
```

![](.gitbook/assets/image%20%28139%29.png)









