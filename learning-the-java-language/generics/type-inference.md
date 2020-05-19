# Type Inference

`Type inference`\(类型推断\)是Java编译器查看每个方法调用和相应的声明以确定适用于函数调用的类型传参的能力。推理算法确定传参的类型，以及确定函数赋值或返回的类型（如果有）。最后，推理算法尝试找到与所有传参一起使用的最具体的类型。

为了说明上面所说的，下面这个例子中，推断确定函数`pick`的第二个传参是`Serializable`类型的：

```java
static <T> T pick(T a1, T a2) { return a2; }
Serializable s = pick("d", new ArrayList<String>());
```

## Type Inference and Generic Methods

[Generic Methods](type-inference.md)向我们介绍了类型推断，使得你可以想调用普通函数一样调用泛型函数，而不需要在尖括号中指定类型。下面这个[BoxDemo](https://docs.oracle.com/javase/tutorial/displayCode.html?code=https://docs.oracle.com/javase/tutorial/java/generics/examples/BoxDemo.java)例子中用到了[Box](https://docs.oracle.com/javase/tutorial/java/generics/examples/Box.java)类：

```java
public class BoxDemo {

  public static <U> void addBox(U u, java.util.List<Box<U>> boxes) {
    Box<U> box = new Box<>();
    box.set(u);
    boxes.add(box);
  }

  public static <U> void outputBoxes(java.util.List<Box<U>> boxes) {
    int counter = 0;
    for (Box<U> box: boxes) {
      U boxContents = box.get();
      System.out.println("Box #" + counter + " contains [" +
             boxContents.toString() + "]");
      counter++;
    }
  }

  public static void main(String[] args) {
    java.util.ArrayList<Box<Integer>> listOfIntegerBoxes =
      new java.util.ArrayList<>();
    BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(30), listOfIntegerBoxes);
    BoxDemo.outputBoxes(listOfIntegerBoxes);
  }
}
```

例子的输入如下：

```text
Box #0 contains [10]
Box #1 contains [20]
Box #2 contains [30]
```

泛型函数`addBox`定义了一个名为`U`的类型参数。通常，Java编译器可以推断出泛型函数调用是的类型参数，所以在大多数情况下，我们不需要指定类型参数。举例来说，在调用泛型函数`addBox`时我们可以使用`type witness`（参数见证）指定类型参数：

```java
BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);
```

或者，我们省略参数见证，Java编译器自动推断出（通过函数传参）类型参数是`Integer`:

```java
BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);
```

## Type Inference and Instantiation of Generic Classes

我们可以在调用泛型类的构造函数时将所需的类型传参替换成空的类型参数集合（&lt;&gt;）编译器也会通过上下文推断出类型传参。一对尖括号我们称之为[the diamond](type-inference.md)（菱形符）.

以下面的变量声明为例：

```java
Map<String, List<String>> myMap = new HashMap<String, List<String>>();
```

我们可以将传入构造函数的参数类型替换成空的集合（&lt;&gt;）：

```java
Map<String, List<String>> myMap = new HashMap<>();
```

需要注意的是在泛型类型实例化中我们必须使用棱形符才能用到类型推断。下面这个例子中编译器会生成一个转换未检查的警告，因为构造函数`HashMap()`指的是`HashMap`的原生类型而不是`Map<String, List<String>>`类型：

```java
Map<String, List<String>> myMap = new HashMap(); // unchecked conversion warning
```

## Type Inference and Generic Constructors of Generic and Non-Generic Classes

需要注意的是在泛型和非泛型类中构造函数都可以是泛型的（也就是声明自己的形式类型参数），以下面的代码为例：

```java
class MyClass<X> {
  <T> MyClass(T t) {
    // ...
  }
}
```

在看下面这段实例化`MyClass`类的代码：

```java
new MyClass<Integer>("")
```

这条语句创建了一个参数化类型`MyClass<Integer>`的实例；该语句为泛型类`MyClass <X>`的形式类型参数`X`明确的指定了类型`Integer`。注意这个泛型类的构造函数也包含了一个形式类型参数`T`。编译器将该泛型类的构造函数的形式类型参数`T`推断为`String`类型（因为该构造函数的实际参数是`String`对象）。

Java SE 7之前的编译器可以像泛型函数一样推断泛型构造函数的真实类型参数。然而在Java SE 7及以后的版本当我们使用棱形符实例化泛型类时编译器可以推断真实类型参数。参考下面这个例子：

```java
MyClass<Integer> myObject = new MyClass<>("");
```

在这个例子中，编译器推断泛型类`MyClass<X>`的形式类型参数`X`的类型为`Integer`。推断该泛型类构造函数的形式类型参数`T`为`String`类型。

> 需要注意的是推理算法仅使用调用参数，目标类型，以及明显的预期返回类型来推断类型。推断算法不会使用代码中接下来的一些结果。

## Target Types

Java编译器利用目标类型来推断泛型函数调用的类型参数。表达式的`target type`\(目标类型\)是Java编译器期望的数据类型，具体取决于表达式出现的位置。参考函数`Collections.emptyList`,该函数声明如下：

```java
static <T> List<T> emptyList();
```

再看下面的赋值语句：

```java
List<String> listOne = Collections.emptyList();
```

这个语句期望一个`List<String>`的实例，这个数据类型就是目标类型。因为函数`emptyList`返回一个类型为`List<T>`的值，而编译器推断出类型传参`T`肯定是`String`。以上在Java SE 7和8中都能正常工作。或者我们可以使用类型见证来指定`T`的值：

```java
List<String> listOne = Collections.<String>emptyList();
```

但是这在当前上下文中是不需要的。它在别的上下文中需要用到，以下面的代码为例：

```java
void processStringList(List<String> stringList) {
    // process stringList
}
```

设想你想要在调用`processStringList`函数时传入空的list。在Java SE 7中该语句不能正常编译：

```java
processStringList(Collections.emptyList());
```

Java SE 7的编译器会生成如下报错信息：

```text
List<Object> cannot be converted to List<String>
```

编译器需要类型传参`T`的具体值所以它使用值`Object`，因此调用`Collections.emptyList`的返回值是`List<Object>`类型，该类型与函数`processStringList`冲突。也就是说在Java SE 7我们必须指定类型传参的值，如下所以：

```java
processStringList(Collections.<String>emptyList());
```

在Java SE 8不再需要。什么是目标类型的概念已扩展为包括方法参数，例如方法`processStringList`的参数。在这个例子中`processStringList`需要一个类型为`List<String>`的参数。函数`Collections.emptyList`返回一个类型为`List<T>`的值，所以使用`List<String>`的目标类型，编译器推断类型参数`T`的值是`String`。因此在Java SE 8中，下面这条语句能正常编译：

```java
processStringList(Collections.emptyList());
```

查看[Lambda Expressions](type-inference.md)中的[Target Typing](type-inference.md)了解更多。

