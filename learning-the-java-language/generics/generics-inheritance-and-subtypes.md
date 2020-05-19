# Generics, Inheritance, and Subtypes

众所周知，只要类型兼容，我们就可以将一种类型的对象赋值给另外一个类型的对象。举例来说，我们可以将`Integer`类型的对象赋值给一个`Object`类型，因为`Object`类是`Integer`的超类之一：

```java
Object someObject = new Object();
Integer someInteger = new Integer(10);
someObject = someInteger;   // OK
```

在面向对象术语中，我们称这种关系为`is a`\(某某是一种/个\)。因为一个`Integer`是一种`Object`，所以赋值是允许的，但是`Integer`也是一种`Number`，所以下面的代码也是成立的：

```java
public void someMethod(Number n) { /* ... */ }

someMethod(new Integer(10));   // OK
someMethod(new Double(10.1));   // OK
```

在泛型中也同样成立。下面我们使泛型类型调用，`Number`作为类型传参，那么后续所有和`Number`兼容的传参都是允许的：

```java
Box<Number> box = new Box<Number>();
box.add(new Integer(10));   // OK
box.add(new Double(10.1));  // OK
```

现在我们来看下面这个函数：

```java
public void boxTest(Box<Number> n) { /* ... */ }
```

它能接受什么类型的传参？从定义上看，我们能发现它接受单个类型为`Box<Number>`的传参。这意味什么？你可能会想我们可以传入`Box<Integer>`或者`Box<Double>`吗？答案是`NO`，因为`Box<Integer>`和`Box<Double>`不是`Box<Number>`的子类型。

当使用泛型编程时会有一个普通的误解，确实一个需要理解的重要概念。

![Box&amp;lt;Integer&amp;gt; is not a subtype of Box&amp;lt;Number&amp;gt; even though Integer is a subtype of Number.](https://docs.oracle.com/javase/tutorial/figures/java/generics-subtypeRelationship.gif)

> 给定两个具体的类型A和B（例如，`Number`和`Integer`），无论A和B是否相关，`MyClass <A>`与`MyClass <B>`没有关系。`MyClass <A>`与`MyClass <B>`共同的父类是`Object`。有关在类型参数相关时如何在两个泛型类之间创建类似子类型的关系的信息，请参考[Wildcards and Subtyping](generics-inheritance-and-subtypes.md)

## Generic Classes and Subtyping

我们可以通过扩展泛型类或着实现接口来子类型化。一个类或接口的类型参数与另一类或接口的类型参数之间的关系由`extends`和`implements`语句确定。

这里使用`Collections`类作为例子，`ArrayList<E>`实现`List<E>`，而`List<E>`扩展`Collection<E>`。所以`ArrayList<String>`是`List<String>`的一个子类型，`List<String>`则是`Collection<String>`的子类型。只要类型传参保持不变，子类型关系就保存在类型之间。

![A sample Collections hierarchy](https://docs.oracle.com/javase/tutorial/figures/java/generics-sampleHierarchy.gif)

现在想象一下，我们想要定义我们自己的list接口`PayloadList`，它将泛型类型为`P`的可选值与每个元素相关联。它的声明如下：

```java
interface PayloadList<E,P> extends List<E> {
  void setPayload(int index, P val);
  ...
}
```

下面这些参数化的`PayloadList`都是`List<String>`的子类：

* PayloadList
* PayloadList
* PayloadList

![A sample PayloadList hierarchy](https://docs.oracle.com/javase/tutorial/figures/java/generics-payloadListHierarchy.gif)

