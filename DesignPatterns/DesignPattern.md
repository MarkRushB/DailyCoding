# Design Pattern

设计模式（Design pattern）是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。

**设计模式的目的：**
1. 代码重用性（相同功能的代码，不用多次编写）
2. 可读性（编程规范性，便于其他程序员的阅读和理解）
3. 可扩展性（当需要增加新的功能时，非常方便，称之为可维护）
4. 可靠性（当增加新的功能后，对原来的功能没有影响）
5. 使程序呈现高内聚，低耦合的特性

> 设计模式包含了面向对象的精髓，“懂了设计模式，你就懂了面向对象分析和设计（OOA/D）的精要”


## Seven principles of OOD

**Principle of single responsibility 单一职责原则**

Single responsibility principle (SRP). It says that there should be no more than one reason for class changes, which is an embodiment of high cohesion and low coupling.

对类来说的，即一个类应该只负责一项职责。如类 A 负责两个不同职责：职责 1，职责 2。当职责 1 需求变更而改变 A 时，可能造成职责 2 执行错误，所以需要将类 A 的粒度分解为 A1，A2

单一职责原则注意事项和细节

- 降低类的复杂度，一个类只负责一项职责。
- 提高类的可读性，可维护性
- 降低变更引起的风险
- 通常情况下，我们应当遵守单一职责原则，只有逻辑足够简单，才可以在代码级违反单一职责原则；只有类中方法数量足够少，可以在方法级别保持单一职责原则

>[单一职责原则](https://www.bilibili.com/video/av57936239?p=6)
>[单一职责原则小结](https://www.bilibili.com/video/av57936239?p=7)

**Interface isolation principle 接口隔离原则**

Interface segmentation principle (ISP), the principle of interface isolation, “breaking up a large interface into several small independent interfaces”. Because Java classes support multiple interfaces, it is easy to make classes have the characteristics of multiple interfaces, and each class can selectively only implement the target interface.

客户端不应该依赖它不需要的接口，即一个类对另一个类的依赖应该建立在最小的接口上

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200823233320.png)

类 A 通过接口 Interface1 依赖类 B，类 C 通过接口 Interface1 依赖类 D，如果接口 Interface1 对于类 A 和类 C
来说不是最小接口，那么类 B 和类 D 必须去实现他们不需要的方法。

按隔离原则应当这样处理：
将接口 Interface1 拆分为独立的几个接口(这里我们拆分成 3 个接口)，类 A 和类 C 分别与他们需要的接口建立依赖关系。也就是采用接口隔离原则

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200823233540.png)



>[接口隔离原则 (1)](https://www.bilibili.com/video/av57936239?p=8)
>[接口隔离原则 (2)](https://www.bilibili.com/video/av57936239?p=9)
>[接口隔离原则小结](https://www.bilibili.com/video/av57936239?p=10)

**Principle of Dependence Inversion 依赖倒转原则**

Dependency Inversion Principle (DIP). It says that “design and implementation depend on abstraction rather than concrete”. On the one hand, abstraction is more in line with people’s thinking habits; on the other hand, according to the Richter’s replacement principle, it is easy to replace the original abstraction with the expanded concrete, which can well support the open close principle.

- 高层模块不应该依赖低层模块，二者都应该依赖其抽象
- 抽象不应该依赖细节，细节应该依赖抽象
- 依赖倒转(倒置)的中心思想是面向接口编程
- 依赖倒转原则是基于这样的设计理念：相对于细节的多变性，抽象的东西要稳定的多。以抽象为基础搭建的架构比以细节为基础的架构要稳定的多。在 java 中，抽象指的是接口或抽象类，细节就是具体的实现类
- 使用接口或抽象类的目的是制定好规范，而不涉及任何具体的操作，把展现细节的任务交给他们的实现类去完成

注意事项：
低层模块尽量都要有抽象类或接口，或者两者都有，程序稳定性更好.
变量的声明类型尽量是抽象类或接口, 这样我们的变量引用和实际对象间，就存在一个缓冲层，利于程序扩展和优化
继承时遵循里氏替换原则

>[依赖倒转原则 (1)](https://www.bilibili.com/video/av57936239?p=11)
>[依赖倒转原则 (2)](https://www.bilibili.com/video/av57936239?p=12)
>[依赖倒转原则小结](https://www.bilibili.com/video/av57936239?p=13)

**Richter’s principle of substitution 里氏替换原则**

Liskov Substitution Principle (LSP). The principle states that “a subclass must be able to replace its parent, otherwise it should not be designed as a subclass.”. In other words, where the parent class appears, it should be able to be replaced by its child class. Therefore, the subclass can only extend the base class, not hide or cover the base class.

OO 中的继承性的思考和说明
1. 继承包含这样一层含义：父类中凡是已经实现好的方法，实际上是在设定规范和契约，虽然它不强制要求所有的子类必须遵循这些契约，但是如果子类对这些已经实现的方法任意修改，就会对整个继承体系造成破坏。

2. 继承在给程序设计带来便利的同时，也带来了弊端。比如使用继承会给程序带来侵入性，程序的可移植性降低， 增加对象间的耦合性，如果一个类被其他的类所继承，则当这个类需要修改时，必须考虑到所有的子类，并且父类修改后，所有涉及到子类的功能都有可能产生故障

2. 问题提出：在编程中，如何正确的使用继承? => 里氏替换原则

基本介绍：
- 里氏替换原则(Liskov Substitution Principle)在 1988 年，由麻省理工学院的以为姓里的女士提出的。
- 如果对每个类型为 T1 的对象 o1，都有类型为 T2 的对象 o2，使得以 T1 定义的所有程序 P 在所有的对象 o1 都代换成 o2 时，程序 P 的行为没有发生变化，那么类型T2 是类型 T1 的子类型。换句话说，所有引用基类的地方必须能透明地使用其子类的对象。
- 在使用继承时，遵循里氏替换原则，**在子类中尽量不要重写父类的方法**
- 里氏替换原则告诉我们，继承实际上让两个类耦合性增强了，在适当的情况下，可以通过**聚合，组合，依赖 来解决问题**。


>[里氏替换原则 (1)](https://www.bilibili.com/video/av57936239?p=14)
>[里氏替换原则 (2)](https://www.bilibili.com/video/av57936239?p=15)

**Open close principle 开闭原则**

Open close principle (OCP). Open means to open an extension, that is, to support convenient extension; closed means to close a modification, that is, to strictly limit the modification of existing content. Open close principle is the most abstract and important ood principle. Simple factory mode, factory method mode and abstract factory mode all mentioned how to follow the open close principle through good design.

基本介绍：
- 开闭原则（Open Closed Principle）是编程中最基础、最重要的设计原则
- 一个软件实体如类，模块和函数应该对扩展开放(对提供方)，对修改关闭(对使用方)。用抽象构建框架，用实现扩展细节。
- 当软件需要变化时，尽量通过扩展软件实体的行为来实现变化，而不是通过修改已有的代码来实现变化。
- 编程中遵循其它原则，以及使用设计模式的目的就是遵循开闭原则。


>[开闭原则 (1)](https://www.bilibili.com/video/av57936239?p=16)
>[开闭原则 (2)](https://www.bilibili.com/video/av57936239?p=17)
>[开闭原则小结](https://www.bilibili.com/video/av57936239?p=18)

**Dimitar’s law / the least known principle 迪米特法则**

Law of Demeter or least knowledge principle (LOD or LKP). It’s about “one object understands as few other objects as possible” to achieve loose coupling. If a class has too many responsibilities, any change of responsibilities may cause problems of other responsibilities due to the coupling of multiple responsibilities, which seriously affects the maintainability and reusability of the code.

基本介绍：
- 一个对象应该对其他对象保持最少的了解
- 类与类关系越密切，耦合度越大
- 迪米特法则(Demeter Principle)又叫最少知道原则，即一个类对自己依赖的类知道的越少越好。也就是说，对于被依赖的类不管多么复杂，都尽量将逻辑封装在类的内部。对外除了提供的 public 方法，不对外泄露任何信息
- 迪米特法则还有个更简单的定义：只与直接的朋友通信
- 直接的朋友：每个对象都会与其他对象有耦合关系，只要两个对象之间有耦合关系，我们就说这两个对象之间是朋友关系。耦合的方式很多，依赖，关联，组合，聚合等。其中，我们称出现成员变量，方法参数，方法返回值中的类为直接的朋友，而出现在局部变量中的类不是直接的朋友。也就是说，陌生的类最好不要以局部变量的形式出现在类的内部。

迪米特法则注意事项和细节：
1. 迪米特法则的核心是降低类之间的耦合
2. 但是注意：由于每个类都减少了不必要的依赖，因此迪米特法则只是要求降低类间(对象间)耦合关系， 并不是要求完全没有依赖关系

>[迪米特法则 (1)](https://www.bilibili.com/video/av57936239?p=19)
>[迪米特法则 (2)](https://www.bilibili.com/video/av57936239?p=20)
>[迪米特法则注意事项和小结](https://www.bilibili.com/video/av57936239?p=21)

**Principle of composite / Aggregate Reuse 合成复用原则**

Composite / Aggregate Reuse Principle (carp / CRP). If some functions of the new object have been implemented in other created objects, the functions provided by other objects should be used as much as possible to make it a part of the new object instead of re creating. New objects can reuse existing functions by delegating them. In short, try to use composition / aggregation rather than inheritance.

原则是尽量使用合成/聚合的方式，而不是使用继承

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200823234600.png)

核心思想：
1. 找出应用中可能需要变化之处，把它们独立出来，不要和那些不需要变化的代码混在一起。
2. 针对接口编程，而不是针对实现编程。
3. 为了交互对象之间的松耦合设计而努力

>[合成复用原则](https://www.bilibili.com/video/av57936239?p=22)

## 设计模式类型
Creational Design Patterns / 创建者模式
- Singleton Pattern / 单例模式
- Factory Pattern / 工厂模式
- Abstract Factory Pattern / 抽象工厂模式
- Builder Pattern / 建造者模式
- Prototype Pattern / 原型模式

Structural Design Patterns / 结构型模式
- Adapter Pattern / 适配器模式
- Composite Pattern / 组合模式
- Proxy Pattern / 代理模式
- Flyweight Pattern / 享元模式
- Facade Pattern / 外观模式
- Bridge Pattern / 桥接模式
- Decorator Pattern / 装饰模式

Behavioral Design Patterns / 行为型模式
- Template Method Pattern / 模板方法模式
- Mediator Pattern / 中介者模式
- Chain of Responsibility Pattern / 职责链模式
- Observer Pattern / 观察者模式
- Strategy Pattern / 策略模式
- Command Pattern / 命令模式
- State Pattern / 状态模式
- Visitor Pattern / 访问者模式
- Interpreter Pattern / 解释器模式
- Iterator Pattern / 迭代器模式
- Memento Pattern/ 备忘录模式