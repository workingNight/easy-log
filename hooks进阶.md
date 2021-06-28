## hooks进阶



## 文章摘要

1. 时序控制，如果 `userId` 连续变化了 5 次，发送了 5 个网络请求，我们要保证总是最后一次网络请求有效。也就是经常说的的“竞态处理”或者“时序控制”。

   >1. 组件加载
   >2. 发起网络请求
   >3. 组件卸载
   >4. 网络请求请求成功，触发 `setState`
   >
   >看出问题了吗？组件已经卸载了，还去 `setState`，造成了内存溢出。
   >
   >`useEffect` *默认*就会处理，它会在调用一个新的 effect 之前对前一个 effect 进行清理

2. 常见逻辑

   >- 网络请求
   >- loading
   >- `userId` 变化重新发起请求
   >- 竞态处理
   >- 组件卸载放弃网络请求
   >

3. Hooks 最重要的能力就是逻辑复用

4. useRef()是干什么的

5. 通过使用 Hook，你可以把组件内相关的副作用组织在一起（例如创建订阅及取消订阅），而不要把它们拆分到不同的生命周期函数里。ook 是一种复用*状态逻辑*的方式，它不复用 state 本身。事实上 Hook 的每次*调用*都有一个完全独立的 state —— 因此你可以在单个组件中多次调用同一个自定义 Hook。

6. 两个函数间共享逻辑，把它提取到第三个函数中。

   ```
     const [recipientID, setRecipientID] = useState(1);
     const isRecipientOnline = useFriendStatus(recipientID);
     接受state为usehooks的参数，复用useHooks的逻辑
   ```

7. const [todos, dispatch] = useReducer(todosReducer, []);  复合到dispatch中了

8. ```
   被更新的state需要基于之前的state
   setCount(prevCount => prevCount - 1)}
   更新对象
   setState(prevState => {
     // 也可以使用 Object.assign
     return {...prevState, ...updatedValues};
   });
   ```

9. React 使用 [`Object.is` 比较算法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is#Description) 来比较 state。   Object.is(null,null)true,   对于基础类型比较值，引用类型判断引用地址是否相同

10. [`useLayoutEffect`](https://react.docschina.org/docs/hooks-reference.html#uselayouteffect)

11. useContext的使用

    - 父级先调用  React.createContext()创建一个context对象，然后用这个对象的Provider把父组件包裹起来，然后里面的子组件就可以通过useContext（context对象）就能拿到state对象了。

12. 惰性useReducer

      const [state, dispatch] = useReducer(reducer, initialCount, init);   第三个参数是函数，第二个参数是函数的参数

    在reset的场景下比较有用。

13. `useCallback(fn, deps)` 相当于 `useMemo(() => fn, deps)`。

    useCallback相当给函数加一层依赖，返回一个memoized函数，如果依赖没变函数就不动。可以联想类比useEffect依赖。

    区别是一个返回函数，一个返回值。useMemo可以封装一个需要昂贵开销计算的函数，避免每次渲染都进行高开销的计算

14. **useRef**

返回一个可变的ref对象，ref对象在组件的整个生命周期内保持不变

.current属性被初始化为传入的参数(initialValue)   

```
 .current属性可以被覆盖
<input ref={inputEl} type="text" />
```

15. useRef会在每次渲染时返回同一个ref对象，但是当ref对象内容发生变化时，useRef并不会通知你，.current属性不会引发组件重新渲染。实例变量， ref对象是一个current属性可变并且可以容纳任意值的通用容器。refs 就像是一个 class 的实例变量。**当 ref 是一个对象时它并不会把当前 ref 的值的 *变化* 通知到我们**

16. useImperativeHandle + forwardRef

    > Imperative至关重要的；紧要的；命令的；强制的；专横的
    >
    > useImperativeHandle 可以暴露一些命令式的方法给父组件

17. useLayoutEffect 在所有的dom变更之后同步调用effect

18. useState的技巧

    ```
     // 展开 「...state」 以确保我们没有 「丢失」 width 和 height
     setState(state => ({ ...state, left: e.pageX, top: e.pageY }));
    ```

19. 获取上一轮的props或state,可以通过ref来实现，如果你刻意地想要从某些异步回调中读取 *最新的* state，你可以用 [一个 ref](https://react.docschina.org/docs/hooks-faq.html#is-there-something-like-instance-variables) 来保存它，修改它，并从中读取。

20. 强制更新的方法

    ```
     const [ignored, forceUpdate] = useReducer(x => x + 1, 0);
      function handleClick() {
        forceUpdate();
      }
    ```

21. 测量dom节点

22. useEffect，一个依赖对应一个业务逻辑。

23. ```
    function ProductPage({ productId }) {
      // ✅ 用 useCallback 包裹以避免随渲染发生改变
      const fetchProduct = useCallback(() => {
        // ... Does something with productId ...
      }, [productId]); // ✅ useCallback 的所有依赖都被指定了
    
      return <ProductDetails fetchProduct={fetchProduct} />;
    }
    
    function ProductDetails({ fetchProduct }) {
      useEffect(() => {
        fetchProduct();
      }, [fetchProduct]); // ✅ useEffect 的所有依赖都被指定了
      // ...
    }
    useCallback的一个使用场景，封装好了之后，传给子组件，子组件可以依赖上，在useEffect里面使用
    ```

24. setState的函数式更新，函数将接受先前的state,并返回一个更新后的值，setCount(prevCount => prevCount - 1)

25. useRef可以实现类似this的功能

26. React.memo包裹一个组件来对它的props进行浅比较

27. 惰性初始化一个组件

    ```
    可以允许跳过一次子节点的昂贵的重新渲染
    const child1 = useMemo(() => <Child1 a={a} />, [a]);
    ```

28. 惰性单例

    ```
    function Image(props) {
      const ref = useRef(null);
    
      // ✅ IntersectionObserver 只会被惰性创建一次
      function getObserver() {
        if (ref.current === null) {
          ref.current = new IntersectionObserver(onIntersect);
        }
        return ref.current;
      }
    
      // 当你需要时，调用 getObserver()
      // ...
    }
    ```

29. useContext和useReducer经常会混掉需要常看

30. 实例变量，实例变量由对象私有，某一个对象将其值改变，不影响其他对象，实例只改变自身

31. 一个优化，一个场景transition组件，我们可以立即退出第一次渲染并用更新后的state重新运行组件以避免耗费太多性能

32. 用数组来解决hooks的复用问题，共享同一个memoizedState，共享同一个顺序。

    React 是如何把对 Hook 的调用和组件联系起来的？每个组件内部都有一个记忆的单元格，当你用 `useState()` 调用一个 Hook 的时候，它会读取当前的单元格（或在首次渲染时将其初始化），然后把指针移动到下一个

### ahooks学习

1. hooks的参数和返回值的规范值得学习

2. 按需加载ahook/es目录， es module的tree shaking

3. 防抖   debouncedValue 在值输出多少秒后才开始变化

4. 一些参数命名论

   -params\ options \ result输出 
   

5.useUnmountedRef 竞态处理，当组件卸载后放弃请求，或放弃更新状态，用于避免因组件卸载后更新状态而导致的内存泄漏

6. 一个只在依赖更新时执行的useEffect hooks,也就是忽略首次渲染，并且只在依赖更新时运行

7. 状态变化历史，在状态变化历史中穿梭-前进后退

8. uesEventTarget  受控组件，表单控件

9. 获取响应式信息，在组件中调用useResponsive来获取并订阅浏览器窗口大小，然后再进行适配。

   >bootstrap 的响应式配置  xs:0  sm:576 md 768  lg:992 xl 1200

10. 页面滚动状态、监听节点尺寸

11. 监听用户选择的文本，可以做划词翻译

12. useTitle 页面标题， 还有设置页面fav.ico的hooks

13. useMemo不能保证memo的值一定不会被重计算，你可以使用 `useCreation` 创建一些常量，复杂常量的创建，useRef容易出现潜在的性能隐患。const a = useRef(new Subject()) // 每次重渲染，都会执行实例化 Subject 的过程，即便这个实例立刻就被扔掉。要通过单例模式来执行实例化subject的过程

14. 对于**子组件**通知**父组件**的情况，我们仍然推荐直接使用 `props` 传递一个 `onEvent` 函数。而对于**父组件**通知**子组件**的情况，可以使用 `forwardRef` 获取子组件的 ref ，再进行子组件的方法调用。 `useEventEmitter` 适合的是在**距离较远**的组件之间进行事件通知，或是在**多个**组件之间共享事件通知。

15. useEventEmitter和redux的区别，  

16.  useLockFn 添加竞态锁，防止并发执行

17. usePersistFn保证函数的地址永远不会发生变化

18.useReactive

    提供一种数据响应式的操作体验,定义数据状态不需要写`useState` , 直接修改属性即可刷新视图。

`useReactive`产生可操作的代理对象一直都是同一个引用，`useEffect` , `useMemo` ,`useCallback` ,`子组件属性传递` 等如果依赖的是这个代理对象是**不会**引起重新执行。所有这个只是作为单纯的展示state，不能作为交互依赖

19.ts是否滥用。  函数的参数校验，函数的输出校验

20. 创建一个自定义长度的大数组    Array.from(Array(9999).keys())





### 源码解读

**ahooks    useThrottleFn**

```
import throttle from 'lodash.throttle';
import { useRef } from 'react';
import useCreation from '../useCreation';
import { ThrottleOptions } from '../useThrottle/throttleOptions';
import useUnmount from '../useUnmount';

type Fn = (...args: any) => any;

function useThrottleFn<T extends Fn>(fn: T, options?: ThrottleOptions) {
  const fnRef = useRef<T>(fn);
  fnRef.current = fn;

  const wait = options?.wait ?? 1000;

  const throttled = useCreation(
    () =>
      throttle<T>(
        ((...args: any[]) => {
          return fnRef.current(...args);
        }) as T,
        wait,
        options,
      ),
    [],
  );

  useUnmount(() => {
    throttled.cancel();
  });

  return {
    run: (throttled as unknown) as T,
    cancel: throttled.cancel,
    flush: throttled.flush,
  };
}

export default useThrottleFn;
```

**preflight,  post提交预请求？**