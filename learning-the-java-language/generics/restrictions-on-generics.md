为了更有效的使用泛型，我们需要注意以下这些限制：

* 无法使用基础类型实例化泛型类型
* 不能创建类型参数的实例
* 不能将静态字段的类型定义为类型参数
* 不能对参数化类型使用`Casts`（强转）或者`instanceof`
* 不能创建参数化类型的数组
* 不能对参数化类型进行`Create`，`Catch`或者`Throw`
* 如果重载函数的形参进行类型擦除以后为相同的原始类型，则无法对函数进行重栽

### Cannot Instantiate Generic Types with Primitive Types

以下面的参数化类型为例：

```java
class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    // ...
}
```

在创建一个`Pair`对象时，我们不能使用基础类型来替换类型参数`K`或者`V`：

```java
Pair<int, char> p = new Pair<>(8, 'a');  // compile-time error
```

我们只能将类型参数`K`或者`V`替换为非基础类型：

```java
Pair<Integer, Character> p = new Pair<>(8, 'a');
```

注意这里Java编译器会将`8`自动装箱（`autoboxes`）为`Integer.valueOf(8)`，`a`自动装箱为`Character('a')`：

```java
Pair<Integer, Character> p = new Pair<>(Integer.valueOf(8), new Character('a'));
```

更多关于自动装箱的内容，请参考[Numbers and Strings]()章节中的[Autoboxing and Unboxing]()

### Cannot Create Instances of Type Parameters

我们不对使用类型参数创建实例。下面这段代码将会引起编译时报错：

```java
public static <E> void append(List<E> list) {
    E elem = new E();  // compile-time error
    list.add(elem);
}
```

变通的方式是，我们可以通过类型参数的反射来创建相应的对象：

```java
public static <E> void append(List<E> list, Class<E> cls) throws Exception {
    E elem = cls.newInstance();   // OK
    list.add(elem);
}
```

我们可以像下面这段代码一样调用`append`函数：

```java
List<String> ls = new ArrayList<>();
append(ls, String.class);
```

### Cannot Declare Static Fields Whose Types are Type Parameters

一个类中的静态字段是类级别的也就是被该类的所有非静态实例所共享的。因此，不允许有类型参数的静态字段。以下面的类为例：

```java
public class MobileDevice<T> {
    private static T os;

    // ...
}
```

如果允许存在类型参数的静态字段，那么下面这段代码是令人困惑的：

```java
MobileDevice<Smartphone> phone = new MobileDevice<>();
MobileDevice<Pager> pager = new MobileDevice<>();
MobileDevice<TabletPC> pc = new MobileDevice<>();
```
由于静态字段`os`是`phone, pager, pc`共享的，那么`os`真正的类型是什么呢？它不可能同时是`Smartphone, Pager, TabletPC`。所以我们不能创建类型参数的静态字段。

### Cannot Use Casts or instanceof with Parameterized Types

由于Java编译器会擦出所有泛型代码中的类型参数，所以在运行时，我们无法检验使用的是泛型类型的哪种参数化类型：

```java
public static <E> void rtti(List<E> list) {
    if (list instanceof ArrayList<Integer>) {  // compile-time error
        // ...
    }
}
```

传入`rtti`函数的参数化类型的集合是：

```
S = { ArrayList<Integer>, ArrayList<String>, LinkedList<Character>, ... }
```

运行时并不保留类型参数的痕迹，所以并不能区分`ArrayList<Integer>`和`ArrayList<String>`。我们能做的很自由使用无边界通配去检验队列是否是`ArrayList`：

```java
public static void rtti(List<?> list) {
    if (list instanceof ArrayList<?>) {  // OK; instanceof requires a reifiable type
        // ...
    }
}
```


通常，除非使用无边界通配对其进行参数化，否则无法将其强转为参数化类型。以下面代码为例：

```java
List<Integer> li = new ArrayList<>();
List<Number>  ln = (List<Number>) li;  // compile-time error
```

然而，有些情况下编译器能识别类型参数，此时强转是允许并有效的。比如：

```
List<String> l1 = ...;
ArrayList<String> l2 = (ArrayList<String>)l1;  // OK
```

### Cannot Create Arrays of Parameterized Types

我们不能创建参数化类型的数组，比如下面这段代码无法编译通过：

```java
List<Integer>[] arrayOfLists = new List<Integer>[2];  // compile-time error
```

下面这段代码展示了将不同类型的数据插入同一个数组是会发生什么：

```java
Object[] strings = new String[2];
strings[0] = "hi";   // OK
strings[1] = 100;    // An ArrayStoreException is thrown.
```

当我对泛型列表做同样的操作，就会产生问题：

```java
Object[] stringLists = new List<String>[];  // compiler error, but pretend it's allowed
stringLists[0] = new ArrayList<String>();   // OK
stringLists[1] = new ArrayList<Integer>();  // An ArrayStoreException should be thrown,
                                            // but the runtime can't detect it.
```

如果允许使用参数化列表的数组，那么上面的代码将无法抛出应有的`ArrayStoreException`

### Cannot Create, Catch, or Throw Objects of Parameterized Types

泛型类不可以直接或者间接的扩展`Throwable`类，下面的类无法编译通过：

```java
// Extends Throwable indirectly
class MathException<T> extends Exception { /* ... */ }    // compile-time error

// Extends Throwable directly
class QueueFullException<T> extends Throwable { /* ... */ // compile-time error
```

函数中无法捕获类型参数的实例

```java
public static <T extends Exception, J> void execute(List<J> jobs) {
    try {
        for (J job : jobs)
            // ...
    } catch (T e) {   // compile-time error
        // ...
    }
}
```

然而，我们可以在`throws`条件中使用类型参数：

```java
class Parser<T extends Exception> {
    public void parse(File file) throws T {     // OK
        // ...
    }
}
```

### Cannot Overload a Method Where the Formal Parameter Types of Each Overload Erase to the Same Raw Type

一个类不能具有两个在类型擦除后将具有相同的签名的重载函数

```java
public class Example {
    public void print(Set<String> strSet) { }
    public void print(Set<Integer> intSet) { }
}
```

上面的重载将共享相同的类文件，所以将生成编译时报错