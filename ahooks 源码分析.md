## ahooks 源码分析

目标: 能够写出一些基本常用的hooks



useLockFn:  返回一个添加了竞态锁的函数

```
import { useRef, useCallback } from 'react';

function useLockFn<P extends any[] = any[], V extends any = any>(fn: (...args: P) => Promise<V>) {
  const lockRef = useRef(false);

  return useCallback(
    async (...args: P) => {
      if (lockRef.current) return;    
      lockRef.current = true;
      try {
        const ret = await fn(...args);
        lockRef.current = false;
        return ret;
      } catch (e) {
        lockRef.current = false;
        throw e;
      }
    },
    [fn],
  );
}

export default useLockFn;
```

分析： 

useCallback：   

把内联回调函数及依赖项数组作为参数传入 `useCallback`它将返回该回调函数的 memoized 版本，该回调函数仅在某个依赖项改变时才会更新。

```
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
当依赖改变时才会更新
```



https://github.com/alibaba/hooks/blob/master/README.zh-CN.md)





## 一些常用hooks用法记录



### useRequest

总览暴露对象和配置项

```javascript
const {
  data,
  error,
  loading,
  run,   //配合手动触发
  params,
  cancel,
  refresh,
  mutate,
  fetches,   //存储了多个请求的状态
} = useRequest(service, {
  manual,
  initialData,
  refreshDeps,
  onSuccess,
  onError,
  formatResult,
  cacheKey,     请求唯一标识。如果设置了 cacheKey，我们会缓存data , error , params , loading
    					Symbol('xxxx')?   或许源码在底层会接受这个字符串然后自己弄成Symbol
  loadingDelay,
  defaultParams,
  pollingInterval,
  pollingWhenHidden,
  fetchKey,     //可以实现多个请求并行，
  refreshOnWindowFocus,
  focusTimespan,
  debounceInterval,
  throttleInterval,
  ready,
  throwOnError,
});
```

```
// 有解构的写法，也有通过句柄去使用的写法
比如  const  reqhand  = useRequest(xxx)
reqhand.run()
reqhand.data
reqhand.loading
如果一个组件中有多个useRequest，建议用句柄的写法去使用，不会造成命名冲突

const { data, error, loading } = useRequest(getUsername);

//手动触发  run(state) 会传值给params
const { loading, run } = useRequest(changeUsername, {
    manual: true,
    onSuccess: (result, params) => {
      if (result.success) {
        setState('');
        message.success(`The username was changed to "${params[0]}" !`);
      }
    },
  });
  
  
//配置轮询
const { data, loading, run, cancel } = useRequest(getUsername, {
    pollingInterval: 1000,
    pollingWhenHidden: false,
    manual: true   //第一次run之后才开始轮询
});

//请求分类，每一类请求都有独立的状态
 const { run, fetches } = useRequest(deleteUser, {
    manual: true,
    //通过id进行了分类
    fetchKey: (id) => id,
    onSuccess: (result, params) => {
      if (result.success) {
        message.success(`Disabled user ${params[0]}`);
      }
    },
  });
//这里暴露出来的fetches对象拥有独立的属性
{fetches[user.id]?.loading ? 'loading' : `delete ${user.username}`}  


//串行依赖请求
const userIdRequest = useRequest(getUserId);
const usernameRequest = useRequest(() => getUsername(userIdRequest.data), {
    ready: !!userIdRequest.data,
});


//开启请求防抖
//节流是一段时间内只执行一次，防抖是如果在一段时间内没有触发再执行，如果一段时间内一直触发就一直不执行
const { data, loading, run } = useRequest(getEmail, {
	throttleInterval: 500,
    debounceInterval: 500,
    manual: true,
});

//缓存
//描述：会将当前的请求结果缓存起来，下次组件初始化的时候有缓存数据我们先展示缓存数据，然后在背后发送
//新的请求，也就是SWR的能力。这个key的名字似乎不是很重要？
const { data, loading } = useRequest(getArticle, {
    cacheKey: 'article',
    cacheTime   设置缓存数据清理时间
    staleTime   设置数据保存新鲜时间，也就是说到一定时间自动请求
});
//预请求，同一个cacheKey的请求是全局共享的，可以提前加载数据
onMouseEnter={() => run()}   一个例子，在鼠标指针移动到上面就执行

//聚焦屏幕重新请求,也就是你点了别处再回到这个页面的时候会重新刷新
refreshOnWindowFocus： true
focusTimespan:   5000,   刷新间隔默认是5s

// 突变 我们可以通过mutate直接修改data
// 场景，一些需要快速展示的场景，且数据的变化简单可预测，而且你很肯定
 const { data, mutate } = useRequest(getUsername, {
    onSuccess: (result) => {
      setState(result);
    },
 });
 
//loadingDelay  可以延迟loading变成true的时间，防止闪烁

//超棒的语法糖，少写
useEffect(() => {
  run();
}, [userId]);
我们直接在options里面配置就行
refreshDeps: [userId],
```

基于基础的useRequest我们还可以进一步封装，实现更高级的定制需求。

集成请求库：如果接受的参数不是promise的时候就调用fetch来发送网络请求

分页

>  配置里面 paginated = true
>
> service 返回的数据结构必须为 `{list: Item[], total: number}` ，
>
> 如果不满足，可以通过 `options.formatResult` 转换一次,对字段的处理。
>
> 会额外返回 `pagination` 字段，包含所有分页信息，及操作分页的函数。
>
> ```
> 实践：
> function getUserList(params: {
>   current: number;
>   pageSize: number;
>   gender?: string;
>   //我们可以在这里进行对数据的约束，强制要求返回的需要是list,我们在resolve的时候修改下字段名
> })Promise<{ total: number; list: UserListItem[] }> {}
>  const { data, loading, pagination } = useRequest(
>  //这里是接受请求的参数配置
>     ({ current, pageSize }) => getUserList({ current, pageSize }),
>     {
>       paginated: true,
>     },
>   );
>   
>   
>  <Pagination
>         {...(pagination as any)}    //分页信息直接解构
>         showQuickJumper
>         showSizeChanger
>         onShowSizeChange={pagination.onChange}  //内置函数
>         style={{ marginTop: 16, textAlign: 'right' }}
>  />
> ```
>
> 
>
> `refreshDeps` 变化，会重置 `current` 到第一页，并重新发起请求，一般你可以把 pagination 依赖的条件放这里。
>
> 简直太棒了，可以少写太多业务逻辑了。

集成了antd-table

```tsx
 const { tableProps, params, refresh } = useRequest(
    ({ current, pageSize, sorter: s, filters: f }) => {
      const p: any = { current, pageSize };
      if (s?.field && s?.order) {
        p[s.field] = s.order;
      }
      if (f) {
        Object.entries(f).forEach(([filed, value]) => {
          p[filed] = value;
        });
      }
      console.log(p);
      return getUserList(p);
    },
    {
      paginated: true,
      defaultPageSize: 5,
    },
  );
<Table columns={columns} rowKey="id" {...tableProps} />
直接用几十行代码直接接管了table，可以少写太多业务逻辑。

带缓存的分页：
在 cacheKey 场景下， run 的参数 params 是可以缓存的，利用这个特点，我们可以实现 pagination 相关条件的缓存。
const [gender, setGender] = useState<string>(params[1]); 从params里抽离设置，进行表单联动
 useUpdateEffect(() => {
    // reload when gender change
    run(
      {
        current: 1,
        pageSize: 10,
      },
      gender,
    ); //run 的第二个参数
  }, [gender]);
```

加载更多

const containerRef = useRef<HTMLDivElement>(null);  

```tsx
ref: containerRef,
必须设置isNoMore,以便让useRequest知道何时终止
isNoMore: (d) => (d ? d.list.length >= d.total : false),
```







## loose ending

### `React.forwardRef`

返回一个组件，组件将其接受的ref属性转发到组件树下的另一个组件中去。

> 场景：  转发refs到DOM组件;在高阶组件中转发refs

```
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

//使用，当你这样做的时候，ref.current将直接指向<button>实例
// You can now get a ref directly to the DOM button:
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```





