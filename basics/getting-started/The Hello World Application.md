这里我们介绍如何在`macOS`环境下编写程序。如果过程中你遇到了困难可以参考[Common Problems (and Their Solutions).](https://docs.oracle.com/javase/tutorial/getStarted/problems/index.html)

### Checklist

在开始之前，请检查：

1. **The Java SE Development Kit 8 (JDK 8)** 

	下载地址[Java SE Downloads](https://www.oracle.com/technetwork/java/javase/downloads/index.html)，我们使用的是`JDK 8`。如果你遇到问题可以参考[installation instructions](https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)

2. **文本编辑器**
	任何的文本编辑器都可以，比如`vi`
	
### HelloWorld

我们的第一个程序是简单的输出`Hello world!`。请按照以下步骤：

1. **创建源文件**

源文件包含使用Java语言编写的源代码。你可以使用文本编辑器创建和修改源代码。创建一个名为`HelloWorldApp.java`的文件，文件内容是：

```java
/**
 * The HelloWorldApp class implements an application that
 * simply prints "Hello World!" to standard output.
 */
class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!"); // Display the string.
    }
}
```
> 注意编译器和启动器都是大小写敏感的，`HelloWorldApp`和`helloworldapp`是不一样的

2.	**将源文件编译成`.class`文件**

Java编译器`javac`会将你的源代码翻译成Java虚拟机可以理解的指令。这些包含在`.class`文件中的指令我们称之为`bytecodes`。

* 打开`terminal(终端)`将当前的目录切换到`.java`源文件所在目录。比如你的源文件在`/tmp/examples/java`，就输入`cd /tmp/examples/java`然后按`Return`（回车键）
* 输入`javac HelloWorldApp.java`命令然后按`Return`。编译器将生成一个`bytecode`文件`HelloWorldApp.class`。现在你会在当前目录下看到两个文件`HelloWorldApp.java`和`HelloWorldApp.class`

3. **运行程序**

Java应用`launcher tool (java)`使用Java虚拟机来运行你的程序。在当面目录直接输入`java HelloWorldApp`