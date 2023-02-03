[[0 Android 工程目录结构]]

> [!程序设计规范]
> 利用 XML 标记描绘应用界面，使用 Java 代码书写程序逻辑。

# 生成 Hello World 程序

## 创建项目

在 New Project 中选择 `Empty Activity`

![[Pasted image 20230101163344.png|600]]

编写项目基本信息

![[Pasted image 20230101164239.png|600]]

进入项目目录（首次进入需要下载依赖库，源在国外，请耐心等待）

![[Pasted image 20230101164332.png|775]]

> [!Info]
> 进入后会自动帮你打开两个文件，其中
> 1. `res/layout/activity_main.xml` 文件为生成的布局文件
> 2. `java/包名/MainActivity` 文件为逻辑代码文件

## 配置调试设备

### 虚拟机调试

可以使用 IDE 自带的安卓虚拟机进行调试，也可以使用实体机进行调试

IDE 自带的虚拟机存在优化问题，部分 PC 可能会出现不兼容、运行卡顿或者渲染错误的问题

![[Pasted image 20230101172434.png|550]]

> [!Info] API 代表着运行的 Android SDK 版本

### 实体机调试

电脑需要连接一台 Android 设备进行调试

#### 设备要求

设备需搭载 Android 系统或任何兼容 Android 系统的设备

> [!Info]
> 如果使用的设备内安装的是深度定制的 Android 系统或类 Android 系统，在调试时容易报错，且手机上会出现很多风险提示，这类手机不建议作为调试用设备
> 部分轻度定制 Android 系统或者原生、类原生系统，如 OneUI (三星) 、 OOS / HOS (一加) 、 EdgeOS (摩托罗拉) 、 Android 原生(Google Pixel) ，完全可以作为调试用的主力机


#### 连接调试设备

根据各个系统厂商的不同方法来启用开发者选项，打开USB调试功能，使用数据线连接设备与电脑，在调试测试时勾选信任设备

> [!Info] 
> 如果想使用无线调试，也可以根据  IDE 的无线连接步骤操作
> 该功能需要电脑与调试设备在相同的网络环境
> 部分手机可能不太稳定

在 `Device Manager -> Physical` 下找到自己的设备，如果没有，说明连接异常

![[Pasted image 20230101172124.png]]

绿点表示设备在线， API 代表当前设备

> [!Info] ADB 服务离线与端口占用问题
> 出于 ADB Tools 的节能机制，如果 IDE 挂在后台长时间处于待续状态，连接设备时使用的 ADB Tools 服务会待机或离线
> 
> 或者有其他程序占用了 ADB 服务的端口号 5037(首要) 2080(次要)
> 
> 出现以上问题后 IDE 可能无法连接设备并报错 `adb.exe start-server' failed -- run manually if necessary`
> 
> 这时重启电脑一般可以解决问题，如果问题依旧存在，使用 Windows 的 netstat 工具找到并限制或终止端口占用程序

## 编译运行 Hello World 程序

在 IDE 右上角找到编译菜单

![[Pasted image 20230101172759.png]]

先点击 `绿色的锤子` ，开始构建项目

构建的过程会在下栏里的 Build 菜单里输出日志

![[Pasted image 20230101172933.png]]

> [!Info] 项目首次编译会比较慢，因为编译的同时需要下载和安装依赖

看到日志输出 `BUILD SUCCESSFUL` 说明编译成功

编译完成后选择调试设备，可选虚拟机或者在线实体机

随后点击 `绿色三角形` 运行程序

![[Screenshot_20230101-173820_Hello World.png|250]]

> [!Warning] 为确保程序正常退出，请使用调试工具栏内的红色按钮 ![[Pasted image 20230101174107.png|25]] 来退出程序

