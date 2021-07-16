## 尚硅谷webpack5进阶

react等官方webpack,能够根据需求进行变化



### 初始

create-react-app    

**eject 命令：**  将react-scripts里面的指令打包到外面，暴露webpack出来。   

多了两个文件config  scripts   

 



### config目录

**path**  文件路径

yarn-lock  yarn的缓存工具





### 随笔

代码分割后的文件  contenthash.chunk.js

runtime 文件长期保存的文件。 是否内联runtime文件，可以少发一个请求

优化 splictchunk

环境变量   process.env

bail （ex: 栏杆，防滑杆，保释金）出错就不再执行

```
"scripts" : {
	//调试 虽然不太熟，先写着把 后面如果碰到也混个脸熟
	"test": "node --inspect-brk ./node_modules/webpack/bin/webpack.js"
}
```

const util = require('util');

util.promisify()  把异步方法编程promise风格的异步方法



### 学习方法

1. create-react-app 热然后eject  然后对比官网文档然然后进行对其默认的webpack配置进行解读注释





### loader

npm  i webpack webpack-cli

再写一些webpack配置

loader本质上是一个函数  接受三个参数（content，  map , meta）  主要是content有用

loader 函数，module.exports.pitch  = function () {...}  可以让其loader执行顺序修改

```
module.exports = {
	module: {
		rules: [
			{
				test: /\.js$/,
				//loader: 'loader1'
				use: [
				'loader1',
				'loader2',
				'loader3'
				]
			}
		]
	},
	//配置loader解析规则,webpack5
	resolveLoader: {
		modules: {
			'node_modules',  //默认
			path.resolve(__dirname,'loaders')  //补充
		}
	}
}
```

**同步loader和异步loader**，  推荐使用异步loader

```
const callback = this.async()
setTimeout(() => {
	//第一个参数是报错
	callback(null,content)
}, 1000)
```

**自己配置loader**

webpack.config.js中

```
{
	loader: 'loader3',
	options: {
		name: true
	}
}
```

我们写的loader中

引入一个读取options的库和校验的库,然后进行读取和校验。<br />

```
const {getOptions} = require('loader-utils');
const {validate} = require('schema-utils');
const schema = require('./schema')
```

校验-引入schema.json 配置校验规则



**实战，写一个babel-loader**

1. 校验规则

   ```
   {
   	"type": "object",
   	"properties": {
   		"presets": {
   			"type": "array"
   		}
   	},
   	"addtionalProperties": true
   }
   ```

2. babelLoader.js

   ```
   const {getOptions} = require('loader-utils');
   const {validate} = require('schema-utils');
   const babel = require('@babel/core');
   const util = require('util');
   
   const babelSchema = require('./babelSchema.json')
   
   //bable.transform 用来编译代码的方法
   //一个普通异步方法
   //util.promisify 将普通异步方法转化成基于promise的异步方法
   const transform = util.promisify(babel.transform)
   
   module.exports = function (content, map, meta) {
   	const options = getOptions(thiis) || {};
   	validate(babelSchema, options, {
   		name: 'Babel Loader'
   	});
   	
   	//创建异步
   	const callback = this.async();
   	
   	//使用babel编译代码
   	transform(content, options)
   		.then(({code,map} => callback(null,code,map,meta)))
   		.catch((e) => callback(e))
   }
   ```

   





### plugins 插件

每一个插件都是一个类

compiler模块  主要引擎，继承自Tapable类，用来注册和调用插件

compiltion 钩子  编译

我们需要先搞清tapable  去tapable文档

sync 同步   async异步

tapable.test.js

```
const { SyncHook , SyncBailHook, AsyncParallelHook, AsyncSeriesHook } = require('tapable');

class Lesson {
	constructor() {
		//初始化hooks容器
		this.hooks = {
			go:  new SyncHook(['address'])，
			
			//异步hooks
			//AsyncParallelHook 异步并行， 同理那个就是异步串行
			leave: new AsyncParallelHook(['name', 'age'])
		}
	}
	tap() {
		//往hooks容器中注册事件/添加回调函数 
		this.hooks.go.tap('class0318', (address) => {
			console.log('class0318', address);
		})
		this.hooks.leave.tapAsync('class0318', (name,age, cb) => {
			setTimeout(() => {
				console.log('async',name, age)
				cb()
			}, 1000)
		})
		//另一种异步写法
		//this.hooks.leave.tapPromise('class0318', (name,age) => {
			return new Promise((resolve) => {
				...异步
			})
		})
	}
	start() {
		//触发hooks
		this.hooks.go.call('ce18')
	}
}

const l  = new Lesson()
l.tap()   //先注册
l.start()   //再触发

```

SyncBailHook  // 一旦有返回值就会退出

**小demo**

webpack.config.js 

```
const Plugin1  = require('./plugins/Plugin1')    //引入插件

module.exports = {
	Plugins: [ 
		{
			new Plugin1()    //new 方法会调用插件类中的apply
		}
	]
}
```

Plugin1.js

```
class Plugin1 {
	//主要引擎会传进来
	//里面的回调是串行执行的？
	apply(complier) {
		//emit assets输出到output之前
		complier.hooks.emit.tap('Plugins1', (compilationn) => {
			console.log('emit.tap 111')
		})
		//绑定异步 ， 同理也可以用tapPromise写法
		complier.hooks.emit.tapAsync('Plugins1', (compilation, cb) => {
			setTimeout(() => {
				console.log('emit.tapAsync')
				cb()
			}, 1000)
		})
	}
}

export default Plugin1;
```

Plugin2.js

```
cosnt util = require('util')
const fs = require('fs')

const webpack = require('webpack')
const { Rawsource } = webpack.sources

//fs.readFile 处理成promise风格方法
const readFile = util.promisify(fs.readFile)

class Plugin2 {
	apply(compiler) {
		//初始化compilation 钩子
		compiler.hooks.thisCompilation.tap('Plugin2',(compilation) => {
			//添加资源 additionalAssets 为compilation创建额外asset
			compilation.hooks.additionalAassets.tapAsync('Plugin2', (cb) => {
				const content = 'hello plugin2'
			
				//往要输出资源中添加一个a.txt
				compilation.assets['a.txt'] = {
					size() {
						return content.length
					},
					//文件内容
					source() {
						return content
					}
				}
				
				//专业一点的方法
				//path.resolve() 将相对路径处理成绝对路径
				const data = await readFile(path.resolve(__dirname, 'b.txt'))
				compilation.assets['b.txt'] = new RawSource(data)
				//另一种写法
				//compilation.emitAsset('b.txt', new RawSource(data));
				cb()
 			})
		})
	}
}
```

**实战**

将pubilc里面的资源除了index.html之外的都复制到dist目录里去

schema.json

```
{
	"type": "object",
	"properties": {
		"from": {
			"type":"string"
		},
		"to": {
			"type":"string"
		},
		"ignore": {
			"type":"array"
		}
	},
	"additionlProperty": false
}
```

CopyWebpackPlugin.js

```
const path = require('path');
const fs = require('fs');
const {promisify} = require('util')

const { validate } = require('schema-utils')
const globby = require('globby')  //获取所有文件的列表路径
const webpack = require('webpack')

const schema = require('./schema.json')

const readFile = promisify(fs.readFile)
const {RawSource} = webapck.sources

class CopyWebpackPlugin {
	constructor(options = {}) {
		//验证options是否规范
		this.options = options
	}
	apply(compiler) {
		compiler.hooks.thisCompilation.tap('CopyWebpackPlugin', (compilation) => {
			compilation.hooks.additionalAssets.tapAsync('CopyWebpackPlugin', (cb) => {
				//将from中的资源复制到to中，输出出去
				//1. 读取from中所有资源
				//2. 过滤掉ignore的文件
				//3. 生成webpack格式的资源
				//4. 添加compilation中输出出去
				const {from, ignore } = this.options;
				cosnt to = this.options.to ? this.options.to:'.';
				
				//运行指令的目录
				const context = compiler.options.context; //process.cwd()
				//将输入路径变成绝对路径
				const absoluteFrom = path.isAbsolute(from)? from: path.resolve(context, from)
				
				//globby(要处理的文件夹，options)
				const paths = await globby(from, {ignore}); //这里的path是所有要加载的文件路径数组
				
				//读取paths里面的所有文件
				//用promise.all处理数组异步,每一个都为成功态才会往下执行
				cosnt files = await Promise.all(
					paths.map(async (absolutePath) => {
						//读取文件
						const data = await readFile(path)
						//获取文件名称 basename 得到最后的文件名称
						const relativePath = path.basename(absolutePath);
						//如果to有值，我们这个时候应该要跟着to来		
                        const filename = path.join(to, relativePath)
						return {
							//文件数据
							data,
							//文件名称
							filename
						}
					})
				)
				//生成webpack格式的资源
				const assets = files.map((file) => {
					cosnt source = new RawSource(file.data)
					return {
						source,
						filename: file.filename
					}
				})
				//添加到compilation中输出出去
				assetts.forEach((asset) => {
					compilation.emitAsset(asset.filename, asset.source)
				})
				
				cb()
			})
		})
	}
}
```

webpack.config.js

```
module.exports = {
	Plugins: [
		new CopyWebpackPlugin({
			from: 'public',
			//to: '.',
			ignore: ['**/index.html']
		})
	]
}
```







### 手写一个小型的webpack

webpack 执行流程

1. 初始化Compiler: webpack(config) 得到Compiler对象
2. 开始编译：调用Compiler对象run方法开始执行编译
3. 确定路口：跟据配置中的entry找出所有的入口文件
4. 编译模块：从入口文件出发，调用所有配置的Loader对模块进行编译，再找出该模块依赖的模块，递归直到所有的模块被加载进来
5. 完成模块编译：在经过第四步使用Loader编译完所有模块后，得到了每个模块被编译后的最终内容已经他们之间 的依赖关系
6. 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的chunk,再把每个chunk转换成一个单独的文件加入到输出列表（这是可以修改输出内容的最后机会）
7. 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统





babel/parser  转译

babel/traverse  收集依赖

babel/core   transformFromAst()



**分析：**  

compiler是一个函数，利用babel进行转义，处理。

需要递归收集依赖

```
const fs = require('fs')
const path = require('path')
const {getAst, getDeps, getCode} = require('./parser')

class Compiler {
	constructor(options = {}) {
		this.options = options;
		//所有依赖的容器
		this.modules = [];
	}
	//启动webpack 打包
	run () {
		//入口文件路径
		const filePath = this.options.entry;
		//第一次构建，得到入口文件的信息
		this.build(filePath)
		
		this.modules.push(fileInfo)
		this.modules.forEach((fileInfo) => {
			//deps  {'./add.js':'/User/xxx/xx//xxx/add.js'}
			//取出当前文件的所有依赖
			const deps = fileInfo.deps;
			//遍历
			for(const relativePath in deps) {
				//依赖文件的绝对路径
				const absolutePath = deps[relativePath]
				//对依赖文件进行处理，递归build
				const fileInfo = this.build(absolutePath)
				//将处理后的结果添加到modules中，后面可以继续遍历
				this.modules.push(fileInfo)
			}
		})
		//需要将依赖整理成更好依赖关系图，整理成webpack风格的依赖
		cosnt depsGraph = this.modules.reduce((graph,module) => {
			return {
				...graph,
				[module.filePath]: {
					code:module.code,
					deps: module.deps
				}
			}
		}, {})
		
	}
	build(filePath) {
		const ast = getAst(filePath);
		const deps = getDeps(ast, filePath);
		const code = getCode(ast);
		//返回当前文件路径，当前文件所有依赖，当前文件解析后代码
		return {
			filePath, 
			deps,
		}
	}
	generate(depsGraph) {
		//定义匿名执行函数，确保只有自己能够操作
		const bundle = `
			(function(depsGraph) {
				//require目的： 为了加载入口文件
				function require(module) {
					//定义模块内部的require函数
					function localRequire(relavetivePath) {
						//为了找到要引入模块的绝对路路径，通过require加载
						return require(depsGraph[module].deps[relativePath])
					}
					//定义暴露的对象（将来我们模块要暴露的内容）
					var exports = {}
					
					(function(require,exports,code) {
						eval(code)
					})(localRequire,exports, depesGraph[module],code)
					
					//做为require函数的返回值返回出去
					//后面的require函数能够得到暴露的内容
					return exports;
				}
				//加载入口文件
				require('$(this.options.entry)')
			})(${JSON.stringify(depsGraph)})
		`
		const filePath = path.resolve(this.options.output.path, this.options.output.filename)
		file.writeFileSync(filePath, bundle, 'utf-8')
	}
}
```



parser.js   

	1. 获取抽象语法树
 	2. 获取依赖
 	3. 将ast解析成code