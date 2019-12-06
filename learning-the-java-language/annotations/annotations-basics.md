# Annotations Basics

## The Format of an Annotation

最简单的注解如下所示：

```java
@Entity
```

`at`符号（@）告诉编译器接下来是一个注解。下面的例子中，注解的名字是`Override`：

```java
@Override
void mySuperMethod() { ... }
```

注解可以包含`elements`\(元素\)，元素可以`named`或者`unnamed`，这些元素可以被赋值：

```java
@Author(
   name = "Benjamin Franklin",
   date = "3/27/2003"
)
class MyClass() { ... }

@SuppressWarnings(value = "unchecked")
void myMethod() { ... }
```

如果只有一个名为`value`的元素，那么可以省略元素名，像这样：

```java
@SuppressWarnings("unchecked")
void myMethod() { ... }
```

如果注解不包含元素，那么括号可以省略，就像上面提到的`@Override`。

可以在同一个声明中使用多个注解：

```java
@Author(name = "Jane Doe")
@EBook
class MyClass { ... }
```

如果多个注解类型是一样的，则被称为重复注解\(`repeating annotation`\)：

```java
@Author(name = "Jane Doe")
@Author(name = "John Smith")
class MyClass { ... }
```

`Java SE 8 release`提供对重复注解的支持，后面的章节中会提到。

注解的类型可以是`Java SE API`的`java.lang or java.lang.annotation`包中定义的类型中其中的一个，比如前面提到的`Override`和`SuppressWarnings`。当然你可以定义自己的注解类型，比如前面提到的`Author`和`Ebook`就是自定义注解。

## Where Annotations Can Be Used

可以在声明中使用注解：声明类，字段，函数，或者别的程序元素。在声明场景下时候时，默认约定每个声明都独占一行。

`Java SE 8 release`中注解可以应用为`the use of types`（没找到好的翻译），比如下面的例子：

* **Class instance creation expression**：

  ```java
    new @Interned MyObject();
  ```

* **Type cast**：

  ```java
    myString = (@NonNull String) str;
  ```

* **implements clause**：

  ```java
    class UnmodifiableList<T> implements
        @Readonly List<@Readonly T> { ... }
  ```

* **Thrown exception declaration**：

  ```java
    void monitorTemperature() throws
        @Critical TemperatureException { ... }
  ```

这种类型的注解被称为`type annotation`，后面的章节将会提到。

