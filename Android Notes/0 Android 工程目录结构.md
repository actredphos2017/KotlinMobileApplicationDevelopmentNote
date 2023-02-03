
> [!Info] 本笔记的 Android 开发环境为 Google Android Studio 原生开发，使用 Kotlin 语言
> [[0-0 第一个 Kotlin 程序]]

> [!Note]
> ![[Pasted image 20230101152433.png]]
> 一个初始工程清单

> [!Gradle] 
> Gradle 为编译配置文件，使用的语言是 Groovy ，跟 Java / Kotlin 一样是一种基于 JVM 的编译语言
> 但主要是用于对编译进行编译时配置和插件的导入，类似于 CMake

> [!app]
> app 为项目文件，包含以下文件
> 1. 项目配置 `manifests` 
> 2. 项目源码 `java` 
> 3. 项目资源 `res`

# Gradle

## build.gradle

在新创建的项目中有两个 `build.gradle` 一个是项目配置，一个是模块配置

模块依附于项目，每个项目至少有一个模块，也能有多个模块，因此一个项目不一定只有两个 `build.gradle` 文件

一般所言 `编译运行App` ，指的是运行某个模块，而非运行某个项目，因为模块才对应实际的 App

### 项目级 build.gradle

里面详细说明了项目需要导入哪些插件，也可以在这里创建任务

``` groovy
plugins {  
    id 'com.android.application' version '7.3.1' apply false  
    id 'com.android.library' version '7.3.1' apply false  
    id 'org.jetbrains.kotlin.android' version '1.7.20' apply false  
}
```

### 模块级 build.gradle

``` groovy
plugins {  
    id 'com.android.application'  
    id 'org.jetbrains.kotlin.android'  
}  
  
android {  
    namespace 'com.example.myapplication'  
  
    // 指定编译用的SDK版本号  
    // 这里的 32 代表着支持 Android 12    
    compileSdk 32  
  
    defaultConfig {  
        // 指定该模块的应用编号，也就是 App 的包名  
        // 作为该应用的唯一标识，防止跟别的 App 冲突  
        applicationId "com.example.myapplication"  
  
        // 要求SDK最小版本号，这里的 26 代表着最低支持 Android 8
        // 在这种情况下，低于 Android 8 的系统不一定能正常运行
        minSdk 26  
  
        // 指定设备SDK，表示最希望在哪个版本的 Android 上运行  
        targetSdk 32  
  
        // App的应用版本号  
        versionCode 1  
  
        // App的应用版本名称  
        versionName "1.0"  
  
        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"  
    }  
  
    buildTypes {  
        release {  
            minifyEnabled false  
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'  
        }  
    }    compileOptions {  
        sourceCompatibility JavaVersion.VERSION_1_8  
        targetCompatibility JavaVersion.VERSION_1_8  
    }  
    kotlinOptions {  
        jvmTarget = '1.8'  
    }  
    buildFeatures {  
        viewBinding true  
    }  
}  
  
// 指定 App 编译的依赖信息  
dependencies {  
  
    implementation 'androidx.core:core-ktx:1.7.0'  
    implementation 'androidx.appcompat:appcompat:1.4.1'  
    implementation 'com.google.android.material:material:1.5.0'  
    implementation 'androidx.constraintlayout:constraintlayout:2.1.3'  
    implementation 'androidx.navigation:navigation-fragment-ktx:2.4.1'  
    implementation 'androidx.navigation:navigation-ui-ktx:2.4.1'  
    testImplementation 'junit:junit:4.13.2'  
    androidTestImplementation 'androidx.test.ext:junit:1.1.3'  
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'  
}
```

## 其他文件

1.  `proguard-rules.pro`  描述 Java 代码的混淆规则
2.  `gradle-properties` 配置编译工程的命令行参数，一般不用改动
3.  `settings.gradle` 该文件配置了需要编译哪些模块
4.  `local.properties` 项目的本地配置文件

# app

## manifests/AndroidManifest.xml

`AndroidManifest.xml` 为项目的清单文件，是 App 的运行配置文件，系统需要根据里面的内容运行 App 的代码，显示界面

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
        android:theme="@style/Theme.MyApplication"  
        tools:targetApi="31">  
        <activity  
            android:name=".MainActivity"  
            android:exported="true"  
            android:label="@string/app_name"  
            android:theme="@style/Theme.MyApplication.NoActionBar">  
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

### application 参数

> [!Note] 基本参数
> - `android:allowBackup` ，是否允许应用备份。允许用户备份系统应用和第三方应用的 apk 安装包和应用数据，以便在刷机或者数据丢失后恢复应用，用户即可通过 `adb backup` 和 `adb restore` 来进行对应用数据的备份和恢复。为 `true` 表示允许，为 `false` 则表示不允许。
> - `android:icon` ，指定 App 在手机屏幕上显示的图标。
> - `android:label` ，指定 App 在手机屏幕上显示的名称。
> - `android:roundIcon` ，指定 App 的圆角图标。
> - `android:supportsRtl` ，是否支持阿拉伯语/波斯语这种从右往左的文字排列顺序。为 `true` 表示支持，为 `false` 则表示不支持。
> - `android:theme`，指定 App 的显示风格。

注意到 `application` 下面还有个 `activity` 节点，它是活动页面的注册声明，只有在 `AndroidManifest.xml` 中正确配置了 `activity` 节点，才能在运行时访问对应的活动页面。初始配置的 MainActivity 正是 App 的默认主页，之所以说该页面是 App 主页，是因为它的 `activity` 节点内部还配置了以下的过滤信息：

``` xml
<intent-filter>  
	<action android:name="android.intent.action.MAIN" />  
	<category android:name="android.intent.category.LAUNCHER" />  
</intent-filter>  
```

其中 `action` 节点设置的 `android.ntent.action.MAIN` 表示该页面是 App 的入口页面，启动 App 时会最先打开该页面。

而 `category` 节点设置的 `android.intent.category.LAUNCHER` 类别决定了是否在手机屏幕上显示 App 图标

如果同时有两个 `activity` 节点内部都设置了 `android.intent.category.LAUNCHER` ，那么桌面就会显示两个App图标。

以上的两种节点规则可能一开始不太好理解，只需记住默认主页必须同时配置这两种过滤规则即可。

> `Activity` 是一个应用程序组件，提供一个屏幕，用户可以用来交互为了完成某项任务

> [!Note]
> 在以上文件中加了 `@` 符号的参数都表示引用
> 在 Android Studio IDE 当中，对于大多数引用，我们可以直接通过 `Ctrl + 左键点击` 找到该引用的目标

## java 与 res

> [!java]
> 在 `java` 文件下有三个名字相同的包，里面存放的是源文件
> 1. com.example.myapplication
> 2. com.example.myapplication (androidTest)
> 3. com.example.myapplication (test)
> 
> 其中第一个为当前模块的源文件，另外两个为测试用的源文件

> [!res]
> res 存放当前模块的资源文件，一般有以下四个常用子目录
> 1. drawable 图片与矢量图片描述文件
> 2. layout 应用页面布局描述文件
> 3. mipmap 存放 app 启动图标
> 4. values 常量自定文件，常用常量文件如下
> 	1. strings.xml 字符串常量
> 	2. dimens.xml 像素常量
> 	3. colors.xml 颜色常量
> 	4. styles.xml 样式风格定义

