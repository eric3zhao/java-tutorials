
在学习使用泛型编程时，最令人困惑的方面之一是确定何时使用上界通配符以及何时使用下界通配符。此小节提供了一些在设计代码时要遵循的准则。

为了便于讨论，将变量视为提供两个功能之一是很有帮助的：

**An "In" Variable**

`in`（输入）变量将数据提供给代码。想象一个具有两个参数的复制函数：`copy（src，dest）`。`src`参数提供要复制的数据，因此它是`in`参数。

**An "Out" Variable**

`out`（输出）变量保存要在其他地方使用的数据。在复制函数`copy（src，dest）`中，`dest`参数接受数据，因此它是`out`参数。

当然有些变量是输入也是输出，这种场景也会讨论到。

在决定是否使用通配符以及使用哪种类型的通配符时，可以使用“输入”和“输出”原则。

**Wildcard Guidelines:**
> * `in`变量使用通过`extends`关键字定义的上界通配符
> * `out`变量使用通过`super`关键字定义的下界通配符
> * 如果可以使用`Object`类中定义的方法访问`in`变量，请使用无界通配符。
> * 如果代码需要同时使用`in`和`out`变量来访问变量，则不要使用通配符。

这些准则不适用于函数的返回类型。应该避免使用通配符作为返回类型，因为这样会迫使程序员使用代码来处理通配符。

由`List<? extends ...>`定义的列表可以非正式地当作是只读的，但这不并是严格的保证。假设我们有如下两个类：

```java
class NaturalNumber {

    private int i;

    public NaturalNumber(int i) { this.i = i; }
    // ...
}

class EvenNumber extends NaturalNumber {

    public EvenNumber(int i) { super(i); }
    // ...
}
```

再看下面的代码：

```java
List<EvenNumber> le = new ArrayList<>();
List<? extends NaturalNumber> ln = le;
ln.add(new NaturalNumber(35));  // compile-time error
```

因为`List <EvenNumber>`是`List <？extends  NaturalNumber>`的子类型，我们可以将le赋值给ln。但是我们不能使用ln将自然数添加到偶数列表中。列表中的以下操作是可能的：

* 添加`null`元素
* 调用`clear`
* 获取`iterator`并且调用`remove`
* 捕获通配并写入从列表中读取的元素

我们可以看到由`List <？extended NaturalNumber>`定义的列表，从严格意义上讲不是只读的，但是我们可以认为它是只读，因为我们无法插入新元素或更改列表中的现有元素。