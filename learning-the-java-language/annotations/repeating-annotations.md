# Repeating Annotations

在一些场景中你想要在声明或者类型使用上添加一些相关的注解。`Java SE 8 release`中提供了`repeating annotations`实现这个功能。

例如，你想使用定时服务编写代码使得在指定时间或者是按特定计划运行一个函数，类似于`UNIX`中的`cron`服务。现在你想设置一个定时器去运行函数`doPeriodicCleanup`，该定时器在每个月的最后一天和每个星期五晚上11点整启动。为了实现定时器，创建一个名为`@Schedule`的注解，并在`doPeriodicCleanup`函数上使用两次。第一次我们指定每个月的最后一天，第二次我们指定星期五晚上11点整，具体代码如下所示：

```java
@Schedule(dayOfMonth="last")
@Schedule(dayOfWeek="Fri", hour="23")
public void doPeriodicCleanup() { ... }
```

这个例子在函数上应用注解。你可以使用标准注解的所有地方重复注解。例如，你有一个处理未授权访异常的类，。你在类上为`manager`和其他`admin`添加一个`@Alert`注解：

```java
@Alert(role="Manager")
@Alert(role="Administrator")
public class UnauthorizedAccessException extends SecurityException { ... }
```

由于兼容的原因，重复注解被保存由Java编译器自动生成的`container annotation`中。为了让编译器实现上述功能，在你代码中需要两个声明。

## Step 1: Declare a Repeatable Annotation Type

注解类型必须被`@Repeatable`元注解标记，下面这个例子定义了一个名为`@Schedule`的可重复注解：

```java
import java.lang.annotation.Repeatable;

@Repeatable(Schedules.class)
public @interface Schedule {
  String dayOfMonth() default "first";
  String dayOfWeek() default "Mon";
  int hour() default 12;
}
```

`@Repeatable`元注解括号里面的值，就是Java编译器用来生成并保存重复注解的`container annotation`（容器注解）的类型。这个例子中，容器注解的类型是`Schedules`，也就是说重复的`@Schedule`注解保存在一个`@Schedules`注解中。

将相同的未声明可重复的注解应用在同一个声明中会导致编译时报错。

## Step 2: Declare the Containing Annotation Type

容器注解类型必须包含一个数组类型的`value`元素。数组中元素的类型必须是可重复注解类型。名为`Schedules`的容器注解声明如下：

```java
public @interface Schedules {
    Schedule[] value();
}
```

**Retrieving Annotations**

`Reflection API`提供了一些函数用来获取注解。有些函数返回单个注解，比如[ AnnotatedElement.getAnnotation\(Class\)](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html#getAnnotation-java.lang.Class-)，如果存在所请求类型的一个注释，它们仅返回单个注释，如果存在一个以上的注解，你可以先通过使用它们的容器注解来获取它们。通过这样，确保遗产代码能正常运行。Java SE 8中的其他函数可以扫描容器注解使得一次放回多个注解，比如[AnnotatedElement.getAnnotationsByType\(Class\)](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html#getAnnotationsByType-java.lang.Class-)。阅读[AnnotatedElement](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/AnnotatedElement.html)的规范获取跟多信息。

**Design Considerations**

当设计一个注解类型时，你必须考虑该类型的注解`cardinality`（基数）。现在可以不使用注解或者使用一次，如果注解的类型被标记为`@Repeatable`可以使用多次。可以通过使用`@Target`来限制使用一个注解的地点。例如你可以创建一个只能用在函数和属性上的可重复注解。认真的设计你的注解类型是很重要的，这样可以确保开发在使用发现它既灵活有强大

