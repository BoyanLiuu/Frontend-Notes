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

### The main rules of hooks:

* Don’t call Hooks inside loops, conditions, or nested functions
* Only Call Hooks from React Functions



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

### How does useState\(\) work internally:

{% embed url="https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e" %}



####  Initialisation:

![](.gitbook/assets/image%20%28143%29.png)

* Create two empty arrays: \`setters\` and \`state\`
* Set a cursor to 0

#### First render

* Run the component function for the first time.
*  Each `useState()` call, when first run, pushes a setter function \(bound to a cursor position\) onto the `setters` array and then pushes some state on to the `state` array.

![](.gitbook/assets/image%20%28147%29.png)

Each subsequent render the cursor is reset and those values are just read from each array.

### 



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

#### 每次 Render 都有自己的事件处理

```text
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

*  在 `log` 函数执行的那个 Render 过程里，`temp` 的值可以看作常量 `5`，**执行 `setTemp(3)` 时会交由一个全新的 Render 渲染**，所以不会执行 `log` 函数。**而 3 秒后执行的内容是由 `temp` 为 `5` 的那个 Render 发出的**，所以结果自然为 `5`。

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



### useMemo\(\):

{% embed url="https://codesandbox.io/s/react-usememo-voelc?file=/src/index.js:0-1222" %}





```text
//基本样子
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

* _useMemo is used to memoize values._
* Pass a “create” function and an array of dependencies
* If no array is provided, a new value will be computed on every render.
*  `useMemo` invokes the provided function and caches its result.
*  `useMemo` can cache a function value too. In other words, it is a generalised version of `useCallback` and can replace it as in the following example

![](.gitbook/assets/image%20%28141%29.png)

```text
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

* 这里的  filteredUsers function 在我们每次输入的时候 都会 run， 因为我们每次输入时候 都会 setText\(\) 然后导致App 重新 render
*   However, if we would deal with a large set of data in this array and run the filter's callback function for every keystroke, we would maybe slow down the application. Therefore, you can use React's useMemo Hook to **memoize a functions return value\(s\)** and to run a function only if its dependencies \(here `search`\) have changed:

![](.gitbook/assets/image%20%28142%29.png)

*  Now, this function is only executed once the `search` state changes. It doesn't run if the `text` state changes, because that's not a dependency for this filter function and thus not a dependency in the dependency array for the useMemo hook

### useRef

```text
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

*  The “ref” object is a generic container whose `current` property is mutable and can hold any value, similar to an instance property on a class
* 普遍操作，用来操作dom
*  **可以认为 `ref` 在所有 Render 过程中保持着唯一引用，因此所有对 `ref` 的赋值或取值，拿到的都只有一个最终状态**，而不会在每个 Render 间存在隔离。

### 自定义Hooks

```text
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



React as a UI Runtime

{% embed url="https://overreacted.io/react-as-a-ui-runtime/" %}



## How does React tell a Class from a function?

{% embed url="https://overreacted.io/how-does-react-tell-a-class-from-a-function/" %}

*  React needs to call classes \(including Babel output\) _with_ `new` but it needs to call regular functions or arrow functions \(including Babel output\) _without_ `new`. And there is no reliable way to distinguish them.
* We just detect only **React.Component** descendants

```text
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

###  1: **Function components capture the rendered values.**

```text
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

![Solution 2](.gitbook/assets/image%20%28144%29.png)

![Solution 3 ](.gitbook/assets/image%20%28145%29.png)

*  This class method reads from `this.props.user`. Props are immutable in React so they can never change. **However, `this`** _**is**_**, and has always been, mutable.**
*  So if our component re-renders while the request is in flight, `this.props` will change, **the event handlers are a part of the render result — just like the visual output**. Our event handlers “belong” to a particular render with particular props and state.
* How to fix this?
  * Solution 1: Use functional components
  * Solution 2: Another way to do it would be to read this.props early during the event, and then explicitly pass them through into the timeout completion handler:
    * Disadvantages: make the code more verbose and error-prone with time.
  * Solution 3 Another way is to use closure , we captured props at the time of render

### 2. React lifecycle methods \(for example, componentDidMount\) cannot be used in functional components.

### 3. There is no render method used in functional components.

### 4. Functional Component or Stateless Component:

* It is simple javascript functions that simply returns html UI
* It is also called “stateless” components because they simply accept data and display them in some form that is they are mainly responsible for rendering UI.
* These can be typically defined using arrow functions but can also be created with the regular function keyword.



### Why you should use functional components at all? 

* Functional component are much **easier to read and test** because they are plain JavaScript functions without state or lifecycle-hooks
* You end up with **less code**
* **They help you to use best practices. It will get easier to separate container and presentational components because you need to think more about your component’s state if you don’t have access to setState\(\) in your component**
* \*\*\*\*



