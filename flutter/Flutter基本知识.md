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
