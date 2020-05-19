# Raw Types

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

但是如果我们将原生类型赋值给一个参数化类型，将会得到警告：

```java
Box rawBox = new Box();           // rawBox is a raw type of Box<T>
Box<Integer> intBox = rawBox;     // warning: unchecked conversion
```

如果我们使用原生类型来调用在相应的泛型类型中定义的泛型方法，也会收到警告：

```java
Box<String> stringBox = new Box<>();
Box rawBox = stringBox;
rawBox.set(8);  // warning: unchecked invocation to set(T)
```

该警告表明原生类型会绕过泛型类型检查，从而导致不安全代码的捕获推迟到运行时。因此，我们应避免使用原生类型。

在后面的`Type Erasure`部分将会介绍更多的Java编译器如何使用原生类型的信息。

## Unchecked Error Messages

就像上面提到的，当遗留代码和泛型代码混合在一起时，我们可能遇到如下的告警信息：

```text
Note: Example.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
```

当我们使用操作原生类型的老旧`API`就会发生这种情况，以下面的代码为例：

```java
public class WarningDemo {
    public static void main(String[] args){
        Box<Integer> bi;
        bi = createBox();
    }

    static Box createBox(){
        return new Box();
    }
}
```

`unchecked`这个术语的意思是编译器没有足够的类型信息来进行全类型检查从而确保类型安全。虽然编译器给出了提示，但是默认情况下`unchecked`警告是无效的。使用`-Xlint:unchecked`参数重新编译可以查看所有`unchecked`警告。

使用`-Xlint:unchecked`参数重新编译上面的例子会得到如下信息：

```text
WarningDemo.java:4: warning: [unchecked] unchecked conversion
found   : Box
required: Box<java.lang.Integer>
        bi = createBox();
                      ^
1 warning
```

想要完全警用未检查警告，可以使用参数`-Xlint:-unchecked`。注解`@SuppressWarnings("unchecked")`阻止所有的未检查警告。如果对`@SuppressWarnings`语法不熟悉，参考`Annotations`章节的相关内容。

