# Wildcard Capture and Helper Methods

有些情况下，编译器会推断统配的类型。比如，一个列表可能定义为`List<?>`，但是当计算表达式时，编译器通过代码推断出特定的类型。这个场景被称为`wildcard capture`（通配符捕获）

在大多数情况下，我们无需担心通配符捕获，除非我们看到一条错误消息，其中包含`capture of`

例子[WildcardError](https://docs.oracle.com/javase/tutorial/java/generics/examples/WildcardError.java)在编译时产生一个捕获错误：

```java
import java.util.List;

public class WildcardError {

    void foo(List<?> i) {
        i.set(0, i.get(0));
    }
}
```

在上面的代码中，编译器将输入参数`i`处理为`Object`类型。当函数`foo`调用[ List.set\(int, E\)](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#set-int-E-)时。编译器无法确认插入列表对象的类型，因此生成了一个错误。当发生这种类型的错误时，通常意味着编译器认为我们正在将错误的类型分配给变量。Java语言中加入泛型为的是————在编译时确保类型安全。

当使用Oracle's JDK 7`javac`实现来编译上面`WildcardError`的例子会产生如下报错：

```text
WildcardError.java:6: error: method set in interface List<E> cannot be applied to given types;
    i.set(0, i.get(0));
     ^
  required: int,CAP#1
  found: int,Object
  reason: actual argument Object cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Object from capture of ?
1 error
```

这个例子中，代码试图进行安全操作，所以我们怎么处理编译器的报错呢。我们可通过编写一个捕获通配的`private helper method`（私有辅助函数）来修复。在这个例子中，我们可以通过创建一个私有辅助函数`fooHelper`来修复问题，[WildcardFixed](https://docs.oracle.com/javase/tutorial/java/generics/examples/WildcardFixed.java)代码如下:

```java
public class WildcardFixed {

    void foo(List<?> i) {
        fooHelper(i);
    }

    // Helper method created so that the wildcard can be captured
    // through type inference.
    private <T> void fooHelper(List<T> l) {
        l.set(0, l.get(0));
    }
}
```

归功于辅助函数，编译器在调用中使用推断来确定`T`为`CAP＃1`（捕获变量），现在例子能顺利编译了。

按照约定，辅助方法通常命名为`originalMethodNameHelper`

现在来看一个更复杂的例子[WildcardErrorBad](https://docs.oracle.com/javase/tutorial/java/generics/examples/WildcardErrorBad.java):

```java
import java.util.List;

public class WildcardErrorBad {

    void swapFirst(List<? extends Number> l1, List<? extends Number> l2) {
      Number temp = l1.get(0);
      l1.set(0, l2.get(0)); // expected a CAP#1 extends Number,
                            // got a CAP#2 extends Number;
                            // same bound, but different types
      l2.set(0, temp);        // expected a CAP#1 extends Number,
                            // got a Number
    }
}
```

在这个例子中，代码正进行不安全的操作。比如，看下面对于`swapFirst`函数的操作：

```java
List<Integer> li = Arrays.asList(1, 2, 3);
List<Double>  ld = Arrays.asList(10.10, 20.20, 30.30);
swapFirst(li, ld);
```

`List <Integer>`和`List <Double>`都满足条件`List <？extends Number>`，但是从`Integer`列表中取出一项并将其放入`Double`值列表中显然是不正确的。

使用Oracle's JDK `javac`编译器编译此段代码会产生如下报错：

```text
WildcardErrorBad.java:7: error: method set in interface List<E> cannot be applied to given types;
      l1.set(0, l2.get(0)); // expected a CAP#1 extends Number,
        ^
  required: int,CAP#1
  found: int,Number
  reason: actual argument Number cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Number from capture of ? extends Number
WildcardErrorBad.java:10: error: method set in interface List<E> cannot be applied to given types;
      l2.set(0, temp);      // expected a CAP#1 extends Number,
        ^
  required: int,CAP#1
  found: int,Number
  reason: actual argument Number cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Number from capture of ? extends Number
WildcardErrorBad.java:15: error: method set in interface List<E> cannot be applied to given types;
        i.set(0, i.get(0));
         ^
  required: int,CAP#1
  found: int,Object
  reason: actual argument Object cannot be converted to CAP#1 by method invocation conversion
  where E is a type-variable:
    E extends Object declared in interface List
  where CAP#1 is a fresh type-variable:
    CAP#1 extends Object from capture of ?
3 errors
```

没有辅助函数能修复这个问题，因为代码从根本上就是错误的。

