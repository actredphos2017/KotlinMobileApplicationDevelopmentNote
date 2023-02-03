[[0 Android 工程目录结构]]

# Activity 的启动和结束

## 启动 Activity

使用 startActivity 函数启动一个 Activity

``` kotlin
startActivity(Intent(this@父活动, 目标活动::class.java))
```

``` java
startActivity(new Intent(父活动.this, 目标活动.class))
```

> [!Info] Intent 意图将会在本节后面 [[#Intent 意图]] 中介绍

## 结束 Activity

> [!Note] 使用 finish() 函数便能结束一个 Activity

# Activity 生命周期

![[Pasted image 20230104194824.png|350]]

| 过程名 | 含义 |
| ---- | ---- |
| onCreate() | 创建活动，对活动的页面和逻辑进行初始化 |
| onStart() | 启动活动，屏幕上开始显示活动 |
| onResume() | 开始或恢复屏幕上的活动页面 |
| onPause() | 活动被暂停，无法进行交互，随时可以用 onResume() 函数恢复活动 |
| onStop() | 停止活动，界面将会从屏幕上隐藏，而活动占用的内存会被托管，当**更高优先级的程序需要内存**且**此界面属于非活跃状态**时，会将此活动删除，一旦活动删除后要启动，就需要使用 onCreate() 在一个新的内存分配位置重新创建该活动 |
| onRestart() | 重启活动，活动结束后如果原来占用的内存没有被别的程序占用，那么就在原内存处重新启动该活动，否则将会用 onCreate() 在一个新的内存分配位置重新创建该活动 |
| onDestory() | 销毁活动，直接清空占用的内存 |

> [!Info] 活跃界面的父级界面也属于活跃界面，在 onStop 之后一般不会被删除

## 调用 startActivity 函数时，父级、子级界面实际调用顺序

> [!Note] 顺序
> **程序启动**
> 1. 父界面 `onCreate`
> 2. 父界面 `onStart`
> 3. 父界面 `onResume`
> 
> **切换界面**
> 1. 父界面 `onPause`
> 2. 子界面 `onCreate`
> 3. 子界面 `onStart`
> 4. 子界面 `onResume`
> 5. 父界面 `onStop`
> 
> **按下返回键**
> 1. 子界面 `onPause`
> 2. 父界面 `onRestart`
> 3. 父界面 `onStart`
> 4. 父界面 `onResume`
> 5. 子界面 `onStop`
> 6. 子界面 `onDestroy`

### 测试代码

#### 父界面
``` kotlin
package com.sakuno.helloworld  
  
import android.app.Activity  
import android.content.ContentValues.TAG  
import android.content.Intent  
import android.os.Bundle  
import android.util.Log  
import android.widget.Button  
  
class MainActivity : Activity() {  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_main)  
  
        findViewById<Button>(R.id.jumpButton).setOnClickListener {  
            startActivity(  
                Intent(  
                    this@MainActivity,  
                    SwitchInterfaceTestActivity::class.java  
                )  
            )  
        }  
        Log.d(TAG, "MainAct onCreate")  
    }  
  
    override fun onStart() {  
        super.onStart()  
        Log.d(TAG, "MainAct onStart")  
    }  
  
    override fun onResume() {  
        super.onResume()  
        Log.d(TAG, "MainAct onResume")  
    }  
  
    override fun onPause() {  
        super.onPause()  
        Log.d(TAG, "MainAct onPause")  
    }  
  
    override fun onStop() {  
        super.onStop()  
        Log.d(TAG, "MainAct onStop")  
    }  
  
    override fun onRestart() {  
        super.onRestart()  
        Log.d(TAG, "MainAct onRestart")  
    }  
  
    override fun onDestroy() {  
        super.onDestroy()  
        Log.d(TAG, "MainAct onDestroy")  
    }  
}
```

#### 子界面
``` kotlin
package com.sakuno.helloworld  
  
import android.content.ContentValues  
import androidx.appcompat.app.AppCompatActivity  
import android.os.Bundle  
import android.util.Log  
  
class SwitchInterfaceTestActivity : AppCompatActivity() {  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        Log.d(ContentValues.TAG, "ChildAct onCreate")  
        setContentView(R.layout.activity_switch_interface_test)  
    }  
  
    override fun onStart() {  
        super.onStart()  
        Log.d(ContentValues.TAG, "ChildAct onStart")  
    }  
  
    override fun onResume() {  
        super.onResume()  
        Log.d(ContentValues.TAG, "ChildAct onResume")  
    }  
  
    override fun onPause() {  
        super.onPause()  
        Log.d(ContentValues.TAG, "ChildAct onPause")  
    }  
  
    override fun onStop() {  
        super.onStop()  
        Log.d(ContentValues.TAG, "ChildAct onStop")  
    }  
  
    override fun onRestart() {  
        super.onRestart()  
        Log.d(ContentValues.TAG, "ChildAct onRestart")  
    }  
  
    override fun onDestroy() {  
        super.onDestroy()  
        Log.d(ContentValues.TAG, "ChildAct onDestroy")  
    }  
}
```

#### 运行日志
![[Pasted image 20230104201818.png|175]]

# Activity 的启动方式

> [!Info] 一个父 Activity 与子 Activity 在内存上为栈关系，该栈称之为 **Task 栈** ，当该 Task 栈的栈顶 Activity 处于活跃状态时，该栈称之为**活动栈**

该启动方式可以在 `AndroidManifest.xml` 清单文件中对某个 Activity 进行今天单独设置

``` xml
<android android:name=".SecondActivity" android:launchMode="standard"/>
```

## 四个启动方式

### 默认启动方式 standard

> [!Note] 
> 该模式为 Manifest 的默认设定。在该模式下，启动的 Activity 会依照启动顺序被依次压入Task 栈中

![[Pasted image 20230104203910.png]]

### 栈顶复用 singleTop

> [!Note] 在该模式下，如果栈顶 Activity 为我们要新建的 Activity  ( 目标 Activity ) ，那么就不会重复创建新的 Activity 。

下图中的 Activity2 为 singleTop 模式

![[Pasted image 20230104204326.png]]

### 栈内复用 singlaTask

> [!Note]
> 与 singleTop 模式相似，只不过 singleTop 模式是只是针对栈顶的元素，而 singleTask 模式下，如果 Task 栈内存在目标 Activity 实例，则将 task 内的对应 Activity 实例之上的所有 Activity 弹出栈，并将对应 Activity 置于栈顶，获得焦点。

下图中的 Activity2 为 singleTop 模式

![[Pasted image 20230104204619.png]]

### 全局唯一 singleInstance

> [!Note] 在该模式下，我们会为目标 Activity 创建一个新的 Task 栈，将目标 Activity 放入新的 Task 栈 ，并让目标 Activity 获得焦点。新的 Task 栈有且只有这一个 Activity 实例。如果已经创建过目标 Activity 实例，则不会创建新的 Task 栈，而是将以前创建过的 Activity 唤醒。

下图中的 Activity2 为 singleTop 模式

![[Pasted image 20230104204910.png]]

## 使用代码动态设置模式

> [!Note] Intent 对象中还存在 flags 参数，该参数可临时设置准备启动的应用将以什么模式启动

``` kotlin
val intent = Intent(this@MainActivity, SecondActivity::class.java)

intent.setFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP)
```

| lntent 类的启动标志 | 说明 |
| ---- | ---- |
| Intent.FLAG_ACTIVITY_NEW_TASK | 开辟一个新的任务栈，该值类似于 `launchMode="standard"` ；不同之处在于，如果原来不存在活动栈，则 FLAG_ACTIVITY_NEW_TASK 会创建一个新栈 |
| Intent.FLAG_ACTIVITY_SINGLE_TOP | 当栈顶为待跳转的活动实例之时，则重用栈顶的实例。该值等同于 `launchMode="singleTop"` |
| Intent.FLAG_ACTIVITY_CLEAR_TOP | 当栈中存在特跳转的活动实例时，则重新创建一个新实例，并清除原实例上方的所有实例。该值与 `launchMode="singleTask"` 类似，但 singleTask 采取 onNewIntent 方法启用原任务,而 FLAG_ACTIVITY_CLEAR_TOP 采取先调用 onDestroy 再调用 onCreate 来创建新任务 |
| Intent.FLAG_ACTIVITY_NO_HISTORY | 该标志与 `launchMode="standard"` 情况类似，但栈中不保存新启动的活动实例。这样下次无论以何种方式再启动该实例，也要走 standard 模式的完整流程 |
| Intent.FLAG_ACTIVITY_CLEAR_TASK | 该标志非常暴力，跳转到新页面时，钱中的原有实例都被清空。注意该标志需要结合 FLAG_ACTIVITY_NEW_TASK 使用，即 setFlags 方法的参数为 Intent.FLAG_ACTIVITY_CLEAR_TASK \| Intent.FLAG_ACTIVITY_NEW_TASK |

# Intent 意图

> [!Note] Intent 意图类可以实现许多活动、服务、通信对象之间的操作过程，包括活动切换，设置活动的临时启动模式，活动间传递信息等功能
> Intent是各个组件之间信息沟通的桥梁，它用于 Android 各组件之间的通信，主要完成下列工作:
>  - 标明本次通信请求从哪里来、到哪里去、要怎么走。
>  - 发起方携带本次通信需要的数据内容，接收方从收到的意图中解析数据。
>  - 发起方若想判断接收方的处理结果，意图就要负责让接收方传回应答的数据内容。


| 元素名称 | 设置方法 | 说明与用途 |
| ---- | ---- | ---- |
| Component | setComponent | 组件，它指定意图的来源与目标 |
| Action | setAction | 动作，它指定意图的动作行为 |
| Data | setData | 即 Uri ，它指定动作要操纵的数据路径 |
| Category | addCategory | 类别，它指定意图的操作类别 |
| Type | setType | 数据类型，它指定消息的数据类型 |
| Extras | putExtras | 扩展信息，它指定装载的包裹信息 |
| Flags | setFlags | 标志位，它指定活动的启动标志 |

## 显式 Intent

> [!Note] 显式 Intent ，直接指定来源活动与目标活动，属于精确匹配。它有三种构建方式：
> - 在 Intent 的构造函数中指定。
> - 调用意图对象的 setClass 方法指定。
> - 调用意图对象的 setComponent 方法指定。

``` kotlin
val intent1 = Intent(this@当前活动, 目标活动::class.java)

val intent2 = Intent()
intent2.setClass(this@当前活动, 目标活动::class.java)

val component = ComponentName(this@当前活动, 目标活动::class.java)
val intent3 = Intent()
intent3.setComponent(component)
```

> [!Info] Component 功能更为强大，除了设置以上活动之外，还能指定活动包名，呼出第三方应用 `ComponentName(pkg: String, cls: String)` 但前提是知道第三方应用的类名

## 隐式 Intent

> [!Note] 隐式Intent，没有明确指定要跳转的目标活动，只给出一个动作字符串让系统自动匹配，属于模糊匹配
> 通常 App 不希望向外部暴露活动名称，只给出一个事先定义好的标记串，这样大家约定俗成、按图索骥就好，隐式 Intent 便起到了标记过滤作用。这个动作名称标记串，可以是自己定义的动作，也可以是已有的系统动作。

| Intent类的系统动作常量名 | 系统动作的常量值 | 说明 |
| ---- | ---- | ---- |
| ACTION_MAIN | android.intent.action.MAIN | App 启动时的入口 |
| ACTION_VIEW | android.intent.action.VIEW | 向用户显示数据 |
| ACTION_SEND | android.intent.action.SEND | 分享内容 |
| ACTION_CALL | android.intent.action.CALL | 直接拨号 |
| ACITON_DIAL | android.intent.action.DIAL | 准备拨号 |
| ACTION_SENDTO | android.intent.action.SENDTO | 发送短信 |
| ACTION_ANSWER | android.intent.action.ANSWER | 接听电话 |

> 以下代码实现点按按钮后给 12345 打电话

``` kotlin
class IntentTest : AppCompatActivity(), OnClickListener {  
    override fun onCreate(savedInstanceState: Bundle?) {  

		private var phoneNum = "12345"

        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_intent_test)  
  
        findViewById<Button>(R.id.btn).setOnClickListener {  
			val intent = Intent()  
			val uri = Uri.parse("tel:$phoneNum")  
			intent.action = Intent.ACTION_DIAL  
			intent.data = uri  
			startActivity(intent)  
		}    
    }  
}
```

### 为自己的应用设置启动标签

> [!Info] 以 [[8 实例：简易计算器]] 为例设置启动标签

打开一个自己的应用项目，打开 AndroidManifest.xml 如下

``` xml
<?xml version="1.0" encoding="utf-8"?>  
<manifest xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools">  
  
    <application  
        android:allowBackup="true"  
        android:dataExtractionRules="@xml/data_extraction_rules"  
        android:fullBackupContent="@xml/backup_rules"  
        android:icon="@mipmap/ic_launcher"  
        android:label="@string/app_name"  
        android:roundIcon="@mipmap/ic_launcher_round"  
        android:supportsRtl="true"  
        android:theme="@style/Theme.EasyCalculater"  
        tools:targetApi="31">  
        <activity  
            android:name=".MainActivity"  
            android:exported="true">  
            <intent-filter>  
                <action android:name="android.intent.action.MAIN" />  
  
                <category android:name="android.intent.category.LAUNCHER" />  
            </intent-filter>  
  
            <meta-data  
                android:name="android.app.lib_name"  
                android:value="" />  
        </activity>  
    </application>  
  
</manifest>
```

与 `<intent-filter>` 同级处创建一个新的 `<intent-filter>` 内容如下

``` xml
<intent-filter>  
    <action android:name="android.intent.action.NING" />  
  
    <category android:name="android.intent.category.DEFAULT" />  
</intent-filter>
```

其中 `android.intent.action.NING` 就是你这个应用的隐式调用名

category 为该调用明对应本程序的操作类别类别 

``` kotlin
intent = Intent()  
intent.action = "android.intent.action.NING"  
intent.addCategory(Intent.CATEGORY_DEFAULT)  
startActivity(intent)
```

## 传递信息

### 发送信息

> [!Note] 使用 putExtra 或 putExtras 函数为一个 Intent 对象放置信息

``` kotlin
val intent = Intent(this@父进程, 目标进程::class.java)

intent.putExtra("request_time", SimpleDateFormat("HH:mm:ss").format(Date()))

startActivity(intent)
```

> [!Info] 也可以使用 Bundle (数据包) 对象作为载体

``` kotlin
val intent = Intent(this@父进程, 目标进程::class.java)

val bundle = Bundle()

bundle.putString("request_username", "Donny233")
bundle.putString("request_password", "abc123")

intent.putExtras(bundle)

startActivity(intent)
```

### 接收信息

``` kotlin
val bundle = this.intent.extras
val username_str = bundle?.get("request_username")?.toString() ?: "_NOT_GIVE"
val password_str = bundle?.get("request_password")?.toString() ?: "_NOT_GIVE"
```

## 返回信息

> [!Info] 返回信息的实现需要父级活动的支持

### startActivityForResult

> [!Info] 该方法只能返回一个 Int 类的值

> [!Note] 使用方法
> `startActivityForResult(intent: Intent, requestCode: Int)`
> - 此方法除了意图之外还需要一个请求码 `requestCode`
> 该请求码用于区别返回数据的活动
> 建议使用大于 3 的数作为请求码
> - 覆写 `onActivityResult` 函数
> 该函数会在从子活动结束后执行
> 生成的三个参数中
> `requestCode` 为请求码，与 startActivityForResult 函数中的请求码是一致的
> `resultCode` 为结果码，由子活动返回
> - 在子活动中使用 `setResult` 函数设置返回值，然后用 `finish` 结束活动

官方提供了三种例值
| 结果值 | 对应值 |
| ---- | ---- |
| RESULT_OK | -1 |
| RESULT_CANCELED (默认) | 0 |
| RESULT_FIRST_USER | 1 |

``` kotlin
package com.sakuno.helloworld  
  
class MainActivity : Activity() {  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_main)  
  
        findViewById<Button>(R.id.jumpButton).setOnClickListener {  
            val intent = Intent(this@MainActivity, IntentTest::class.java)  
            startActivityForResult(intent, 3)  
        }  
    }  
  
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {  
        super.onActivityResult(requestCode, resultCode, data)  
        when(requestCode) {  
            3 -> findViewById<TextView>(R.id.textView).text = "子活动返回 $resultCode"  
        }  
    }  
}
```

子活动代码如下

``` xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_height="match_parent"  
    android:layout_width="match_parent"  
    android:orientation="vertical"  
    android:gravity="center">  
  
    <Button  
        android:id="@+id/okBtn"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="返回 RESULT_OK"        android:textSize="24sp">  
  
    </Button>  
  
    <Button  
        android:id="@+id/cancelBtn"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="返回 RESULT_CANCELED"        android:textSize="24sp">  
  
    </Button>  
  
</LinearLayout>
```

``` kotlin
class IntentTest : AppCompatActivity(), OnClickListener {  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_intent_test)  
  
        val username = intent.extras?.get("user_name")?.toString()  
  
        findViewById<Button>(R.id.okBtn).setOnClickListener(this)  
        findViewById<Button>(R.id.cancelBtn).setOnClickListener(this)  
    }  
  
    override fun onClick(v: View?) {  
        when(v!!.id) {  
            R.id.okBtn -> {  
                setResult(RESULT_OK)  
                finish()  
            }  
            R.id.cancelBtn -> {  
                setResult(RESULT_CANCELED)  
                finish()  
            }  
        }  
    }  
}
```