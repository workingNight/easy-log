## ts补漏

[TOC]







### **元组**

限制数组元素的个数和类型，用来实现多值返回。

可以参考 React Hooks。const [count, setCount] = useState(0)

 



### **unknown**

描述类型并不确定的变量。如果你需要用any的时候，考虑一下unkonwn

使用时需要类型缩小。

```
let result: unknown;
//这个判断操作就让类型缩小了
if (typeof result === 'number') {
  result.toFixed(); // 此处 hover result 提示类型是 number，不会提示错误
}
```





###  **void、undefined、null**

undefined 的最大价值主要体现在接口类型

null 的价值我认为主要体现在接口制定，属性、对象都可能是 null 空对象





### **never**

never 表示永远不会有返回值的类型.用于死循环，统一抛错等函数返回值定义。或者应用于属性只读

never 是所有类型的子类型，它可以给所有类型赋值.变量的类型缩小为never





### **类型断言**

静态类型对运行时的逻辑无能为力，场景报错：ts2322，不能把类型undifined分配给number

类型断言，告诉ts按照我们的方式做类型检查     语法：xxx  as number

mayNullOrUndefinedOrString!.toString(); // ok非空断言。尽量用类型守卫替换，为空断言，只能在检查时候没有问题，但是不能避免在运行时候出错

非空断言  !.

```
场景： 
a.list   如果报错，可以进行类型断言
(a as {list: Array<xxx>}).list   这个时候就不会报错
```







### **类型推断**

 let x1 = 42; // 推断出 x1 的类型是 number

 /** 根据参数的类型，推断出返回值的类型也是 number */
  function add1(a: number, b: number) {
    return a + b;
  }

函数返回值的类型可以在 TypeScript 中被推断出来，即可缺省，特例：特例需要我们显式声明返回值类型， Generator 函数的返回值。

还能有上下文推断，这个时候类型标注是可以省去的





### **字面量类型**

可以进行组合

```
size: 'small' | 'big';
isEnable:  true | false;
margin: 0 | 2 | 4;
```

赋默认值，产生的效果。默认参数 'hello' ，TypeScript 推断出了 x 的类型为 string | undefined。

let strFun = (str = 'this is string') => str; // 类型是 (str?: string) => string;





### **类型推广和类型收缩**

类型拓展：

```
let x = null; // 类型拓宽成 any
let y = undefined; // 类型拓宽成 any
```

类型收缩：简单理解，通过条件判断收缩它的类型范围

```
let func = (anything: any) => {
  if (typeof anything === 'string') {
   return anything; // 类型是 string 
 } else if (typeof anything === 'number') {
    return anything; // 类型是 number
 }
```

我们也可以通过字面量类型等值判断（===）或其他控制流语句（包括但不限于 if、三目运算符、switch 分支）将联合类型收敛为更具体的类型





### **类型谓词**

```
我们通过“参数名 + is + 类型”的格式明确表明了参数的类型，进而引起类型缩小
function isString(s): s is string { // 类型谓词
  return typeof s === 'string';
}
```





### **类类型**

派生类如果包含一个构造函数，必须在构造函数中调用 super() 方法。super会调用基类的构造函数

只读修饰符： public readonly firstName: string;

存取器：  get set.截取对类成员的读写访问

静态属性：静态属性的特性，我们往往会把与类相关的常量、不依赖实例 this 上下文的属性和方法定义为静态属性，从而避免数据冗余，进而提升运行性能。不在实例上，而是通过基类类名访问MyArray.displayName





### **接口类型和类型别名**

interface   typeof

类型别名，我们只是给类型取了一个新名字，而不是创建了一个新的类型

interface  可以重复定义，它的属性会叠加，我们可以方便对全局变量，第三方库的类型做拓展





### **索引签名**

```
interface LanguageRankInterface {
  [rank: number]: string;
}
```





### **联合类型和交叉类型**

“|”操作符分隔类型的语法来表示联合类型。

使用“&”操作符来声明交叉类型

联合操作符 | 的优先级低于交叉操作符 &

联合类型真正的用武之地就是将多个接口类型合并成一个类型，从而实现等同接口继承的效果，也就是所谓的合并接口类型

```
  type IntersectionType = { id: number; name: string; } & { age: number };
```

如果交叉为空集，那么就是一个nerver类型，我们必须在一个子集中写明是never不然不会报错

<u>联合类型导致的类型缩减</u>  type URStr = 'string' | string; // 类型是 string

避免这个缩减： type BorderColor = 'black' | 'red' | 'green' | 'yellow' | 'blue' | string & {}; // 字面类型都被保留





### **枚举类型**

```
转译为js
   var Day = void 0;
    (function (Day) {
        Day[Day["SUNDAY"] = 0] = "SUNDAY";
        Day[Day["MONDAY"] = 1] = "MONDAY";
        Day[Day["TUESDAY"] = 2] = "TUESDAY";
        Day[Day["WEDNESDAY"] = 3] = "WEDNESDAY";
        Day[Day["THURSDAY"] = 4] = "THURSDAY";
        Day[Day["FRIDAY"] = 5] = "FRIDAY";
        Day[Day["SATURDAY"] = 6] = "SATURDAY";
    })(Day || (Day = {}));
```

使用例子：

```
 const enum Day {
    SUNDAY = 1,   //指定了从 1 开始递增
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY
  }
  const work = (d: Day) => {
    switch (d) {
      case Day.SUNDAY:
        return 'take a rest';
      case Day.MONDAY:
        return 'work hard';
    }
  }
}
```

外部枚举：通过 declare 描述一个在其他地方已经定义过的变量

使用 declare 描述一个在其他地方已经定义过的枚举类型，通过这种方式定义出来的枚举类型，被称之为外部枚举。外部枚举的作用在于为两个不同枚举（实际上是指向了同一个枚举类型）的成员进行兼容、比较、被复用提供了一种途径。总结：外部枚举，就是声明一下已经定义过的变量，避免报错





### **泛型**

借用 Java 中泛型的释义来回答这个问题：泛型指的是类型参数化，即将原来某种具体的类型进行参数化。和定义函数一样去控制类型，并在调用时给泛型传入明确的类型参数。设计泛型的目的在于有效约束类型成员之间的关系，比如函数参数和返回值、类或者接口成员和方法之间的关系。

关键词： 约束关系，定义函数一样，将类型参数化，调用时再传入明确的参数

所以： *函数的泛型入参必须和参数/参数成员建立有效的约束关系才有实际意义*

泛型约束： 泛型入参限定在一个相对更明确的集合内，以便对入参进行约束。





### **增强类型**

interface  重载，后面的优先级更高

decare:    看g了一下概念还是有些模糊





### **类型兼容**

子类型可以给父类类型赋值，因为类型缩小了。

顶层unKnown  ->    基础类型 ->  never





### **官方工具类**

*本质是自定义的复杂类型构造器—泛型	*

Partial  属性全部设置为可选

Required  属性全部设置为必选

Readonly  只读

Pick

Omit 

联合类型:  Exclude   Extract

NonNullable   在联合类型中去除null和unfined的类型 

Record





### **打造属于自己的工具类型**

使用泛型进行变量抽离，逻辑封装其实就是在造类型的轮子。

把确切的类型抽离为入参，然后封装成一个可复用的泛型。

```
//一个判断是否有继承关系的泛型，利用三元运算符
 type isSubTying<Child, Par> = Child extends Par ? true : false;
```

type 常用于定义泛型，给类型赋别名

索引访问类型

typeof  在类型上下文中获取变量或者属性的类型。任何未显式添加类型注解或值与类型注解一体（比如函数、类）的变量或属性，我们都可以使用 typeof 提取它们的类型，方便，有用。in 和  typeof都只能在类型别名中组合使用。

我们还可以在映射类型的索引签名中使用类型断言

实例：自定义工具类 merge

```
 type Merge<A, B> = {
    [key in keyof A | keyof B]: key extends keyof A
      ? key extends keyof B
        ? A[key] | B[key]
        : A[key]
      : key extends keyof B
      ? B[key]
      : never;
  };
  type Merged = Merge<{ id: number; name: string }, { id: string; age: number }>;
```

自定义工具了 equal	,能使识别any,也能处理条件分配类型和never的问题

```
 type IsAny<T> = 0 extends (1 & T) ? true : false;
  type EqualV3<S, T> = IsAny<S> extends true
    ? IsAny<T> extends true
      ? true
      : false
    : IsAny<T> extends true
    ? false
    : [S] extends [T]    // []能解除条件分配
    ? [T] extends [S]
      ? true
      : false
    : false;
```

名词解释： 

分配条件类型：如果参数是联合类型，会被拆解为一个个独立的类型然后分个进行校验

never陷阱： 如果把never作为泛型入参时会出问题，因为never是所有类型的子类型

 [] 解除条件分配类型和 never “陷阱”



### tsconfig.json

定制ts的行为。

compilerOptions {

target

module

jsx

incremental

declaration

sourceMap

lib

strict 为 true 时，一般我们会开启以下编译配置。

> alwaysStrict：保证编译出的文件是 ECMAScript 的严格模式，并且每个文件的头部会添加 'use strict'。
>
> strictNullChecks：更严格地检查 null 和 undefined 类型，比如数组的 find 方法的返回类型将是更严格的 T | undefined。
>
> strictBindCallApply：更严格地检查 call、bind、apply 函数的调用，比如会检查参数的类型与函数类型是否一致。
>
> strictFunctionTypes：更严格地检查函数参数类型和类型兼容性。
>
> strictPropertyInitialization：更严格地检查类属性初始化，如果类的属性没有初始化，则会提示错误。
>
> noImplicitAny：禁止隐式 any 类型，需要显式指定类型。TypeScript 在不能根据上下文推断出类型时，会回退到 any 类型。
>
> noImplicitThis：禁止隐式 this 类型，需要显示指定 this 的类型。

额外检查 eslint

**模块解析** {

moduleResolution

baseUrl

paths

}

}



## ts  7-16补漏



### 类型断言

- 2种写法

  你确定这个当前数据的类型，规避报错

  (someValue **as** string).length

  还有一种写法是   (<string/>someValue).length

- 非空断言 ！ 

  自动规避null 和 undifined,但是最好避免使用，因为只是编译的时候没有报错，运行的时候可能还是会出错

- 确定断言赋值



### 类型守卫

收缩类型

- in

  ` **if** ("privileges" **in** emp) {    console.log("Privileges: " + emp.privileges);  } `

  当我们使用 . 运算符去访问对象是，我们可以添加类型守卫，避免访问异常缺失。不过es10有 `?.`运算符

- typeof

- intanceof

- is



### interface 和 type

- 类型别名可以用原始类型、联合类型、元组
- interface 可以extends type
- type  一般用  & 运算符
- interface 同名的会自动合并



### ts类

static 标识

\# 私有字段前缀

抽象类  abstract  去定义抽象方法，等待实例去实现



### 泛型

写法    , 常用简写值： K V E

```
function identity <T, U>(value: T, message: U) : T {
  console.log(message);
  return value;
}
<> 尖括号写法.     
```

工具：

- typeof
- keyof
- in
- infer  推断 。   =>  infer R ? R : any   如果返回的是R就返回R，不是就默认any
- entends  限定约束

实例， 官方的 Partial<T\> 就是一个例子，把某个类型里的属性全部变为可选项 ？ 

总结：  泛型，就是都将要实现的东西的一种类型约束，他表现了类型之间的关系，如果不进行约束那么默认就是全是any





###  装饰器

> 还是实验性的属性，tsconfig.json 启用 experimentalDecorators  	

是一个表达式，表达式执行后返回一个函数（target, name , descriptor),执行该函数后，可能返回descriptor对象，用于配置target对象

分类

- 类装饰
- 方法
- 属性
- 参数

作用是什么呢？  可以简化一些重复的定义，只需要把装饰器写在上面就行。一些生成打印日志的。





### ts4 新特性

- 构造函数会去推断类的属性，我们属性就可以不用显示声明类型。

- 带标记的元组类型。 (使用该函数时，提示会更加优雅）

  ```
  function addPerson(...args: [name: string, age: number]): void {
    console.log(`Person info: name: ${args[0]}, age: ${args[1]}`);
  }
  ```

  



### 推荐工具

- 可以用 js 的 json 转 ts 插件，我们处理后端的接口的时候就可以非常迅速了

- [ts 演练场](www.typescriptlang.org/play/)
