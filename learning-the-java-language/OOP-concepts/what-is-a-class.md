在现实世界中，你会经常发现许多同一类型的独立对象。在生活中有成千上万相同的自行车。每个自行车采用相同的设计制造，所以包含相同的部件。在面向对象术语中，我们会这样说：你的自行车是自行车`objects`（对象）对应的`class`（类）的一个实例。类就是单个对象创建时使用的蓝图。

下面`Bicycle`类就是自行车实现的一种可能：

```java
class Bicycle {

    int cadence = 0;
    int speed = 0;
    int gear = 1;

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

你可能对Java编程语言的语法感到陌生，但是这个类的设计师基于前面套路的自行车对象。属性`cadence`，`speed`和`gear`表示对象的状态，函数（`changeCadence`，`changeGear`，`speedUp` etc.）定义了它与外界的交互。

你可能发现`Bicycle`类不包含一个`main`函数。这是因为它不是一个完整的应用。它只是可能在程序中使用的自行车的蓝图。创建和使用新`Bicycle`对象是应用中其他类的责任。

下面`BicycleDemo`类会创建两个单独的`Bicycle`对象然后调用他们的函数

```java
class BicycleDemo {
    public static void main(String[] args) {

        // Create two different 
        // Bicycle objects
        Bicycle bike1 = new Bicycle();
        Bicycle bike2 = new Bicycle();

        // Invoke methods on 
        // those objects
        bike1.changeCadence(50);
        bike1.speedUp(10);
        bike1.changeGear(2);
        bike1.printStates();

        bike2.changeCadence(50);
        bike2.speedUp(10);
        bike2.changeGear(2);
        bike2.changeCadence(40);
        bike2.speedUp(10);
        bike2.changeGear(3);
        bike2.printStates();
    }
}
```

以下是该测试输出的两个自行车的最终踏板节奏，速度和档位

```
cadence:50 speed:10 gear:2
cadence:40 speed:20 gear:3
```