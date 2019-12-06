# A Closer Look at the Hello World Application

现在你已经成功运行你的`Hello World!`程序，你可能很好奇其中的原理。现在再来看一遍代码：

```java
class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!"); // Display the string.
    }
}
```

这个程序由以下三个重要的部分组成。

## 源代码注释

下面这段代码中加粗的部分就是`Comments`\(注释\)

```text

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

编译器会忽略注释的内容但是对其他开发者来说是很有用的，Java支持一下三种注释：

* `/* text */`，编译器会忽略`/*`到`*/`之间所有的内容
* `/** documentation */`，这样的文档注释\(简称`doc comment`\)，编译器会和第一种注释一样忽略这种注释。而自动生成文档时`javadoc`会使用这些注释。更多内容请参考[Javadoc™ tool documentation ](https://docs.oracle.com/javase/8/docs/technotes/guides/javadoc/index.html)
* `// text`，从`//`开始到一行的行尾内容都会被编译器忽略

## 类定义

下面这段代码中加粗就是类定义代码块的开头：

```text

/**
 * The HelloWorldApp class implements an application that
 * simply displays "Hello World!" to the standard output.
 */
class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!"); // Display the string.
    }
}
```

如上所示，最基础的类定义就是：

```java
class name {
    . . .
}
```

关键字\(`keyword`\)`class`表示类定义的开始，这里的类名是`name`，类的内容写在两个花括号`{`和`}`之间，这里我们用`. . .`表示

## main    函数

下面这段代码中加粗就是`main`函数的开头：

```text

/**
 * The HelloWorldApp class implements an application that
 * simply displays "Hello World!" to the standard output.
 */
class HelloWorldApp {
    public static void main(String[] args) {
        System.out.println("Hello World!"); //Display the string.
    }
}
```

在Java中，所有的应用必须包含一个`main`函数：

```java
public static void main(String[] args)
```

函数必须包含修饰符`public`和`static`（修饰符顺序没有关系）但是一般会这样写`public static`，参数名也可以随意制定，但是一般选择`args`或`argv`。 Java的`main`函数与C或者C++的很相似，是程序的入口随后将调用程序所需的其他函数。`main`函数使用字符串数组作为唯一的参数。运行时此数组将被传递给程序，比如运行`java MyApp arg1 arg2`。此数组的每个字符串称为`command-line argument`（命令行参数），通过它可以控制程序而不需要重新编译，比如一个排序程序在命令行参数填入`-descending`就可以对数据进行倒序排列。在`Hello World!`程序中我们忽略了命令行参数，但是你应该知道它的存在。

最后，我们来看输出`Hello World!`的代码：

```java
System.out.println("Hello World!");
```

这里我们使用核心包里的`System`将消息`Hello World!`打印到`standard output`。

