[[1-0 容器类]]

## 字符串模板

> 在 Kotlin 中使用字符串模板可以很方便地构建字符串

``` kotlin
val name = "小红"  
val score = 92f  
println("这次考试${name}考了${score}分")
```

## substring 函数

`substring` 函数接收两个下标 `Int` 或者一个范围 `IntRange`

> 字符串下标的概念 Kotlin 与 Java 一致，在 Java 里已讲，下标指在字符与字符之间

``` java
String str = "abcdef";  
// 下标 0 指在 a 的前面， d 与 e 之间的下标为 4
//  
// 下标  0   1   2   3   4   5   6
// 字符  ^ a ^ b ^ c ^ d ^ e ^ f ^
```

Kotlin 的 `substring` 也是一样

``` kotlin
val str = "abcdef"

val part1 = str.substring(0, 4) // 得到 abcd

val part2 = str.substring(4, str1.length)
```

但在 Kotlin 里也可以使用 Range 表达式

``` kotlin
val str2 = "Hello, World!"

val part3 = str2.substring(0 until str2.indexOf(',')) // Hello
```


## split 函数

`split` 可以获取到指定分割符分割出来的集合

`split` 既可以接收 `Char` 型也可以接收 `String` 型

``` kotlin
val jsonText = "Donny,Tony,Lance,George"

val aList = jsonText.split(',') // aList 为 List<String>

println(aList)

// [Donny, Tony, Lance, George]

```

`split` 可以接收多个分隔符

``` kotlin
val jsonText2 = "Hello! My name is Donny Yi!"  
  
val aList2 = jsonText2.split('!', ' ')  
  
println(aList2)

// [Hello, , My, name, is, Donny, Yi, ]
```

## format 函数

`format()` 可以填充一串 `format` 字符串中待填的内容

`format` 字符串指类似于 `printf` 接着的一串字符串

``` kotlin
var str = "Int:%d \nChar:%c \nDouble:%.2f \nString:%s"  
str = str.format(16, 'f', 16.3124, "Hello")  
println(str)
```

输出

```
Int:16 
Char:f 
Double:16.31 
String:Hello
```

另外如果 `format` 函数参数数量与字符串中实际所需参数数量不一致会抛出异常

## replace 函数

`replace` 可以把一串字符串的某种字符全部换成另一种字符
或者把某种字符集换成另一种字符集

``` kotlin
val text3 = "This is an apple"  

println(text3.replace('a', 'o'))  
// This is on opple

println(text3.replace("is", "IS"))
// ThIS IS an apple
```

除此之外， `replace` 还支持对所支持的字符集进行逐一匿名函数修改

``` kotlin
val pwd = "PASSWORD"  
  
val codedPWD = pwd.replace(Regex("[PSW]")) { it.hashCode().toString() }  
  
println(codedPWD)
// 997608398A197333689312128998361174290147ORD
```

以上代码的作用是将字符串中 `P S W` 三种字符逐一进行哈希编码，然后以字符串的方式进行连接

非目标字符不会进行处理

## String 常量池

如下面代码

``` kotlin
val str1 = "Hello"
val str2 = "Hello"

println(str1 === str2) // true
```

一个 `String` 对象的创建都会在一个常量池里声明这个实例

在此之后对于新的 `String` 对象定义时，如果在这个常量池中存在该对象的实例，便会直接引用它

因此以上代码的 `str1` 与 `str2` 指向的是同一个实例

可以通过一些办法摆脱常量池的束缚

``` kotlin
val str1 = "Hello"  
val strBuild2 = StringBuilder()  
val str2: String  
  
strBuild2.append("Hello")  
str2 = strBuild2.toString()  
  
println(str1 == str2)   // true
println(str1 === str2)  // false
```

但没什么必要，反而占用了资源（笑）

## 遍历每一个字符

Kotlin 中无法使用 `for` 语句对 `String` 类型进行遍历

但是可以使用 `String` 自带的 `forEach` 函数进行遍历

``` kotlin
val str = "Hello"  
str.forEach {  
    println("正在遍历字符：$it")  
}
```

输出内容为

```
正在遍历字符：H
正在遍历字符：e
正在遍历字符：l
正在遍历字符：l
正在遍历字符：o
```