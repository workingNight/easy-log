# 读clean-code-javaScript

[链接：github/clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)







## 提取一些平时没注意的

1. 不要把标志当做函数参数然然后来进行判断然后进行两次判断

   好的写法是直接写2个函数

   **Bad:**

   ```
   function createFile(name, temp) {
     if (temp) {
       fs.create(`./temp/${name}`);
     } else {
       fs.create(name);
     }
   }
   ```
   **Good:**

    ```
    function createFile(name) {
      fs.create(name);
    }
   
    function createTempFile(name) {
      createFile(`./temp/${name}`);
    }
    ```
   
2. 我们操作全局对象的时候，尽量以入参的形式进行修改，而不是直接在函数里面引用并操作。

   - [ ] ```
     function splitIntoFirstAndLastName() {
       name = name.split(" ");
     }
     ```

   - [x] ```
     function splitIntoFirstAndLastName(name) {
       return name.split(" ");
     }
     ```

3. 不用push而是用拓展符去重新生成新的数组

    - [ ] ```
      const addItemToCart = (cart, item) => {
        cart.push({ item, date: Date.now() });
      };
      ```

    - [x] ```
      const addItemToCart = (cart, item) => {
        return [...cart, { item, date: Date.now() }];
      };
      ```
4. 用类里面的静态方法去取代全局函数，避免去命名冲突
5. 用reduce，map, 等函数去代替for循环。
6. 把状态判断封装成函数
7. 避免双重否定  例如。if (!isDOMNodeNotPresent(node))
8. 函数精神，一个函数就负责一件事情。Remember, just do one thing.
9. 避免类型检查，视乎你被js这种自由的类型所伤害时，类型检查很有必要，但是很多时候我们有很多方式去避免他，我们首先可以考虑的是使用一致的api
> consistent 前后一致的；一贯的；坚持的；一致的；连贯的；符合的；协调的；和谐的；不矛盾的
10. 避免过度优化

```
//我们常常在算法题里面看到这样的写法
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

现代浏览器已经优化了，不需要再去那样写了

11. 不要写太多注释，只写关键的就行
12. 不要写日志注释，git log，git len就够了
13. 避免段分割标记，一些注释





## 几个面对对象原则

1. 单职责优先
2. 开放封闭原则
3. 利斯科夫他替代原则， 子类可以替代父类
4. 接口分离原则
5. 依赖倒置原则
