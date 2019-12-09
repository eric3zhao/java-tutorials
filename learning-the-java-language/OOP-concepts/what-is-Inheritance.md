不同类型的对象之间经常会有某些相同的地方。比如，山地自行车，公路自行车和双人自行车都具有自行车的特征（当前速度、当前踏板节奏、当前档位）。然而它们同样定义了使它们不同的附加特性：双人自行车有两个座位和两幅车把；公路自行车有赛车车把；有些山地有一个额外的连环，使得它们有更低的档位比。

面向对象编程允许类从其他类` inherit`（继承）通用的状态和行为。在这个例子中，`Bicycle`是`MountainBike`，`RoadBike`和`TandemBike`的`superclass`（超类）。在Java编程语言中每个类可以有一个直接超类，每个超类可能有无限数量的`subclass`子类：

![ A hierarchy of bicycle classes ](https://docs.oracle.com/javase/tutorial/figures/java/concepts-bikeHierarchy.gif)

创建子类的语法很简单。在你类定义的前端使用`extends`关键字，后面紧跟着被继承类的名字：

<pre>
class MountainBike <strong>extends</strong> Bicycle {
// new fields and methods defining
// a mountain bike would go here
}
</pre>

这让`MountainBike`拥有`Bicycle`一样的属性和函数，但允许其代码只关注使其独特的特性。这使得你的子类的代码更易于阅读。但是，你必须注意正确地为每个超类定义的状态和行为提供文档，因为超类代码不会出现在每个子类的源文件中。

