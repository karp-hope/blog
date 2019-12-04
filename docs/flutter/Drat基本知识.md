## Dart 是 Flutter 的主要开发语言

#### mixins（为类添加新的功能）
Mixins 是一种在多类继承中重用 一个类代码的方法。

使用 with 关键字后面为一个或者多个 mixin 名字来使用 mixin。

#Specifying a library prefix
If you import two libraries that have conflicting identifiers, then you can specify a prefix for one or both libraries. For example, if library1 and library2 both have an Element class, then you might have code like this:
```
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// Uses Element from lib1.
Element element1 = Element();

// Uses Element from lib2.
lib2.Element element2 = lib2.Element();
```

## AssetBundle class

A collection of resources used by the application.

Asset bundles contain resources, such as images and strings, that can be used by an application. Access to these resources is asynchronous so that they can be transparently loaded over a network (e.g., from a NetworkAssetBundle) or from the local file system without blocking the application's user interface.

Applications have a rootBundle, which contains the resources that were packaged with the application when it was built. To add resources to the rootBundle for your application, add them to the assets subsection of the flutter section of your application's pubspec.yaml manifest.

## navigator and routes
A Route is an abstraction for a “screen” or “page” of an app, and a Navigator is a widget that manages routes. A route roughly maps to an Activity, but it does not carry the same meaning. A navigator can push and pop routes to move from screen to screen. Navigators work like a stack on which you can push() new routes you want to navigate to, and from which you can pop() routes when you want to “go back”.

In Android, you declare your activities inside the app’s AndroidManifest.xml.

In Flutter, you have a couple options to navigate between pages:

- Specify a Map of route names. (MaterialApp)
- Directly navigate to a route. (WidgetApp)
