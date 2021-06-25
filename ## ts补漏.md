## ts补漏





**元组**

限制数组元素的个数和类型，用来实现多值返回。

可以参考 React Hooks。const [count, setCount] = useState(0)



**unknown**

描述类型并不确定的变量。如果你需要用any的时候，考虑一下unkonwn

使用时需要类型缩小。

```
let result: unknown;
//这个判断操作就让类型缩小了
if (typeof result === 'number') {
  result.toFixed(); // 此处 hover result 提示类型是 number，不会提示错误
}
```



 **void、undefined、null**

undefined 的最大价值主要体现在接口类型

null 的价值我认为主要体现在接口制定，属性、对象都可能是 null 空对象



**never**

never 表示永远不会有返回值的类型.用于死循环，统一抛错等函数返回值定义。或者应用于属性只读

never 是所有类型的子类型，它可以给所有类型赋值.变量的类型缩小为never



**类型断言**

静态类型对运行时的逻辑无能为力，场景报错：ts2322，不能把类型undifined分配给number

类型断言，告诉ts按照我们的方式做类型检查     语法：xxx  as number

mayNullOrUndefinedOrString!.toString(); // ok非空断言。尽量用类型守卫替换，为空断言，只能在检查时候没有问题，但是不能避免在运行时候出错



**类型推断**

 let x1 = 42; // 推断出 x1 的类型是 number

 /** 根据参数的类型，推断出返回值的类型也是 number */
  function add1(a: number, b: number) {
    return a + b;
  }

函数返回值的类型可以在 TypeScript 中被推断出来，即可缺省，特例：特例需要我们显式声明返回值类型， Generator 函数的返回值。

还能有上下文推断，这个时候类型标注是可以省去的





**字面量类型**

可以进行组合

```
size: 'small' | 'big';
isEnable:  true | false;
margin: 0 | 2 | 4;
```

赋默认值，产生的效果。默认参数 'hello' ，TypeScript 推断出了 x 的类型为 string | undefined。

let strFun = (str = 'this is string') => str; // 类型是 (str?: string) => string;



**类型推广和类型收缩**

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



**类型谓词**

```
我们通过“参数名 + is + 类型”的格式明确表明了参数的类型，进而引起类型缩小
function isString(s): s is string { // 类型谓词
  return typeof s === 'string';
}
```



**类类型**

派生类如果包含一个构造函数，必须在构造函数中调用 super() 方法。super会调用基类的构造函数

只读修饰符： public readonly firstName: string;

存取器：  get set.截取对类成员的读写访问

静态属性：静态属性的特性，我们往往会把与类相关的常量、不依赖实例 this 上下文的属性和方法定义为静态属性，从而避免数据冗余，进而提升运行性能。不在实例上，而是通过基类类名访问MyArray.displayName



**接口类型和类型别名**

interface   typeof

类型别名，我们只是给类型取了一个新名字，而不是创建了一个新的类型

interface  可以重复定义，它的属性会叠加，我们可以方便对全局变量，第三方库的类型做拓展



**索引签名**

```
interface LanguageRankInterface {
  [rank: number]: string;
}
```



**联合类型和交叉类型**

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



**枚举类型**

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



**泛型**

借用 Java 中泛型的释义来回答这个问题：泛型指的是类型参数化，即将原来某种具体的类型进行参数化。和定义函数一样去控制类型，并在调用时给泛型传入明确的类型参数。设计泛型的目的在于有效约束类型成员之间的关系，比如函数参数和返回值、类或者接口成员和方法之间的关系。

关键词： 约束关系，定义函数一样，将类型参数化，调用时再传入明确的参数

所以： *函数的泛型入参必须和参数/参数成员建立有效的约束关系才有实际意义*

泛型约束： 泛型入参限定在一个相对更明确的集合内，以便对入参进行约束。