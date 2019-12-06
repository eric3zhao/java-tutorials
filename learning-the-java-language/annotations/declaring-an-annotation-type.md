# Declaring an Annotation Type

许多注解在代码中中来代替注释。

假设软件团队的传统时在类的`body`开头提供重要信息：

```java
public class Generation3List extends Generation2List {
   // Author: John Doe
   // Date: 3/17/2002
   // Current revision: 6
   // Last modified: 4/12/2004
   // By: Jane Doe
   // Reviewers: Alice, Bill, Cindy
   // class code goes here
}
```

使用注解添加相同的元数据，首先你需要定义注解类型。语法如下所示：

```java
@interface ClassPreamble {
   String author();
   String date();
   int currentRevision() default 1;
   String lastModified() default "N/A";
   String lastModifiedBy() default "N/A";
   // Note use of array
   String[] reviewers();
}
```

注解类型的定义看起来和接口的定义很想，最是在`interface`关键词前加了`at`标志\(@\) \(@ = AT, as in annotation type\)。注解类型是接口的一种形式，在后面的章节中我们会介绍。

上面注解的定义中包含`annotation type element`的声明，看起来像函数。注意它们可以可选定义默认值。

当注解已经定义，你可以使用它，记得填写值，例子如下：

```java
@ClassPreamble (
   author = "John Doe",
   date = "3/17/2002",
   currentRevision = 6,
   lastModified = "4/12/2004",
   lastModifiedBy = "Jane Doe",
   // Note array notation
   reviewers = {"Alice", "Bob", "Cindy"}
)
public class Generation3List extends Generation2List {
// class code goes here
}
```

-

> **Note**：为了确保`@ClassPreamble`的信息出现在`Javadoc`生成的文档里，你必须在`@ClassPreamble`的定义中添加`@Documented`注解：

```java
 // import this to use @Documented
import java.lang.annotation.*;

@Documented
@interface ClassPreamble {
   // Annotation element definitions
}
```

