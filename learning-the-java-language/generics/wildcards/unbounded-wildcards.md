# Unbounded Wildcards

无界通配类型是通配符`?`的特殊用法。比如 `List<?>`，我们称之为`list of unknown type`（未知类型的列表）。在两种情况下，无界通配符能起很大作用：

* 如果我们正在编写一个可以使用`Object`类中提供的功能来实现的函数
* 在代码使用泛型类中不依赖于类型参数的函数。比如`List.size`或者`List.clear`，实际上`Class<?>`经常被用到，因为`Class<T>`中的大多数函数都不依赖于`T`

参考下面这个函数`printList`:

```java
public static void printList(List<Object> list) {
    for (Object elem : list)
        System.out.println(elem + " ");
    System.out.println();
}
```

该函数的目的是以列表形式打印任何类型，但是该目标是无法实现的————它只能打印`Object`实例的列表，而不能打印`List<Integer>, List<String>, List<Double>`等等别的类型，因为它们不是`List<Object>`的子类型。使用`List<?>`编写通用的`printList`函数：

```java
public static void printList(List<?> list) {
    for (Object elem: list)
        System.out.print(elem + " ");
    System.out.println();
}
```

对于任何具体类型`A`来说，`List<A>`是`List<?>`的子类型，所以我们可以用`printList`打印任何类型的列表：

```java
List<Integer> li = Arrays.asList(1, 2, 3);
List<String>  ls = Arrays.asList("one", "two", "three");
printList(li);
printList(ls);
```

> 在本课程的示例中，会使用[Arrays.asList](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html#asList-T...-)函数。此静态工厂方法将指定的数组转换并返回固定大小的列表。

意识到`List<Object>`和`List<?>`是不一样的是很重要的。你可以往`List<Object>`中插入`Object`或者`Object`的子类型。但是你只能向`List<?>`中插入`null`。在[Guidelines for Wildcard Use](unbounded-wildcards.md)部分中有更多关于如何确定在给定情况下应使用哪种通配符（如果有）的信息。

