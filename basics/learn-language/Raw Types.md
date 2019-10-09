`raw type`（原生类型）是不带任何类型传参的泛型类或者接口。例如，下面这个`Box`泛型类：

```java
public class Box<T> {
    public void set(T t) { /* ... */ }
    // ...
}
```

创建带参数的`Box<T>`类型，你需要为类型参数`T`提供实际的类型传参：

```java
Box<Integer> intBox = new Box<>();
```

如果类型传参被省略，你就创建了一个`Box<T>`的原生类型：

```java
Box rawBox = new Box();
```

也就是说，`Box`是泛型类型`Box<T>`的原生类型。但是，非泛型类或者接口类型不是原生类型。

原生类型在旧版本代码中，因为在JDK5.0之前许多API类（比如`Collections`类）。当使用原生类型时，你其实使用的是`pre-generics`行为-`Box`为你提供对象。为了向下兼容，可以将参数化的类型负值给它的原生类型：

```java
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;               // OK
```