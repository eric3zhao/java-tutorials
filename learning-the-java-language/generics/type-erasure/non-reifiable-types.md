# Non-Reifiable Types

在[类型擦除](non-reifiable-types.md)部分中已经讨论编译器删除与类型参数和类型传参有关信息的过程。类型擦除具有与可变参数（也被称为varargs）的函数有关的结果，这些函数的可变形参具有不可具体化类型。更多可变参数函数的信息请查看[Passing Information to a Method or a Constructor](non-reifiable-types.md)中的[Arbitrary Number of Arguments](non-reifiable-types.md)部分。

本小节包含以下几点：

* [不可具体化类型](non-reifiable-types.md)
* [堆污染](non-reifiable-types.md)
* [具有不可具体化形参的可变参数函数的潜在隐患](non-reifiable-types.md)
* [避免来自不可具体化形参的可变参数函数的警告](non-reifiable-types.md)

## Non-Reifiable Types

`reifiable`\(可具体化\)类型指的是在整个运行时可知其类型信息的类型。包括`primitives`\(基础类型\)，`non-generic types`\(非泛型类型\)，`raw types`原始类型以及`invocations of unbound wildcards`\(无界通配符的调用\)

`Non-reifiable types`（不可具体化类型）指的是在编译过程中被类型擦除所移除信息的类型————那些没有被定义成无界通配的泛型类型的调用。一个不可具体化类型在运行时不包含所有的自身信息。举例来说不可具体化类型就是`List<String>`和`List<Number>`，JVM无法在运行时区分这两种类型有何不同。在[Restrictions on Generics](non-reifiable-types.md)会展示在某些特定情形下是不能使用不可具体化类型的：在一个`instanceof`表达式中或者是一个数组中的一个元素。

## Heap Pollution

当一个参数化类型变量引用了一个非参数化类型的对象时，`Heap Pollution`\(堆污染\)就会发生。如果程序执行了某些在编译时引起未经检查警告的操作，则会发生这种情况。`unchecked warning`\(未检查警告\)可能在编译时（取决于编译时类型检查规则的限制）生成也可能在设置运行时生产，这样将会导致无法验证涉及参数化类型（例如，强制类型转换或函数调用）的操作的正确性。举例来说，当原始类型和参数化类型混合使用，或者使用未经检测的类型转化时，堆污染就会发生。

在正常情况下，当所有的代码都是同时编译的，编译器会发出未经检查的警告来提醒你关注潜在的堆污染。如果我们分开编译我们的部分代码，那就很难检测出潜在的堆污染。如果我们保证代码编译没有警告，那么就不会有堆污染。

## Potential Vulnerabilities of Varargs Methods with Non-Reifiable Formal Parameters

包含可变输入参数的泛型函数可能引起堆污染

以下面`ArrayBuilder`类为例：

```java
public class ArrayBuilder {

  public static <T> void addToList (List<T> listArg, T... elements) {
    for (T x : elements) {
      listArg.add(x);
    }
  }

  public static void faultyMethod(List<String>... l) {
    Object[] objectArray = l;     // Valid
    objectArray[0] = Arrays.asList(42);
    String s = l[0].get(0);       // ClassCastException thrown here
  }

}
```

下面的`HeapPollutionExample`将会使用`ArrayBuiler`类：

```java
public class HeapPollutionExample {

  public static void main(String[] args) {

    List<String> stringListA = new ArrayList<String>();
    List<String> stringListB = new ArrayList<String>();

    ArrayBuilder.addToList(stringListA, "Seven", "Eight", "Nine");
    ArrayBuilder.addToList(stringListB, "Ten", "Eleven", "Twelve");
    List<List<String>> listOfStringLists =
      new ArrayList<List<String>>();
    ArrayBuilder.addToList(listOfStringLists,
      stringListA, stringListB);

    ArrayBuilder.faultyMethod(Arrays.asList("Hello!"), Arrays.asList("World!"));
  }
}
```

但编译时，`ArrayBuilder.addToList`函数的定义会产生如下警告：

```text
warning: [varargs] Possible heap pollution from parameterized vararg type T
```

当编译器遇到可变参数函数时，它会将可变形参翻译成一个数组。然而，Java编程语言禁止参数化类型数组的创建。在函数`ArrayBuilder.addToList`中，编译器将可变形参`T... elements`翻译成形参`T[] elements`，自然是一个数组，又因为类型擦除机制，编译器将形参转换为`Object[] elements`。所以，这里就有可能产生堆污染。

下面这条语句将可变形参`l`赋值给`Object`类型的素组`objectArgs`：

```java
Object[] objectArray = l;
```

这条语句可能潜在引起堆污染。一个与可变形参`l`参数化类型不匹配的值也可以赋值给变量`objectArray`，也就是复制给`l`。然而，编译器并不会对该语句生成未检查警告。当编译器将可变形参`List<String>... l`翻译成形参`List[] l`就已经生成警告。这条语句是有效的；变量`l`的类型是`List[]`，同时也是`Object[]`的子类型。

因此，如果我们像下面这条语句一样将任何类型的列表对象赋值给任何`objectArray`数组中的元素时是不会触发编译的警告或者报错的

```java
objectArray[0] = Arrays.asList(42);
```

这条语句将一个包含一个`Integer`类型对象的列表对象赋值给`objectArray`数组的第一个元素。

假设我们通过下面这条语句调用`ArrayBuilder.faultyMethod`:

```java
ArrayBuilder.faultyMethod(Arrays.asList("Hello!"), Arrays.asList("World!"));
```

在运行时，JVM会在下面这条语句抛出`ClassCastException`

···java // ClassCastException thrown here String s = l\[0\].get\(0\); ···

变量`l`数组中的第一个对象是`List<Integer>`类型，但是这条语句需要的是`List<String>`类型的对象。

## Prevent Warnings from Varargs Methods with Non-Reifiable Formal Parameters

如果我们声明了一个包含参数化类型参数的可变形参函数，同时我们能确保函数体内不会抛出`ClassCastException`或者其他相类似的因为处理可变形参引起的错误，我们就可以为这些可变形参函数添加注解来屏蔽编译器生成的警告，该注解可以添加在静态或者非构造函数的声明中：

```text
@SafeVarargs
```

`@SafeVarargs`注解是函数约定的一部分，该注解断言该函数的实现会恰当的处理可变形参。

同时我们也可以（虽然不鼓励这样做）通过加入下面内容到函数声明中来阻止这些警告的产生：

```text
@SuppressWarnings({"unchecked", "varargs"})
```

然而，这种方式不会阻止函数调用的地方产生警告。如果不对`@SuppressWarnings`不熟悉，请查看[Annotations](non-reifiable-types.md)

