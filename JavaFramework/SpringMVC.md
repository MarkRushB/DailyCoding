# Spring MVC

## MVC 设计模式

MVC 设计不仅限于 Java Web 应用，还包括许多应用，比如前端、PHP、.NET 等语言。之所以那么做的根本原因在于解耦各个模块。

MVC 是 Model、View 和 Controller 的缩写，分别代表 Web 应用程序中的 3 种职责。
- 模型：用于存储数据以及处理用户请求的业务逻辑。
- 视图：向控制器提交数据，显示模型中的数据。
- 控制器：根据视图提出的请求判断将请求和数据交给哪个模型处理，将处理后的有关结果交给哪个视图更新显示。

基于 Servlet 的 MVC 模式的具体实现如下。
- 模型：一个或多个 JavaBean 对象，用于存储数据（实体模型，由 JavaBean 类创建）和处理业务逻辑（业务模型，由一般的 Java 类创建）。
- 视图：一个或多个 JSP 页面，向控制器提交数据和为模型提供数据显示，JSP 页面主要使用 HTML 标记和 JavaBean 标记来显示数据。
- 控制器：一个或多个 Servlet 对象，根据视图提交的请求进行控制，即将请求转发给处理业务逻辑的 JavaBean，并将处理结果存放到实体模型 JavaBean 中，输出给视图显示。

基于 Servlet 的 MVC 模式的流程如图：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201009233751.png)

## Spring MVC处理用户请求的完整流程

Spring MVC 框架是高度可配置的，包含多种视图技术，例如 JSP 技术、Velocity、Tiles、iText 和 POI。

Spring MVC 框架并不关心使用的视图技术，也不会强迫开发者只使用 JSP 技术，但教程中使用的视图是 JSP，本节主要介绍 Spring MVC 框架处理用户请求的完整流程和处理中包含的 4 个接口。

###　Spring MVC 工作流程

Spring MVC 框架主要由 DispatcherServlet、处理器映射、控制器、视图解析器、视图组成，其工作原理如图所示。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201009233852.png)

可总结出 Spring MVC 的工作流程如下：
1. 客户端请求提交到 DispatcherServlet。
2. 由 DispatcherServlet 控制器寻找一个或多个 HandlerMapping，找到处理请求的 Controller。
3. DispatcherServlet 将请求提交到 Controller。
4. Controller 调用业务逻辑处理后返回 ModelAndView。
5. DispatcherServlet 寻找一个或多个 ViewResolver 视图解析器，找到 ModelAndView 指定的视图。
6. 视图负责将结果显示到客户端。

### Spring MVC接口

图中包含 4 个 Spring MVC 接口，即 DispatcherServlet、HandlerMapping、Controller 和 ViewResolver。

Spring MVC 所有的请求都经过 DispatcherServlet 来统一分发，在 DispatcherServlet 将请求分发给 Controller 之前需要借助 Spring MVC 提供的 HandlerMapping 定位到具体的 Controller。

HandlerMapping 接口负责完成客户请求到 Controller 映射。

Controller 接口将处理用户请求，这和 Java Servlet 扮演的角色是一致的。一旦 Controller 处理完用户请求，将返回 ModelAndView 对象给 DispatcherServlet 前端控制器，ModelAndView 中包含了模型（Model）和视图（View）。

从宏观角度考虑，DispatcherServlet 是整个 Web 应用的控制器；从微观考虑，Controller 是单个 Http 请求处理过程中的控制器，而 ModelAndView 是 Http 请求过程中返回的模型（Model）和视图（View）。

## Validation
- On client-side (JavaScript, or HTML% validation, i.e, required attribute)
- On server-side (Using Interceptor, or in the Controller, in Validator)
- No validation at all - very bad practise - web application injected with js or sql

YOU CANNOT RELY ON CLIENT-SIDE VALIDATION
- Someone could disable javascript



## 什么是IOC

    Spring是一个开源框架，Spring是于2003 年兴起的一个轻量级的Java 开发框架，由Rod Johnson 在其著作Expert One-On-One J2EE Development and Design中阐述的部分理念和原型衍生而来。它是为了解决企业应用开发的复杂性而创建的。框架的主要优势之一就是其分层架构，分层架构允许使用者选择使用哪一个组件，同时为 J2EE 应用程序开发提供集成的框架。Spring使用基本的JavaBean来完成以前只可能由EJB完成的事情。然而，Spring的用途不仅限于服务器端的开发。从简单性、可测试性和松耦合的角度而言，任何Java应用都可以从Spring中受益。Spring的核心是控制反转（IoC）和面向切面（AOP）。简单来说，Spring是一个分层的JavaSE/EEfull-stack(一站式) 轻量级开源框架。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201026214145.png)

  ioc又名控制反转，说白了就是把对象的创建，销毁以及初始化等一系列操作交给spring容器来处理，由spring来控制对象的声明周期。举个栗子：我们现在写了很多java类，然后书写一个spring的配置文件，将这些类交给spring容器来处理，此时这些对象的初始化以及销毁就会由spring来管理，如果我们需要使用对象的话，只需要遵守spring中的规范来书写代码，就可以得到指定的对象，对于应该遵守什么样的规范，这个以后我们在详细来探讨。这也就是我们ioc(控制反转的来历)。即将对象交给spring来控制管理。那么ioc带给我们的优势又有那些呢？？大家仔细想想，到现在我们如果需要用到某一个类对象，只能通过手动new出来，那有人可能会说了，我不是可以使用工厂模式吗？注意：工厂模式只是提供给我们创建对象的一个简单调用，其内部同样是new一个我们需要的对象，但是如果使用spring以后，我们只需要在spring的配置文件里边配置好需要有spring来管理的类，具体的该对象的创建管理等都是由spring来做的。废话不多说，我们先来看一个例子：

  我们新建一个类`HelloIoc.java`

  ```java
package com.test.ioc;
public class HelloIoc {
	
    public void helloIoc() {
        System.out.println("hello ioc first");
    }
}
  ```

接下来，给大家演示用spring来管理HelloIoc这个类。首先，我新建一个applicationContext.xml文件：内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">
 
</beans>
```

那么，有人可能会问，这么多的东西我怎么记得住呢，不用急，我也是在官方文档中找到的。可以看到，这里的根节点的名称叫做beans，即包含了很多其他的bean，在这里一个bean就是一个具体的类。

说明一下：这里bean中的id就是唯一标识一个bean的，一般是类名称的第一个字母小写。class顾名思义就是这个类的全类名，大家写的时候，可以按住ctrl键然后点击，如果可以进入，说明class的值是正确的。

spring容器到现在已经创建好了，现在我们写一个测试类，加载spring容器，将HelloIoc从容器中拿出来。\

```java
package com.test.ioc;
 
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
 
public class HelloIocTest {
	
	public static void main(String[] args) {
		
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("com/test/ioc/applicationContext.xml");
		HelloIoc helloIoc = (HelloIoc) applicationContext.getBean("helloIoc");
		helloIoc.helloIoc();
	}
}
```

此时运行我们的main方法，会发现`helloIoc()`方法已经被执行了，发现我们是没有`new HelloIoc`这样的方式来创建，而是通过spring容器来为我们创建该对象的。这里需要注意，`ApplicationContext`就是我们的上下文，通过它来加载配置文件的，然后通过
`applicationContext.getBean("helloIoc")`;来获取我们的实体类，这里getBean中的参数就是我们在spring配置文件中配置的bean的id。


接下来我们在HelloIoc中加上构造方法，如下：

```java
package com.test.ioc;
 
public class HelloIoc {
	
	public void helloIoc() {
		System.out.println("hello ioc first");
	}
 
	public HelloIoc() {
		super();
		System.out.println("HelloIoc has been init");
	}
}
```
然后两次获取该HelloIoc的对象，如下：

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("com/test/ioc/applicationContext.xml");
HelloIoc helloIoc = (HelloIoc) applicationContext.getBean("helloIoc");
HelloIoc helloIoc2 = (HelloIoc) applicationContext.getBean("helloIoc");
System.out.println(helloIoc);
System.out.println(helloIoc2);
```
此时，控制台打印结果是这样的：

    HelloIoc has been init
    com.test.ioc.HelloIoc@da3a1e
    com.test.ioc.HelloIoc@da3a1e

 可以发现虽然我们得到了两个HelloIoc对象，可是这两个对象是一样的，同时发现构造方法也执行了一次，这就充分说明spring默认为我们创建对象是用的单例模式。可是这里有一个问题，就是单例模式的时候，会出现线程安全问题，比如我现在在为HelloIoc中添加一个String name变量，并且提供get，set方法：

 ```java
 package com.test.ioc;
 
public class HelloIoc {
	private String name = "";
	public void helloIoc() {
		System.out.println("hello ioc first");
	}
 
	public HelloIoc() {
		super();
		System.out.println("HelloIoc has been init");
	}
 
	public String getName() {
		return name;
	}
 
	public void setName(String name) {
		this.name = name;
	}
	
}
 ```

 然后将测试类的代码修改如下：

 ```java
 ApplicationContext applicationContext = new ClassPathXmlApplicationContext("com/test/ioc/applicationContext.xml");
HelloIoc helloIoc = (HelloIoc) applicationContext.getBean("helloIoc");
HelloIoc helloIoc2 = (HelloIoc) applicationContext.getBean("helloIoc");
helloIoc.setName("xiaoming");
helloIoc2.setName("test");
System.out.println(helloIoc);
System.out.println(helloIoc2);
System.out.println(helloIoc.getName());
System.out.println(helloIoc2.getName());
```
此时打印的结果如下：

    HelloIoc has been init
    com.test.ioc.HelloIoc@feb48
    com.test.ioc.HelloIoc@feb48
    test
    test

根据结果我们可以看出现在由于是单例模式，所以出现了helloIoc2将helloIoc的name变量修改了。那么如何避免该问题的出现呢，其实在配置文件中添加这样一句话"scope="prototype""就可以取消默认的单例模式创建该bean了。如下：
```xml
<bean id="helloIoc" class="com.test.ioc.HelloIoc" scope="prototype">
</bean>
```

此时的打印结果如下：

    HelloIoc has been init
    HelloIoc has been init
    com.test.ioc.HelloIoc@bcda2d
    com.test.ioc.HelloIoc@97d01f
    xiaoming
    test

可以发现这个时候，spring为我们创建了两个不同的对象。

这个时候我们已经利用了spring容器来创建我们的对象，现在，我们来演示对象的初始化和销毁的操作。

在spring配置文件中，bean的配置文件中，spring已经为我们提供了对象的初始化和销毁的设置方法：`init-method=""` `destroy-method=""`可以看到这样的设置就可以了。接下来我新建一个类，用来演示spring操作对象中初始化和销毁的行为。

```java
package com.test.ioc;
 
public class HelloIocInitDes {
 
	public HelloIocInitDes() {
		super();
		System.out.println("HelloIocInitDes runs..");
	}
	
	public void init() {
		System.out.println("init runs ...");
	}
	
	public void destroy() {
		System.out.println("destroy runs ...");
	}
}
```

记住只要我们需要让spring来管理该对象，那么必须在spring的配置文件中配置该bean，这里我加上初始化和销毁方法的设置。

```xml
<bean id="helloIocInitDes" class="com.test.ioc.HelloIocInitDes" init-method="init" destroy-method="destroy"></bean>
```
然后新建一个HelloIocInitDesTest.java来测试初始化和销毁的操作。代码如下：

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("com/test/ioc/applicationContext.xml");
HelloIocInitDes helloIocInitDes = (HelloIocInitDes) applicationContext.getBean("helloIocInitDes");
```
此时运行该 `HelloIocInitDesTest.java`的main方法，结果如下：

    HelloIocInitDes runs..
    init runs ...

可以看到init方法是在构造方法之后执行的，可是我们发现执行了init方法，而destroy方法并没有执行，这是怎么回事，因为spring还没有准备销毁我们的对象，我们现在模拟关闭spring容器，来让destroy方法得到执行。

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("com/test/ioc/applicationContext.xml");
HelloIocInitDes helloIocInitDes = (HelloIocInitDes) applicationContext.getBean("helloIocInitDes");
ClassPathXmlApplicationContext context = (ClassPathXmlApplicationContext) applicationContext;
context.close();
```

此时打印如下：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201026214844.png)

可以发现此时destroy方法得到了执行。

可是，如果此时我在HelloIocInitDes中的bean配置文件中加上这样一句话：scope="prototype"，即不是用默认单例模式创建对象，会发想destroy-method并没有执行。即使我关闭了spring容器。

总结一下：init-method="init"方法是在构造方法之后由spring容器执行的，我们可以用他来做一些初始化的操作，在destroy-method中做关闭资源等操作，如果该bean不是默认的单例模式创建，此时将不会执行destroy-method。那么关闭资源等操作，将由人为来完成。

那么spring又是什么时候帮我们创建bean对象的呢？？在bean的配置文件中有这么一句配置：lazy-init=""，该配置存在两个值：

lazy-init="default" 和lazy-init="true"默认是false。这里，当我lazy-init="default"的时候，即spring容器加载进内存以后就会创建该bean对象，如果lazy-init="true",那么只有在执行geBean("")方法的时候才会创建该bean对象。

大家注意一下，目前我们所采用的spring容器中的所有的bean的配置都是采用构造函数来创建bean对象的，接下来我介绍一种利用静态工厂的方式来创建bean对象。首先我新建一个HelloIocFactory.java里边定义一个getInstance方法，用来创建HelloIoc对象。

```java
package com.test.ioc;
 
public class HelloIocFactory {
	
	public static HelloIoc getInstance() {
		return new HelloIoc();
	}
}
```

  然后再bean中这样配置即可：

  ```xml
  <bean id="helloIoc2" class="com.test.ioc.HelloIocFactory" factory-method="getInstance"></bean>
  ```

此时如果想要到的HelloIoc对象，只需要像往常那样通过getBean("")方法调用就行了。

## 什么是依赖注入

什么是依赖注入，说的通俗一点，就是对属性赋值，也就是说我们利用spring来为我们的类中包含的属性来进行赋值，想想之前我们是通过这样的方式来编写代码的：接口  对象 = new 接口实现类();  再看看我们之前是怎么给属性赋值的 

1. 通过set方法
2. 通过构造方法

首先我新建一个Student.java和一个Teacher.java类，并且提供get和set方法

```java
package com.test.di;
 
public class Student {
	
	private String name;
	private int id;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
}
```
```java
package com.test.di;
 
public class Teacher {
	private String teacherName;
	private Student student;
	public String getTeacherName() {
		return teacherName;
	}
	public void setTeacherName(String teacherName) {
		this.teacherName = teacherName;
	}
	public Student getStudent() {
		return student;
	}
	public void setStudent(Student student) {
		this.student = student;
	}
}
```
然后，我们在spring配置文件中来为这些属性赋值：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd">
 
	<bean id="student" class="com.test.di.Student">
		<property name="name" value="haha"></property>
		<property name="id" value="12"></property>
	</bean>
	<bean id="teacher" class="com.test.di.Teacher">
		<property name="teacherName" value="teacherWang"></property>
		<property name="student" ref="student"></property>
	</bean>
</beans>
```

根据配置文件，我们可以发现，在bean中有个property的配置，其中name就是我要为那个属性赋值，对于属性的值，这里有两种情况：

1. 如果是基本类型，直接在value中写上需要赋的值即可
2. 如果是引用类型，那么需要使用ref来引用对应的类，对于这个栗子，即student这里ref所引用的student就是第一个student的bean中配置的id。

接下来，我编写一个测试类，来测试是否成功的为属性注入对应的值`DiTest.java`

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("com/test/di/applicationContext.xml");
Teacher teacher = (Teacher) applicationContext.getBean("teacher");
Student student = teacher.getStudent();
System.out.println(teacher.getTeacherName());
System.out.println("studentName :"+student.getName()+"==studentId :"+student.getId());
```
此时打印结果如下:

    teacherWang
    studentName :haha==studentId :12

发现这个时候spring容器已经为我们的属性赋值成功了。然而我们却并没有像之前那样调用set方法，或者是构造方法，这里有一点需要说明，就是我们虽然没有自己调用set方法来为属性赋值，但是spring还是会掉用set方法，所以我们如果想对某一个属性进行依赖注入的话，那么我们就需要对该属性写上set方法。

下面我们为teacher注入一些集合，首先需要做的就是在Teacher.java中声明list,set,map这三个属性，然后为这些属性生成set方法，新增属性如下：

```java
private List<String>lists;
private Set<Integer>sets;
private Map<Integer,String>maps;
```
然后再spring的配置文件中这样为其赋值：

```xml
<bean id="teacher" class="com.test.di.Teacher">
		<property name="teacherName" value="teacherWang"></property>
		<property name="student" ref="student"></property>
		<property name="lists">
			<list>
				<value>one</value>
				<value>two</value>
				<value>three</value>
			</list>
		</property>
		<property name="maps">
			<map>
				<entry key="1" value="firstMap"></entry>
				<entry key="2" value="secondMap"></entry>
				<entry key="3" value="thirdMap"></entry>
			</map>
		</property>
		<property name="sets">
			<set>
				<value>111</value>
				<value>222</value>
				<value>333</value>
			</set>
		</property>	
</bean>
```

可以发现这个配置文件写起来和普通的集合对象的形式是很相似的，这里我们都是用的基本的类型来作为集合的泛型，如果使用的是引用类型，这里的配置都有ref对应的属性，只需要将所需要引用的类对象的id写入到ref的值当中即可，举个栗子：

对于list和set如果泛型是引用类型，那么可以这样写：

`<ref bean=""/>`

而对于map如果类型是引用类型，可以这样写：

`<entry key-ref="" value-ref=""></entry>`

好了，是时候验证是否赋值成功了。IocTest.java

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("com/test/di/applicationContext.xml");
Teacher teacher = (Teacher) applicationContext.getBean("teacher");
Student student = teacher.getStudent();
System.out.println(teacher.getTeacherName());
System.out.println("studentName :"+student.getName()+"==studentId :"+student.getId());
		
List<String> lists = teacher.getLists();
for (String string : lists) {
	System.out.println(string);
}
Map<Integer,String>maps = teacher.getMaps();
for(Entry<Integer,String>entry :maps.entrySet()) {
	System.out.println(maps.get(entry.getKey()));
}
Set<Integer>sets = teacher.getSets();
for (Integer integer : sets) {
	System.out.println(integer);
}
```
此时打印的结果如下：

    teacherWang
    studentName :haha==studentId :12
    one
    two
    three
    firstMap
    secondMap
    thirdMap
    111
    222
    333

可以发现这个时候spring成功为所有的写了set方法的属性成功赋值了。好了上边的都是利用set方法来为属性赋值的，下面我们来利用构造方法来为属性赋值，我们写一个ClassInfo.java类：

```java
package com.test.di;
 
public class ClassInfo {
	private String className;
	private Student student;
	public ClassInfo(String className, Student student) {
		super();
		this.className = className;
		this.student = student;
	}
	public String getClassName() {
		return className;
	}
	public Student getStudent() {
		return student;
	}
}
```
可以看到此时我们声明了两个属性，一个基本类型的，一个引用类型的，并且书写了构造方法，这时我们就可以在spring配置文件中，利用构造方法来为属性赋值了，在bean的配置中有这样一个配置

`<constructor-arg index="" type="" ref="" value=""></constructor-arg>`

顾名思义就是根据构造函数来为属性赋值的，说明一下这四个参数的意思：

- index：该参数在构造方法中的位置，默认从0开始
- type：该参数的类型
- ref： 如果该参数是引用类型时候的引用id
- value：如果该参数是基本类型时候的值

知道了每个参数的意思，写起来就很简单了，我的ClassInfo.java对应的bean如下：

```xml
<bean id="classInfo" class="com.test.di.ClassInfo">
	<constructor-arg index="0" type="java.lang.String" value="testConstructor"></constructor-arg>
	<constructor-arg index="1" type="com.test.di.Student" ref="student"></constructor-arg>
</bean>
```
编写测试代码：
```java
ClassInfo classInfo = (ClassInfo) applicationContext.getBean("classInfo");
System.out.println("classInfo.getClassName():"+classInfo.getClassName());
System.out.println("studentId:"+classInfo.getStudent().getId()+"==studentName"+classInfo.getStudent().getName());
```

此时会正确的答应出我们设置的信息，如下：

    classInfo.getClassName():testConstructor
    studentId:12==studentNamehaha

现在我们已经学会了如何在spring中为属性赋值，如之前所属，我们并没有调用set或者构造方法，却能成功为属性赋值，其实我们是把set方法的调用交给spring来处理了，那么依赖注入又有什么用呢？我们为什么要学习依赖注入？还记得我在该篇最开始写了这样一句话：接口  对象 = new 接口实现类(); 这种方式是我们之前创建对象的方法。现在我举个栗子：

我新建一个借口BookRead然后建俩个类实现该接口：

```java
package com.test.why.di;
 
public interface BookRead {
	public void readBook();
}
```

```java
package com.test.why.di;
 
public class KindleRead implements BookRead {
 
	@Override
	public void readBook() {
		System.out.println("use kindle read");
	}
}
```

```java
package com.test.why.di;
 
public class PhoneRead implements BookRead {
 
	@Override
	public void readBook() {
		System.out.println("use phone read");
	}
}
```

```java
package com.test.why.di;
 
public class ReadBy {
	private BookRead bookRead;
 
	public ReadBy(BookRead bookRead) {
		this.bookRead = bookRead;
	}
	
	public void read() {
		bookRead.readBook();
	}
}
```

如果我现在需要先用kindle来读书怎么办呢？按照之前的写法：

```java
BookRead bookRead = new KindleRead();
ReadBy readBy = new ReadBy(bookRead);
readBy.read();
```

那么问题来了，如果我现在需要利用Phone来读书，那么我是不是需要重新new一个PhoneRead呢。这样做并不是我们要的面向接口编程。接下来我们使用spring的依赖注入来为其优化：

首先将两种读书方式的类，在spring容器中进行配置：

```xml
<bean id="kindleRead" class="com.test.why.di.KindleRead">
</bean>
<bean id="phoneRead" class="com.test.why.di.PhoneRead">
</bean>
```

然后配置ReadBy对应的bean：
```xml
<bean id="readBy" class="com.test.why.di.ReadBy">
	<property name="bookRead" ref="kindleRead"></property>
</bean>
```
测试：

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("com/test/di/applicationContext.xml");
ReadBy readBy = (ReadBy) applicationContext.getBean("readBy");
readBy.read();
```
这里我为readBy注入的是kindleRead，因此这时候我调用readBy.read();方法应该是运行的kindleRead的readBook，其实这里已经做到了面向接口编程，就是我的readBy.read();不需要知道bookRead是什么类型，我只需要调用在read方法中调用bookRead.readBook();方法就可以了，具体以哪种方式来读书，我只需要在spring容器当中进行配置即可，这样做也使得代码更加容易维护。




