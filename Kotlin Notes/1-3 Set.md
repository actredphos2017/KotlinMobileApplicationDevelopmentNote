[[1-0 容器类]]

> Set 集合不会出现重复元素

# Set 集合

### 使用 `setOf` 获取一个 `Set` 集合

> `setOf` 函数可以传入重复元素，但是重复元素不会被添加进生成的 `Set` 里

``` kotlin
val set = setOf(1, 2, 3, 4, 5, 5)
// [1, 2, 3, 4, 5]
```

### 获取元素

> `Set` 没有重载 `[]` 运算符，因为从实际意义上讲它不应具备这样的功能
> 但是可以使用 `elementAt` 函数实现这样的功能

``` kotlin
val set = setOf(1, 2, 3, 4, 5, 5)  
println(set.elementAt(0))        // 1
println(set.elementAt(1))        // 2
println(set.elementAt(2))        // 3
println(set.elementAt(3))        // 4
println(set.elementAt(4))        // 5
println(set.elementAtOrNull(5))  // null
```

# MutableSet 可变集合

> 与 List 跟 MutableList 的关系一样，MutableSet 是可以编辑的集合

``` kotlin
val mutableSet = mutableSetOf(1, 2, 3, 4, 5, 5)  
  
mutableSet += 6  
mutableSet.add(6)  
  
mutableSet -= 4  
mutableSet.remove(4)  
  
mutableSet.removeIf { it % 2 == 0 }  
  
mutableSet.forEach { print("$it ") }  
println()
```

输出为

```
1 3 5
```

# SortedSet 排序集合

> 排序集合会保持集合内元素永远有序

``` kotlin
val sortedSet = sortedSetOf(6, 4, 1, 2, 5, 7, 3)  
  
sortedSet.forEach { println(it) }
```

输出

```
1
2
3
4
5
6
7
```