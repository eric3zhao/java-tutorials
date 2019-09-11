`Annotations`（注解），`metadata`（愿数据）的一种形式，提供了程序的相关数据但并不是程序的一部分，注解不会影响它所标注的代码的运行。

注解有很多用途，其中有：

* **Information for the compiler**：编译器可以使用注解检测报错或者禁止警告。
* **Compile-time and deployment-time processing**：软件工具可以处理注释信息用以生成代码，XML文件等。
* **Runtime processing**：有些注释可以在运行时被检查。

这几章我们将说明哪里可以使用注释，怎么应用注释，JavaSE平台(Java SE API)可用的预定义注释类型，类型注解何如结合可插入类型系统来编写强类型检查的代码，以及如何实现重复注解。