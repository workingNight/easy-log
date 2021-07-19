# yml 配置基本知识点



后缀yml,  YAML 语言



### 特点：

- 区分大小写
- 缩进表示层级，缩进要用空格缩进
- 缩进的空格数不重要，只要同层级对其即可

### 数据结构

- 对象

  键值对  pets: dog

- 数组

  \- 的语法，  也可以用行内表示法 animal: [Cat, Dog]

  ```
  - dog
  - cat 
  - fish
  ```

- 元数据（scalars：标量)

  > YAML 允许使用两个感叹号，强制转换数据类型。  !!str 123

  - 字符串 
  - 布尔值
  - 整数         number: 12
  - 浮点数     number: 12.30
  - null          trouble: ~ 
  - 时间         iso8601: 2001-12-14t21:59:43.10-05:00 
  - 日期         date:  1976-07-31

#### 复合结构

#### 字符串

字符串默认不用引号，单引号会对特殊字符转义，尽量用双引号自己处理转义

关于换行，| 保留换行符， > 折叠换行，+表示保留文字末尾的换行，-表示删除字符串末尾的换行？

### 引用

& 锚点

*别名

```
defaults: &defaults
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  <<: *defaults

&用来建立锚点（defaults），<<表示合并到当前数据，*用来引用锚点
```

### 正则

