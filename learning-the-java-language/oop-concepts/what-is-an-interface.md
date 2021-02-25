# What Is an Interface?

正如你前面学到的，对象通过他们所暴露的函数来实现他们与外界的交互。函数形成对象与外界的接口；例如，电视机上的按钮是你与电视机塑料外壳另一侧的电线之间的接口。你通过按`电源`按钮来打开或关闭电视。

接口的常见形式是，一组相互关联的没有函数体的函数。以自行车为例，如果创建一个接口，可能的样子是：

```java
interface Bicycle {

    //  wheel revolutions per minute
    void changeCadence(int newValue);

    void changeGear(int newValue);

    void speedUp(int increment);

    void applyBrakes(int decrement);
}
```

为了实现这个这个接口，类的名字需要修改（使用具体某个品牌的名字，比如，ACMEBicycle），在类的声明中使用`implements`关键字

```java
class ACMEBicycle implements Bicycle {

    int cadence = 0;
    int speed = 0;
    int gear = 1;

   // The compiler will now require that methods
   // changeCadence, changeGear, speedUp, and applyBrakes
   // all be implemented. Compilation will fail if those
   // methods are missing from this class.

    void changeCadence(int newValue) {
         cadence = newValue;
    }

    void changeGear(int newValue) {
         gear = newValue;
    }

    void speedUp(int increment) {
         speed = speed + increment;   
    }

    void applyBrakes(int decrement) {
         speed = speed - decrement;
    }

    void printStates() {
         System.out.println("cadence:" +
             cadence + " speed:" + 
             speed + " gear:" + gear);
    }
}
```

实现接口可以使得类对其承诺提供的行为更为正式，接口在类和外部世界之间形成契约，并且该契约在编译时由编译器强制执行。如果你的类需要实现一个接口，那么编译以前需要在类中实现所以接口定义的函数

> 在实际编译ACMEBicycle类的时候，需要在实现接口函数的时候添加`public`关键字。你将在后续的课程中学到原因[Classes and Objects](https://docs.oracle.com/javase/tutorial/java/javaOO/index.html)和[Interfaces and Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/index.html)