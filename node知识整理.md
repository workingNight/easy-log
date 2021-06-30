## node知识整理计划

计划：看一下阮一峰的koa教程，再过一遍廖雪峰的。过完之后，再去看node，分析node模块

​			最后再egg.js





## record1

1. 三行代码启动服务

   ```
   const Koa = require('koa');
   const app = new Koa();
   
   app.listen(3000);
   ```

2. context 对象

   ```javascript
   const main = ctx => {
     ctx.response.body = 'Hello World';
   };
   app.use(main)
   ```

   ctx.response.type

   ctx.response.body

   ctx.cookies.set

   ctx.cookies.get

3. 路由

   ctx.request.path

   可以用封装好的koa-route模块   route.get('/',main)  mian是定义的中间件

   route.get  和 app.use()  进行类比

4. 静态资源

   ```javascript
   const serve = require('koa-static');
   ```

5. 重定向

   ```javascript
   route.get('/redirect', redirect)  专门处理重定向的中间件
   ```

6. 中间件概念

   洋葱模型，从外到内，再由内到外

   异步中间件  async

   中间件的合成  koa-componse

7. 错误处理

   ctx.throw

   try .. catch   我们可以写一个专门的错误处理中间件放在最外层，负责所有的中间件错误处理

   错误的监听，app.on('err',() => {...})

   如果错误被try catch捕获就不会触发err事件，必须调用app.emit()手动释放error事件





## record2

这一部分主要是廖雪峰相关。

commonjs 模块，  我们把东西通过 module.exports ={ }  这个对象进行暴露，然后再通过require进行引入。

关于cmj模块和es6模块的对比常常忘记，cmj模块应该是整体拷贝一份，es6模式是拷贝一份引用

### node基础模块

1. 内置的fs模块  默认是异步读取，readFile()   同步读取readFileSync()

2. readFile('', 'utf-8',function(err,ctx) {})  如果不添加编码 ，就会变成buffer。（`Buffer`对象就是一个包含零个或任意个字节的数组（注意和Array不同）

3. `writeFile()`的参数依次为文件名、数据和回调函数

4. `fs.stat()`，它返回一个`Stat`对象，能告诉我们文件或目录的详细信息

   > stat.isFile()
   > stat.isDirectory())
   > stat.size
   > stat.birthtime
   > stat.mtime

5. stream 流的概念

   data事件可能会有多次，每次的chunk是流的一部分数据

   ```
   // 打开一个流:
   var rs = fs.createReadStream('sample.txt', 'utf-8');
   
   rs.on('data', function (chunk) {
       console.log('DATA:')
       console.log(chunk);
   });
   
   rs.on('end', function () {
       console.log('END');
   });
   
   rs.on('error', function (err) {
       console.log('ERROR: ' + err);
   });
   ```

   写流和读流  ，pipe接水管。fs.createReadStream(filepath).pipe(response);

6. 原生http服务器

   ```
   server = http.createServer(function (request, response) {
   response.writeHead(200,{...})
   response.end('')
   })
   server.listen(8080)
   ```

7. path 模块，可以方便的构造目录

   path.resolve('.')可以解析当前目录，转化为绝对路径

   erver = http.createServer(function (request, response) {
   
8. crypto模块，提供通用的加密和哈希算法，是用c++实现的这些算法

   1. md5  &  sha1   sha256   sha512
3.  hmac需要一个密钥



跳跳跳，直接到orm框架





