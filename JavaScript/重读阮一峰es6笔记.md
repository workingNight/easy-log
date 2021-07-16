## 重读阮一峰es6笔记

读书百遍其意自现，每次重复或许都会有新的了解





### 字符串篇

codePointAt()   字符串的编码    UTF-16,有些长字符\n

字符串匹配可以用includes()，之前都是用正则.test()。还有

```javascript
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```

`repeat`方法返回一个新字符串，表示将原字符串重复`n`次。

`padStart()`用于头部补全，`padEnd()`用于尾部补全。第一个参数是字符串补全生效的最大长度，第二个参数是用来补全的字符串。

`padStart()`的常见用途是为数值补全指定位数，也可以用来提示字符串格式

[ES2021](https://github.com/tc39/proposal-string-replaceall) 引入了`replaceAll()`方法，可以一次性替换所有匹配。

[ES2019](https://github.com/tc39/proposal-string-left-right-trim) 对字符串实例新增了`trimStart()`和`trimEnd()`这两个方法

字符串可以使用正则的方法：match()`、`replace()`、`search()`和`split()`。



### 正则篇

ES6 对正则表达式添加了`u`修饰符,会正确处理四个字节的 UTF-16 编码

ES6 新增了使用大括号表示 Unicode 字符./\u{20BB7}/u.test('𠮷') // true

```javascript
/\u{61}/.test('a') // false
```

使用`u`修饰符后，所有量词都会正确识别码点大于`0xFFFF`的 Unicode 字符。

正确返回字符串长度的函数。预定义模式有点模糊，不太理解

ES6 还为正则表达式添加了`y`修饰符，叫做“粘连”（sticky）修饰符。

ES6 为正则表达式新增了`flags`属性，会返回正则表达式的修饰符。

ES2018 [引入](https://github.com/tc39/proposal-regexp-dotall-flag)`s`修饰符，使得`.`可以匹配任意单个字符。 正常来说 因为`.`不匹配`\n`，所以正则表达式返回`false`。变通的写法/foo[^]bar/.test('foo\nbar')  这样来表示一个任意的字符

`\p{Script=Greek}`指定匹配一个希腊文字母，所以匹配`π`成功。属性名和属性值。

由于 Unicode 的各种属性非常多，所以这种新的类的表达能力非常强。新的类的写法`\p{...}`和`\P{...}`

```javascript
(?<year>\d{4})  具名组匹配，可以配合解构一起使用
```

相对于返回数组，返回遍历器的好处在于，如果匹配结果是一个很大的数组，那么遍历器比较节省资源





## 数值篇

```
Number.isNaN()`用来检查一个值是否为`NaN
```

```
parseInt()`和`parseFloat()
```

```javascript
Number.EPSILON === Math.pow(2, -52)
```

ES6 引入了`Number.MAX_SAFE_INTEGER`和`Number.MIN_SAFE_INTEGER`这两个常量，用来表示这个范围的上下限。

`Math.trunc`方法用于去除一个数的小数部分，返回整数部分

`Math.sign`方法用来判断一个数到底是正数、负数、还是零。区分正负0

ES6 新增了 6 个双曲函数方法。

[ES2020](https://github.com/tc39/proposal-bigint) 引入了一种新的数据类型 BigInt（大整数），来解决这个问题，这是 ECMAScript 的第八种数据类型

添加后缀`n`

BigInt 与普通整数是两种值，它们之间并不相等

BigInt 可以使用负号（`-`），但是不能使用正号（`+`），因为会与 asm.js 冲突。

转换

```javascript
Number(1n)  // 1
String(1n)  // "1"
```

BigInt 总是带有符号的





## 函数的变化

```javascript
y = y || 'World';
如果参数y赋值了，但是对应的布尔值为false，则该赋值不起作用。就像上面代码的最后一行，参数y等于空字符，结果被改为默认值。if (typeof y === 'undefined') {
  y = 'World';
}
```

箭头函数的使用注意：是因为对象不构成单独的作用域，导致`jumps`箭头函数定义时的作用域就是全局作用域。

尾调用优化。这就叫做“尾调用优化”（Tail call optimization），即只保留内层函数的调用帧。如果所有函数都是尾调用，那么完全可以做到每次执行时，调用帧只有一项，这将大大节省内存。这就是“尾调用优化”的意义。简单的说执行到最后一步可以删除fx的调用帧，只保留最后的调用帧

返回的值是一个单独的函数 return g(m + n);

```javascript
//这样的情况不属于尾调用。
function f(x){
  return g(x) + 1;
}
```

【尾递归】永远不会栈溢出，一些函数式编程语言都将其写入了语言规格

```javascript
function factorial(n, total) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}
```

函数式编程有一个概念，叫做柯里化（currying），意思是将多参数的函数转换成单参数的形式。这里也可以使用柯里化。采用 ES6 的函数默认值可以实现柯里化

```javascript
function factorial(n, total = 1) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}

factorial(5) // 120
```

toString()  可以返回函数的代码

[ES2019](https://github.com/tc39/proposal-optional-catch-binding) 做出了改变，允许`catch`语句省略参数。





## 数组

再次回忆深浅拷贝，

扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。

扩展运算符还可以将字符串转为真正的数组。

【注意多字节u'nicode字符】

拓展运算符可以将类数组转化为真正的数组。只要是部署了 Iterator 接口的数据结构，`Array.from`都能将其转为数组。任何有`length`属性的对象，都可以通过`Array.from`方法转为数组，而此时扩展运算符就无法转换。

array.from更强一些。`Array.from`还可以接受第二个参数，作用类似于数组的`map`方法，用来对每个元素进行处理，将处理后的值放入返回的数组。类似于map

`Array.of()`基本上可以用来替代`Array()`或`new Array()`，并且不存在由于参数不同而导致的重载。它的行为非常统一。

```javascript
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]   从3开始的位置复制到0开始的
```

但是`findIndex`方法可以借助`Object.is`方法做到识别数组中的NaN

```
数组遍历entries()`，`keys()`和`values()都返回一个迭代器对象。对键值对的遍历、对键的遍历、对值的遍历
```

```javascript
for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
也可以手动调用对象的next方法进行遍历
```

【对空位的处理】，数组的不同方法对空位的处理不同。

es6明确将空位转化为undifined

空位的处理规则非常不统一，所以建议避免出现空位



## 对象

console对象的时候如果在外面加大括号，就变成了对象的简洁写法，在每组键值对前面会打印对象名

可枚举性，有些属性不可枚举就可以在遍历的时候忽略掉。

for in 会返回继承的属性，所以尽量不要使用for in循环，而用object.keys()代替，它返回的是一个对象所有可枚举属性的键名，不含symbol.

getOwnPropertyNames 与keys的区别就是包含不可枚举属性

**getOwnPropertySymbols** 获取对象symbol属性的键名

ownKeys  包含symbol 不含继承的

顺序，数值，字符串，symbol键

super关键字指向当前对象的原型对象

**链判断符的前世今生**

```javascript
// 错误的写法
const  firstName = message.body.user.firstName;
// 正确的写法
const firstName = (message
  && message.body
  && message.body.user
  && message.body.user.firstName) || 'default';
```

因此 [ES2020](https://github.com/tc39/proposal-optional-chaining) 引入了“链判断运算符”（optional chaining operator）`?.`，简化上面的写法。

```javascript
const firstName = message?.body?.user?.firstName || 'default';
const fooValue = myForm.querySelector('input[name=foo]')?.value
```

```javascript
iterator.return?.()    
对于那些可能没有实现的方法，这个运算符尤其有用
类似一种短路机制，只要不满足条件，就不再往下执行
```

**新的默认值方法**

[ES2020](https://github.com/tc39/proposal-nullish-coalescing) 引入了一个新的 Null 判断运算符`??`行为类似||，但只有运算符左侧为null或者undifined时才会返回右侧的值。之前的||来赋默认值可能会对false和0和空字符串失效，这个就能规避这种情况。如果和&&同时存在的话记得添加括号。

**对象新增的方法**

`Object.is` 严格对比

assign()对象融合，

> 浅拷贝，只拷贝一层引用;对于同名属性处理方法是替换，而不是添加

希望合并之后返回一个空对像，可以改写上面函数，对一个空对象合并

```javascript
(...sources) => Object.assign({}, ...sources);
```

该方法的引入目的，主要是为了解决`Object.assign()`无法正确拷贝`get`属性和`set`属性的问题。

可以配合配合`Object.create()`方法，将对象属性克隆到一个新对象

\_\_proto\_\_属性,用来读取当前对象的原型对象，只在浏览器端有效，本质上来说是一个内部属性，而不是一个对外的api.

建议不要使用这个属性，而是用`Object.setPrototypeOf()`（写操作，设置对象的原型链对象）、`Object.getPrototypeOf()`（读操作）、`Object.create()`（生成操作）来代替

对象的keys() values() entries()

```javascript
for (let [k, v] of Object.entries(obj)) {
  console.log(
    `${JSON.stringify(k)}: ${JSON.stringify(v)}`
  );
}
```

`Object.entries`方法的另一个用处是，将对象转为真正的`Map`结构。

`Object.fromEntries()`方法是`Object.entries()`的逆操作，特别适合将map结构转为对象。

妙用： 配合`URLSearchParams`对象，将查询字符串转为对象。

```javascript
Object.fromEntries(new URLSearchParams('foo=bar&baz=qux'))
// { foo: "bar", baz: "qux" }
```



## symbol

终于到symbol了开心，以前看到这个属性的时候或许都会有些害怕，let go to see its deep and shadow. hah

为什么会有symbol,从根本上防止属性名的冲突，保证每个属性名的名字都是独一无二的。也即js的第七种数据类型。

`Symbol`函数前不能使用`new`命令 ， let s = Symbol();因为这是一个原始值而不是对象，

```javascript
let s1 = Symbol('foo'); 接受的参数是为了对字符串进行描述。就算传的参数一样两个symbol也是不一样的
```

实例属性，直接返回symbol的描述。description  s1.description = foo

对象定义属性的方法。

```javascript
Object.defineProperty(a, mySymbol, { value: 'Hello!' });
```

对象不能用.去访问symbol 而是要用[]

```javascript
 [s]: function (arg) { ... }
 简洁写法：[s](arg) { ... }
 
   DEBUG: Symbol('debug'),
  INFO: Symbol('info'),
  WARN: Symbol('warn')
```

魔术字符串：魔术字符串指的是，在代码之中多次出现、与代码形成强耦合的某一个具体的字符串或者数值。不利于将来的修改和维护

我们希望重新使用同一个 Symbol 值，`Symbol.for()`方法可以做到这一点。会被登记在全局环境中供搜索，可以破除一些定义域的限制

比如，`foo instanceof Foo`在语言内部，实际调用的是`Foo[Symbol.hasInstance](foo)`。

**Symbol.species** 让子类使用继承的方法时，子类希望返回基类的实例，而不是子类的实例

11个内置的symbol值，指向语言内部使用的方法，

对象的`Symbol.hasInstance`属性

对象的`Symbol.isConcatSpreadable`属性等于一个布尔值 标记对象用于concat时是否可展开

对象的`Symbol.species`属性，指向一个构造函数。创建衍生对象时可以用它

对象的`Symbol.match`属性，指向一个函数。 str.match（）使用时会调用它，可以理解为给match勾上了一个函数，match使用时，它会附带调用，

对象的`Symbol.replace`属性，指向一个方法，和match类似。

对象的`Symbol.search`属性，指向一个方法

对象的`Symbol.split`属性，指向一个方

对象的`Symbol.iterator`属性，指向该对象的默认遍历器方法

对象的`Symbol.toPrimitive`属性，指向一个方法。该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值。比如对象转化为number这中原始类型时就会触发这个附带的函数，

对象的`Symbol.toStringTag`属性，指向一个方法。这个属性可以用来定制`[object Object]`或`[object Array]`中`object`后面的那个字符串。

- `ArrayBuffer.prototype[Symbol.toStringTag]`：'ArrayBuffer'

- `DataView.prototype[Symbol.toStringTag]`：'DataView'

- `Map.prototype[Symbol.toStringTag]`：'Map'

- `Promise.prototype[Symbol.toStringTag]`：'Promise'

  可以利用这个来方便的判断对象的类型

对象的`Symbol.unscopables`属性，指向一个对象。该对象指定了使用`with`关键字时。

with关键字有什么作用，不太清除，好像场景比较少见





## set&map篇

新的数据结构set,类似于数组，但是成员的值都是唯一的，没有重复的值。

可以接受迭代器对象作为参数，用来初始化

数组去重，也可以搭配join('')来字符串去重。

set的判断值重复的规则类似===,两个对象总是不同的，NaN是相同的

set的方法大致可分为2类操作、遍历

add  delete  has clear

keys values enteries forEach

使用set可以方便的进行交、并、差。想在遍历操作中同步改变原来的set结构，利用原set映射出一个新的结构然后赋值给原来的set结构，另一种是利用array.from方法

**关于weakSet**  没有size属性。不可遍历，成员只能为对象，当对象不能被访问到时，就进行垃圾回收

WeakSet 可以接受一个数组或类似数组的对象作为参数。

```javascript
const ws = new WeakSet([3,4]);会报错，因为成员不是对象
```

方法add delete has

因为成员都是弱引用，随时都可能消失，可以用来存储dom节点

**map**和对象类似，键值对，object只能用字符串当关键字,map就可以	

键实际上是跟内存地址绑定的，只要内存地址不一样就视为2个键。

操作方法，get set has delete clear

遍历方法， 和map类似

object.create的作用需要复习一下。

对象转化为map  let map = new Map(Object.entries(obj));

entries作用，将一个对象变为迭代器对象

map转化为j'son，

如果键名都是字符串，可以转化为对象JSON

如果有非字符串，这个时候可以选择转化为数组JSON

foreach的第二个参数可以用来绑定this

**weakMap**只接受对象和null作为键名，不接受其他类型的值作为键名.弱引用。弱引用的只是键名

同理没有遍历操作，也没有size属性，也不支持clear方法，因为里面的键不是稳定的可能上一秒能取到键名，后面也有可能不能取到键名。典型的引用场景就是dom节点作为键名，另一个作用就是部署私有属性



##  proxy 

oww  已经到了高级进阶的部分了，以前学的时候每次看到都主动忽略，但是我都是一个前端练习很多年的老练习生了，哈哈，let us go to adveture.

**是什么**： 给对象添加一层，外界对改对象的访问都必须通过这层拦截，可以对外界的访问进行过滤和改写。可以修改某些操作的默认新闻

**用处**可以用来拦截目标对象的任意属性，这使得它很适合用来写web服务的客户端,也可以用来实现数据库的ORM层。

**详情**: 支持的拦截操作，一共13种

**get(target, propKey, receiver)**

**set(target, propKey, value, receiver)**         receiver----proxy实例本身，最后一个参数可选

**has(target, propKey)**

**deleteProperty(target, propKey)**：

**ownKeys(target)**

**getOwnPropertyDescriptor(target, propKey)**

**defineProperty(target, propKey, propDesc)**

**preventExtensions(target)**

**getPrototypeOf(target)**

**isExtensible(target)**：

**setPrototypeOf(target, proto)**：

**apply(target, object, args)**

**construct(target, args)**

很多的返回值都是返回一个布尔值

**使用**：var object = { proxy: new Proxy(target, handler) }; 一个handler可以设置多层拦截

```javascript
var handler = {
  get: function(target, name) {...},

  apply: function(target, thisBinding, args) {...},
  construct: function(target, args) {...}
};
```

【ctx】在前端一般指目标对象的上下文对象

`has()`方法隐藏某些属性，不被`in`运算符发现。in操作符会触发这个

```javascript
var handler = {
  has (target, key) {
    if (key[0] === '_') {
      return false;
    }
    return key in target;
  }
};
```





## reflect

反射， 

把一些Object对象的一些明显属于语言内部的方法放到Object上

把一些Object操作变成函数行为。Reflect.has(obj,name)  reflect.deleteProperty(obj,name)

和proxy对象的方法对应，proxy上的方法在reflect上都能找到，也就是说你在代理上修改一些默认行为，你总是可以在reflect上获取默认行为。也就是可以确保完成原有功能

其次。一些object的老写法可以修正为更好读

```javascript
// 老写法
Function.prototype.apply.call(Math.floor, undefined, [1.75]) // 1

// 新写法
Reflect.apply(Math.floor, undefined, [1.75]) // 1
```

暂时的，你可以把reflect当成一个更稳定可靠的Object对像来理解





## Promise

promise是老朋友了，但是容易忘，一些细节还是要进行把握

三个状态

`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败

特点： 将异步操作以同步操作的流程表达出来，避免层层嵌套的回调函数。

缺点： 

1. 无法取消

   			2. 不设回调内部抛出的错误不会反应到外部，也就是说promise会吃掉错误，我们需要用catch来捕获错误
   			3. pending状态时，进行状态不详细

**finally**不管上面的promise是fulfilled还是rejected都会执行回调函数

**all**将多个promise实例包装成一个新的promise实例，接受一个数组作为参数。

只有所有实例fulfilled或者有一个rejected才会触发后面的方法，如果其中一个实例定义了自己的catch,就不会触发all()的catch.

**race**和all不同的是race是监听一个发生状态改变的

```javascript
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
]);

p
.then(console.log)
.catch(console.error);
也就给请求加了一个限制，如果5秒内请求未成功就下面的那个promise实例就reject
```

**promise.allSettled**  方法接受一组promise实例做为参数，等到所有参数都返回结果该包装实例才会结束，

我们不关心异步操作的结果，只关心这些操作有没有结束。这时，`Promise.allSettled()`方法就很有用，可以确保所有异步操作都结束，返回的实例对象中有status属性来标记状态的返回值

**any**  和race很像，只要有一个完成，状态就完成，但是和race不同的是不会因为某个promise变成rejected而结束，它会一直等到有一个完成，除非所有的都reject

**resolve**  将现有对象转化为promise对象。

参数有4中情况

>  1.如果包装的是一个resolve对象，原封不动返回，2. 如果是thenable对象(具有then方法的对象），转化为promise并立即执行then 3. 如果不是对象，返回一个新的promise对象，状态为fullfiled 4.不带任何参数，直接返回一个resolved状态的promise对象。
>
> 值得注意的，第四种情况的promise对象在本轮事件循环的结束时执行，而不是下一轮事件循环。

**reject**  和resolve相反	

**try**  promise来包装同步函数会让他在本轮事件循环的末尾执行，也就变成异步的了。我们之前可以用立即执行的匿名函数来处理.实际上promise.try就是模拟try代码块，就像promise.catch模拟的是catch代码块

```javascript
try {
  database.users.get({id: userId})
  .then(...)
  .catch(...)
} catch (e) {
  // ...
}
就可以变成下面这种链式的
Promise.try(() => database.users.get({id: userId}))
  .then(...)
  .catch(...)
```





## 迭代器篇

统一的接口机制来处理所有不同的数据结构，只要部署了迭代器接口就可以进行遍历，主要供for of消费

Symbol.iterator属性，一个数据只要具有这个属性，就说明是可遍历的。该属性是一个函数，执行这个函数就会返回一个遍历器。Symbol.iterator这是一个预定义好的、类型为 Symbol 的特殊值。也就是说这个es自带的特殊的symbol,一个特别的键名。要放在方括号里面。

调用这个接口就会返回一个遍历器对象。let iter = arr\[Symbol.iterator\]();

这个时候iter 就可以 iter.next()往下遍历了。类比联想，Map 结构的 Iterator 接口，默认就是调用`entries`方法。

一个对象要想能够for of 就先要在`Symbol.iterator`的属性上部署遍历器生成方法,我们一个在这个属性上进行定义覆盖原生的方法，实现对于字符串的遍历。

调用iterator接口的场合，解构赋值，拓展运算符，yield.

for of  弥补了  for in 的许多缺陷，可以break,continue return ,所有数据结构统一的操作接口，for ... in顺序可能乱，会遍历到原型上的键，以字符串为键名。。。





## Generator 函数的语法

Generator 函数是 ES6 提供的一种异步编程解决方案

可以把他当成一个状态机，封装了多个内部状态。promise是一个状态容器。new promise(function(resolve, reject) {...})

会返回一个遍历器对象。

特点：  function*  name() {...}  一般来说*紧跟function 关键字

​			yield   关键字 (产出)

变成一个暂缓函数，协程，让出进程，只有调用next方法时，函数f才会执行

`yield`表达式如果用在另一个表达式之中，必须放在圆括号里面

`gen`是一个 Generator 函数，调用它会生成一个遍历器对象`g`。g\[Symbol.iterator\]() === g 类似于它调用自己的迭代器属性。

由于`next`方法的参数表示上一个`yield`表达式的返回值。

语义上第一个next是用来启动遍历器对象，所以不带参数。

用for of 来遍历，就不用手动去调用next()

产生的遍历器对象还有有throw( )和return 方法，终结函数。

**理解next（param）** 实际上是把上一个步骤的yield 替换成 next()里面的参数param.如果`next`方法没有参数，就相当于替换成`undefined`。`throw()`是将`yield`表达式替换成一个`throw`语句。 `return()`是将`yield`表达式替换成一个`return`语句。  关键字：替换

**ES6 提供了`yield*`表达式**  可以在一个Generator函数里面执行另一个Generator函数。等同于for of

**对象属性generator的简写**

```javascript
* myGeneratorMethod() {
    ···
  }
  myGeneratorMethod: function* () {
    // ···
  }
```

**状态机** 本身就包含状态

**协程**  传统子线程，堆栈式后进先出，只有当调用的子函数完全执行完毕才会结束执行父函数。

​		协程，线程可以让出执行权，并行执行。协程同时存在多个栈，协程是以多占用内存为代价实现多任务并行。第一次执行next()把generator 函数的上下文加入堆栈，即开始运行gen内部的代码，等遇到yield 1时，gen上下文退出堆栈，内部状态冻结。

对于yield的概念还是有些模糊，为什么要有这个东西，它带来的改变是什么。

```javascript
function* main() {
  var result = yield request("http://some.url");
  var resp = JSON.parse(result);
    console.log(resp.value);
}

function request(url) {
  makeAjaxCall(url, function(response){
    //这里的next的理解是用response 来替换yield request("http://some.url");
    it.next(response);  
  });
}

var it = main();
it.next();
```

利用 Generator 函数，可以在任意对象上部署 Iterator 接口。



## generator 的异步应用

学完这篇应该能堆generator的理解加深了。

为什么node约定回调函数的一个参数必须是错误对象err,执行分两段，第一段执行完以后，任务所在的上下文环境就已经结束了在这以后抛出的错误原来的上下文环境已经无法捕捉了，只能当作参数传入第二段。 

promise 让异步任务的两段执行变得清除了。缺点代码冗余，原来的任务被promise包装一下不管什么操作，一眼看上去都是一堆then,原来的语义变得很不清楚

协程遇到yield命令就暂停，等到执行权返回，再从暂停的地方继续往后执行，它的最大优点就是代码的写法变得和同步一样。generator函数可以暂停和恢复执行。

函数体内外的数据交换和错误处理机制。next()可以接受参数，向函数体里面输入数据

thunk函数是自动执行generator的一种方法。

求职策略，函数的参数到底该什么时候求值。传值调用和传名调用。

co模块，将两种自动执行器（thunk函数和promise对象）包装成一个对象，限制yield命令后面只能是thunk函数或者Promise对象



## async  

是generator的语法糖，将generator和自动执行器包装在一个函数里面

​	1.内置执行器

​	2.更好的语义

​	3.更强的适用性，yield后面可以是thunk函数或者promise对象，awiat可以是原始类型

​	4.await的返回值是promise对象，比iterator对象方便多了。	

`await`命令后面是一个`thenable`对象（即定义了`then`方法的对象），那么`await`会将其等同于 Promise 对象

promise太多then catch 操作本身的语义不容易看出来

错误处理：

```javascript
  try {
    const val1 = await firstStep();
    const val2 = await secondStep(val1);
    const val3 = await thirdStep(val1, val2);

    console.log('Final: ', val3);
  }
  catch (err) {
    console.error(err);
  }

```

```javascript
for (i = 0; i < NUM_RETRIES; ++i) {
    try {
      await superagent.get('http://google.com/this-throws-an-error');
      break;
    } catch(err) {}
  }
可以实现多次重复尝试
```

**如果是两个独立异步请求**，建议promise.all一下这样会缩短程序的执行时间。

```javascript
let [foo, bar] = await Promise.all([getFoo(), getBar()]);
```





## class

类的方法都定义在prototype对象上

原型对象的constructor属性直接指向类的本身，类中定义的方法都是不可枚举的

静态方法不会被实例继承，而是直接通过类来调用，而不是在类的实例上调用

**实例属性的新写法**： 定义在类的最顶层。

```javascript
 constructor() {
    this._count = 0;
  }


bar = 'hello';
baz = 'world';
constructor() {
    // ...
}
```

****

**静态属性** 和静态方法类似，都是定义在类上面的，而不是在实例对象上的。

```javascript
// 老写法
class Foo {
  // ...
}
Foo.prop = 1;

// 新写法
class Foo {
  static prop = 1;
}
```

**私有方法**

```javascript
// 私有方法和私有属性
_hell  私有属性
  [bar](baz) {
    return this[snaf] = baz;
  }
#a;
#b; //私有属性
//私有方法
#sum() {
  return this.#a + this.#b;
}
```

`in`运算符对于`Object.create()`、`Object.setPrototypeOf`形成的继承，是无效的，因为这种继承不会传递私有属性。

```javascript
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}
//确保类用new命令来生成实例
Class 内部调用new.target，返回当前 Class。
子类继承父类时，new.target会返回子类。利用这个特点。可以写出不能独立使用、必须继承后才能使用的类。
```





## 类的继承

```javascript
class ColorPoint extends Point {
}

//先将父类实例对象的属性和方法加到this上面，然后再用子类的构造函数修改this
// 等同于
class ColorPoint extends Point {
  constructor(...args) {
    super(...args);
  }
}
```

父类的静态方法也会被子类继承

`Object.getPrototypeOf`方法可以用来从子类上获取父类。可以用这个判断一个类是否继承了另一个类

**super**

super()代表父类的构造函数   相当于`父.prototype.constructor.call(this)`。

如果作为对象使用时，在普通方法中指向父类的原型对象指向`A.prototype`，在静态方法中指向父类

**\_\_proto\_\_**  

（1）子类的`__proto__`属性，表示构造函数的继承，总是指向父类。

（2）子类`prototype`属性的`__proto__`属性，表示方法的继承，总是指向父类的`prototype`属性。

属性   都在类上，也就是对象上

方法   都在原型上   prototype

**mixin**  混用，将多个类合并成一个类



## module   

到了module章节了，老是和import export搞混，这回好好加深一下印象



commonJs规范，

```javascript
// CommonJS模块   对象，输入时必须查找对象属性
let { stat, exists, readfile } = require('fs');=
// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
运行时加载，没办法在编译的时候做静态优化
```

es6模块不是对象，而是通过`export`命令显式指定输出的代码，再通过`import`命令输入。

```javascript
// ES6模块
import { stat, exists, readFile } from 'fs';
```

实质是从`fs`模块加载 3 个方法，这种加载称为编译时加载或称静态加载。使得静态分析成为可能

**区分： commonjs是运行时加载，es6模块是编译时加载**

es6模式主要就是export 和import

```javascript
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;
function v1() { ... }
function v2() { ... }
//模块暴露的写法
export {
firstName, lastName, year,
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
export default 的时候就可以让引入的时候不用写{}
```

```javascript
import { lastName as surname } from './profile.js';
//引入的模块，尽量作为只读，不要进行改写
```

如果在一个模块之中，先输入后输出同一个模块，`import`语句可以与`export`语句写在一起。（不怎么常见）

```javascript
export { foo, bar } from 'my_module';   //也就是引入了一个别的模块，然后在这个模块暴露出去
// 可以简单理解为
import { foo, bar } from 'my_module';
export { foo, bar };
```

将这些文件输出的常量，合并在`index.js`里面。然后在index.js里面进行一起暴露

```javascript
// constants/index.js
export {db} from './db';
export {users} from './users';
```

`import`命令叫做“连接” binding 其实更合适

**import()**:   [ES2020提案](https://github.com/tc39/proposal-dynamic-import) 引入`import()`函数，支持动态加载模块。`import()`返回一个 Promise 对象

`import()`函数可以用在任何地方，不仅仅是模块，非模块的脚本也可以使用。它是运行时执行，也就是说，什么时候运行到这一句，就会加载指定的模块。

**`import()`类似于 Node 的`require`方法，区别主要是前者是异步加载，后者是同步加载。**

用于按需加载，条件加载，动态的模块路径

```javascript
button.addEventListener('click', event => {
  import('./dialogBox.js')
  .then(dialogBox => { 
    dialogBox.open();  //import()方法放在click事件的监听函数之中，只有用户点击了按钮，才会加载这个模块。 很多组件库都开始支持按需加载了
  })
  .catch(error => {
    /* Error handling */
  })
});
```

`import()`加载模块成功以后，这个模块会作为一个对象，当作`then`方法的参数。因此，可以使用对象解构赋值的语法，获取输出接口

```javascript
then(({export1, export2}) => {
  // ...·
});
//如果是default 就可以直接使用
.then(myModule => {
  console.log(myModule.default);
});
```

`import()`也可以用在 async 函数之中。





##  module的加载实现

```html
<script type="application/javascript">
  // module code
</script>
```

`defer`要等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成）

`async`一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。

**`defer`是“渲染完再执行”，`async`是“下载完就执行”。**defer是可以按照顺序加载，async不能保证加载顺序的

浏览器加载 ES6 模块，也使用`<script>`标签，但是要加入`type="module"`属性。都是异步加载，不会造成堵塞浏览器

```html
<script type="module" src="./foo.js"></script>
<!-- 等同于 -->
<script type="module" src="./foo.js" defer></script>
```

利用顶层的`this`等于`undefined`这个语法点，可以侦测当前代码是否在 ES6 模块之中。

```javascript
const isNotModuleScript = this !== undefined;
```

**commonjs模块和es6模块对比**

c输出的是值得拷贝，输出后的值的变化不会对原模块造成影响，es6输出的是值的引用

CommonJS 模块的`require()`是同步加载模块，ES6 模块的`import`命令是异步加载，有一个独立的模块依赖的解析阶段。

因此，ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。ES6 输入的模块变量，只是一个“符号连接”，变量是只读的，不要去修改它

**node.js**的模块加载方法

JavaScript 现在有两种模块。一种是 ES6 模块，简称 ESM；另一种是 CommonJS 模块，简称 CJS。

CommonJS 模块是 Node.js 专用的，与 ES6 模块不兼容

CommonJS 模块使用`require()`和`module.exports`

node已经打开对es6模块的支持。Node.js 要求 ES6 模块采用`.mjs`后缀文件名

如果不希望将后缀名改成`.mjs`，可以在项目的`package.json`文件中，指定`type`字段为`module`。

`type`字段为`commonjs`  时，js脚本被解释为commonJs模块

```javascript
{
   "type": "module"  //一旦设置了以后，该目录里面的 JS 脚本，就被解释用 ES6 模块。
}
```

`.mjs`文件总是以 ES6 模块加载，`.cjs`文件总是以 CommonJS 模块加载，`.js`文件的加载取决于`package.json`里面`type`字段的设置。

**pack.json** 配置

使用`main`字段，指定模块加载的入口文件。

`package.json`文件的`exports`字段可以指定脚本或子目录的别名,然后就可以从别名加载这个文件。

别名如果是`.`，就代表模块的主入口，优先级高于`main`字段

利用`.`这个别名，可以为 ES6 模块和 CommonJS 指定不同的入口

**cjs加载es6**  利用 import()

**es6记载cjs**  只能整体加载，不能只加载单一的输出项   import packageMain from 'commonjs-package';不能用{单一}来进行加载

ES6 模块的加载路径必须给出脚本的完整路径，**不能省略脚本的后缀名**。`import`命令和`package.json`文件的`main`字段如果省略脚本的后缀名，会报错。

就是`this`关键字。ES6 模块之中，顶层的`this`指向`undefined`；CommonJS 模块的顶层`this`指向当前模块，这是两者的一个重大差异。

CommonJS 模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存。加载时执行脚本代码在`require`的时候，就会全部执行。一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。

**两者对循环引用的处理**