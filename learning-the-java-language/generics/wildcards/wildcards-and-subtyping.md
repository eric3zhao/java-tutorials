在[Generics, Inheritance, and Subtypes]()已经讨论过，泛型类或者接口之所以是不相关仅因为它们的类型之间存在关系。然而，你可以用通配符去创建泛型类或者接口之间的关系。

以下面两个常规（非泛型）类为例：

```java
class A { /* ... */ }
class B extends A { /* ... */ }
```

下面的代码也是合理的：

```java
B b = new B();
A a = b;
```

上面上代码显示了常规类的继承遵循了子类型规则：如果`B`扩展自`A`，则类`B`是类`A`的子类型。该规则不适用于泛型类型：

```java
List<B> lb = new ArrayList<>();
List<A> la = lb;   // compile-time error
```

既然`Integer`是`Number`的子类型，那么`List <Integer>`和`List <Number>`之间的关系是什么呢？

![The common parent is List<?>.](https://docs.oracle.com/javase/tutorial/figures/java/generics-listParent.gif)

虽然`Integer`是`Number`的子类型，但是`List<Integer>`不是`List<Number>`的子类型，实际上，这两个类型并没有关联。`List<Number>`和`List<Integer>`的共同父类是`List<?>`

为了在这些类之间创建关系，以便代码可以通过`List <Integer>`的元素访问`Number`的方法，请使用上界通配符：

```java
List<? extends Integer> intList = new ArrayList<>();
List<? extends Number>  numList = intList;  // OK. List<? extends Integer> is a subtype of List<? extends Number>
```

因为`Integer`是`Number`的子类型，并且`numList`是`Number`对象的列表，所以现在`intList`（Integer对象的列表）和`numList`之间存在关系。下图展示了了使用上下界通配符声明的几个`List`类之间的关系。

![A hierarchy of several generic List class declarations.](https://docs.oracle.com/javase/tutorial/figures/java/generics-wildcardSubtyping.gif)

[Guidelines for Wildcard Use](https://docs.oracle.com/javase/tutorial/java/generics/wildcardGuidelines.html)部分提供有更多关使用上界和下界通配符的后果的信息