[[1 第一个 Android 应用]]

# activity_main.xml 布局文件

> [!Info] 该文件处于 `res/layout` 文件夹内，属于布局文件

``` xml
<?xml version="1.0" encoding="utf-8"?>  
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:app="http://schemas.android.com/apk/res-auto"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    tools:context=".MainActivity">  
  
    <TextView  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Hello World!"  
        app:layout_constraintBottom_toBottomOf="parent"  
        app:layout_constraintEnd_toEndOf="parent"  
        app:layout_constraintStart_toStartOf="parent"  
        app:layout_constraintTop_toTopOf="parent" />  
  
</androidx.constraintlayout.widget.ConstraintLayout>
```

> [!Info]
> XML 文件为一种较为传统的数据交换格式
> 其基本语法如下

``` xml
<?xml version="XML版本号" encoding="文字编码"?>

<结构名1 结构1主参数名 = 值
	 结构1参数1名 = 值
	 结构1参数2名 = 值>

	<结构名2 结构2主参数名 = 值
	    结构2参数1名 = 值
	    结构2参数2名 = 值>

	</结构名2>
	
 </结构名1>
```


`androidx.constraintlayout.widget.ConstraintLayout` 为一种约束布局，所属于 `http://schemas.android.com/apk/res/android` 命名空间

> [!Info] `androidx.constraintlayout.widget.ConstraintLayout` 为 Android X 预览版中的 **约束布局**
> 该布局使用较为广泛
> 
> 但前期学习应用其他基本类型布局平替

`xmlns` 全称为 XML Name Space 即 **XML 命名空间**，相当于 C/C++ 的 `using namespace`

其参数的一串网址 `http://schemas.android.com/apk/res/android` 为 **命名空间的名字**

命名空间的声明将影响到 **该结构以及其所有的子结构**

`layout_width="match_parent"` 与 `layout_height="match_parent"` 代表该布局的宽度、高度为随父级匹配，意思是有多大就要多大

`context=".MainActivity"` 


`TextView` 为一种文字显示控件，包含在父级的命名空间里

`layout_width="wrap_content"`  与 `layout_height="wrap_content"`  参数名与上面讲的的参数名是一致的，但是值不同，该值意味着内容自适应，也就是说让该控件量力而行，最小要占用多小给它多小

`text="Hello World!"` 代表该文字控件显示的文字

# MainActivity.kt 源文件

> [!Note] 该文件为 Kotlin 源码文件，用于编写应用的交互逻辑

``` kotlin
package com.sakuno.helloworld  
  
import androidx.appcompat.app.AppCompatActivity  
import android.os.Bundle  
  
class MainActivity : AppCompatActivity() {  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_main)  
    }  
}
```

继承类 `AppCompatActivity` 是一种具有顶栏的窗口类型，默认使用的主题会有顶栏（如左图）

使用 `Activity` 类或者修改主题可以让顶栏消失 （如右图）

![[Screenshot_20230101-173820_Hello World.png|200]] ![[Screenshot_20230101-181732_Hello World.png|200]] 

> [!Info] 
> 如果使用的是老版本 SDK ，这里的主类继承的可能是 `ActionBarActivity` 类
> `AppCompatActivity` 是新版本 SDK 的有顶栏类型，在原有继承上能自适应顶栏的主题色

`onCreate` 函数是该活动的入口

> [!Warning]
> 超类中 `onCreate` 函数有两种，一种有两个参数，一种有一个参数
> IDE 会首选具有两个参数的 `onCreate`
> 初学者应选择一个参数的 `onCreate` ，否则可能出现程序布局无法生成的错误

`setContentView` 为设置设置显示布局函数，传入的参数为 [[#activity_main.xml]] 中的布局

> [!Info]
> `R` 为环境提供的连接器，能直接访问 `res` 文件夹，文件目录应使用 `.` 来代替 `/` ，且不用提供文件后缀
> 这里的 `R.layout.activity_main` 实则访问的是 `res/layout/activity_main.xml`
