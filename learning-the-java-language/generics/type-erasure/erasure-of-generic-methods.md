Java编译器也会擦除泛型函数传参中的类型参数，以下面的泛型函数为例：

```java
// Counts the number of occurrences of elem in anArray.
//
public static <T> int count(T[] anArray, T elem) {
    int cnt = 0;
    for (T e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}
```

`T`是无边界的，所以Java编译器将它替换为`Object`：

```java
public static int count(Object[] anArray, Object elem) {
    int cnt = 0;
    for (Object e : anArray)
        if (e.equals(elem))
            ++cnt;
        return cnt;
}
```

假设有如下几个类的定义：

```java
class Shape { /* ... */ }
class Circle extends Shape { /* ... */ }
class Rectangle extends Shape { /* ... */ }
```

我们可以编译一个通用的函数来画不同的形状：

```java
public static <T extends Shape> void draw(T shape) { /* ... */ }
```

Java编译器会将`T`替换为`Shape`：

```java
public static void draw(Shape shape) { /* ... */ }
```