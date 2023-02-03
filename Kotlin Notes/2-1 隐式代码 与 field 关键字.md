[[2-0 类的定义]]

# 定义类

> Kotlin 通过以下方式定义类

``` kotlin
class KtBase3 {  
    var name: String = ""  
    var info: String = ""  
    var score: Double = 0.0  
}
```

# 自动生成读写代码

> 与 Java 不同的是， Kotlin 在创建对象的时候会顺带生成该对象的读写函数
> 例如以上代码的字节码反编译为 Java 代码后得到的结果如下

``` java
public final class KtBase3 {  
    @NotNull  
    private String name = "";  
    @NotNull  
    private String info = "";  
    private double score;  
  
    @NotNull  
    public final String getName() {  
        return this.name;  
    }  
  
    public final void setName(@NotNull String var1) {  
        Intrinsics.checkNotNullParameter(var1, "<set-?>");  
        this.name = var1;  
    }  
  
    @NotNull  
    public final String getInfo() {  
        return this.info;  
    }  
  
    public final void setInfo(@NotNull String var1) {  
        Intrinsics.checkNotNullParameter(var1, "<set-?>");  
        this.info = var1;  
    }  
  
    public final double getScore() {  
        return this.score;  
    }  
  
    public final void setScore(double var1) {  
        this.score = var1;  
    }  
}
```

可以看到它对 `name` 、 `info` 、 `score` 三个变量都生成了读写函数

而在外部代码调用时

``` kotlin
fun main(){  
    val base = KtBase3()  
    base.name = "Donny"  
    base.info = "Successful Person"  
    base.score = 98.5  
  
    println(base.name)  
    println(base.info)  
    println(base.score)  
}
```

生成的字节码的反编译代码如下

``` java
public static final void main() {  
   KtBase3 base = new KtBase3();  
   base.setName("Donny");  
   base.setInfo("Successful Person");  
   base.setScore(98.5);  
   String var1 = base.getName();  
   System.out.println(var1);  
   var1 = base.getInfo();  
   System.out.println(var1);  
   double var3 = base.getScore();  
   System.out.println(var3);  
}
```

可以看到字节码并没有直接处理成员变量，而是一直在调用 `set` 与 `get` 函数

# 类内实际存在的隐式代码

> 在一个类内其实在编译过程中默认生成了一些隐式代码
> 实际编写时不需要写下来

原代码

``` kotlin
class KtBase3 {  
    var name = ""  
}
```

编译时代码

``` kotlin
class KtBase3 {  
    var name = ""  
        get() = field  
        set(name) {  
            field = name  
        }  
        
}
```

其中的 `field` 关键字为变量本身

# 对默认隐式代码进行修改

> [!Info] 以下代码类中的 name 在编辑时可以自动去掉空格，获取时自动设为开头大写

``` kotlin
class KtBase3 {  
    var name = ""  
        get() = field.lowercase().replaceFirstChar { it.uppercaseChar() }  
        set(value) {  
            field = value.replace(" ", "")  
        }  
}  
  
fun main(){  
    val base = KtBase3()  
    base.name = "d on n   y"  // 调用 set 函数
    println(base.name)        // 调用 get 函数
    // Donny
}
```

> [!Warning] val 不能使用 set 函数

``` kotlin
class aClass {  
    val number = 0  
        get() = field + 1  
}
```

# 只读变量

> [!Info] 可以对一个变量的 set 函数设为私有，随后便能实现在类内能修改，类外不能修改

``` kotlin
class Person {  
    var age = 18  
        private set  
  
    fun setPersonAge(newAge: Int) {  
        age = if (newAge > 0) newAge else age  
    }  
}  
  
fun main(args: Array<String>) {  
    val person = Person()  
    // person.age = 8           // 写入时报错  
    person.setPersonAge(16)  
    person.setPersonAge(-2)  
    person.age.run(::println)   // 读取正常  
}
```
