## MarkDown基本语法

### MarkDown斜体和加粗

```
**这是加粗**
```

**这是加粗**

```
*这是斜体文字*
```

*这是斜体文字*

```
***这是加粗同时斜体***
```

***这是加粗同时斜体***

### MarkDown字体颜色和大小

```
<font size=2 color=red>我是设置的字体</font>
```

<font size=2 color=red>我是设置的字体</font>

### 代码的标记

只要把你的代码块包裹在 “` 之间，你就不需要通过无休止的缩进来标记代码块了。 在围栏式代码块中，你可以指定一个可选的语言标识符，然后我们就可以为它启用语法着色了。 举个例子，这样可以为一段 Ruby 代码着色：

    ```ruby
    require 'redcarpet'
    markdown = redcarpet.new("Hello World")
    ```
效果如下：

```ruby
require 'redcarpet'
markdown = redcarpet.new("Hello World")
```

### 代码区域原样输出

当不希望代码区域中的文字不会被处理的时候，按照原样输出，可以在每一行前面加入4个空格或者一个tab

    ```ruby
    require 'redcarpet'
    markdown = redcarpet.new("Hello World")
    ```
这样本来会展示成代码形式的，现在直接把```显示出来了

### 自动链接

```
[链接的文字](https://karp-hope.github.io/blog)
```

### 添加图片

```
![图片的替代文字](/path/to/img.png "Optional title")
```

