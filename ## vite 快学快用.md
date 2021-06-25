## vite 快学快用

【关键词】 **esbuild** 源码 、依赖

区分**依赖、源码**，优化启动时间

Esbuild 使用 Go 编写，并且比以 JavaScript 编写的打包器预构建依赖快 10-100 倍。

Vite 以 [原生 ESM](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) 方式提供源码，让浏览器承担一部分工作，只在源码请求时才进行转换

 HTTP 头来加速整个页面的重新加载（再次让浏览器为我们做更多事情）：**源码模块**的请求会根据 `304 Not Modified` 进行协商缓存，而**依赖模块**请求则会通过 `Cache-Control: max-age=31536000,immutable` 进行强缓存，因此一旦被缓存它们将不需要再次请求。

开发阶段esbuild打包，生产阶段rollup打包

![image-20210625091314536](C:\Users\exbowen\AppData\Roaming\Typora\typora-user-images\image-20210625091314536.png)

展示了esbuild吊打其他打包~~，go还是强啊，得把go的学习纳入安排



[生态：https://github.com/vitejs/awesome-vite#templates](https://github.com/vitejs/awesome-vite#templates)

模板好像没有next.js。模板、插件都在里面

不过有electron