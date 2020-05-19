# Generic Methods

`Generic methods`（泛型函数）是引入自身类型参数的函数。和声明参数类型类似，只不过类型参数的作用域被限制在声明它的函数中。静态和非静态的泛型函数都是允许的，以及泛型类构造函数。

泛型函数的语法包含是在函数的返回类型之前使用两个尖括号中列出所有的类型参数。对于静态泛型函数，类型参数部分必须在函数返回类型之前。

下面这个`Util`类包含一个泛型函数`compare`，改函数用来比较两个`Pair`对象：

```java
public class Util {
    public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
        return p1.getKey().equals(p2.getKey()) &&
               p1.getValue().equals(p2.getValue());
    }
}

public class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public void setKey(K key) { this.key = key; }
    public void setValue(V value) { this.value = value; }
    public K getKey()   { return key; }
    public V getValue() { return value; }
}
```

完整的调用这个函数的语法如下：

```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.<Integer, String>compare(p1, p2);
```

上面第三行中提供了明确的类型。通常，这些可以被省略而编译器将推断出所需的类型：

```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.compare(p1, p2);
```

这个特性，被称为`type inference`（类型推断），这就使得我们可以像普通函数一样调用一个泛型函数而不需要在尖括号中指定类型。更多的内容我们会在后续`Type Inference`进行讨论。

