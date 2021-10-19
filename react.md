# React

react技术栈,推荐阅读的源码是react,react-router,redux,react-redux,axios。

[卡颂的react技术揭秘](https://react.iamkasong.com)



[若川的源码系列](https://juejin.cn/user/1415826704971918/posts)



{% embed url="https://react.iamkasong.com/diff/prepare.html#diff%E6%98%AF%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E7%9A%84" %}

{% embed url="https://yuchengkai.cn/react/2019-07-29.html#setstate-%E8%83%8C%E5%90%8E%E7%9A%84%E6%89%B9%E9%87%8F%E6%9B%B4%E6%96%B0%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0" %}

{% embed url="https://juejin.cn/post/6844903830887366670#heading-24" %}

{% embed url="https://github.com/sl1673495/blogs/issues/52" %}

{% embed url="https://juejin.cn/post/6844904103504527374#heading-20" %}





## JSX

### &#x20;`createElement `&#x20;

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

![](<.gitbook/assets/image (139).png>)



## Hooks:

### The main rules of hooks:

* Don’t call Hooks inside loops, conditions, or nested functions
* Only Call Hooks from React Functions



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



### useEffect():

* &#x20;The function passed to useEffect will run after the render is committed to the screen
* &#x20;** props and state that change over time and that are used by the effect**.

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
* &#x20;the function passed to `useEffect` fires after** layout and paint,** during a deferred event, not all effects can be deferred. For example, a DOM mutation that is visible to the user must fire synchronously before the next paint so that the user does not perceive a visual inconsistency.
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

### useCallback():

```
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

* **useCallback is used to memoize functions **
* 该回调函数仅在某个依赖项改变时才会更新
* &#x20;That’s when `useCallback(callbackFun, deps)` is helpful: given the same dependency values `deps`, the hook returns (aka memoizes) the function instance between renderings:

{% embed url="https://codesandbox.io/s/react-usecallback-with-problem-ckzv4?file=/src/index.js" %}



* 每次点击 Btn 剩余 的 不相干的东西也会 render一遍
* &#x20;In React, whenever a component re-renders, a new instance of the function in it gets generated. Therefore, every time `App` renders, `add`, `increase` and `decrease` are re-created. So their references now points to different functions in memory

![](<.gitbook/assets/image (140).png>)

* ** Solution: useCallBack():** This prevents the unnecessary re-rendering behaviour because it ensures the **same callback function reference** is returned when there is no change in their dependency.
* And use  **`React.memo()`** is a built-in React feature that renders a memoized component and skip unnecessary re-rendering. So each component will only re-render if it detects a change in their props



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

#### _**2. **Each Render Has Its Own Event Handlers_

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

#### _4. How does clean up work? _

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

#### _5. Honest about dependencies _

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
  * **Solution 1: ** Move functions inside effects,  **We no longer have to think about the “transitive dependencies.**
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

* &#x20;This class method reads from `this.props.user`. Props are immutable in React so they can never change. **However, `this` **_**is**_**, and has always been, mutable.**
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

* Functional component are much **easier to read and test **because they are plain JavaScript functions without state or lifecycle-hooks
* You end up with **less code**
* They help you to use best practices. It will get easier to separate container and presentational components because you need to think more about your component’s state if you don’t have access to setState() in your component



## Understanding React's key prop

{% embed url="https://zhuanlan.zhihu.com/p/112917118" %}



* &#x20;React's `key` prop gives you the ability to control component instances. **Each time React renders your components, it's calling your functions to retrieve the new React elements that it uses to update the DOM**. If you return the same element types, it keeps those components/DOM nodes around, even if all\* the props changed.
* &#x20;This allows you to return the exact same element type, but force React to unmount the previous instance, and mount a new one. This means that all state that had existed in the component at the time is completely removed and the component is "reinitialized" for all intents and purposes. For components, this means that React will run cleanup on effects (or `componentWillUnmount`), then it will run state initializers (or the `constructor`) and effect callbacks (or `componentDidMount`).
* **设置了key能够提升渲染效率**

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

Virtual DOM is an in-memory representation of real DOM**. **It is a lightweight JavaScript object which is a copy of Real DOM.

### Updating virtual DOM in ReactJS is faster because ReactJS uses:

* Efficient diff algorithm
* Batched update operations
* Efficient update of subtree only
* Uses observable instead of dirty checking to detect the change

Whenever setState() method is called on any component, ReactJS makes that component dirty and re-renders it.Whenever setState() method is called, ReactJS creates the whole Virtual DOM from scratch. Creating a whole tree is very fast so it does not affect the performance. At any given time, ReactJS maintains two virtual DOM, one with the updated state Virtual DOM and other with the previous state Virtual DOM.

ReactJS using diff algorithm compares both the Virtual DOM to find the minimum number of steps to update the Real DOM

Finding the minimum number of modifications between two trees have complexity in the order of** O(n^3)**. But react uses a heuristic approach with some assumptions which makes the problems to have complexity in the order of** O(n)**

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



### How to find  the difference in both the Virtual DOM’s:

1. **Re-render all the children if parent state has changed**
2. **Breadth First Search**
3. **Reconciliation: **It is the process to determine which parts of the Real DOM need to be updated. It follows the below steps:
   1. Two elements of different types will produce different trees.
   2. The developer can hint at which child elements may be stable across different renders with a key prop.

