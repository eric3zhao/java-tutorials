有界的类型参数是实现通用算法的关键。设想下面这个函数用来计算一个数组`array T[]`中元素的值比某个特定元素`elem`大的数量。

```java
public static <T> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e > elem)  // compiler error
            ++count;
    return count;
}
```

改函数的实现很简单，但是并不能正常编译，因为大于符号`>`只能用于原始类型，比如`short, int, double, long, float, byte, char`。我们不能用`>`来比较对象。为了修复这个问题，我们可以使用接口`Comparable<T>`作为类型参数的上界：

```java
public interface Comparable<T> {
    public int compareTo(T o);
}
```

最终代码如下：

```java
public static <T extends Comparable<T>> int countGreaterThan(T[] anArray, T elem) {
    int count = 0;
    for (T e : anArray)
        if (e.compareTo(elem) > 0)
            ++count;
    return count;
}
```