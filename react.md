# React

react技术栈,推荐阅读的源码是react,react-router,redux,react-redux,axios。

[卡颂的react技术揭秘](https://react.iamkasong.com/)



[若川的源码系列](https://juejin.cn/user/1415826704971918/posts)



{% embed url="https://react.iamkasong.com/diff/prepare.html\#diff%E6%98%AF%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E7%9A%84" %}

{% embed url="https://yuchengkai.cn/react/2019-07-29.html\#setstate-%E8%83%8C%E5%90%8E%E7%9A%84%E6%89%B9%E9%87%8F%E6%9B%B4%E6%96%B0%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0" %}

{% embed url="https://juejin.cn/post/6844903830887366670\#heading-24" %}

{% embed url="https://github.com/sl1673495/blogs/issues/52" %}

{% embed url="https://juejin.cn/post/6844904103504527374\#heading-20" %}





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



## Hooks:

### useState\(\):

* Returns a stateful value, and a function to update it.
* If your update function returns the exact same value as the current state, the subsequent rerender will be skipped completely.

```text
const [state, setState] = useState(initialState);
```

```text
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

```text
const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation(props);
  return initialState;
});
```

* **Lazy initial state**:The initialState argument is the state used during the initial render. In subsequent renders, it is disregarded. If the initial state is the result of an expensive computation, you may provide a function instead, which will be executed only on the initial render





### useEffect\(\):

*  The function passed to useEffect will run after the render is committed to the screen
*   **props and state that change over time and that are used by the effect**.

#### cleaning up an effect:

```text
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
*   Additionally, if a component renders multiple times \(as they typically do\), the **previous effect is cleaned up before executing the next effect**
*  the function passed to `useEffect` fires after **layout and paint,** during a deferred event, not all effects can be deferred. For example, a DOM mutation that is visible to the user must fire synchronously before the next paint so that the user does not perceive a visual inconsistency.
* Although `useEffect` is deferred until after the browser has painted, it’s guaranteed to fire before any new renders. React will always flush a previous render’s effects before starting a new update.

#### **Conditionally firing an effect**

```text
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

#### useContext\(\):

```text
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

### useReducer\(\):

{% embed url="https://codesandbox.io/s/react-usereducer-redux-forked-m6wu4?file=/src/index.js" %}

```text
const [state, dispatch] = useReducer(reducer, initialState);
```

*  Redux 的核心概念是，组件发出 action 与状态管理器通信。状态管理器收到 action 以后，使用 Reducer 函数算出新的状态， Accepts a reducer of type `(state, action) => newState`, and returns the current state paired with a `dispatch` method. 

### useCallback\(\):

```text
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

* **useCallback is used to memoize functions** 
* 该回调函数仅在某个依赖项改变时才会更新
*  That’s when `useCallback(callbackFun, deps)` is helpful: given the same dependency values `deps`, the hook returns \(aka memoizes\) the function instance between renderings:

{% embed url="https://codesandbox.io/s/react-usecallback-with-problem-ckzv4?file=/src/index.js" %}



* 每次点击 Btn 剩余 的 不相干的东西也会 render一遍
*  In React, whenever a component re-renders, a new instance of the function in it gets generated. Therefore, every time `App` renders, `add`, `increase` and `decrease` are re-created. So their references now points to different functions in memory

![](.gitbook/assets/image%20%28140%29.png)

*  **Solution: useCallBack\(\):** This prevents the unnecessary re-rendering behaviour because it ensures the **same callback function reference** is returned when there is no change in their dependency.
* And use  **`React.memo()`** is a built-in React feature that renders a memoized component and skip unnecessary re-rendering. So each component will only re-render if it detects a change in their props



