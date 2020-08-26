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

### Singleton Pattern

所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，**对某个类只能存在一个对象实例**， 并且该类只提供一个取得其对象实例的方法(静态方法)。

比如 Hibernate 的 SessionFactory，它充当数据存储源的代理，并负责创建 Session 对象。SessionFactory 并不是轻量级的，一般情况下，一个项目通常只需要一个 SessionFactory 就够，这是就会使用到单例模式。

单例模式有八种方法：

1. 饿汉式(静态常量)
2. 饿汉式（静态代码块）
3. 懒汉式(线程不安全)
4. 懒汉式(线程安全，同步方法)
5. 懒汉式(线程安全，同步代码块)
6. 双重检查
7. 静态内部类
8. 枚举

#### 饿汉式(静态常量)：

1. 构造器私有化 (防止 new )
2. 类的内部创建对象
3. 向外暴露一个静态的公共方法。getInstance
4. 代码实现

```java
public class SingletonTest01 {

    public static void main(String[] args) {
    //测试
    Singleton instance = Singleton.getInstance(); 
    Singleton instance2 = Singleton.getInstance(); 
    System.out.println(instance == instance2); // true
    System.out.println("instance.hashCode=" + instance.hashCode()); 
    System.out.println("instance2.hashCode=" + instance2.hashCode());
    }

}

//饿汉式(静态变量) 
class Singleton {
//1. 构造器私有化, 外部能 new 
    private Singleton() {
    }

    //2.本类内部创建对象实例
    private final static Singleton instance = new Singleton();

    //3. 提供一个公有的静态方法，返回实例对象
    public static Singleton getInstance() {
        return instance;
    }
}
```

优缺点说明：

1. **优点**：这种写法比较简单，就是在类装载的时候就完成实例化。避免了线程同步问题。
2. **缺点**：在类装载的时候就完成实例化，没有达到 Lazy Loading 的效果。如果从始至终从未使用过这个实例，则会造成内存的浪费
3. 这种方式基于 classloder 机制避免了多线程的同步问题，不过，instance 在类装载时就实例化，在单例模式中大多数都是调用 getInstance 方法， 但是导致类装载的原因有很多种，因此不能确定有其他的方式（或者其他的静态方法）导致类装载，这时候初始化 instance 就没有达到 lazy loading 的效果
4. 结论：这种单例模式可用，**可能**造成内存浪费


#### 饿汉式（静态代码块）:

```java
public class SingletonTest02 {

    public static void main(String[] args) {
    //测试
    Singleton instance = Singleton.getInstance(); 
    Singleton instance2 = Singleton.getInstance(); 
    System.out.println(instance == instance2); // true
    System.out.println("instance.hashCode=" + instance.hashCode()); 
    System.out.println("instance2.hashCode=" + instance2.hashCode());
    }

}

//饿汉式(静态变量) 
class Singleton {
//1. 构造器私有化, 外部能 new 
    private Singleton() {

    }


    //2.本类内部创建对象实例
    private static Singleton instance;

    static { // 在静态代码块中，创建单例对象
        instance = new Singleton();
    }

    //3. 提供一个公有的静态方法，返回实例对象
    public static Singleton getInstance() {
        return instance;
    }

}
```

优缺点说明：

1. 这种方式和上面的方式其实类似，只不过将类实例化的过程放在了静态代码块中，也是在类装载的时候，就执行静态代码块中的代码，初始化类的实例。优缺点和上面是一样的。
2. 结论：这种单例模式可用，但是可能造成内存浪费


#### 懒汉式(线程不安全)：

```java
public class SingletonTest03 {
    public static void main(String[] args) { 
        System.out.println("懒汉式 1 ， 线程不安全~"); 
        Singleton instance = Singleton.getInstance(); 
        Singleton instance2 = Singleton.getInstance(); 
        System.out.println(instance == instance2); // true
        System.out.println("instance.hashCode=" + instance.hashCode()); 
        System.out.println("instance2.hashCode=" + instance2.hashCode());
    }
}

class Singleton {
    private static Singleton instance;

    private Singleton() {

    }

    //提供一个静态的公有方法，当使用到该方法时，才去创建 instance
    //即懒汉式
    public static Singleton getInstance() { 
        if(instance == null) {
            instance = new Singleton();
        }
    return instance;
    }
}
```

优缺点说明：

1. 起到了 Lazy Loading 的效果，但是只能在单线程下使用。
2. **如果在多线程下，一个线程进入了 if (singleton == null)判断语句块，还未来得及往下执行，另一个线程也通过了这个判断语句，这时便会产生多个实例。所以在多线程环境下不可使用这种方式**
3. 结论：在实际开发中，**不要使用**这种方式.


#### 懒汉式(线程安全，同步方法)：	
```java
public class SingletonTest04 {

    public static void main(String[] args) { 
        System.out.println("懒汉式 2 ， 线程安全~"); 
        Singleton instance = Singleton.getInstance(); 
        Singleton instance2 = Singleton.getInstance(); 
        System.out.println(instance == instance2); // true
        System.out.println("instance.hashCode=" + instance.hashCode()); 
        System.out.println("instance2.hashCode=" + instance2.hashCode());
    }

}
// 懒汉式(线程安全，同步方法) 
class Singleton {
    private static Singleton instance;

    private Singleton() {}

    //提供一个静态的公有方法，加入同步处理的代码，解决线程安全问题
    //即懒汉式
    public static synchronized Singleton getInstance() { 
        if(instance == null) {
            instance = new Singleton();
        }
    return instance;
    }
}
```

优缺点说明：

1. 解决了线程安全问题
2. **效率太低了**，每个线程在想获得类的实例时候，执行 getInstance()方法都要进行同步。而其实这个方法只执行一次实例化代码就够了，后面的想获得该类实例，直接 return 就行了。方法进行同步效率太低
3. 结论：在实际开发中，**不推荐**使用这种方式

#### 懒汉式(线程安全，同步代码块)：

```java
public class SingletonTest04 {

    public static void main(String[] args) { 
        System.out.println("懒汉式 2 ， 线程安全~"); 
        Singleton instance = Singleton.getInstance(); 
        Singleton instance2 = Singleton.getInstance(); 
        System.out.println(instance == instance2); // true
        System.out.println("instance.hashCode=" + instance.hashCode()); 
        System.out.println("instance2.hashCode=" + instance2.hashCode());
    }

}
// 懒汉式(线程安全，同步方法) 
class Singleton {
    private static Singleton singleton;

    private Singleton() {}

    //提供一个静态的公有方法，加入同步处理的代码，解决线程安全问题
    //即懒汉式
    public static  Singleton getInstance() { 
        if(singleton == null) {
            synchronized(Singleton.class){
                singleton = new Singleton();                                                
            }   
        }
    return singleton;
    }
}
```

优缺点说明：

1. 这种方式，本意是对第四种实现方式的改进，因为前面同步方法效率太低，改为同步产生实例化的代码块。
2. **但是这种方式不能起到线程同步的作用**。跟第三种实现方式i遇到的情形一致，假如一个线程进入了`if(singleton ==
null)`判断语句块，还未来得及往下执行，了，另一个线程也通过了这个判断语句。这时候便会产生多个实例
3. 结论：在实际开发中，**不能使用**这种方式。

#### 双重检查

    volatile是Java提供的一种轻量级的同步机制。Java 语言包含两种内在的同步机制：同步块（或方法）和 volatile 变量，相比于synchronized（synchronized通常称为重量级锁），volatile更轻量级，因为它不会引起线程上下文的切换和调度。但是volatile 变量的同步性较差（有时它更简单并且开销更低），而且其使用也更容易出错。
    
    当写一个volatile变量时，JMM会把该线程对应的本地内存中的共享变量值刷新到主内存.
    当读一个volatile变量时，JMM会把该线程对应的本地内存置为无效。线程接下来将从主内存中读取共享变量,并更新本地内存的值.

```java
public class SingletonTest06 {

    public static void main(String[] args) { 
        System.out.println("双重检查");
        Singleton instance = Singleton.getInstance();
        Singleton instance2 = Singleton.getInstance(); 
        System.out.println(instance == instance2); // true 
        System.out.println("instance.hashCode=" + instance.hashCode()); 
        System.out.println("instance2.hashCode=" + instance2.hashCode());
    }

}

// 懒汉式(线程安全，同步方法) 
class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    //提供一个静态的公有方法，加入双重检查代码，解决线程安全问题, 同时解决懒加载问题
    //同时保证了效率, 推荐使用

    public static synchronized Singleton getInstance() { 
        if(instance == null) {
        synchronized (Singleton.class) { 
            if(instance == null) {
                instance = new Singleton();
            }
        }
    }
    return instance;
    }
}
```

优缺点说明：

1. **Double-Check 概念是多线程开发中常使用到的**，如代码中所示，我们进行了两次 if (singleton == null)检查，这样就可以保证线程安全了。
2. 这样，实例化代码只用执行一次，后面再次访问时，判断 if (singleton == null)，直接 return 实例化对象，也避免的反复进行方法同步.
3. 线程安全；延迟加载；效率较高
4. 结论：在实际开发中，推荐使用这种单例设计模式

#### 静态内部类

- 当`Singleton`进行类装载的时候，静态内部类`SingletonInstance`是不会被装载的

- 当调用`getInstance`方法用到了`SingletonInstance`静态变量的时候，会导致静态类被装载，此时线程也是安全的

```java
public class SingletonTest07 {

    public static void main(String[] args) { 
        System.out.println("使用静态内部类完成单例模式"); 
        Singleton instance = Singleton.getInstance();
        Singleton instance2 = Singleton.getInstance(); 
        System.out.println(instance == instance2); // true 
        System.out.println("instance.hashCode=" + instance.hashCode()); 
        System.out.println("instance2.hashCode=" + instance2.hashCode());
    }
}

// 静态内部类完成， 推荐使用
class Singleton {
    private static volatile Singleton instance;

    //构造器私有化
    private Singleton() {}

    //写一个静态内部类,该类中有一个静态属性 Singleton 
    private static class SingletonInstance {
        private static final Singleton INSTANCE = new Singleton();
    }

    //提供一个静态的公有方法，直接返回 SingletonInstance.INSTANCE 
    public static synchronized Singleton getInstance() {
        return SingletonInstance.INSTANCE;
    }
}
```

优缺点说明：

1. 这种方式采用了类装载的机制来保证初始化实例时只有一个线程。
2. 静态内部类方式在 Singleton 类被装载时并不会立即实例化，而是在需要实例化时，调用 getInstance 方法，才会装载 SingletonInstance 类，从而完成 Singleton 的实例化。
3. 类的静态属性只会在第一次加载类的时候初始化，所以在这里，JVM 帮助我们保证了线程的安全性，在类进行初始化时，别的线程是无法进入的。
4. 优点：**避免了线程不安全**，利用静态内部类特点实现延迟加载，效率高
5. 结论：推荐使用.

#### 枚举

```java
public class SingletonTest08 {
    public static void main(String[] args) { 
        Singleton instance = Singleton.INSTANCE;
        Singleton instance2 = Singleton.INSTANCE; 
        System.out.println(instance == instance2);

        System.out.println(instance.hashCode()); 
        System.out.println(instance2.hashCode());

        instance.sayOK();
    }
}

//使用枚举，可以实现单例, 推荐
enum Singleton {
    INSTANCE; //属性
    public void sayOK() { 
        System.out.println("ok~");
    }
}
```

优缺点说明：

1. 这借助 JDK1.5 中添加的枚举来实现单例模式。不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象。
2. 这种方式是 Effective Java 作者 Josh Bloch  提倡的方式
3. 结论：推荐使用

#### 单例模式在JDK应用的源码分析

JDK 中，java.lang.Runtime 就是经典的单例模式(饿汉式)

代码分析+Debug 源码+代码说明

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200825181209.png)

#### 单例模式注意事项和细节说明

- 单例模式保证了 系统内存中该类只存在一个对象，节省了系统资源，对于一些需要频繁创建销毁的对象，使用单例模式可以提高系统性能

- 当想实例化一个单例类的时候，必须要记住使用相应的获取对象的方法，而不是使用 new

- 单例模式使用的场景：**需要频繁的进行创建和销毁的对象**、创建对象时耗时过多或耗费资源过多(即：**重量级对象**)，但又经常用到的对象、**工具类对象**、频繁访问数据库或文件的对象(比如**数据源、session 工厂**等)

### Factory Pattern

#### 先从具体的需求入手

一个披萨的项目: 要便于披萨种类的扩展,要便于维护
1. 披萨的种类有很多(比如 GreekPizz、CheesePizz 等)
2. 披萨的制作有 prepare，bake, cut, box
3. 完成披萨的订购功能
   
#### 传统方式

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200825192716.png)

```java
//将Pizza类做成抽象 
public abstract class Pizza {
	protected String name; //名字

	//准备原材料,不同的披萨不一样,因此,我们做成抽象方法
	public abstract void prepare();

	
	public void bake() {
		System.out.println(name + " baking;");
	}

	public void cut() {
		System.out.println(name + " cutting;");
	}

	//打包
	public void box() {
		System.out.println(name + " boxing;");
	}

	public void setName(String name) {
		this.name = name;
	}
}
```

```java
public class CheesePizza extends Pizza {

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		System.out.println("给制作奶酪披萨 准备原材料");
	}
}
```

```java
public class GreekPizza extends Pizza {

	@Override
	public void prepare() {
		// TODO Auto-generated method stub
		System.out.println("给制作希腊披萨 准备原材料");
	}
}
```

```java
public class OrderPizza {
    //构造器
	public OrderPizza() {
        Pizza pizza = null;
        String orderType; //  订购披萨的类型
        do {

            orderType = getType();

            if (orderType.equals("greek")) {
                pizza = new GreekPizza();
                pizza.setName(" 希腊披萨 ");
            } else if (orderType.equals("cheese")) {
                pizza = new CheesePizza();
                pizza.setName(" 奶酪披萨 ");
            } else {
                break;  
            }
            //输出 pizza 制作过程
            pizza.prepare();
            pizza.bake();
            pizza.cut();
            pizza.box();

        }while (true);
	}
}
```
```java
//写一个方法,可以获取客户希望订购的披萨种类
private String getType(){
    try{
        BufferedReader strin = new BufferedReader(new InputStreamReader(System.in));\
        System.out.print("input pizza type");
        String str = strin.readLine();
        return str;
    }catch(IOException e){
        e.printStackTrace();
        return "";
    }
}
```
```java
//相当于一个客户端,发送订单
public class PizzaStore{
    public static void main(String[] args){
        new OrderPizza();
    }
}
```

传统的方式的优缺点

1. 优点是比较好理解，简单易操作。
2. 缺点是  ，即**对扩展开放，对修改关闭**。即当我们给类增加新功能的时候，尽量不修改代码，或者尽可能少修改代码.
3. 比如我们这时要新增加一个 Pizza 的种类(Pepper 披萨)，我们需要做如下修改. 如果我们增加一个 Pizza 类，只要是订购 Pizza 的代码都需要修改

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200825221730.png)

改进的思路分析:

**分析**：修改代码可以接受，但是如果我们在其它的地方也有创建 Pizza 的代码，就意味着，也需要修改，而创建 Pizza
的代码，往往有多处。

**思路**：把创建 Pizza 对象封装到一个类中，这样我们有新的 Pizza 种类时，只需要修改该类就可，其它有创建到 Pizza
对象的代码就不需要修改了.-> 简单工厂模式


https://www.bilibili.com/video/av57936239?p=40 未完待续