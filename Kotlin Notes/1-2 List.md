[[1-0 容器类]]

# List 列表

### 使用 `listOf` 函数获取一个 `List`

> `listOf<T>(vararg elements: T)` 将返回一个以输入参数集构成的一个 List

``` kotlin
val list: List<Double> = listOf<Double>(87.0, 76.5, 68.0, 92.0, 81.0)
```

`listOf` 传入的参数支持类型推断

以上代码可简写为

``` kotlin
val list = listOf(87.0, 76.5, 68.0, 92.0, 81.0)
```

### 取值方法

#### 使用 `[]` 或 `get` 获取对应下标的 `List` 成员

``` kotlin
list[0]       // 第一个值
list.get(3)   // 第四个值

// 越界
list[5]       // 第六个值（不存在）
list.get(10)  // 第十一个值（不存在）
// 抛出异常 java.lang.ArrayIndexOutOfBoundsException
```

#### 使用 `getOrElse` 或 `getOrNull` 安全取值

> `getOrElse` 函数接收一个索引和一个匿名函数参数
> 如果越界，将会执行匿名函数，这个匿名函数可以得到用户传入的索引，同时需要返回一个与 `List` 类型一致的值

``` kotlin
val list = listOf(87.0, 76.5, 68.0, 92.0, 81.0)  
list.getOrElse(6) {  
    println("你想获取的第${it + 1}个元素不存在")  
    println("已自动返回 0")  
    0  
}
```

> `getOrNull` 函数接收一个索引
> 当越界时返回为 `null`

``` kotlin
list.getOrNull(6).run(::println)  // null
```

### 遍历方法

> 可以使用 `for` 语句遍历

``` kotlin
val list = listOf(1, 2, 3, 4, 5)  
for (i in list)  
    println(i)
```

> 也可以使用内置的 `forEach` 函数遍历

``` kotlin
val list = listOf(1, 2, 3, 4, 5)  
list.forEach { it.run(::println) }
```

> 使用 `forEachIndexed` 函数同时遍历下标与元素

``` kotlin
val list = listOf(87.0, 76.5, 68.0, 92.0, 81.0)  
list.forEachIndexed{ item, index ->  
    println("下标：${index}, 元素：${item}")  
}
```

输出

```
下标：87.0, 元素：0
下标：76.5, 元素：1
下标：68.0, 元素：2
下标：92.0, 元素：3
下标：81.0, 元素：4
```

> 注：以上遍历均为只读遍历

### 查值方法

> 可以使用 `indexOf` 获取某个值存在的位置下标

``` kotlin
val list = listOf(1, 3, 3, 4, 5)  
list.indexOf(3)      // 1
list.lastIndexOf(3)  // 2
```

> 可以使用 `indexOfFirst` 或者 `indexOfLast` 获取符合条件的值

``` kotlin
val list = listOf(87.0, 76.5, 68.0, 92.0, 81.0) 

list.indexOfFirst { it >= 80 }  // 0
list.indexOfLast { it >= 80 }   // 4
```

### 去重方法

> 转 `Set` 去重

``` kotlin
val list = listOf(1, 1, 2, 3, 3, 4, 5, 6, 6)  
  
list.toSet().toList().run(::println)
// [1, 2, 3, 4, 5, 6]
```

> 使用 `distinct()` 函数去重

``` kotlin
val list = listOf(1, 1, 2, 3, 3, 4, 5, 6, 6)  
  
list.distinct().run(::println)  
// [1, 2, 3, 4, 5, 6]
```

# MutableList 可变列表

> `MutableList` 可变列表具有很多对列表元素进行增删改查的函数

### 使用 `mutableListOf` 获取一个 `MutableList`

``` kotlin
val list = mutableListOf(87.0, 76.5, 68.0, 92.0, 81.0)
```

### 使用 `toList` 和 `toMutableList` 实现两种 `List` 的互相转换

``` kotlin
val mutableList = mutableListOf(87.0, 76.5, 68.0, 92.0, 81.0)  
val list = listOf(87.0, 76.5, 68.0, 92.0, 81.0)  

val baseList = mutableList.toList()  
val specialList = list.toMutableList()
```

### 修改可变列表

#### 使用 `add` 与 `remove` 函数添加 / 删除元素

``` kotlin
val list = mutableListOf(87.0, 76.5, 68.0, 92.0, 81.0)  

list.add(100.0)   // 在列表尾部添加元素
list.add(3, 0.0)  // 在下标为3的位置插入元素
println(list)
// [87.0, 76.5, 68.0, 0.0, 92.0, 81.0, 100.0]

list.remove(92.0)         // 找到并删除元素
list.removeAt(2)          // 删除第二个元素
list.removeIf { it < 60 } // 匿名函数返回 true 时删除
println(list)
// [87.0, 76.5, 81.0, 100.0]

list.remove(92.0)  // 如果没有找到指定元素，那么列表不会有任何变化

// 越界  
list.add(100, 15.5)  
list.add(-1, 9.5)  
list.removeAt(100)  
list.removeAt(-1)
// 抛出异常 java.lang.IndexOutOfBoundsException
```

#### 使用重载运算符添加、删除元素

``` kotlin
val list = mutableListOf(87.0, 23.5, 45.0, 92.0, 81.0)  
  
list += 51.5   // 与 add 函数作用一致
list -= 45.0   // 与 remove 函数作用一致
// [87.0, 23.5, 92.0, 81.0, 51.5]

list -= 110.5  // 没有找到指定元素，那么列表不会有任何变化
// [87.0, 23.5, 92.0, 81.0, 51.5]
```

#### 使用下标修改元素

> 可变列表的运算符 `[]` 返回的是可改类型，这点与 `List` 不同

``` kotlin
val list = mutableListOf(87.0, 76.5, 68.0, 92.0, 81.0)  
list[1] = 15.5
// [87.0, 15.5, 68.0, 92.0, 81.0]
```

#### 使用 `sort` 排序元素

``` kotlin
val list = mutableListOf(87.0, 76.5, 68.0, 92.0, 81.0) 

list.sort()
// [68.0, 76.5, 81.0, 87.0, 92.0]
```

#### 使用 `replaceAll` 函数编辑元素

``` kotlin
val list = mutableListOf(87.0, 23.5, 45.0, 92.0, 81.0) 

list.replaceAll { if (it >= 60.0) 100.0 else it }  
// [100.0, 23.5, 45.0, 100.0, 100.0]
```

#### 使用 `shuffled` 打乱元素

``` kotlin
val list = (1..10).toList().shuffled()
println(list)
```