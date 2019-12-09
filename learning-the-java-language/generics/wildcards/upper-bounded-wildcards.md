我们能使用上边界通配符来放松一个变量的限制。比如说，你想要写一个函数对于`List<Integer>, List<Double>,List<Number>`	功能使用，你可以使用上边界通配符打到目的。

定义一个上边界通配符时使用通配字符`?`，后面紧跟`extends`关键字，最后是它的`upper bound`（上边界）。请注意，在这语境中，`extends`被用在一个普通场景表示对类的`extends`（扩展）或者对接口的`implements`（实现）。

为了编写的函数可以同时作用于`Number`类型及其子类型（`Integer,Double,Float`）的列表，你可以指定`List<? extends Number>`。`List<Number>`比`List<? extends Number>`更有限制性，因为它只能适用于`Number`类型的列表，而后者适用于`Number`类型及其子类型的列表。

以下面的`process`函数为例：

```java
public static void process(List<? extends Foo> list) { /* ... */ }
```

上边界通配符`<? extends Foo>`，无论`Foo`是何类型，匹配`Foo`类型以及它的子类型。`process`函数可以以`Foo`类型访问列表元素：

```java
public static void process(List<? extends Foo> list) {
    for (Foo elem : list) {
        // ...
    }
}
```

在`foreach`条件中，变量`elem`遍历列表中的每个元素。任何`Foo`类中定义的函数都能被`elem`使用。

函数`sumOfList`返回列表中数字的和：

```java
public static double sumOfList(List<? extends Number> list) {
    double s = 0.0;
    for (Number n : list)
        s += n.doubleValue();
    return s;
}
```

下面这段代码使用`Integer`实例的列表，打印`sum = 6.0`:

```java
List<Integer> li = Arrays.asList(1, 2, 3);
System.out.println("sum = " + sumOfList(li));
```

`Double`类型的列表也同样适用于`sumOfList`，下面这段代码打印`sum = 7.0`

```java
List<Double> ld = Arrays.asList(1.2, 2.3, 3.5);
System.out.println("sum = " + sumOfList(ld));
```