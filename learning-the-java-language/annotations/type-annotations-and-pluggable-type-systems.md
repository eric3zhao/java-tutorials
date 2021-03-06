# Type Annotations and Pluggable Type Systems

在Java SE 8 release之前，注释只能用来声明。在Java SE 8 release，注解可以用作任何的`type use`。这就意味着注解可以在任何能使用类型（`type`）的地方使用。这里举一些哪里可以使用类型的例子，比如类的实例创建表达式（new），类型转换，`implements`语句和`throws`语句。这种形式的注解被称为`type annotation`（类型注解），一些例子已经在`Annotations Basics`中提到。

类型注解被创建用来改进分析Java程序，以确保更强的类型检查。Java SE 8 release不提供类型检查框架，但是允许你编写或者下载一个类型检查框架，该框架实现一个或者多个结合Java编译器使用的可插拔模块。

比如，你想确保你的代码中的一个变量不可以被设置为null，但是你想避免触发`NullPointerException`。为此你可以自己编写自定义插件来实现检查。接下来你可以修改你的代码在指定的变量上加注解，表示这个变量不能被设置为null。这个变量的声明可能看起来像这样：

```java
@NonNull String str;
```

当你编译代码的时候，在`command line`\(命令行\)中包含`NonNull`模块，当编译器检测到钱在问题时会输出一个告警，提醒你修改代码避免报错。当你修正代码确保没有告警以后，相同类型的报错就不会在程序运行时发生。

你可以使用多个类型检查模块，不同的模块用来检查不同类型的报错。这样，你可以构建在Java类型系统之上，在你希望的时间和地方添加特定的检查。

通过明智地使用类型注释和可插入类型检查器，你可以编写更强大且更不容易出错的代码。

多数情况下，你不需要编写自己的类型检查模块。有第三方为你实现这个功能。比如，你可能想使用华盛顿大学创建的`Checker Framework`。这个框架中包含`NonNull`模块，正则表达式模块和互斥锁模块。查看[Checker Framework](http://types.cs.washington.edu/checker-framework/)了解更多

