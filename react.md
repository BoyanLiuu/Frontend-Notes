# React

react技术栈,推荐阅读的源码是react,react-router,redux,react-redux,axios。

[卡颂的react技术揭秘](https://react.iamkasong.com)



[若川的源码系列](https://juejin.cn/user/1415826704971918/posts)



{% embed url="https://react.iamkasong.com/diff/prepare.html#diff%E6%98%AF%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E7%9A%84" %}

{% embed url="https://yuchengkai.cn/react/2019-07-29.html#setstate-%E8%83%8C%E5%90%8E%E7%9A%84%E6%89%B9%E9%87%8F%E6%9B%B4%E6%96%B0%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0" %}

{% embed url="https://juejin.cn/post/6844903830887366670#heading-24" %}

{% embed url="https://github.com/sl1673495/blogs/issues/52" %}

{% embed url="https://juejin.cn/post/6844904103504527374#heading-20" %}





##



## JSX&#x20;

### &#x20;`createElement`&#x20;

```
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
    
React.createElement(
  type,
  [props],
  [...children]
)
```

* 第一个参数：如果是组件类型，会传入组件对应的类或函数；如果是 dom 元素类型，传入 div 或者 span 之类的字符串。
* 第二个参数：一个对象，在 dom 类型中为标签属性，在组件类型中为 props 。
* 其他参数：依次为 children，根据顺序排列。

```
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

#### 转换类型表格

![](<.gitbook/assets/image (157).png>)

![](<.gitbook/assets/image (139).png>)

![](<.gitbook/assets/image (154).png>)

### 问与答：

1: 问：老版本的 React 中，为什么写 jsx 的文件要默认引入 React?

```
import React from 'react'
function Index(){
    return <div>hello,world</div>
}
```

答：因为 jsx 在被 babel 编译后，写的 jsx 会变成上述 React.createElement 形式，所以需要引入 React，防止找不到 React 引起报错。





## 组件中的通讯

### props 和 callback 方式&#x20;

props 和 callback 可以作为 React 组件最基本的通信方式，父组件可以通过 props 将信息传递给子组件，子组件可以通过执行 props 中的回调函数 callback 来触发父组件的方法，实现父与子的消息通讯。

父组件 -> 通过自身 state 改变，重新渲染，传递 props -> 通知子组件

子组件 -> 通过调用父组件 props 方法 -> 通知父组件。

```
/* 子组件 */
function Son(props){
    const {  fatherSay , sayFather  } = props
    return <div className='son' >
         我是子组件
        <div> 父组件对我说：{ fatherSay } </div>
        <input placeholder="我对父组件说" onChange={ (e)=>sayFather(e.target.value) }   />
    </div>
}
/* 父组件 */
function Father(){
    const [ childSay , setChildSay ] = useState('')
    const [ fatherSay , setFatherSay ] = useState('')
    return <div className="box father" >
        我是父组件
       <div> 子组件对我说：{ childSay } </div>
       <input placeholder="我对子组件说" onChange={ (e)=>setFatherSay(e.target.value) }   />
       <Son fatherSay={fatherSay}  sayFather={ setChildSay }  />
    </div>
}
```

### ref 方式。&#x20;

### React-redux 或 React-mobx 状态管理方式。&#x20;

### context 上下文方式。&#x20;

### event bus 事件总线。

当然利用 eventBus 也可以实现组件通信，但是在 React 中并不提倡用这种方式，我还是更提倡用 props 方式通信。如果说非要用 eventBus，我觉得它更适合用 React 做基础构建的小程序，比如 Taro。接下来将上述 demo 通过 eventBus 方式进行改造。

```
import { BusService } from './eventBus'
/* event Bus  */
function Son(){
    const [ fatherSay , setFatherSay ] = useState('')
    React.useEffect(()=>{ 
        BusService.on('fatherSay',(value)=>{  /* 事件绑定 , 给父组件绑定事件 */
            setFatherSay(value)
       })
       return function(){  BusService.off('fatherSay') /* 解绑事件 */ }
    },[])
    return <div className='son' >
         我是子组件
        <div> 父组件对我说：{ fatherSay } </div>
        <input placeholder="我对父组件说" onChange={ (e)=> BusService.emit('childSay',e.target.value)  }   />
    </div>
}
/* 父组件 */
function Father(){
    const [ childSay , setChildSay ] = useState('')
    React.useEffect(()=>{    /* 事件绑定 , 给子组件绑定事件 */
        BusService.on('childSay',(value)=>{
             setChildSay(value)
        })
        return function(){  BusService.off('childSay') /* 解绑事件 */ }
    },[])
    return <div className="box father" >
        我是父组件
       <div> 子组件对我说：{ childSay } </div>
       <input placeholder="我对子组件说" onChange={ (e)=> BusService.emit('fatherSay',e.target.value) }   />
       <Son  />
    </div>
}
```



这样做不仅达到了和使用 props 同样的效果，还能跨层级，不会受到 React 父子组件层级的影响。但是为什么很多人都不推荐这种方式呢？因为它有一些致命缺点。

* 需要手动绑定和解绑。
* 对于小型项目还好，但是对于中大型项目，这种方式的组件通信，会造成牵一发动全身的影响，而且后期难以维护，组件之间的状态也是未知的。
* 一定程度上违背了 React 数据流向原则。

## 组件的强化方式

### **1: class component 继承**

对于类组件的强化，首先想到的是继承方式，之前开发的开源项目 react-keepalive-router 就是通过继承 React-Router 中的 Switch 和 Router ，来达到缓存页面的功能的。因为 React 中类组件，有良好的继承属性，所以可以针对一些基础组件，首先实现一部分基础功能，再针对项目要求进行有方向的**改造**、**强化**、**添加额外功能**。

```
/* 人类 */
class Person extends React.Component{
    constructor(props){
        super(props)
        console.log('hello , i am person')
    }
    componentDidMount(){ console.log(1111)  }
    eat(){    /* 吃饭 */ }
    sleep(){  /* 睡觉 */  }
    ddd(){   console.log('打豆豆')  /* 打豆豆 */ }
    render(){
        return <div>
            大家好，我是一个person
        </div>
    }
}
/* 程序员 */
class Programmer extends Person{
    constructor(props){
        super(props)
        console.log('hello , i am Programmer too')
    }
    componentDidMount(){  console.log(this)  }
    code(){ /* 敲代码 */ }
    render(){
        return <div style={ { marginTop:'50px' } } >
            { super.render() } { /* 让 Person 中的 render 执行 */ }
            我还是一个程序员！    { /* 添加自己的内容 */ }
        </div>
    }
}
export default Programmer

```

我们从上面不难发现这个继承增强效果很优秀。它的优势如下：

1. 可以控制父类 render，还可以添加一些其他的渲染内容；
2. 可以共享父类方法，还可以添加额外的方法和属性。

但是也有值得注意的地方，就是 state 和生命周期会被继承后的组件修改。像上述 demo 中，Person 组件中的 componentDidMount 生命周期将不会被执行。

### **2: 函数组件自定义 Hooks**

### **3: HOC高阶组件**

## Props

```
/* children 组件 */
function ChidrenComponent(){
    return <div> In this chapter, let's learn about react props ! </div>
}
/* props 接受处理 */
class PropsComponent extends React.Component{
    componentDidMount(){
        console.log(this,'_this')
    }
    render(){
        const {  children , mes , renderName , say ,Component } = this.props
        const renderFunction = children[0]
        const renderComponent = children[1]
        /* 对于子组件，不同的props是怎么被处理 */
        return <div>
            { renderFunction() }
            { mes }
            { renderName() }
            { renderComponent }
            <Component />
            <button onClick={ () => say() } > change content </button>
        </div>
    }
}
/* props 定义绑定 */
class Index extends React.Component{
    state={  
        mes: "hello,React"
    }
    node = null
    say= () =>  this.setState({ mes:'let us learn React!' })
    render(){
        return <div>
            <PropsComponent  
               mes={this.state.mes}  // ① props 作为一个渲染数据源
               say={ this.say  }     // ② props 作为一个回调函数 callback
               Component={ ChidrenComponent } // ③ props 作为一个组件
               renderName={ ()=><div> my name is alien </div> } // ④ props 作为渲染函数
            >
                { ()=> <div>hello,world</div>  } { /* ⑤render props */ }
                <ChidrenComponent />             { /* ⑥render component */ }
            </PropsComponent>
        </div>
    }
}
```



如上看一下 props 可以是什么？

* ① props 作为一个子组件渲染数据源。
* ② props 作为一个通知父组件的回调函数。
* ③ props 作为一个单纯的组件传递。
* ④ props 作为渲染函数。
* ⑤ render props ， 和④的区别是放在了 children 属性上。
* ⑥ render component 插槽组件。



```
<Container>
   { (ContainerProps)=> <Children {...ContainerProps}  /> }
</Container>

```

这种情况，在 Container 中， props.children 属性访问到是函数，并不是 React element 对象，针对这种情况，像下面这种情况下 children 是不能直接渲染的，直接渲染会报错。

```
function  Container(props) {
     return  props.children
}
```



![](<.gitbook/assets/image (156).png>)

改成如下方式，就可以了。

```
function  Container(props) {
    const  ContainerProps = {
        name: 'alien',
        mes:'let us learn react'
    }
     return  props.children(ContainerProps)
}
```

### 如果 Container 的 children 既有函数也有组件，这种情况应该怎么处理呢？

```
<Container>
    <Children />
    { (ContainerProps)=> <Children {...ContainerProps} name={'haha'}  />  }
</Container>
```

![](<.gitbook/assets/image (152).png>)

```
const Children = (props)=> (<div>
    <div>hello, my name is {  props.name } </div>
    <div> { props.mes } </div>
</div>)

function  Container(props) {
    const ContainerProps = {
        name: 'alien',
        mes:'let us learn react'
    }
     return props.children.map(item=>{
        if(React.isValidElement(item)){ // 判断是 react elment  混入 props
            return React.cloneElement(item,{ ...ContainerProps },item.props.children)
        }else if(typeof item === 'function'){
            return item(ContainerProps)
        }else return null
     })
}

const Index = ()=>{
    return <Container>
        <Children />
        { (ContainerProps)=> <Children {...ContainerProps} name={'haha'}  />  }
    </Container>
}
```

















States notes

如果 setState 在 React 能够控制的范围被调用，它就是异步的。

比如合成事件处理函数, 生命周期函数, 此时会进行批量更新, 也就是将状态合并后再进行 DOM 更新。

如果 setState 在原生 JavaScript 控制的范围被调用，它就是同步的。

比如原生事件处理函数中, 定时器回调函数中, Ajax 回调函数中, 此时 setState 被调用后会立即更新 DOM 。



```
export default class index extends React.Component{
    state = { number:0 }
    handleClick= () => {
          this.setState({ number:this.state.number + 1 },()=>{   console.log( 'callback1', this.state.number)  })
          console.log(this.state.number)
          this.setState({ number:this.state.number + 1 },()=>{   console.log( 'callback2', this.state.number)  })
          console.log(this.state.number)
          this.setState({ number:this.state.number + 1 },()=>{   console.log( 'callback3', this.state.number)  })
          console.log(this.state.number)
    }
    render(){
        return <div>
            { this.state.number }
            <button onClick={ this.handleClick }  >number++</button>
        </div>
    }
} 
```

点击打印：**0, 0, 0, callback1 1 ,callback2 1 ,callback3 1**

****![](<.gitbook/assets/image (155).png>)****

### **batchUpdate 异步操作会在如下情况 被打破**

比如用 promise 或者 setTimeout 在 handleClick 中这么写：

```
setTimeout(()=>{
    this.setState({ number:this.state.number + 1 },()=>{   console.log( 'callback1', this.state.number)  })
    console.log(this.state.number)
    this.setState({ number:this.state.number + 1 },()=>{    console.log( 'callback2', this.state.number)  })
    console.log(this.state.number)
    this.setState({ number:this.state.number + 1 },()=>{   console.log( 'callback3', this.state.number)  })
    console.log(this.state.number)
})
```

打印 ： **callback1 1 , 1, callback2 2 , 2,callback3 3 , 3**

![](<.gitbook/assets/image (158).png>)

### 怎么在异步情况下 开启 batchupdate?

React-Dom 中提供了批量更新方法 `unstable_batchedUpdates`，可以去手动批量更新，可以将上述 setTimeout 里面的内容做如下修改:

在实际工作中，unstable\_batchedUpdates 可以用于 Ajax 数据交互之后，合并多次 setState，或者是多次 useState 。**原因很简单，所有的数据交互都是在异步环境下，如果没有批量更新处理，一次数据交互多次改变 state 会促使视图多次渲染**

```
import ReactDOM from 'react-dom'
const { unstable_batchedUpdates } = ReactDOM

setTimeout(()=>{
    unstable_batchedUpdates(()=>{
        this.setState({ number:this.state.number + 1 })
        console.log(this.state.number)
        this.setState({ number:this.state.number + 1})
        console.log(this.state.number)
        this.setState({ number:this.state.number + 1 })
        console.log(this.state.number) 
    })
})
```

### **那么如何提升更新优先级呢？**

```
  handleClick = () => {
    setTimeout(() => {
      this.setState(() => {
        console.log(1);
        return { number: 1 };
      });
    });
    this.setState(() => {
      console.log(2);
      return { number: 2 };
    });
    ReactDOM.flushSync(() => {
      console.log(3);
      this.setState({ number: 3 });
    });
    this.setState(() => {
      console.log(4);
      return { number: 4 };
    });
  };
```

打印 **3, 2,4 1** ，相信不难理解为什么这么打印了。



* 首先 `flushSync` `this.setState({ number: 3 })`设定了一个高优先级的更新，所以 2 和 3 被批量更新到 3 ，所以 3 先被打印。
* 更新为 4。
* 最后更新 setTimeout 中的 number = 1。
* **flushSync补充说明**：flushSync 在同步条件下，会合并之前的 setState | useState，可以理解成，如果发现了 flushSync ，就会先执行更新，如果之前有未更新的 setState ｜ useState ，就会一起合并了，所以就解释了如上，2 和 3 被批量更新到 3 ，所以 3 先被打印。
* **flushSync 中的 setState > 正常执行上下文中 setState > setTimeout ，Promise 中的 setState。**

```
const [ number , setNumber ] = React.useState(0)
const handleClick = ()=>{
    ReactDOM.flushSync(()=>{
        setNumber(2) 
        console.log(number) 
    })
    setNumber(1) 
    console.log(number)
    setTimeout(()=>{
        setNumber(3) 
        console.log(number)
    })   
}
```

打印：**结果： 0 0 0** 就是当调用改变 state 的函数dispatch，在本次函数执行上下文中，是获取不到最新的 state 值的。

相同state 值不会更新 ui

```
export default function Index(){
    const [ state  , dispatchState ] = useState({ name:'alien' })
    const  handleClick = ()=>{ // 点击按钮，视图没有更新。
        state.name = 'Alien'
        dispatchState(state) // 直接改变 `state`，在内存中指向的地址相同。
    }
    return <div>
         <span> { state.name }</span>
        <button onClick={ handleClick }  >changeName++</button>
    </div>
}
```

在 useState 的 dispatchAction 处理逻辑中，会浅比较两次 state ，发现 state 相同，不会开启更新调度任务； demo 中两次 state 指向了相同的内存空间，所以默认为 state 相等，就不会发生视图更新了。

解决问题： 把上述的 dispatchState 改成 dispatchState({...state}) 根本解决了问题，浅拷贝了对象，重新申请了一个内存空间。

### useState():

* Returns a stateful value, and a function to update it.
* If your update function returns the exact same value as the current state, the subsequent rerender will be skipped completely.

```
const [state, setState] = useState(initialState);
```

```
function Counter({initialCount}) {
  const [count, setCount] = useState(initialCount);
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(initialCount)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    </>
  );
}
```

* The function will receive the previous value, and return an updated value.

```
const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation(props);
  return initialState;
});
```

* **Lazy initial state**:The initialState argument is the state used during the initial render. In subsequent renders, it is disregarded. If the initial state is the result of an expensive computation, you may provide a function instead, which will be executed only on the initial render
* **Where to call useState()?**
  * **Only Call Hooks at the Top Level**: you cannot call useState() in loops, conditions, nested functions, etc. On multiple useState() calls, the invocation order must be the same between renderings.
  * **Only Call Hooks from React Functions**: you must call useState() only inside the functional component or a custom hook.

![](<.gitbook/assets/image (154) (1).png>)

* **Stale state**
  * [**https://codesandbox.io/s/react-usestate-async-broken-uzzvg**](https://codesandbox.io/s/react-usestate-async-broken-uzzvg)****
  * Closures (e.g. event handlers, callbacks) might capture state variables from the functional component scope. Because state variables change between renderings, closures should capture variables with the latest state value. That is why we need to use prev state.

### How does useState() work internally:

{% embed url="https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e" %}



#### &#x20;Initialisation:

![](<.gitbook/assets/image (144).png>)

* Create two empty arrays: \`setters\` and \`state\`
* Set a cursor to 0

#### First render

* Run the component function for the first time.
* &#x20;Each `useState()` call, when first run, pushes a setter function (bound to a cursor position) onto the `setters` array and then pushes some state on to the `state` array.

![](<.gitbook/assets/image (145).png>)

Each subsequent render the cursor is reset and those values are just read from each array.

###



## Hooks:

### The main rules of hooks:

* Don’t call Hooks inside loops, conditions, or nested functions
* Only Call Hooks from React Functions

### useEffect():

* &#x20;The function passed to useEffect will run after the render is committed to the screen
* &#x20;**props and state that change over time and that are used by the effect**.

#### cleaning up an effect:

```
useEffect(() => {
  const subscription = props.source.subscribe();
  return () => {
    // Clean up the subscription
    subscription.unsubscribe();
  };
});
```

* effects create resources that need to be cleaned up before the component leaves the screen
* The clean-up function runs before the component is removed from the UI to prevent memory leak
* &#x20; Additionally, if a component renders multiple times (as they typically do), the **previous effect is cleaned up before executing the next effect**
* &#x20;the function passed to `useEffect` fires after **layout and paint,** during a deferred event, not all effects can be deferred. For example, a DOM mutation that is visible to the user must fire synchronously before the next paint so that the user does not perceive a visual inconsistency.
* Although `useEffect` is deferred until after the browser has painted, it’s guaranteed to fire before any new renders. React will always flush a previous render’s effects before starting a new update.

#### **Conditionally firing an effect**

```
useEffect(
  () => {
    const subscription = props.source.subscribe();
    return () => {
      subscription.unsubscribe();
    };
  },
  [props.source],
);
```











#### useContext():

```
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};
//第一步就是使用 React Context API，在组件外部建立一个 Context
const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```

* Context lets you “broadcast” such data, and changes to it, to all components below. Common examples where using context might be simpler than the alternatives include managing the current locale, theme, or a data cache

#### 每次 Render 都有自己的事件处理

```
const App = () => {
  const [temp, setTemp] = React.useState(5);

  const log = () => {
    setTimeout(() => {
      console.log("3 秒前 temp = 5，现在 temp =", temp);
    }, 3000);
  };

  return (
    <div
      onClick={() => {
        log();
        setTemp(3);
        // 3 秒前 temp = 5，现在 temp = 5
      }}
    >
      xyz
    </div>
  );
};

```

* &#x20;在 `log` 函数执行的那个 Render 过程里，`temp` 的值可以看作常量 `5`，**执行 `setTemp(3)` 时会交由一个全新的 Render 渲染**，所以不会执行 `log` 函数。**而 3 秒后执行的内容是由 `temp` 为 `5` 的那个 Render 发出的**，所以结果自然为 `5`。

### useReducer():

{% embed url="https://codesandbox.io/s/react-usereducer-redux-forked-m6wu4?file=/src/index.js" %}

```
const [state, dispatch] = useReducer(reducer, initialState);
```

* &#x20;Redux 的核心概念是，组件发出 action 与状态管理器通信。状态管理器收到 action 以后，使用 Reducer 函数算出新的状态， Accepts a reducer of type `(state, action) => newState`, and returns the current state paired with a `dispatch` method.&#x20;

### useCallback(): for functions

* Use this tool to check if it is worth to use this function [https://developer.chrome.com/docs/devtools/evaluate-performance/](https://developer.chrome.com/docs/devtools/evaluate-performance/)

```
function MyComponent() {
  // handleClick is re-created on each render
  const handleClick = () => {
    console.log('Clicked!');
  };
  // ...
}
```

* `handleClick` is a different function object on every rendering of `MyComponent`.
* Because inline functions are cheap, the re-creation of functions on each rendering is not a problem. _A few inline functions per component are acceptable._

```
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

* **useCallback is used to memoize functions**&#x20;
* 该回调函数仅在某个依赖项改变时才会更新
* &#x20;That’s when `useCallback(callbackFun, deps)` is helpful: given the same dependency values `deps`, the hook returns (aka memoizes) the function instance between renderings:

```
import { useCallback } from 'react';
export function MyParent({ term }) {
  const onItemClick = useCallback(event => {
    console.log('You clicked ', event.currentTarget);
  }, [term]);
  return (
    <MyBigList
      term={term}
      onItemClick={onItemClick}
    />
  );
}
```

*   `onItemClick` callback is memoized by `useCallback()`. As long as `term` is the same, `useCallback()` returns the same function object.

    When `MyParent` component re-renders, `onItemClick` function object remains the same and doesn’t break the memoization of `MyBigList`

#### A bad example:

```
import { useCallback } from 'react';
function MyComponent() {
  // Contrived use of `useCallback()`
  const handleClick = useCallback(() => {
    // handle the click event
  }, []);
  return <MyChild onClick={handleClick} />;
}
function MyChild ({ onClick }) {
  return <button onClick={onClick}>I am a child</button>;
}
```

* Most likely not because `<MyChild>` component is light and its re-rendering doesn’t create performance issues.

{% embed url="https://codesandbox.io/s/react-usecallback-with-problem-ckzv4?file=/src/index.js" %}



* 每次点击 Btn 剩余 的 不相干的东西也会 render一遍
* &#x20;In React, whenever a component re-renders, a new instance of the function in it gets generated. Therefore, every time `App` renders, `add`, `increase` and `decrease` are re-created. So their references now points to different functions in memory

![](<.gitbook/assets/image (140).png>)

* &#x20;**Solution: useCallBack():** This prevents the unnecessary re-rendering behaviour because it ensures the **same callback function reference** is returned when there is no change in their dependency.
* And use  **`React.memo()`** is a built-in React feature that renders a memoized component and skip unnecessary re-rendering. So each component will only re-render if it detects a change in their props



### React.memo: for component

* When a component is wrapped in `React.memo()`, React renders the component and memoizes the result. Before the next render, if the new props are the same, React reuses the memoized result _skipping the next rendering_.

{% embed url="https://codesandbox.io/s/react-memo-demo-c9dx1?file=%2Fsrc%2Findex.js" %}

* You will see that React renders `<MemoizedMovie>` just once, while `<Movie>` re-renders every time.
* By default `React.memo()` does a [shallow](https://github.com/facebook/react/blob/v16.8.6/packages/shared/shallowEqual.js) comparison of props and objects of props
* To customize the props comparison you can use the second argument to indicate an equality check function: Here is the example:

```
function moviePropsAreEqual(prevMovie, nextMovie) {
  return prevMovie.title === nextMovie.title
    && prevMovie.releaseDate === nextMovie.releaseDate;
}
const MemoizedMovie2 = React.memo(Movie, moviePropsAreEqual);
```

* When to use useMemo():
  * a pure functional component giving same props will always give same result
  * Your components render often
  * Your component is medium to large size
* If the component _isn’t heavy_ and usually _renders with different props_, most likely you don’t need `React.memo()`.

### useMemo():

{% embed url="https://codesandbox.io/s/react-usememo-voelc?file=/src/index.js:0-1222" %}





```
//基本样子
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

* _useMemo is used to memoize values._
* Pass a “create” function and an array of dependencies
* If no array is provided, a new value will be computed on every render.
* &#x20;`useMemo` invokes the provided function and caches its result.
* &#x20;`useMemo` can cache a function value too. In other words, it is a generalised version of `useCallback` and can replace it as in the following example

![](<.gitbook/assets/image (142).png>)

```
import React, { useState } from "react";
import ReactDOM from "react-dom";
import "./styles.css";

const users = [
  { id: "a", name: "Robin" },
  { id: "b", name: "Dennis" }
];

const App = () => {
  const [text, setText] = useState("");
  const [search, setSearch] = useState("");
  console.log("App is running");
  const handleText = (event) => {
    setText(event.target.value);
  };

  const handleSearch = () => {
    setSearch(text);
  };

  const filteredUsers = users.filter((user) => {
    console.log("Filter function is running ...");
    return user.name.toLowerCase().includes(search.toLowerCase());
  });

  return (
    <div>
      <input type="text" value={text} onChange={handleText} />
      <button type="button" onClick={handleSearch}>
        Search
      </button>

      <List list={filteredUsers} />
    </div>
  );
};

const List = ({ list }) => {
  return (
    <ul>
      {list.map((item) => (
        <ListItem key={item.id} item={item} />
      ))}
    </ul>
  );
};

const ListItem = ({ item }) => {
  return <li>{item.name}</li>;
};

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);

```

* 这里的  filteredUsers function 在我们每次输入的时候 都会 run， 因为我们每次输入时候 都会 setText() 然后导致App 重新 render
* &#x20; However, if we would deal with a large set of data in this array and run the filter's callback function for every keystroke, we would maybe slow down the application. Therefore, you can use React's useMemo Hook to **memoize a functions return value(s)** and to run a function only if its dependencies (here `search`) have changed:

![](<.gitbook/assets/image (141).png>)

* &#x20;Now, this function is only executed once the `search` state changes. It doesn't run if the `text` state changes, because that's not a dependency for this filter function and thus not a dependency in the dependency array for the useMemo hook

### useRef

```
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

* &#x20;The “ref” object is a generic container whose `current` property is mutable and can hold any value, similar to an instance property on a class
* 普遍操作，用来操作dom
* &#x20;**可以认为 `ref` 在所有 Render 过程中保持着唯一引用，因此所有对 `ref` 的赋值或取值，拿到的都只有一个最终状态**，而不会在每个 Render 间存在隔离。

### 自定义Hooks

```
const usePerson = (personId) => {
  const [loading, setLoading] = useState(true);
  const [person, setPerson] = useState({});
  useEffect(() => {
    setLoading(true);
    fetch(`https://swapi.co/api/people/${personId}/`)
      .then(response => response.json())
      .then(data => {
        setPerson(data);
        setLoading(false);
      });
  }, [personId]);  
  return [loading, person];
};

// 在其余组件：
const Person = ({ personId }) => {
  const [loading, person] = usePerson(personId);

  if (loading === true) {
    return <p>Loading ...</p>;
  }

  return (
    <div>
      <p>You're viewing: {person.name}</p>
      <p>Height: {person.height}</p>
      <p>Mass: {person.mass}</p>
    </div>
  );
};

```

### react 是怎么保证 多个 state 读取正确：

* react是根据useState出现的顺序来定的
* 所以我们没法usestate 写到 if else 里面



### More information on useEffect:

#### _1. Each Render Has Its Own Props and State_

```
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

&#x20;The first time our component renders, the `count` variable we get from `useState()` is `0`. When we call `setCount(1)`, React calls our component again. This time, `count` will be `1`. And so on:

```
// During first render
function Counter() {
  const count = 0; // Returned by useState()
  // ...
  <p>You clicked {count} times</p>
  // ...
}

// After a click, our function is called again
function Counter() {
  const count = 1; // Returned by useState()
  // ...
  <p>You clicked {count} times</p>
  // ...
}

// After another click, our function is called again
function Counter() {
  const count = 2; // Returned by useState()
  // ...
  <p>You clicked {count} times</p>
  // ...
}
```

* **Whenever we update the state, React calls our component. Each render result “sees” its own `counter` state value which is a **_**constant**_** inside our function.**

#### _**2.** Each Render Has Its Own Event Handlers_

{% embed url="https://codesandbox.io/s/use-effect-example-85vf6?file=/src/index.js" %}

```
// During first render
function Counter() {
  const count = 0; // Returned by useState()
  // ...
  function handleAlertClick() {
    setTimeout(() => {
      alert('You clicked on: ' + count);
    }, 3000);
  }
  // ...
}

// After a click, our function is called again
function Counter() {
  const count = 1; // Returned by useState()
  // ...
  function handleAlertClick() {
    setTimeout(() => {
      alert('You clicked on: ' + count);
    }, 3000);
  }
  // ...
}

// After another click, our function is called again
function Counter() {
  const count = 2; // Returned by useState()
  // ...
  function handleAlertClick() {
    setTimeout(() => {
      alert('You clicked on: ' + count);
    }, 3000);
  }
  // ...
}
```

* &#x20;each render returns its own “version” of `handleAlertClick`. Each of those versions “remembers” its own `count`:
* The alert will “capture” the state at the time I clicked the button
* **our function gets called many times (once per each render), but every one of those times the count value inside of it is constant and set to a particular value (state for that render).**

#### _3. Each Render Has Its Own Effects_

```
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

**how does the effect read the latest `count` state?**&#x20;

* **It’s the effect function itself that’s different on every render.**

```
// During first render
function Counter() {
  // ...
  useEffect(
    // Effect function from first render
    () => {
      document.title = `You clicked ${0} times`;
    }
  );
  // ...
}

// After a click, our function is called again
function Counter() {
  // ...
  useEffect(
    // Effect function from second render
    () => {
      document.title = `You clicked ${1} times`;
    }
  );
  // ...
}

// After another click, our function is called again
function Counter() {
  // ...
  useEffect(
    // Effect function from third render
    () => {
      document.title = `You clicked ${2} times`;
    }
  );
  // ..
}
```

* &#x20;So even if we speak of a single conceptual effect here (updating the document title), it is represented by a different function on every render — and each effect function “sees” props and state from the particular render it “belongs” to

#### _4. How does clean up work?_&#x20;

* **The effect cleanup doesn’t read the “latest” props, whatever that means. It reads props that belong to the render it’s defined in:**

```
 useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.id, handleStatusChange);
    };
  });
  //Say props is {id: 10} on the first render, and {id: 20} on the second render.
```

* &#x20;React only runs the effects after letting the browser paint. This makes your app faster as most effects don’t need to block screen updates. Effect cleanup is also delayed. **The previous effect is cleaned up **_**after**_** the re-render with new props:**
  * **React renders UI for `{id: 20}`.**
  * The browser paints. We see the UI for `{id: 20}` on the screen.
  * **React cleans up the effect for `{id: 10}`.**
  * React runs the effect for `{id: 20}`.

#### _5. Honest about dependencies_&#x20;

```
function Counter() {
  const [count, setCount] = useState(0);
  const [step, setStep] = useState(1);
  useEffect(() => {
    const id = setInterval(() => {
      setCount(c => c + step);
    }, 1000);
    return () => clearInterval(id);
  }, [step]);

  return (
    <>
      <h1>{count}</h1>
      <input value={step} onChange={e => setStep(Number(e.target.value))} />
    </>
  );
}
```

* include all the values inside the component that are used inside the effect.&#x20;
* change our effect code so that it wouldn’t need a value that changes more often than we want
* &#x20;**When setting a state variable depends on the current value of another state variable, you might want to try replacing them both with `useReducer`.**

{% embed url="https://codesandbox.io/s/usereducer-tqv3p?file=/src/index.js" %}

* Moving functions Inside effects &#x20;
  * If we forget to update the deps of any effects that call these functions (possibly, through other functions!), our effects will fail to synchronize changes from our props and state.
  * **Solution 1:** Move functions inside effects,  **We no longer have to think about the “transitive dependencies.**
  * **Solution 2:Wrap function into useCallBack() Hook,** if we do not wrap and use that function as dependencies, it would change too often,  if `query` is the same, `getFetchUrl` also stays the same, and our effect doesn’t re-run

```
// 有问题的 effect
function SearchResults() {
  const [query, setQuery] = useState('react');

  // Imagine this function is also long
  function getFetchUrl() {
    return 'https://hn.algolia.com/api/v1/search?query=' + query;
  }

  // Imagine this function is also long
  async function fetchData() {
    const result = await axios(getFetchUrl());
    setData(result.data);
  }

  useEffect(() => {
    fetchData();
  }, []);

  // ...
}

// Solution 2: 
function SearchResults() {
  // ✅ Preserves identity when its own deps are the same
  const getFetchUrl = useCallback((query) => {
    return 'https://hn.algolia.com/api/v1/search?query=' + query;
  }, []);  // ✅ Callback deps are OK

  useEffect(() => {
    const url = getFetchUrl('react');
    // ... Fetch data and do something ...
  }, [getFetchUrl]); // ✅ Effect deps are OK

  useEffect(() => {
    const url = getFetchUrl('redux');
    // ... Fetch data and do something ...
  }, [getFetchUrl]); // ✅ Effect deps are OK

  // ...
}
```



## How does React tell a Class from a function?

{% embed url="https://codesandbox.io/s/function-class-component-difference-1z0xt?file=/src/index.js" %}



{% embed url="https://overreacted.io/how-does-react-tell-a-class-from-a-function/" %}

* &#x20;React needs to call classes (including Babel output) _with_ `new` but it needs to call regular functions or arrow functions (including Babel output) _without_ `new`. And there is no reliable way to distinguish them.
* We just detect only **React.Component** descendants

```
// `extends` chain
Greeting
  → React.Component
    → Object (implicitly)

// `__proto__` chain
new Greeting()
  → Greeting.prototype
    → React.Component.prototype
      → Object.prototype
console.log(Greeting.prototype instanceof React.Component);
// greeting
//   .__proto__ → Greeting.prototype (🕵️‍ We start here)
//     .__proto__ → React.Component.prototype (✅ Found it!)
//       .__proto__ → Object.prototype      
```



## How are function components different from classes?

### &#x20;1: **Function components capture the rendered values.**

```
function ProfilePage(props) {
  const showMessage = () => {
    alert('Followed ' + props.user);
  };

  const handleClick = () => {
    setTimeout(showMessage, 3000);
  };

  return (
    <button onClick={handleClick}>Follow</button>
  );
}


class ProfilePage extends React.Component {
  showMessage = () => {
    alert('Followed ' + this.props.user);
  };

  handleClick = () => {
    setTimeout(this.showMessage, 3000);
  };

  render() {
    return <button onClick={this.handleClick}>Follow</button>;
  }
}
```

![Solution 2](<.gitbook/assets/image (147).png>)

![Solution 3 ](<.gitbook/assets/image (148).png>)

* &#x20;This class method reads from `this.props.user`. Props are immutable in React so they can never change. **However, `this`  **_**is**_**, and has always been, mutable.**
* &#x20;So if our component re-renders while the request is in flight, `this.props` will change, **the event handlers are a part of the render result — just like the visual output**. Our event handlers “belong” to a particular render with particular props and state.
* How to fix this?
  * Solution 1: Use functional components
  * Solution 2: Another way to do it would be to read this.props early during the event, and then explicitly pass them through into the timeout completion handler:
    * Disadvantages: make the code more verbose and error-prone with time.
  * Solution 3 Another way is to use closure , we captured props at the time of render

### 2. React lifecycle methods (for example, componentDidMount) cannot be used in functional components.

### 3. There is no render method used in functional components.

### 4. Functional Component or Stateless Component:

* It is simple javascript functions that simply returns html UI
* It is also called “stateless” components because they simply accept data and display them in some form that is they are mainly responsible for rendering UI.
* These can be typically defined using arrow functions but can also be created with the regular function keyword.



## Why you should use functional components at all?&#x20;

* Functional component are much **easier to read and test** because they are plain JavaScript functions without state or lifecycle-hooks
* You end up with **less code**
* They help you to use best practices. It will get easier to separate container and presentational components because you need to think more about your component’s state if you don’t have access to setState() in your component



## Understanding React's key prop

{% embed url="https://zhuanlan.zhihu.com/p/112917118" %}



* &#x20;React's `key` prop gives you the ability to control component instances. **Each time React renders your components, it's calling your functions to retrieve the new React elements that it uses to update the DOM**. If you return the same element types, it keeps those components/DOM nodes around, even if all\* the props changed.
* &#x20;This allows you to return the exact same element type, but force React to unmount the previous instance, and mount a new one. This means that all state that had existed in the component at the time is completely removed and the component is "reinitialized" for all intents and purposes. For components, this means that React will run cleanup on effects (or `componentWillUnmount`), then it will run state initializers (or the `constructor`) and effect callbacks (or `componentDidMount`).
* **设置了key能够提升渲染效率**
* Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:
* When children have keys, React uses the key to match children in the original tree with children in the subsequent tree. For example

****

## 提升React 性能

### 方法 1

{% embed url="https://kentcdodds.com/blog/optimize-react-re-renders" %}

{% embed url="https://codesandbox.io/s/react-performance-1-slow-q9swc?file=/src/index.js" %}

#### Problem:

* When that's run, "counter rendered" will be logged to the console initially, and each time the count is incremented, "counter rendered" will be logged to the console.
* This happens because when the button is clicked, state changes and React needs to get the new React elements to render based on that state change. When it gets those new elements, it renders and commits them to the DOM.
* the label prop for the Logger element is unchanged. However the props object itself changes every render,React needs to re-run the Logger function to make sure that it doesn't get any new JSX based on the new props object (in addition to effects that may need to be run based on the props change)
* **But  Logger component never changed so it should not rerender**

{% embed url="https://codesandbox.io/s/react-performance-1-fast-sgz8f?file=/src/index.js" %}

#### Solution:

* &#x20;we prevent the props from changing between renders
* We created JSX element once and re-uise that same one
* "Lift" the expensive component to a parent where it will be rendered less often.
* Then pass the expensive component down as a prop.

## 为什么选择使用框架而不是原生?

* 组件化: 其中以 React 的组件化最为彻底,甚至可以到函数级别的原子组件,高度的组件化可以是我们的工程易于维护、易于组合拓展。
* 生态: 现在主流前端框架都自带生态,不管是数据流管理架构还是 UI 库都有成熟的解决方案。
* 开发效率: 现代前端框架都默认自动更新DOM,而非我们手动操作,解放了开发者的手动DOM成本,提高开发效率,从根本上解决了UI 与状态同步问题.
* **Large Community**
  * React has more than 165K stars on Github and around 10 million npm downloads.
* Reusable Components: Each React component that you have developed can be reused in other parts of the app, or you can create wrapper components that provide structure and reusability.
* Unidirectional data flow:
  * The downward directional binding makes the code stable and consistent as any child components’ changes won’t affect the siblings or parent components. To modify an object, you just have to update the state. ReactJS will automatically alter the valid details to maintain consistency

## setState到底是异步还是同步?

* 有时表现出异步,有时表现出同步



## &#x20;

## **how exactly browser renders a web page:**

![](<.gitbook/assets/image (151).png>)

* &#x20;Rendering engines which are responsible for displaying or rendering the webpage on the browser screen parses the HTML page to create DOM. It also parses the CSS and applies the CSS to the HTML, thus creating a render tree, this process is called as _attachment_.
* Layout process gives exact coordinates to each node of the render tree, where the node gets painted and displayed

## **virtual dom in react:**

Virtual DOM is an in-memory representation of real DOM**.** It is a lightweight JavaScript object which is a copy of Real DOM.

### Updating virtual DOM in ReactJS is faster because ReactJS uses:

* Efficient diff algorithm
* Batched update operations
* Efficient update of subtree only
* Uses observable instead of dirty checking to detect the change

Whenever setState() method is called on any component, ReactJS makes that component dirty and re-renders it.Whenever setState() method is called, ReactJS creates the whole Virtual DOM from scratch. Creating a whole tree is very fast so it does not affect the performance. At any given time, ReactJS maintains two virtual DOM, one with the updated state Virtual DOM and other with the previous state Virtual DOM.

ReactJS using diff algorithm compares both the Virtual DOM to find the minimum number of steps to update the Real DOM

Finding the minimum number of modifications between two trees have complexity in the order of **O(n^3)**. But react uses a heuristic approach with some assumptions which makes the problems to have complexity in the order of **O(n)**

* 只对同级元素进行Diff。如果一个DOM节点在前后两次更新中跨越了层级，那么React不会尝试复用他。
* 两个不同类型的元素会产生出不同的树。如果元素由div变为p，React会销毁div及其子孙节点，并新建p及其子孙节点。
* 开发者可以通过 key prop来暗示哪些子元素在不同的渲染下能保持稳定。考虑如下例子：

```
// 更新前
<div>
  <p key="ka">ka</p>
  <h3 key="song">song</h3>
</div>

// 更新后
<div>
  <h3 key="song">song</h3>
  <p key="ka">ka</p>
</div>

```

如果没有`key`，`React`会认为`div`的第一个子节点由`p`变为`h3`，第二个子节点由`h3`变为`p`。这符合限制2的设定，会销毁并新建。

但是当我们用`key`指明了节点前后对应关系后，`React`知道`key === "ka"`的`p`在更新后还存在，所以`DOM节点`可以复用，只是需要交换下顺序。



### Diff 是如何实现的：



### What is virtual dom?

* React creates a tree of custom objects representing a part of the DOM. For example, instead of creating an actual DIV element containing a UL element, it creates a React.div object that contains a React.ul object. It can manipulate these objects very quickly without actually touching the real DOM or going through the DOM API. Then, when it renders a component, it uses this virtual DOM to figure out what it needs to do with the real DOM to get the two trees to match.
* It can be think as a js object, it contains like type, props and child field.

### How to find  the difference in both the Virtual DOM’s:

1. **Re-render all the children if parent state has changed**
2. **Breadth First Search**
3. **Reconciliation:** It is the process to determine which parts of the Real DOM need to be updated. It follows the below steps:
   1. Two elements of different types will produce different trees.
   2. The developer can hint at which child elements may be stable across different renders with a key prop.

### What is difference between state and props?

* Props and state are related. The state of one component will often become the props of a child component.
* React components **re-render** if _props_ or _state_ **have changed**. Any update from anywhere in the code triggers an automatic re-render of the appropriate part of the User Interface.&#x20;
* _Props_ and _state_ are **JS objects**,
* **State**
  * state is used communication in the component, it is mutable using useState.
  * State has to have the initial value,
  * Make the components interactive
  * Update the UI when the state has been changed
* **Props**
  * It usually used for communication between components.&#x20;
  * it is immutable which lets React do fast reference checks
  * props initial value can be empty
  * To display static non-interactive components and data



