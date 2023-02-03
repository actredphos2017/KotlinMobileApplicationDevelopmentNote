[[0-0 第一个 Kotlin 程序]]

## 基本用法

Range 表达式是 Kotlin 中比较常用的区间表示方法

例如 `x in num1 .. num2` 表示 `x >= num1 && x <= num2` 即代表着判断一个变量是否在 num1 和 num2 之间的闭区间内

``` kotlin
var score = 80  
  
if(score == 0)  
    println("零分！")  
else if(score in 1 .. 59)  
    println("不及格")  
else if(score in 60 .. 74)  
    println("及格")  
else if(score in 75 .. 89)  
    println("良好")  
else if(score in 90 .. 100)  
    println("优秀")  
else if(score !in 0 .. 100)  
    println("成绩不在分数区间内")
```

在 Kotlin 中 switch 语法已被更强大的 when 语法所替代

例如以下语句可完全替代上面的语句

``` kotlin
when(score){  
    0 -> println("零分！")  
    in 1 .. 59 -> println("不及格")  
    in 60 .. 74 -> println("及格")  
    in 75 .. 89 -> println("良好")  
    in 90 .. 100 -> println("优秀")  
    else -> println("成绩不在分数区间内")  
}
```

在 Kotlin 中， for 语句已完全成为单一功能的 for each 语句

``` kotlin
val aArray = intArrayOf(1, 2, 3, 4, 5)

for(it in aArray)  
    println(it)
```

但可以用 range 语法来比较方便地实现循环功能

``` kotlin
for(i in 0..4)
	println(aArray[i])
```

## Range 高级用法

### 使用 `until` 来实现左闭右开区间

``` kotlin
for(i in 0 until 10)
	print("$i ")
```

输出

```
0 1 2 3 4 5 6 7 8 9 
```

### 使用 `downTo` 获取逆序输出

``` kotlin
for(i in 10 downTo 0)
	print("$i ")
```

输出

```
10 9 8 7 6 5 4 3 2 1 0 
```

### 使用 `step` 设置步长

``` kotlin
for(i in 0 .. 10 step 2)
	print("$i ")
```

输出

```
0 2 4 6 8 10
```