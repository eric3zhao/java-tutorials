# Predefined Annotation Types

在`Java SE API`已经预定义了一些注解类型。其中有些注解被Java编译器所使用，有些作用于别的注解。

## Annotation Types Used by the Java Language

`java.lang`中预定义的注解有`@Deprecated`，`@Override`和`@SuppressWarnings`。

**@Deprecated**

[@Deprecated](https://docs.oracle.com/javase/8/docs/api/java/lang/Deprecated.html)注解用来标识元素已经`deprecated`（废弃）不能继续使用。当程序中用到带有`deprecated`注解的类，函数或者字段时编译器将生成一个警告。当一个元素被弃用，应该在文档注释中使用`Javadoc`的`@deprecated`tag，如下面的例子所示。在`Javadoc`注释和注解中使用`at`符号\(@\) 并不是巧合：它们在概念上是相关的。注意，`Javadoc`的tag首字母是小写`d`而注释的首字母是大写`D`。

```java
// Javadoc comment follows
/**
* @deprecated
* explanation of why it was deprecated
*/
@Deprecated
static void deprecatedMethod() { }
```

**@Override**

[@Override](https://docs.oracle.com/javase/8/docs/api/java/lang/Override.html)注解通知编译器当前的元素重写了父类\(`superclass`\)中声明的元素。重写将在后面的章节中讲解。

```java
// mark method as a superclass method
// that has been overridden
@Override 
int overriddenMethod() { }
```

当然重新函数时不一定要使用`@Override`注解，但是它可以防止报错。如果带有`@Override`的函数没有正确重写父类的函数，编译器会生成一个报错。

**@SuppressWarnings**

[@SuppressWarnings](https://docs.oracle.com/javase/8/docs/api/java/lang/SuppressWarnings.html)注解告诉编译器忽略特定的警告。下面的例子中使用了一个已经废弃的函数，通常编译器会生成一个警告。然而这个例子中`@SuppressWarnings`注释使得警告被忽略。

```java
// use a deprecated method and tell 
// compiler not to generate a warning
@SuppressWarnings("deprecation")
void useDeprecatedMethod() {
    // deprecation warning
    // - suppressed
    objectOne.deprecatedMethod();
}
```

每一个的编译器警告都归属于一个种类。`The Java Language Specification`列出两个种类：`deprecation`和`unchecked`。`unchecked`警告可能出现在范型（`generics`）出现之前编写的遗产代码中。可以使用下面这个语法或略多种类型的警告：

```java
@SuppressWarnings({"unchecked", "deprecation"})
```

**@SafeVarargs**

[@SafeVarargs](https://docs.oracle.com/javase/8/docs/api/java/lang/SafeVarargs.html)注解使用在函数或者构造方法时，断定代码在操作它的可变长变量参数\(`varargs`\)时不会发生潜在的不安全操作。当这个注解被使用时，可变长参数的非检查警告将被忽略。

**@FunctionalInterface**

[@FunctionalInterface](https://docs.oracle.com/javase/8/docs/api/java/lang/FunctionalInterface.html)注解，Java SE 8中引入，在`Java Language Specification`中表示声明的类型是一个方法接口（`functional interface`）

## Annotations That Apply to Other Annotations

作用于其他注解的注解被称为`meta-annotations`（元注解），在`java.lang.annotation`中定义了一些元注解类型。

**@Retention**

[@Retention](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Retention.html)注解指定被标记的注解是如何存储的：

* RetentionPolicy.SOURCE：被标记的注解只保留在原代码级别，将会被编译器忽略。
* RetentionPolicy.CLASS：被标记的注解将会被编译器保留，但是会被java虚拟机（`JVM`）忽略。
* RetentionPolicy.RUNTIME：被标记的将会被`JVM`保留，所以它可以被运行时环境使用。

**@Documented**

[@Documented](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Documented.html)注解表示无论何时使用特定注解的元素应该被`Javadoc`工具生成文档（默认，注解不包含在`Javadoc`中）。详情见[Javadoc tools page](https://docs.oracle.com/javase/8/docs/technotes/guides/javadoc/index.html)。

**@Target**

[@Target](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Target.html)注解标记其他的注解只能用在限定的Java元素上。target注解指定以下元素类型中的一个作为它的值：

* ElementType.ANNOTATION\_TYPE 可以作用在注解类型
* ElementType.CONSTRUCTOR 可以作用在构造函数
* ElementType.FIELD 可以作用在属性或者property
* ElementType.LOCAL\_VARIABLE 可以作用在局部变量
* ElementType.METHOD 可以作用在函数及表的注解
* ElementType.PACKAGE 可以作用在包的声明
* ElementType.PARAMETER 可以作用在函数的参数
* ElementType.TYPE 可以作用在类的任何元素

**@Inherited**

[@Inherited](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Inherited.html)注解表示该类型的注解可以从父类中继承（默认是不可以的）。当用户查询此类型注解并且该类没有此类型的注释时，将会查询父类以获取该类型的注解，该注解只适用于类的声明。

**@Repeatable**

[@Repeatable](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/Repeatable.html)注解，Java SE 8中引入，表示被标记的注解可以多次用于同一个声明或者类型。后面我们会介绍详细内容。

