## Flutter代码分析和观察

```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```

- 本示例创建了一个具有 [Material Design](https://material.io/design/) 风格的应用， Material 是一种移动端和网页端通用的视觉设计语言， Flutter 提供了丰富的 Material 风格的 widgets。
- 主函数（main）使用了 (=>) 符号，这是 Dart 中单行函数或方法的简写。
- 该应用程序继承了 StatelessWidget，这将会使应用本身也成为一个 widget。 在 Flutter 中，几乎所有都是 widget， 包括对齐 (alignment)、填充 (padding) 和布局 (layout)。
- Scaffold 是 Material 库中提供的一个 widget， 它提供了默认的导航栏、标题和包含主屏幕 widget 树的 body 属性。 widget 树可以很复杂。
- 一个 widget 的主要工作是提供一个 build() 方法来描述如何根据其他较低级别的 widgets 来显示自己。
- 本示例中的 body 的 widget 树中包含了一个 Center widget， Center widget 又包含一个 Text 子 widget， Center widget 可以将其子 widget 树对齐到屏幕中心。

## 使用外部 package

开始使用一个名为 english_words 的开源软件包，其中包含数千个最常用的英文单词以及一些实用功能。

你可以在 [Pub site](https://pub.dev/flutter) 上找到 english_words 软件包以及其他许多开源软件包。
1. pubspec 文件管理 Flutter 应用程序的 assets（资源，如图片、package等）。 在pubspec.yaml 中，将 english_words（3.1.0或更高版本）添加到依赖项列表， 如下面高亮显示的行：
2. 在Android Studio 的编辑器视图中查看 pubspec 时， 单击右上角的 Packages get，这会将依赖包安装到你的项目。 你可以在控制台中看到以下内容：
3. 在 lib/main.dart 中引入，如下所示：
```
import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';
```
4. 接下来，我们使用 English words 包生成文本来替换字符串”Hello World”：替换方式为：child: Text('Hello World'),替换为 child: Text(wordPair.asPascalCase),
5. 如果应用程序正在运行，请使用热重载按钮更新正在运行的应用程序。每次单击热重载或保存项目时，都会在正在运行的应用程序中随机选择不同的单词对。 这是因为单词对是在 build 方法内部生成的。每次 MaterialApp 需要渲染时或者在 Flutter Inspector 中切换平台时 build 都会运行。

## 添加一个 Stateful widget
Stateless widgets 是不可变的，这意味着它们的属性不能改变 —— 所有的值都是 final。

Stateful widgets 持有的状态可能在 widget 生命周期中发生变化， 实现一个 stateful widget 至少需要两个类： 1）一个 StatefulWidget 类；2）一个 State 类，StatefulWidget 类本身是不变的， 但是 State 类在 widget 生命周期中始终存在。

在这一步，你将添加一个 stateful widget（有状态的 widget）—— RandomWords， 它会创建自己的状态类 —— RandomWordsState，然后你需要将 RandomWords 内嵌到已有的无状态的 MyApp widget。

## 创建一个无限滚动的 ListView
