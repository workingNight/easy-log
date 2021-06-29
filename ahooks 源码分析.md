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

