# 注释

```kotlin
// 这是一个行注释

/* 这是一个多行的
   块注释。 */
```

Kotlin 的块注释可以嵌套。

# 类型

# 表达式

# 控制结构

# 函数

## 定义函数

```kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}
```

将表达式作为函数体、返回值类型自动推断的函数：

```kotlin
fun sum(a: Int, b: Int) = a + b
```

Kotlin的函数都是有返回值的，如果要表示返回值无意义，可以返回Unit类型：

```kotlin
fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
}
```

`Unit` 返回类型可以省略：

```kotlin
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}
```



# 包

## 定义包

包的声明应处于源文件顶部：

```kotlin
package my.demo
```

目录与包的结构无需匹配：源代码可以在文件系统的任意位置。