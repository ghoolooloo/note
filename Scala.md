# REPL

在交互式环境中，可以使用res0、res1、……等变量来访问之前执行的语句的结果，例如：

```shell
scala> 8 * 5 + 2
res0: Int = 42
scala> "Hello, " + res0
res1: String = Hello, 42
```

除了使用res0等这些名称之外，你还可以定义自己的名称：

```shell
scala> val answer = 8 * 5 + 2
answer: Int = 42
```

# 注释

# 声明

## 常量

```scala
val counter = 0  //自动推断类型
val greeting: String = null
val x, y = 100  //声明多个常量
val a, b: String = "Hello"
```

## 变量

```scala
var greeting, message: String = null
greeting = "Hello"
```

# 类型

## 数值类型

Byte

Char、RichChar

Short

Int、RichInt

Long

Float

Double、RichDouble

BigInt

BigDecimal

BigInt和BigDecimal可以使用普通的“+、-、*、/”等等算术操作符：

```scala
val x: BigInt = 1234567890
x * x
```

Scala的数值类型都是类。

```scala
1.to(3)  //Range(1,2,3)
```

## 布尔型

Boolean

## 字符串

String

StringOps

## 数组

## 映射

## 元组

## 类

### 构造器

### 方法

### 属性

### 嵌套类

### 对象

#### 单例对象

#### 伴生对象

#### apply方法

### 继承

### 扩展类

## 特质（trait）

## 集合

## 泛型

## 单例类型

## 类型投影

## 类型别名

## 结构类型

## 复合类型

## 中置类型

## 存在类型

## 自身类型



# 表达式

## 算术表达式

Scala没有`++`和`--`操作符。

## 方法调用

在Scala中，方法调用`a.方法(b)`可以简写为`a 方法 b`：

```scala
1 to 10  //1.to(10)
5 + 8    //5.+(8)
```

方法没有参数时，不需要使用括号。

> 约定：不修改对象的无参方法不带括号，而修改对象的无参方法带括号。

# 控制结构

# 函数

# 操作符

# 包

## 导入

# IO

# 正则表达式

# 模式匹配

## 样例类

# 注解