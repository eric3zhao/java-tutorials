# Lower Bounded Wildcards

在[Upper Bounded Wildcards](lower-bounded-wildcards.md)部分中已经展示上界通配符如何通过`extends`关键字将未知类型限制成特定类型或该类型的子类型。同样的，`lower bounded`\(下界\)通配将未知类型限制成特定类型或该类型的`super type`（超类型）

一个下界通配表示如下：先写一个通配符`?`，后面紧跟`super`关键字，最后是它的下界：`<? super A>`

> 你不能同时指定通配的上界和下界，只能两者选其一

假设我们想写一个函数将`Integer`类型的对象添加到列表中，我们希望函数对`List<Integer>, List<Number>, List<Object>`（任何能持有`Integer`值的）都起作用

为了编写一个函数对于`Integer`及其超类型（比如`Number`和`Object`）的列表都能工作，我们可以指定`List<? super Integer>`。`List<Integer>`比`List<? super Integer>`的限制更大，因为它只接受`Integer`类型的列表，而后者接受任何`Integer`的超类型。

下面一段代码在列表尾部添加数字1到10:

```java
public static void addNumbers(List<? super Integer> list) {
    for (int i = 1; i <= 10; i++) {
        list.add(i);
    }
}
```

[Guidelines for Wildcard Use](lower-bounded-wildcards.md)部分介绍了何时使用上界通配或者下界通配。

