## hooks要点

核心api

**useState, useEffect，useCallback，useMemo，useRef**



优化： 

1. 父组件手动更新任何state时，子组件都会重新渲染，为了避免这种情况出现，我们用memo将函数组件包裹，达到class组件的pureComponent的效果。

2. **子组件用到了容器组件的某个引用类型的变量或者函数，那么当容器内部的state更新之后，这些变量和函数都会重新赋值，这样就会导致即使子组件使用了memo包裹也还是会重新渲染**，那么这个时候我们就需要使用**useMemo**和**useCallback**了。
3. **useMemo**可以帮我们将变量缓存起来，**useCallback**可以缓存回调函数，通过配置依赖项数组来决定是否更新



### 快速

const [state, setState] = useState(initialState);

const [state, dispatch] = useReducer(reducer, initialState);



### 封装工具Hooks

useToggle开关

useArray简化数组状态操作等



### hooks模拟生命周期

useEffect(() => {

​	fn()  

return () => {

​	fn()    进行卸载等操作。

}

}, [])   //第二个参数设置为[]， 表示不必对任何数据，只在首次渲染时调用。



### 自定义事件封装





### 补充

React.FC 是函数式组件，是在TypeScript使用的一个泛型，FC就是**FunctionComponent**的缩写，事实上 React.FC 可以写成 React.FunctionComponent。使用

`export const ThemeProvider: FC<{ theme: object }> = props => {}`



### 随笔

**当我不再透过熟悉的class生命周期方法去窥视`useEffect` 这个Hook的时候，我才得以融会贯通**

和`componentDidMount`不一样，`useEffect`会*捕获* props和state。

追求状态同步，而不是拘泥于生命周期事件



## 快简

const [ age , setAge ] = useState(18)

```
 useEffect(()=>{
        console.log(`useEffect=>You clicked ${count} times`)

//解绑
        return ()=>{
            console.log('====================')
        }
    },[count])
 //如果是[]就可以像类似的 didMount和WillUnmount
 
 //createContext 创建、提供、接受
 const CountContext = createContext()
  <CountContext.Provider value={count}>
            </CountContext.Provider>
 const count = useContext(CountContext)  //一句话就可以得到count
 <CountContext.Provider value={count}>
    <Counter />
</CountContext.Provider>

 const [ count , dispatch ] =useReducer((state,action)=>{
        switch(action){
            case 'add':
                return state+1
            case 'sub':
                return state-1
            default:
                return state
        }
    },0)
  
  
  可以用useContext 和 useReducer代替Redux一些小的操作。

```

onst actionXiaohong = useMemo(()=>changeXiaohong(name),[name]) 

const inputEl = useRef(null)

**`useEffect` 做了什么？** 通过使用这个 Hook，你可以告诉 React 组件需要在渲染后执行某些操作。React 会保存你传递的函数（我们将它称之为 “effect”），并且在执行 DOM 更新之后调用它。在这个 effect 中，我们设置了 document 的 title 属性，不过我们也可以执行数据获取或调用其他命令式的 API。

### 通过跳过 Effect 进行性能优化



自定义hook,将组件逻辑提取到可重用的函数中





useMemo    返回一个memoized值

useCallback 返回一个memoized回调函数



### `useLayoutEffect` 

在dom变更之后同步调用effect的情况，建议用useEffect只有当它出问题的时候再尝试使用 `useLayoutEffect`。

获取上一轮的props或state





```
function Example({ someProp }) {
  function doSomething() {
    console.log(someProp);  }

  useEffect(() => {
    doSomething();
  }, []); // 🔴 这样不安全（它调用的 `doSomething` 函数使用了 `someProp`）}
  function Example({ someProp }) {
  useEffect(() => {
    function doSomething() {
      console.log(someProp);
    }

    doSomething();
  }, [someProp]); // ✅ 安全（我们的 effect 仅用到了 `someProp`）
}
useEffect(() => {
  function doSomething() {
    console.log('hello');
  }

  doSomething();
}, []); // ✅ 在这个例子中是安全的，因为我们没有用到组件作用域中的 *任何* 值
我们要确定没有用到组件作用域中的任何值
```

useMemo可以跳过一些昂贵的渲染。

```
const child1 = useMemo(() => <Child1 a={a} />, [a]);
```

hook调用不能被放在循环中，你可以为列表项抽取一个单独的组件



惰性： 确保一个对象只创建一次

为避免重新创建被忽略的初始 state，我们可以传一个 **函数** 给 `useState`：

```
function Table(props) {
// ⚠️ createRows() 每次渲染都会被调用
  const [rows, setRows] = useState(createRows(props.count));
  // ✅ createRows() 只会被调用一次
  const [rows, setRows] = useState(() => createRows(props.count));
  // ...
}
```

需要用 [`useCallback`](https://react.docschina.org/docs/hooks-reference.html#usecallback) 记住一个回调

useMemo， 将一些昂贵运算需要依赖的值，让更新更为精准，避免无意义的昂贵计算

[`useCallback`](https://react.docschina.org/docs/hooks-reference.html#usecallback) 返回的是缓存的函数。场景：父组件其中包含子组件，子组件接受一个函数作为props,通常父组件的更新会导致子组件的更新，把这个useCallback包含的函数作为props传递给子组件，子组件就能避免不必要的更新。



用ref来捕获上一次的状态。



###  用ref做缓存