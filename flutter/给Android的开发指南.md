#### Okhttp在flutter中对应物

Flutter 中使用流行的 http 包 进行网络请求是很简单的。

虽然 http 包没有 OkHttp 中的所有功能，但是它抽象了很多通常你会自己实现的网络功能，这使其 本身在执行网络请求时简单易用。

#### 如何为耗时任务显示进度条
在 Flutter 中，我们使用 ProgressIndicator Widget。通过代码逻辑使用一个布尔标记值 控制进度条的渲染。

#### 工程结构和资源文件

###### 在哪里放置分辨率相关的图片文件？
虽然 Android 区分对待资源文件 (resources) 和资产文件 (assets)，但是 Flutter 应用只有资产文件 (assets)。 所有原本在 Android 中应该放在 res/drawable-* 文件夹中的资源文件，在 Flutter 中都放在一个 assets 文件夹中。

Flutter 遵循一个简单的类似 iOS 的密度相关的格式。文件可以是一倍 (1.0x)、两倍 (2.0x)、三倍 (3.0x) 或其它的任意倍数。Flutter 没有 dp 单位，但是有逻辑像素尺寸，基本和设备无关的像素尺寸是一样的。名称为 devicePixelRatio 的尺寸表示在单一逻辑像素标准下设备物理像素的比例。

###### 字符串储存在哪里？如何处理本地化？
Flutter 当下并没有一个特定的管理字符串的资源管理系统。目前来讲，最好的办法是将字符串作为静态域 存放在类中，并通过类访问它们。

#### Gradle 文件的对应物是什么？我该如何添加依赖？
在 Android 中，你在 Gradle 构建脚本中添加依赖。Flutter 使用 Dart 自己的构建系统以及 Pub 包管理器。 构建工具会将原生 Android 和 iOS 壳应用的构建代理给对应的构建系统。

虽然在你的 Flutter 项目的 android 文件夹下有 Gradle 文件，但是它们只用于给对应平台的集成添加原生依赖。 一般来说，在 pubspec.yaml 文件中定义在 Flutter 里使用的外部依赖。[Pub](https://pub.flutter-io.cn/flutter/packages/) 是查找 Flutter 包的好地方。

#### Activity 和 Fragment

###### Activity 和 Fragment 在 Flutter 中的对应概念是什么？
在 Android 中，一个 Activity 代表用户可以完成的一件独立任务。一个 Fragment 代表一个 行为或者用户界面的一部分。Fragment 用于模块化你的代码，为大屏组合复杂的用户界面，并适配应用的界面。 在 Flutter 中，这两个概念都对应于 Widget。

###### 如何监听 Android Activity 的生命周期事件？
在 Android 中，你可以覆写 Actvity 的生命周期方法来监听其生命周期，也可以在 Application 上 注册 ActivityLifecycleCallbacks。在 Flutter 中，这两种方法都没有，但是你可以通过绑定 WidgetsBinding 观察者并监听 didChangeAppLifecycleState() 的变化事件来监听生命周期。

可以被观察的生命周期事件有：

- inactive — 应用处于非活跃状态并且不接收用户输入。这个事件只适用于 iOS，Android 上没有对应 的事件
- paused — 应用当前对用户不可见，无法响应用户输入，并运行在后台。这个事件对应于 Android 中的 onPause()
- resumed — 应用对用户可见并且可以响应用户的输入。这个事件对应于 Android 中的 onPostResume()
- suspending — 应用暂时被挂起。这个事件对应于 Android 中的 onStop；iOS 上由于没有对应的事件， 因此不会触发此事件

#### 布局
LinearLayout:在 Android 中，LinearLayout 用于线性布局 widget 的—水平或者垂直。 在 Flutter 中，使用 Row 或者 Column Widget 来实现相同的效果。

RelativeLayout：RelativeLayout 通过 Widget 的相互位置对它们进行布局。在 Flutter 中， 有几种实现相同效果的方法。

你可以通过组合使用 Column、Row 和 Stack Widget 实现 RelativeLayout 的效果。 你还可以在 Widget 构造器内声明孩子相对父亲的布局规则。

Collin 在 [StackOverflow](https://stackoverflow.com/questions/44396075/equivalent-of-relativelayout-in-flutter) 上的回答是一个在 Flutter 中构建相对布局的好例子。

ScrollView:在 Android 中，使用 ScrollView 布局 widget—如果用户的设备屏幕比应用的内容区域小，用户可以滑动内容。

在 Flutter 中，实现这个功能的最简单的方法是使用 ListView Widget。从 Android 的角度看，这样做可能 是杀鸡用牛刀了，但是 Flutter 中 ListView Widget 既是一个 ScrollView，也是一个 Android 中的 ListView。

#### 在 Flutter 中如何处理屏幕旋转
FlutterView 会处理配置的变化，前提条件是在 AndroidManifest.xml 文件中声明了：

#### 手势监听和触摸事件处理
###### Flutter 中如何为一个 Widget 添加点击监听器？
在 Android 中，你可以通过调用 setOnClickListener 方法在按钮这样的 View 上 添加点击监听器。
在 Flutter 中有两种添加触摸监听器的方法：
1. 如果 Widget 支持事件监听，那么向它传入一个方法并在方法中处理事件。例如，RaisedButton 有 一个 onPressed 参数
2. 如果 Widget 不支持事件监听，将 Widget 包装进一个 GestureDetector 中并向 onTap 参数 传入一个方法。
###### 如何处理 Widget 上的其它手势？
使用 GestureDetector 可以监听非常多的手势

#### Listviews 和 adapters

#### 文字处理
###### 如何为 Text Widget 设置自定义字体？
在 Android SDK 中（从 Android O 开始），你可以创建一个字体资源文件并将其 传给 TextView 的 FontFamily 参数。

在 Flutter 中，将字体文件放入一个文件夹，并在 pubspec.yaml 文件中引用它，就 和导入图片一样。

#### 表单输入
在 Flutter 中，你可以简单地通过向 Text Widget 构造器的 decoration 参数传入一个 InputDecoration 对象 来为输入框展示一个“提示”或占位文本。

#### 如何创建自己的自定义原生集成插件？

#### 如何对应用使用主题？
Flutter 提供开箱即用的优美的 Material Design 实现，可以满足你通常需要的各种样式和主题的需求。 不同于 Android 中你在 XML 文件中定义主题并在 AndroidManifest.xml 中将其赋值给你的应用， Flutter 中是在顶层 Widget 上声明主题。

#### 数据库和本地存储
###### 如何使用 Shared Preferences？
###### 在 Flutter 中如何使用 SQLite？
