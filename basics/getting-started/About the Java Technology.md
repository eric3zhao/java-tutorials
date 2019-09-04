Java技术既是一种编程语言也是一个平台

### Java语言

Java语言是一个高级语言，可以用如下这些时髦词语来描述它：
<table>
<tr>
<td>
<ul>
<li>简单</li>
<li>面向对象</li>
<li>分布式</li>
<li>多线程</li>
<li>动态</li>
</ul>
</td>
<td>
<ul>
<li>架构独立</li>
<li>可移植</li>
<li>高性能</li>
<li>鲁棒性</li>
<li>安全</li>
</ul>
</td>
</tr>
</table>

`James Gosling`和`Henry McGilto`的文章[The Java Language Environment](https://www.oracle.com/technetwork/java/langenv-140151.html)中对这些词进行了解释

在Java语言中，所有的源代码以`.java`扩展名结尾的纯文本文件编写。接下来使用`java compiler`将原文件编译为`.class`文件。`.class`文件并不包含处理器可以直接使用的代码，而是`Java Virtual Machine（JVM）`使用的机器语言被称之为`bytecodes`。Java启动工具通过一个`JVM`实例运行你的程序。

![An overview of the software development process.](https://docs.oracle.com/javase/tutorial/figures/getStarted/getStarted-compiler.gif)

由于`JVM`可以运行在不同的操作系统之上，所以`.class`文件就有能力在`Microsoft Windows`，`the Solaris™ Operating System (Solaris OS)`，` Linux`，`Mac OS`系统中运行。有些虚拟机比如[Java SE HotSpot at a Glance](https://www.oracle.com/technetwork/java/javase/tech/index-jsp-136373.html)，会在运行时进行额外的任务来提升程序的性能，包括发现程序的性能瓶颈和将经常运行的代码片段重新编译成`native code`。

![Through the Java VM, the same application is capable of running on multiple platforms.](https://docs.oracle.com/javase/tutorial/figures/getStarted/helloWorld.gif)

### Java平台

`Platform`(平台)指的是一个程序运行的软硬件环境。我们已经提到过一些著名的平台比如`Microsoft Windows`，`Linux`，`Solaris OS`，`Mac OS`。大多数平台都可以描述成操作系统和底层硬件的混合体。Java平台不同于其他的平台因为它只是在其他基于硬件的平台之上的一个软件平台。

Java平台包含两个组件：

* `Java Virtual Machine`
* `Java Application Programming Interface (API)`

上文已经介绍过`Java Virtual Machine`，它是Java平台的基础并且被移植到许多不同的硬件平台。

`API`是一个现成的供许多有用功能的软件组件集合，它由许多关联的类和接口组成不同的`libraries`，这些`libraries`一般称之为包。下一章会介绍`API`的一些功能。

![The API and Java Virtual Machine insulate the program from the underlying hardware.](https://docs.oracle.com/javase/tutorial/figures/getStarted/getStarted-jvm.gif)

作为一个平台无关的环境，Java平台可能比`native code`（机器码）慢一些。但是编译器与虚拟机技术的进步使得性能接近于`native code`，同时不会影响到可移植性。

`Java Virtual Machine`和`JVM`都指的是Java平台的虚拟机。