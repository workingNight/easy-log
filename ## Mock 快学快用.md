## Mock 快学快用

安装使用

cnpm install mockjs  |  当然也可以全局安装  npm install mockjs -g

const Mock = require('mockjs')

```
let data = Mock.mock({

	'list|1-10': [{
		'id|+1':1
	}]

})
```

mock能够以一定规则生成随机数



### 语法规范

```
// 属性名   name
// 生成规则 rule
// 属性值   value
'name|rule': value
```

类型： string, number,布尔型，对象Object,数组，函数，正则表达式

**string**： 规则   min-max    count

**number**： +1   min-max    min-max.dmin-dmax 浮点数

**bool**:        配置   1   、   min-max    概率   min / (min+max)

**array**      'name|1': array    从数组中随机选一个   +1 按顺序选   min-max  count 是通过重复属性值

**reg**          正则，当然是输出正则匹配的



### 占位符

@占位符   比如  @city

| Basic         | boolean, natural, integer, float, character, string, range, date, time, datetime, now |
| ------------- | ------------------------------------------------------------ |
| Image         | image, dataImage                                             |
| Color         | color                                                        |
| Text          | paragraph, sentence, word, title, cparagraph, csentence, cword, ctitle |
| Name          | first, last, name, cfirst, clast, cname                      |
| Web           | url, domain, email, ip, tld                                  |
| Address       | area, region, city                                           |
| Helper        | capitalize, upper, lower, pick, shuffle                      |
| Miscellaneous | guid, id                                                     |



### mock函数

mock(rurl,  rtype,  template | funtion(options))

可以拦截匹配的rurl ajax请求，把数据模板作为结果返回



### 配置项

```
Mock.setup({
	timeout: '100-300'
})
默认值是'10-100'
```





### 一些配置实例

看一些配置实例能更快上手

```
 "array|1-10": [
    {
      "foo|+1": 5,
      "bar|1-10": "★"
    }
  ],
  ||
  ||
  ||
   V
"array": [
    {
      "foo": 1,
      "bar": "★★★★★★"
    },
    {
      "foo": 2,
      "bar": "★★★★★★★★★"
    },
    {
      "foo": 3,
      "bar": "★★★★★★★★"
    },
    {
      "foo": 4,
      "bar": "★★★★★★"
    }
  ],
```



```
{
  "base": {
    "range": "@range(3, 7)", 返回一个整型数组。Random.range(3, 7)[3, 4, 5, 6]
    "string": "@string(7, 20)",     返回一个随机字符串
    "character": "@character(\"abcde\")",   返回一个随机字符。
    "float": "@float(60, 100)",
    "integer": "@integer(60, 100)",
    "natural": "@natural(60, 100)",    返回一个随机的自然数（大于等于 0 的整数）
    "boolean": "@boolean"
  },
  "date": {
    "date": "@date",     //"date": "1986-05-10",
    "time": "@time",           //"time": "02:25:21",
    "datetime": "@datetime",      //"datetime": "1976-08-09 14:38:24",
    "now": "@now"          //"now": "2021-07-08 19:42:45"
  },
  "image": {
    "image": "@image(\"200x200\", \"#50B347\", \"#FFF\", \"FastMock\")"
  },
  "color": {
    "color": "@color",        "color": "#79baf2",
    "hex": "@hex",
    "rgb": "@rgb",
    "rgba": "@rgba",
    "hsl": "@hsl"
  },
  "text": {
    "paragraph": "@paragraph(1, 3)",
    "sentence": "@sentence(3, 5)",
    "word": "@word(3, 5)",
    "title": "@title(3, 5)",
    "cparagraph": "@cparagraph(1, 3)",   //"cparagraph": "从南已酸权变据研果果从可。他认南自育科设热事还压领毛打一格全。",
    "csentence": "@csentence(3, 5)",    后面的参数应该是限制多少个
    "cword": "@cword(\"零一二三四五六七八九十\", 5, 7)",
    "ctitle": "@ctitle(3, 5)"              //带c的就是中文
  },
  "name": {
    "first": "@first",
    "last": "@last",
    "name": "@name",
    "cfirst": "@cfirst",
    "clast": "@clast",
    "cname": "@cname"                //姓名
  },
  "web": {
    "url": "@url",
    "domain": "@domain",
    "protocol": "@protocol",
    "tld": "@tld",
    "email": "@email",
    "ip": "@ip"
  },
  "address": {
    "region": "@region",
    "province": "@province",
    "city": "@city(true)",
    "county": "@county(true)",
    "zip": "@zip"
  },
  "helper": {
    "capitalize": "@capitalize(\"hello\")",   //首字母大写
    "upper": "@upper(\"hello\")",
    "lower": "@lower(\"HELLO\")",
    "pick": "@pick([\"h\", \"e\", \"llo\"])",
    "shuffle": "@shuffle([\"h\", \"e\", \"llo\"])"
  },
  "miscellaneous": {
    "id": "@id",                //随机生成18位身份证
    "guid": "@guid",             //生成一个guid
    "increment": "@increment(1000)"
  }
}
```





### 总结

其实核心就3点：

1. 规范  'name|rule': value

2. @占位符，可以直接用上。

3. rule   

   >1. `'name|min-max': value`
   >2. `'name|count': value`
   >3. `'name|min-max.dmin-dmax': value`
   >4. `'name|min-max.dcount': value`
   >5. `'name|count.dmin-dmax': value`
   >6. `'name|count.dcount': value`
   >7. `'name|+step': value`





### 补充

mockjs随机生成的颜色。"color": "@color", 

16进制颜色码，红、绿、蓝 三基色

想要一组红色调的图片，就应该让第一个十六进制数的值大于其他两个数的值。

 \# （150~200）（70-110）（70-110）

同理，浅色蓝绿调。

最后，浅色的图。\# （180-225）（140-255）（120-255）

随机数的分布区间大，三个色的强度都足够大。

```
for (let i = 0; i < 16; i++) {
  	// 利用mockjs的Random随机生成数字并转成十六进制，拼接。
    const a = '#' + Random.integer(180, 255).toString(16) +
      Random.integer(140, 255).toString(16) +
      Random.integer(120, 220).toString(16)
    data.push({
      image: Random.image('140x140', a, ' IMAGE ')
    })
  }
```

不过话说回来，mock的数据，开发中不会去浪费时间弄这个，重要的是红绿蓝编码的思想。



2. JSON.stringify(data, null, 4)

   ```
   JSON.stringify(value[, replacer [, space]])
   中间的是转换函数，最末尾的是缩进空格
   ```

