# Generic Types

`generic types`泛型类型指的是通过类型进行参数化的通用类或者接口。下面通过修改`Box`类来掩饰这一概念。

## A Simple Box Class

首先我们来看通过使用对象来操作任意类型的非泛型`Box`类。它只需要两个函数：`set`向`box`中添加对象和`get`从`box`中获取对象。

```java
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```

因为`box`的函数接受或者返回一个对象，所以只要不使用原始类型，你可以任意的传递你想要的内容。同时在贬义的时候没办法验证类被如何使用。在一部分代码中可能将一个`Integer`添加到`box`中并希望取出`IntegerS`，但是在另外的一部分代码中错误的传入了一个`String`，这样会导致`runtime error`（运行时报错）

## A Generic Version of the Box Class

`generic class`（泛型类）定义格式如下：

```java
class name<T1, T2, ..., Tn> { /* ... */ }
```

类名后面使用尖括号（&lt;&gt;）分隔出类型参数部分。它指定`type parameters`（类型参数，也叫`type variables`-类型变量）`T1，T2, ...,Tn`

对上面的`Box`类使用泛型，将代码从`public class Box`改成`public class Box<T>`来创建一个`generic type declaration` \(泛型类型声明\)。这里引入可以在类内部任何地方使用的类型变量`T`。

新的`Box`类如下所示：

```java
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```

如你所见所有出现`Object`的地方被替换为`T`。一个类型变量可是你指定的任何`non-primitive type`（非原始类型）:任意的类类型，任意的接口类型，任意的数组类型，甚至可以是其他的类型变量。

我们可以使用同样的方式创建泛型接口。

## Type Parameter Naming Conventions

按照惯例，类型参数的命名使用单个大写字母。这种方式和你已知的[变量命名](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html#naming)规约有着鲜明的对比，但是这是有原因的：没有这个惯例，那么将很难区分一个类型变量和一个普通的类或者接口名。

经常被使用的类型变量名如下：

* E - Element \(used extensively by the Java Collections Framework\)
* K - Key
* N - Number
* T - Type
* V - Value
* S,U,V etc. - 2nd, 3rd, 4th types

你可以在`Java SE API`和接下来的课程中找到这些命名方式。

## Invoking and Instantiating a Generic Type

在你的代码中引用泛型`Box`类型，你必须用到`generic type invocation`（泛型类型调用），也就是将`T`替换为具体的值，比如`Integer`:

```java
Box<Integer> integerBox;
```

你可以认为泛型类型调用和普通的函数调用类似，不同的是函数调用是传入参数，而泛型类型调用是传一个类型参数-这个例子中是传Integer到Box类。

> Type Parameter and Type Argument Terminology:许多开发者会混淆术语`type parameter`\(类型参数\)和`type argument`（类型传参），但是它们是不同的。在编程时，提供`type argument`是为了创建一个参数化的类型，因此`Foo<T>`中的`T`就是`type parameter`，而`Foo<String> f`中的`String`就是`type argument`，本文遵循此定义。

与其他的变量声明一样，这段代码不会真的创建一个新的`Box`对象。它只是声明`integerBox`持有一个`Box of Integer`的引用，这就是读取`Box<Integer>`的方式。

调用泛型类型也被称为`parameterized type`\(参数化类型\)

为了实例化对象，我们通常使用`new`关键字，在类名和圆括号之间添加`<Integer>`:

```java
Box<Integer> integerBox = new Box<Integer>();
```

## The Diamond

在`Java SE 7`和以后版本，只要编译器能从上下文中确定或者推断出类型参数，就可以使用一组空的类型参数`<>`来替换掉用泛型类的构造函数所需的类型参数。这对尖括号`<>`，被称为`the diamond`\(菱形运算符\)。例如，你可以用下面这条语句创建一个`Box<Integer>`实例：

```java
Box<Integer> integerBox = new Box<>();
```

更多的菱形元算符信息请参考[Type Inference](https://docs.oracle.com/javase/tutorial/java/generics/genTypeInference.html)

## Multiple Type Parameters

上面提到过，一个泛型类可以有多个类型参数（parameters）。例如，实现`Pair`泛型接口的`OrderedPair`泛型类：

```java
public interface Pair<K, V> {
    public K getKey();
    public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

    private K key;
    private V value;

    public OrderedPair(K key, V value) {
    this.key = key;
    this.value = value;
    }

    public K getKey()    { return key; }
    public V getValue() { return value; }
}
```

下面的语句创建了两个`OrderedPair`类的实例：

```java
Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
Pair<String, String>  p2 = new OrderedPair<String, String>("hello", "world");
```

代码`new OrderedPair<String, Integer>`将`K`实例化为`String`，将`V`实例化为`Integer`。所以`OrderedPair`构造函数的参数类型分别是是`String`和`Integer`。根据[autoboxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)，传入`String`和`int`也是有效的。

根据上面提到的菱形运算符，由于Java编译器可以通过`OrderedPair<String, Integer>`的声明推断出`K`和`V`的类型，所以上面的语句可以通过菱形运算符缩短成：

```java
OrderedPair<String, Integer> p1 = new OrderedPair<>("Even", 8);
OrderedPair<String, String>  p2 = new OrderedPair<>("hello", "world");
```

和创建泛型类一样使用相同的规则创建泛型接口。

## Parameterized Types

你也可以将一个类型参数 \(i.e., K or V\) 替换为参数化类型\(i.e., List\)。拿`OrderedPair<K, V>`举例：

```java
OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<Integer>(...));
```

