# Bounded Type Parameters

有时我们想要限制参数化类型中所使用的类型参数。比如，一个操作数字的函数可能只接受`Number`类及其子类的实例。这就是我们所说的`Bounded Type Parameters`（有界类型参数）。

在定义有界类型参数时，先列出类型参数名，后面紧跟`extends`关键字，最后是参数的`upper bound`\(上界\)，例子中我们用的是`Number`。请注意，在这语境中，`extends`被用在一个普通场景表示对类的`extends`（扩展）或者对接口的`implements`（实现）。

```java
public class Box<T> {

    private T t;          

    public void set(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }

    public <U extends Number> void inspect(U u){
        System.out.println("T: " + t.getClass().getName());
        System.out.println("U: " + u.getClass().getName());
    }

    public static void main(String[] args) {
        Box<Integer> integerBox = new Box<Integer>();
        integerBox.set(new Integer(10));
        integerBox.inspect("some text"); // error: this is still String!
    }
}
```

修改`Box`类中的泛型函数使得它包含有界类型参数，编译就会失败，因为我们在调用`inspect`时任然使用了`String`类型：

```java
Box.java:21: <U>inspect(U) in Box<java.lang.Integer> cannot
  be applied to (java.lang.String)
                        integerBox.inspect("10");
                                  ^
1 error
```

除了限制可用于实例化泛型类型的类型之外，有界类型参数还允许我们调用边界类型中定义的方法：

```java
public class NaturalNumber<T extends Integer> {

    private T n;

    public NaturalNumber(T n)  { this.n = n; }

    public boolean isEven() {
        return n.intValue() % 2 == 0;
    }

    // ...
}
```

`isEven`函数中通过`n`调用`Integer`类中定义的`intValue`函数。

## Multiple Bounds

上面的例子说明了单个上界类型的类型参数的使用方式，然而一个类型参数可以有`Multiple Bounds`\(多个上界类型\)：

```java
<T extends B1 & B2 & B3>
```

一个有多个上界类型的类型变量即为所有上界类型的子类型。如果其中一个上界类型是一个累的话，他应该出现在第一位，比如：

```text
Class A { /* ... */ }
interface B { /* ... */ }
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }
```

如果类`A`没有出现在第一位，在编译过程中将会报错：

```text
class D <T extends B & A & C> { /* ... */ }  // compile-time error
```

