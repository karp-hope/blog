## Packages & plugins

#### Using packages

#### Developer package & plugins
If you want to develop a package that calls into platform-specific APIs, you need to develop a plugin package. A plugin package is a specialized version of a Dart package, that in addition to the content described above also contains platform-specific implementations written for Android (Java or Kotlin code), for iOS (Objective-C or Swift code), or for both. The API is connected to the platform-specific implementation(s) using [platform channels](https://flutter.dev/docs/development/platform-integration/platform-channels).

#### Writing custom platform-specific code
Flutter’s platform-specific API support does not rely on code generation, but rather on a flexible message passing style:
- The Flutter portion of the app sends messages to its host, the iOS or Android portion of the app, over a platform channel.
- The host listens on the platform channel, and receives the message. It then calls into any number of platform-specific APIs—using the native programming language—and sends a response back to the client, the Flutter portion of the app.

## Navigator and Routes
[home page](https://flutter.dev/docs/development/ui/navigation)
In Flutter, screens and pages are called routes. The remainder of this recipe refers to routes.

In Android, a route is equivalent to an Activity. In iOS, a route is equivalent to a ViewController. In Flutter, a route is just a widget.

Navigate to a new route using the [Navigator](https://api.flutter.dev/flutter/widgets/Navigator-class.html)

## GlobalKey 使用
当我们通过setState方法变更一个widget的状态的时候，Flutter会重新调用其对应的state的build方法来重新构造Widget tree， Flutter获取到最新的Widget Tree后会与当前的Widget tree做对比，当每个Widget在树中的位置一致时（只修改Widget的属性），flutter将识别出来，并会复用之前的State对象，因此这个widget的状态就会得到保留。

当我们改变Widget的位置时，使得内部的 tree differ 无法确定新出现的widget是否是原来的那个widget， ，所以 Flutter 就会决定不复用它的 State，并通过 createState 来重新创建一个新的 State

此时GlobalKey的作用就出现了，通过 key 可以唯一标识一个 Widget，key 在列表类视图中尤为重要，因为它会让 differ 更高效地计算出哪些元素是新添加的，哪些元素是被移除或交换位置的，Flutter 也是如此。这个 key 比较特殊，因为它是整个 app 中唯一的，Flutter 负责创建它，并保证它的唯一性。这种 key 在我们这个例子中比较适用，但是在列表类视图中就不那么适用了，我们希望视图与数据一一对应，这时候就需要 ValueKey 了。

## 导航返回拦截WillPopScope
为了避免用户误触返回按钮而导致APP退出，在很多APP中都拦截了用户点击返回键的按钮，当用户在某一个时间段内点击两次时，才会认为用户是要退出（而非误触）。Flutter中可以通过WillPopScope来实现返回按钮拦截

上面的解释其实不够精确，api中显示是这样的意思
Registers a callback to veto attempts by the user to dismiss the enclosing ModalRoute.