- [Mybatis框架概述](#mybatis框架概述)
- [IDEA Mybatis环境搭建](#idea-mybatis环境搭建)
  - [写在前面](#写在前面)
  - [创建maven工程](#创建maven工程)
  - [加入打包方式](#加入打包方式)
  - [添加Mybatis依赖（坐标）](#添加mybatis依赖坐标)
  - [编写实体类](#编写实体类)
  - [编写持久层接口](#编写持久层接口)
  - [创建Mybatis的主配置文件](#创建mybatis的主配置文件)
  - [编写持久层接口的映射文件](#编写持久层接口的映射文件)
  - [环境搭建的注意事项](#环境搭建的注意事项)
  - [Mybatis核心对象](#mybatis核心对象)
  - [编写测试类](#编写测试类)
  - [使用注解配置](#使用注解配置)
  - [手动写dao实现类](#手动写dao实现类)
- [自定义Mybatis](#自定义mybatis)
  - [分析流程](#分析流程)
  - [Mybatis透过入门案例看到的类](#mybatis透过入门案例看到的类)
  - [前期准备](#前期准备)
    - [创建Maven工程](#创建maven工程-1)
    - [引入相关坐标](#引入相关坐标)
    - [引入工具类到项目中](#引入工具类到项目中)
    - [编写SqlMapConfig.xml](#编写sqlmapconfigxml)
    - [编写读取配置文件](#编写读取配置文件)
    - [编写Mapper类](#编写mapper类)
    - [编写Configurarion配置类](#编写configurarion配置类)
    - [编写User实体类](#编写user实体类)
  - [基于XML的自定义Mybatis框架](#基于xml的自定义mybatis框架)
    - [编写持久层接口和IUserDao.xml](#编写持久层接口和iuserdaoxml)
    - [编写构建者类](#编写构建者类)
    - [编写SqlSessionFactory接口和实现类](#编写sqlsessionfactory接口和实现类)
    - [编写SqlSession接口和实现类](#编写sqlsession接口和实现类)
    - [编写用于创建Dao接口代理对象的类](#编写用于创建dao接口代理对象的类)
    - [运行测试类](#运行测试类)
  - [基于注解方式定义Mybatis框架](#基于注解方式定义mybatis框架)
    - [自定义@Select注解](#自定义select注解)
    - [更改持久层接口](#更改持久层接口)
    - [修改SqlMapConfig.xml](#修改sqlmapconfigxml)
  - [自定义Mybatis的设计模式说明](#自定义mybatis的设计模式说明)
    - [工厂模式(SqlSessionFactory)](#工厂模式sqlsessionfactory)
    - [代理模式(MapperProxyFactory)](#代理模式mapperproxyfactory)
    - [构建者模式(SqlSessionFactoryBuilder)](#构建者模式sqlsessionfactorybuilder)
  - [最后回顾](#最后回顾)
- [Mybatis的CRUD (Create, Retrieve, Update, Delete)](#mybatis的crud-create-retrieve-update-delete)
  - [Retrieve](#retrieve)
  - [Retrieve（模糊查询）](#retrieve模糊查询)
  - [Retrieve（聚合函数）](#retrieve聚合函数)
  - [Create](#create)
  - [Update](#update)
  - [Delete](#delete)
- [Mybatis的参数深入](#mybatis的参数深入)
  - [Mybatis参数](#mybatis参数)
    - [parameterType配置参数](#parametertype配置参数)
    - [传递pojo对象](#传递pojo对象)
    - [传递pojo包装对象](#传递pojo包装对象)
  - [Mybatis的输出结果封装](#mybatis的输出结果封装)
    - [resultType（输出类型）](#resulttype输出类型)
- [Mybatis实现DAO层开发（了解）](#mybatis实现dao层开发了解)
- [SqlMaoConfig.xml配置文件](#sqlmaoconfigxml配置文件)
  - [配置内容](#配置内容)
  - [properties（属性）](#properties属性)
  - [typeAliases（类型别名）](#typealiases类型别名)
  - [mappers（映射器）](#mappers映射器)
- [Mybatis连接池与事务深入](#mybatis连接池与事务深入)
  - [Mybatis 的连接池技术](#mybatis-的连接池技术)
# Mybatis框架概述

Mybatis 是一个优秀的基于 **Java** 的持久层框架，它**内部封装了 JDBC** ，使开发者**只需要关注 sql 语句本身**，而不需要花费精力去处理加载驱动，创建链接，创建 statement 等繁杂的过程。

Mybatis 通过 xml 或注解的方式将要执行的各种 statement 配置起来，并通过 Java 对象和 statement 中 sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 Mybatis 框架执行 sql 并将结果映射为 Java 对象并返回。

采用 **ORM** 思想解决了实体和数据库映射的问题，对 JDBC 进行了封装，屏蔽了 JDBC api 底层访问细节，是我们不用与 JDBC api 打交道，就可以完成对数据库的持久化操作。

> **ORM**： Object Relationship Mapping 对象关系映射
> 简单地说，就是把数据库表和实体类及实体类的属性对应起来，让我们可以操作实体类就实现操作数据库表。
>|数据库表|实体类|
>|:-:|:-:|
>|user|User|
>|id|userId|
>|user_name|userName|

想要建立关系，我们需要做到：**实体类中的属性和数据库表的字段名称保持一致**

>|数据库表|实体类|
>|:-:|:-:|
>|user|user|
>|id|id|
>|user_name|user_name|

# IDEA Mybatis环境搭建

## 写在前面

我在运行的时候，遇到这样一个报错：`Error : java 不支持发行版本5`
经过在网上搜索，找到解决办法：[Error : java 不支持发行版本5的解决办法](https://blog.csdn.net/qq_22076345/article/details/82392236)

## 创建maven工程

**Creatw New Project → Maven → Next → GroupId & ArtifactId**

## 加入打包方式

```xml
<packaging>jar</packaging>
```
## 添加Mybatis依赖（坐标）

使用 Maven 来构建项目，则需将下面的依赖代码置于 pom.xml 文件中：

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>x.x.x</version>
</dependency>
```
因为是数据库的操作，所以还需要mysql的坐标
```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.15</version>
</dependency>
```
如果还需要日志
```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.12</version> 
</dependency>
```
如果想单元测试
```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>3.8.2</version>
    <scope>test</scope>
</dependency>
```
## 编写实体类

接下来用一个小例子：
首先编写一个User实体类
```java
public class User implements Serializable {
    private Integer id;
    private String username;
    private Date birthday;
    private String sex;
    private String address;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday=" + birthday +
                ", sex='" + sex + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}
```
## 编写持久层接口

IUserDao 接口就是我们的持久层接口（也可以写成 UserDao 或者 UserMapper）,具体代码如下：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200725231547.png)

```java
public interface IUserDao {

    /**
    * 查询所有用户
    * @return
    */

    List<User> findAll();
}
```
## 创建Mybatis的主配置文件
名字：`SqlMapConfig.xml`

文件结构：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200726112115.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
 <!-- mybatis的主配置文件 -->
<configuration>
    <!-- 配置 mybatis 的环境 -->
    <environments default="mysql">
    <!-- 配置 mysql 的环境 -->
        <environment id="mysql">
            <!-- 配置事务的类型 -->
            <transactionManager type="JDBC"></transactionManager>
            <!-- 配置连接数据库的信息：用的是数据源(连接池) -->
            <dataSource type="POOLED">
                <!-- 配置链接数据库的4个基本信息 -->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/sample01"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

<!-- 告知 mybatis 映射配置的位置 -->
    <mappers>
        <mapper resource="org/practice/dao/IUserDao.xml"/>
    </mappers>
</configuration>

```

## 编写持久层接口的映射文件

名字：`IUserDao.xml`

文件结构：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200726112115.png)

**创建位置**：必须和持久层接口在相同的包中。
**名称**：必须以持久层接口名称命名文件名，扩展名是`.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.practice.dao.IUserDao">
    <!-- 配置查询所有操作 -->
    <!-- 注意这里一定要有resultType -->
    <!-- 这样才在最后才会把结果集封装到User对象里面，并把User对象添加到List -->
    <select id="findAll" resultType="org.practice.domain.User">
        select * from user 
    </select>
</mapper>
```

## 环境搭建的注意事项

1. 创建 `IUserDao.xml` 和 `IuserDao.java` 时名称是为了和我们之前的知识保持一致。在Mybatis中它把持久层的操作接口名称和映射文件也叫做：`Mapper`。所以： `IUserDao` 和 `IUserMapper` 是一样的。
2. 在idea中创建目录的时候，它和包是不一样的：
**包(Package)在创建时**：`org.practice.dao` 自动出来的是三级目录，如果没有显示三级目录，可以：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200726113207.png)

将 `Compact Middle Packages` 前面的勾取消
**目录(Direcory)在创建时**：`org.practice.dao` 是一级目录
3. Mybatis的映射配置文件必须和dao接口的包结构相同
4. 映射配置文件的 `mapper` 标签 `namespace` 属性的取值必须是dao接口的全限定类名
5. 映射配置文件的操作配置(select)，id属性的取值必须是dao接口的方法名

当我们遵从了第三、四、五点之后，我们在开发中就无需再写dao的实现类。

## Mybatis核心对象
**SqlSessionFactory**
`SqlSessionFactory` 是MyBatis中十分重要的对象,它是单个数据库映射关系经过编译后的内存镜像,其作用是创建 `SqlSession`. SqlSessionFactory对象是线程安全的,它一旦被创建,在整个应用执行期间都会存在.如果我们多次地创建同一个数据库的SqlSessionFactory势必会耗尽数据库资源! 通常每一个数据库都会只对应一个SqlSessionFactory,所以在构建SqlSessionFactory时建议使用**单例模式**哟 !

**SqlSession**
`SqlSession` 是MyBatis框架中另一个重要的对象,它是应用程序与持久层之间执行交互操作的一个单线程对象,其主要作用是执行持久化操作. 注意: 每一个线程都应该有一个自己的SqlSession实例,并且该实例是不能被共享的,同时 `SqlSession` 实例也是线程不安全的,因此其使用范围最好在一次请求或一个方法中,绝不能将其放在一个类的静态字段,实例或任何类型的管理范围中使用.使用后理应及时地关闭它 !

## 编写测试类

文件结构：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200726152630.png)

```java
public class MybatisTest {
    public static void main(String[] args)throws Exception {
        //1.读取配置文件
        //第一个：使用类加载器，它只能读取类路径的配置文件
        //第二个：使用ServletContext对象的getRealPath()
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");

        //2.创建 SqlSessionFactory 的构建者对象
        //方便下面利用这个builder.nuild去构建工厂
        //创建工厂mybatis使用了构建者模式，优点：把对象的创建细节隐藏，使用者直接调用方法即可拿到对象
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();

        //3.使用构建者创建工厂对象 SqlSessionFactory
        //注意这个SqlSessionFactory是一个Interface
        //builder就是构建者，in相当于给包工队的图纸
        SqlSessionFactory factory = builder.build(in);

        //4.使用 SqlSessionFactory 生产 SqlSession 对象
        //生产SqlSession使用了工厂模式，优势：解耦（降低类之间的依赖关系）
        SqlSession session = factory.openSession();

        //5.使用 SqlSession 创建 dao 接口的代理对象
        //创建Dao接口实现类使用了代理模式，优势：不修改源码的基础上对已有方法增强
        IUserDao userDao = session.getMapper(IUserDao.class);

        //6.使用代理对象执行查询所有方法
        List<User> users = userDao.findAll();

        for(User user : users) {
            System.out.println(user);
        }

        //7.释放资源
        session.close();
        in.close();
    }
}
```
**注意事项**：不要忘记在映射配置中告知mybatis要封装到哪个实体类中，配置的方式：指定实体类的全限定类名。

上述代码要做的简单，很容易：
```java
public class MybatisTest {
    public static void main(String[] args) throws Exception {
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml").getMapper(IUserDao.class);
        List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
        }
        session.close();
        in.close();
    }
}
```
**但是**，为什么还要写这么多类名？为了**灵活**，每多加一个类，就可以选择更灵活的配置。


## 使用注解配置

在一开始的介绍中：Mybatis 通过 xml 或**注解**的方式将要执行的各种 statement 配置起来。
这里我们还可以使用**注解**来替代**xml**进行配置工作。

这里我们就不需要`org.practice.dao.IUserDao.xml`，取而代之就是在 `dao` 接口上写上`@Select`注解

```java
public interface IUserDao {

    /**
     * 查询所有用户
     * @return
     */
    @Select("select * from user")
    List<User> findAll();
}
```
同时在 `SqlMapConfig.xml` 中指定映射配置文件的位置中做修改，如果是用注解来配置的话，此处应该使用class属性指定被注解的dao全限定类名

```xml
    <!-- 告知 mybatis 映射配置的位置 -->
    <mappers>
        <mapper class="org.practice.dao.IUserDao"/>
    </mappers>
```

## 手动写dao实现类

[9:00 - end](https://www.bilibili.com/video/BV1mE411X7yp?p=9)
目的旨在说明：
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.practice.dao.IUserDao">
    <!-- 配置查询所有操作 -->
    <!-- 注意这里一定要有resultType -->
    <!-- 这样才在最后才会把结果集封装到User对象里面，并把User对象添加到List -->
    <select id="findAll" resultType="org.practice.domain.User">
        select * from user 
    </select>
</mapper>
```
中 `namespace` 和 `id` 的作用，光靠 `id` 是不能定位到这条sql语句的，所以还需要一个namespce，mybatis用的代理接口也是需要通过这两个来定位的


# 自定义Mybatis

Mybatis在使用代理dao的方式实现增删改查时做什么事呢？
1. 创建代理对象
2. 在代理对象中调用selectList

将使用前面所学的基础知识来构建一个属于自己的持久层框架，将会涉及到的一些知识点：**工厂模式
（Factory 工厂模式）、构造者模式（Builder 模式）、代理模式，反射，自定义注解，注解的反射，xml 解析，
数据库元数据，元数据的反射**等。
## 分析流程

**执行查询所有分析**
[点击观看视频](https://www.bilibili.com/video/BV1mE411X7yp?p=11)
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/查询所有的分析.png)

**创建代理对象的分析**
[点击观看视频](https://www.bilibili.com/video/BV1mE411X7yp?p=12)
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/自定义Mybatis分析.png)

## Mybatis透过入门案例看到的类

```java
class Resources
class SqlSessionFactoryBuilder
class SqlSessionFactory
interface SqlSessionFactory
interface SqlSession
```
## 前期准备
### 创建Maven工程

创建 mybatis02 的工程，工程信息如下：

    Groupid:org.practice
    ArtifactId:mybatis02
    Packing:jar

### 引入相关坐标

```xml
<dependencies>
    <!-- 日志坐标 -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.12</version>
    </dependency>
<!-- 解析 xml 的 dom4j -->
    <dependency>
        <groupId>dom4j</groupId>
        <artifactId>dom4j</artifactId>
        <version>1.6.1</version>
    </dependency>
    <!-- mysql 驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.6</version>
    </dependency>
    <!-- dom4j 的依赖包 jaxen -->
    <dependency>
        <groupId>jaxen</groupId>
        <artifactId>jaxen</artifactId>
        <version>1.1.6</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.10</version>
    </dependency>
</dependencies>
```
### 引入工具类到项目中
```java
/**
* 用于解析配置文件
*/
public class XMLConfigBuilder {
 /**
 * 解析主配置文件，把里面的内容填充到 DefaultSqlSession 所需要的地方
 * 使用的技术：
 * dom4j+xpath
 * @param session
 */
    public static void loadConfiguration(DefaultSqlSession session, InputStream config){
        try{
        //定义封装连接信息的配置对象（mybatis 的配置对象）
        Configuration cfg = new Configuration();

        //1.获取 SAXReader 对象
        SAXReader reader = new SAXReader();

        //2.根据字节输入流获取 Document 对象
        Document document = reader.read(config);

        //3.获取根节点
        Element root = document.getRootElement();

        //4.使用 xpath 中选择指定节点的方式，获取所有 property 节点
        List<Element> propertyElements = root.selectNodes("//property");
        
        //5.遍历节点
        for(Element propertyElement : propertyElements){
            //判断节点是连接数据库的哪部分信息
            //取出 name 属性的值
            String name = propertyElement.attributeValue("name");
            if("driver".equals(name)){
                //表示驱动
                //获取 property 标签 value 属性的值
                String driver = propertyElement.attributeValue("value");
                cfg.setDriver(driver);
            }
            if("url".equals(name)){
                //表示连接字符串
                //获取 property 标签 value 属性的值
                String url = propertyElement.attributeValue("value");
                cfg.setUrl(url);
            }
            if("username".equals(name)){
                //表示用户名
                //获取 property 标签 value 属性的值
                String username = propertyElement.attributeValue("value");
                cfg.setUsername(username);
            }
            if("password".equals(name)){
                //表示密码
                //获取 property 标签 value 属性的值
                String password = propertyElement.attributeValue("value");
                cfg.setPassword(password);
            }
        }
        //取出 mappers 中的所有 mapper 标签，判断他们使用了 resource 还是 class 属性
        List<Element> mapperElements = root.selectNodes("//mappers/mapper");

        //遍历集合
        for(Element mapperElement : mapperElements){
            //判断 mapperElement 使用的是哪个属性
            Attribute attribute = mapperElement.attribute("resource");

            if(attribute != null){
                System.out.println("使用的是 XML");
                //表示有 resource 属性，用的是 XML
                //取出属性的值
                String mapperPath = attribute.getValue();// 获 取 属 性 的 值
                "com/itheima/dao/IUserDao.xml"
                //把映射配置文件的内容获取出来，封装成一个 map
                Map<String,Mapper> mappers = loadMapperConfiguration(mapperPath);
                //给 configuration 中的 mappers 赋值
                cfg.setMappers(mappers);
            }else{
                System.out.println("使用的是注解");
                //表示没有 resource 属性，用的是注解
                //获取 class 属性的值
                String daoClassPath = mapperElement.attributeValue("class");
                //根据 daoClassPath 获取封装的必要信息
                Map<String,Mapper> mappers = loadMapperAnnotation(daoClassPath);
                //给 configuration 中的 mappers 赋值
                cfg.setMappers(mappers);
            }
        }
        //把配置对象传递给 DefaultSqlSession
        session.setCfg(cfg);

        }catch(Exception e){
            throw new RuntimeException(e);
        }finally{
            try {
                config.close();
            }catch(Exception e){
                e.printStackTrace();
            }
        }
    }

 /**
 * 根据传入的参数，解析 XML，并且封装到 Map 中
 * @param mapperPath 映射配置文件的位置
 * @return map 中包含了获取的唯一标识（key 是由 dao 的全限定类名和方法名组成）
 * 以及执行所需的必要信息（value 是一个 Mapper 对象，里面存放的是执行的 SQL 语句和
要封装的实体类全限定类名）
 */
 private static Map<String,Mapper> loadMapperConfiguration(String mapperPath) throws IOException {
    InputStream in = null;
    try{
        //定义返回值对象
        Map<String,Mapper> mappers = new HashMap<String,Mapper>();

        //1.根据路径获取字节输入流
        in = Resources.getResourceAsStream(mapperPath);

        //2.根据字节输入流获取 Document 对象
        SAXReader reader = new SAXReader();
        Document document = reader.read(in);

        //3.获取根节点
        Element root = document.getRootElement();

        //4.获取根节点的 namespace 属性取值
        String namespace = root.attributeValue("namespace");//是组成 map 中 key 的部分

        //5.获取所有的 select 节点
        List<Element> selectElements = root.selectNodes("//select");

        //6.遍历 select 节点集合
        for(Element selectElement : selectElements){
            //取出 id 属性的值 组成 map 中 key 的部分
            String id = selectElement.attributeValue("id");
            //取出 resultType 属性的值 组成 map 中 value 的部分
            String resultType = selectElement.attributeValue("resultType");
            //取出文本内容 组成 map 中 value 的部分
            String queryString = selectElement.getText();
            //创建 Key
            String key = namespace+"."+id;
            //创建 Value
            Mapper mapper = new Mapper();
            mapper.setQueryString(queryString);
            mapper.setResultType(resultType);
            //把 key 和 value 存入 mappers 中
            mappers.put(key,mapper);
        }
        return mappers;
    }catch(Exception e){
        throw new RuntimeException(e);
    }finally{
        in.close();
    }
 }

 /**
 * 根据传入的参数，得到 dao 中所有被 select 注解标注的方法。
 * 根据方法名称和类名，以及方法上注解 value 属性的值，组成 Mapper 的必要信息
 * @param daoClassPath
 * @return
 */
 private static Map<String,Mapper> loadMapperAnnotation(String daoClassPath) throws Exception{
    //定义返回值对象
    Map<String,Mapper> mappers = new HashMap<String, Mapper>();

    //1.得到 dao 接口的字节码对象
    Class daoClass = Class.forName(daoClassPath);

    //2.得到 dao 接口中的方法数组
    Method[] methods = daoClass.getMethods();

    //3.遍历 Method 数组
    for(Method method : methods){
        //取出每一个方法，判断是否有 select 注解
        boolean isAnnotated = method.isAnnotationPresent(Select.class);
        if(isAnnotated){
            //创建 Mapper 对象
            Mapper mapper = new Mapper();
            //取出注解的 value 属性值
            Select selectAnno = method.getAnnotation(Select.class);
            String queryString = selectAnno.value();
            mapper.setQueryString(queryString);
            //获取当前方法的返回值，还要求必须带有泛型信息
            Type type = method.getGenericReturnType();//List<User>
            //判断 type 是不是参数化的类型
            if(type instanceof ParameterizedType){
                //强转
                ParameterizedType ptype = (ParameterizedType)type;
                //得到参数化类型中的实际类型参数
                Type[] types = ptype.getActualTypeArguments();
                //取出第一个
                Class domainClass = (Class)types[0];
                //获取 domainClass 的类名
                String resultType = domainClass.getName();
                //给 Mapper 赋值
                mapper.setResultType(resultType);
            }
            //组装 key 的信息
            //获取方法的名称
            String methodName = method.getName();
            String className = method.getDeclaringClass().getName();
            String key = className+"."+methodName;
            //给 map 赋值
            mappers.put(key,mapper);
        }
    }
    return mappers;
}
}

/**
* 负责执行 SQL 语句，并且封装结果集
*/
public class Executor {
    public <E> List<E> selectList(Mapper mapper, Connection conn) {
        PreparedStatement pstm = null;
        ResultSet rs = null;
        try {
            //1.取出 mapper 中的数据
            String queryString = mapper.getQueryString();//select * from user
            String resultType = mapper.getResultType();//com.itheima.domain.User
            Class domainClass = Class.forName(resultType);//User.class
            //2.获取 PreparedStatement 对象
            pstm = conn.prepareStatement(queryString);
            //3.执行 SQL 语句，获取结果集
            rs = pstm.executeQuery();
            //4.封装结果集
            List<E> list = new ArrayList<E>();//定义返回值
            while(rs.next()) {
                //实例化要封装的实体类对象
                E obj = (E)domainClass.newInstance();//User 对象
                //取出结果集的元信息：ResultSetMetaData
                ResultSetMetaData rsmd = rs.getMetaData();
                //取出总列数
                int columnCount = rsmd.getColumnCount();
                //遍历总列数
                for (int i = 1; i <= columnCount; i++) {
                    //获取每列的名称，列名的序号是从 1 开始的
                    String columnName = rsmd.getColumnName(i);
                    //根据得到列名，获取每列的值
                    Object columnValue = rs.getObject(columnName);
                    //给 obj 赋值：使用 Java 内省机制（借助 PropertyDescriptor 实现属性的封装）
                    PropertyDescriptor pd = new PropertyDescriptor(columnName,domainClass);//要求：实体类的属性和数据库表的列名保持一种
                    //获取它的写入方法
                    Method writeMethod = pd.getWriteMethod();//setUsername(String
                    username);
                    //把获取的列的值，给对象赋值
                    writeMethod.invoke(obj,columnValue);
                }
                //把赋好值的对象加入到集合中
                list.add(obj);
            }
            return list;
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            release(pstm,rs);
        }
    }

    private void release(PreparedStatement pstm,ResultSet rs){
        if(rs != null){
            try {
                rs.close();
            }catch(Exception e){
                e.printStackTrace();
            }
        }

        if(pstm != null){
            try {
                pstm.close();
            }catch(Exception e){
                e.printStackTrace();
            }
        }
    }
}

/**
*
* <p>Title: DataSourceUtil</p>
* <p>Description: 数据源的工具类</p>
*/
public class DataSourceUtil {
    /**
    * 获取连接
    * @param cfg
    * @return
    */
    public static Connection getConnection(Configuration cfg) {
        try {
            Class.forName(cfg.getDriver());
            Connection conn =
            DriverManager.getConnection(cfg.getUrl(),cfg.getUsername() , cfg.getPassword());
            return conn;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```
### 编写SqlMapConfig.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver" ></property>
                <property name="url" value="jdbc:mysql:///sample01" ></property>
                <property name="username" value="root"></property>
                <property name="password" value="123456"></property>
            </dataSource>
        </environment>
    </environments>
</configuration>
```
**注意**：此处我们直接使用的是 mybatis 的配置文件，但是由于我们没有使用 mybatis 的 jar 包，所以要把配
置文件的约束删掉否则会报错（如果电脑能接入互联网，不删也行）

### 编写读取配置文件

```java
/**
*
* <p>Title: Resources</p>
* <p>Description: 用于读取配置文件的类</p>
*/
public class Resources {
    /**
    * 用于加载 xml 文件，并且得到一个流对象
    * @param xmlPath
    * @return
    * 在实际开发中读取配置文件:
    * 第一：使用类加载器。但是有要求：a 文件不能过大。 b 文件必须在类路径下(classpath)
    * 第二：使用 ServletContext 的 getRealPath()
    */
    public static InputStream getResourceAsStream(String xmlPath) {
        return Resources.class.getClassLoader().getResourceAsStream(xmlPath);
    }
}
```

### 编写Mapper类

```java
/**
*
* <p>Title: Mapper</p>
* <p>Description: 用于封装查询时的必要信息：要执行的 SQL 语句和实体类的全限定类名</p>
* <p>Company: http://www.itheima.com/ </p>
*/
public class Mapper {
    private String queryString;//sql
    private String resultType;//结果类型的全限定类名
    public String getQueryString() {
        return queryString;
    }
    public void setQueryString(String queryString) {
        this.queryString = queryString;
    }
    public String getResultType() {
        return resultType;
    }
    public void setResultType(String resultType) {
        this.resultType = resultType;
    }
}
```
### 编写Configurarion配置类
```java
/**
* 核心配置类
* 1.数据库信息
* 2.sql 的 map 集合
*/
public class Configuration {
    private String username; //用户名
    private String password;//密码
    private String url;//地址
    private String driver;//驱动
    //map 集合 Map<唯一标识，Mapper> 用于保存映射文件中的 sql 标识及 sql 语句
    private Map<String,Mapper> mappers;
    public String getUsername() {
        return username;
    }
    public void setUsername(String username) {
        this.username = username;
    }
    public String getPassword() {
        return password;
    }
    public void setPassword(String password) {
        this.password = password;
    }
    public String getUrl() {
        return url;
    }
    public void setUrl(String url) {
        this.url = url;
    }
    public String getDriver() {
        return driver;
    }
    public void setDriver(String driver) {
        this.driver = driver;
    }
    public Map<String, Mapper> getMappers() {
        return mappers;
    }
    public void setMappers(Map<String, Mapper> mappers) {
        this.mappers = mappers;
    }
}
```
### 编写User实体类
```java
public class User implements Serializable {
    private int id;
    private String username;// 用户姓名
    private String sex;// 性别
    private Date birthday;// 生日
    private String address;// 地址
    //省略 getter 与 setter
    @Override
    public String toString() {
        return "User [id=" + id + ", username=" + username + ", sex=" + sex
        + ", birthday=" + birthday + ", address=" + address + "]";
    }
}
```
## 基于XML的自定义Mybatis框架

### 编写持久层接口和IUserDao.xml
```java
/**
*
* <p>Title: IUserDao</p>
* <p>Description: 用户的持久层操作</p>
*/
public interface IUserDao {
/**
* 查询所有用户
* @return
*/
    List<User> findAll();
}

<?xml version="1.0" encoding="UTF-8"?>
<mapper namespace="org.practice.dao.IUserDao">
    <!-- 配置查询所有操作 -->
    <select id="findAll" resultType="org.practice.domain.User">
        select * from user
    </select>
</mapper>
```
**注意：**此处我们使用的也是 mybatis 的配置文件，所以也要把约束删除了

### 编写构建者类
```java
/**
*
* <p>Title: SqlSessionFactoryBuilder</p>
* <p>Description: 用于构建 SqlSessionFactory 的</p>
*/
public class SqlSessionFactoryBuilder {
    /**
    * 根据传入的流，实现对 SqlSessionFactory 的创建
    * @param in 它就是 SqlMapConfig.xml 的配置以及里面包含的 IUserDao.xml 的配置
    * @return
    */
    public SqlSessionFactory build(InputStream in) {
        DefaultSqlSessionFactory factory = new DefaultSqlSessionFactory();
        //给 factory 中 config 赋值
        factory.setConfig(in);
        return factory;
    }
}
```

### 编写SqlSessionFactory接口和实现类
```java
/**
*
* <p>Title: SqlSessionFactory</p>
* <p>Description: SqlSessionFactory 的接口</p>
*/
public interface SqlSessionFactory {
    /**
    * 创建一个新的 SqlSession 对象
    * @return
    */
    SqlSession openSession();
}
/**
*
* <p>Title: DefaultSqlSessionFactory</p>
* <p>Description:SqlSessionFactory 的默认实现 </p>
*/
public class DefaultSqlSessionFactory implements SqlSessionFactory {
    private InputStream config = null;
    public void setConfig(InputStream config) {
        this.config = config;
    }
    @Override
    public SqlSession openSession() {
        DefaultSqlSession session = new DefaultSqlSession();
        //调用工具类解析 xml 文件
        XMLConfigBuilder.loadConfiguration(session, config);
        return session;
    }
}
```
### 编写SqlSession接口和实现类

```java
/**
*
* <p>Title: SqlSession</p>
* <p>Description: 操作数据库的核心对象</p>
*/
public interface SqlSession {
    /**
    * 创建 Dao 接口的代理对象
    * @param daoClass
    * @return
    */
    <T> T getMapper(Class<T> daoClass);
    /**
    * 释放资源
    */
    void close();
}

/**
*
* <p>Title: DefaultSqlSession</p>
* <p>Description: SqlSession 的具体实现</p>
*/
public class DefaultSqlSession implements SqlSession {
    //核心配置对象
    private Configuration cfg;

    public void setCfg(Configuration cfg) {
        this.cfg = cfg;
    }
    //连接对象
    private Connection conn;
    //调用 DataSourceUtils 工具类获取连接
    public Connection getConn() {
        try {
            conn = DataSourceUtil.getDataSource(cfg).getConnection();
            return conn;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
    * 动态代理：
    * 涉及的类：Proxy
    * 使用的方法：newProxyInstance
    * 方法的参数：
    * ClassLoader：和被代理对象使用相同的类加载器,通常都是固定的
    * Class[]：代理对象和被代理对象要求有相同的行为。（具有相同的方法）
    * InvocationHandler：如何代理。需要我们自己提供的增强部分的代码
    */
    @Override
    public <T> T getMapper(Class<T> daoClass) {
        conn = getConn();
        System.out.println(conn);
        T daoProxy = (T) Proxy.newProxyInstance(daoClass.getClassLoader(),new Class[] {daoClass}, new MapperProxyFactory(cfg.getMappers(),conn));
        return daoProxy;
    }
    //释放资源
    @Override
    public void close() {
        try {
            System.out.println(conn);
            conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    //查询所有方法
    public <E> List<E> selectList(String statement){
        Mapper mapper = cfg.getMappers().get(statement);
        return new Executor().selectList(mapper,conn);
    }
}
```

### 编写用于创建Dao接口代理对象的类
```java
/**
*
* <p>Title: MapperProxyFactory</p>
* <p>Description: 用于创建代理对象是增强方法</p>
*/
public class MapperProxyFactory implements InvocationHandler {
    private Map<String,Mapper> mappers;
    private Connection conn;

    public MapperProxyFactory(Map<String, Mapper> mappers,Connection conn) {
        this.mappers = mappers;
        this.conn = conn;
    }
    /**
    * 对当前正在执行的方法进行增强
    * 取出当前执行的方法名称
    * 取出当前执行的方法所在类
    * 拼接成 key
    * 去 Map 中获取 Value（Mapper)
    * 使用工具类 Executor 的 selectList 方法
    */
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //1.取出方法名
        String methodName = method.getName();
        //2.取出方法所在类名
        String className = method.getDeclaringClass().getName();
        //3.拼接成 Key
        String key = className+"."+methodName;
        //4.使用 key 取出 mapper
        Mapper mapper = mappers.get(key);
        if(mapper == null) {
            throw new IllegalArgumentException("传入的参数有误，无法获取执行的必要条件");
        }
        //5.创建 Executor 对象
        Executor executor = new Executor();
        return executor.selectList(mapper, conn);
    }
}
```
### 运行测试类

```java
/**
*
* <p>Title: MybatisTest</p>
* <p>Description: 测试 mybatis 的环境</p>
*/
public class MybatisTest {
    public static void main(String[] args)throws Exception {
    //1.读取配置文件
    InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
    //2.创建 SqlSessionFactory 的构建者对象
    SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
    //3.使用构建者创建工厂对象 SqlSessionFactory
    SqlSessionFactory factory = builder.build(in);
    //4.使用 SqlSessionFactory 生产 SqlSession 对象
    SqlSession session = factory.openSession();
    //5.使用 SqlSession 创建 dao 接口的代理对象
    IUserDao userDao = session.getMapper(IUserDao.class);
    //6.使用代理对象执行查询所有方法
    List<User> users = userDao.findAll();
    for(User user : users) {
        System.out.println(user);
    }
    //7.释放资源
    session.close();
    in.close();
    }
}
```

## 基于注解方式定义Mybatis框架
### 自定义@Select注解
```java
/**
*
* <p>Title: Select</p>
* <p>Description: 自定义查询注解</p>
*/
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)

public @interface Select {
    String value();
}
```

### 更改持久层接口
```java
/**
*
* <p>Title: IUserDao</p>
* <p>Description: 用户的持久层操作</p>
*/
public interface IUserDao {
    /**
    * 查询所有用户
    * @return
    */
    @Select("select * from user")
    List<User> findAll();
}

```

### 修改SqlMapConfig.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!-- 配置 mybatis 的环境 -->
    <environments default="mysql">
        <!-- 配置 mysql 的环境 -->
        <environment id="mysql">
            <!-- 配置事务的类型 -->
            <transactionManager type="JDBC"></transactionManager>
            <!-- 配置连接数据库的信息：用的是数据源(连接池) -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/sqmple01"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    
    <!-- 告知 mybatis 映射配置的位置 -->
    <mappers>
        <mapper class="org.practice.dao.IUserDao"/>
    </mappers>
</configuration>
```

## 自定义Mybatis的设计模式说明

### 工厂模式(SqlSessionFactory)

工厂模式是我们最常用的实例化对象模式了，是用工厂方法代替new操作的一种模式。著名的Jive论坛 ,就大量使用了工厂模式，工厂模式在Java程序系统可以说是随处可见。因为工厂模式就相当于创建实例对象的new，我们经常要根据类Class生成实例对象，如A a=new A() 工厂模式也是用来创建实例对象的，所以以后new时就要多个心眼，是否可以考虑使用工厂模式，虽然这样做，可能多做一些工作，但会给你系统带来更大的可扩展性和尽量少的修改量。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200728175110.png)

### 代理模式(MapperProxyFactory)

**组成**：
- 抽象角色：通过接口或抽象类声明真实角色实现的业务方法。
- 代理角色：实现抽象角色，是真实角色的代理，通过真实角色的业务逻辑方法来实现抽象方法，并可以附加自己的操作。
- 真实角色：实现抽象角色，定义真实角色所要实现的业务逻辑，供代理角色调用。

**代理模式分为静态和动态代理**。静态代理，我们通常都很熟悉。有一个写好的代理类，实现与要代理的类的一
个共同的接口，目的是为了约束也为了安全。具体不再多说。

这里主要想说的是关于动态代理。我们知道静态代理若想代理多个类，实现扩展功能，那么它必须具有多个代理类分别取代理不同的实现类。这样做的后果是造成太多的代码冗余。那么我们会思考如果做，才能既满足需求，又没有太多的冗余代码呢？——————动态代理。通过前面的课程我们已经学过了基于 JDK 的动态代理实现方式，今天我们就会使用 JDK 动态代理方式来编写 MapperProxyFactory 类。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200728175453.png)

### 构建者模式(SqlSessionFactoryBuilder)

构建者模式是java23种设计模式之一，英文叫Builder Pattern。其核心思想是将一个“复杂对象的构建算法”与它的“部件及组装方式”分离，使得构件算法和组装方式可以独立应对变化；复用同样的构建算法可以创建不同的表示，不同的构建过程可以复用相同的部件组装方式。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200728175640.png)

从图中我们可以看出，创建者模式由四部分组成。

**抽象创建者角色**：给出一个抽象接口，以规范产品对象的各个组成成分的建造。一般而言，此接口独立于应用
程序的商业逻辑。模式中直接创建产品对象的是具体创建者角色。具体创建者必须实现这个接口的两种方法：一是
建造方法，比如图中的 buildPart1 和 buildPart2 方法；另一种是结果返回方法，即图中的 getProduct 方
法。一般来说，产品所包含的零件数目与建造方法的数目相符。换言之，有多少零件，就有多少相应的建造方法
。
**具体创建者角色**：他们在应用程序中负责创建产品的实例。这个角色要完成的任务包括：
1. 实现抽象创建者所声明的抽象方法，给出一步一步的完成产品创建实例的操作。
2. 在创建完成后，提供产品的实例。

**导演者角色**：这个类调用具体创建者角色以创建产品对象。但是导演者并没有产品类的具体知识，真正拥有产
品类的具体知识的是具体创建者角色。

**产品角色**：产品便是建造中的复杂对象。一般说来，一个系统中会有多于一个的产品类，而且这些产品类并不
一定有共同的接口，而完全可以使不相关联的。

## 最后回顾

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/自定义mybatis开发流程图.png)

---

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/非常重要的一张图.png)

---

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/非常重要的一张图-分析编写dao实现类Mybatis的执行过程(1).png)

---

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/非常重要的图-分析代理dao的执行过程.png)


# Mybatis的CRUD (Create, Retrieve, Update, Delete)

**使用要求：**
1. 持久层接口和持久层接口的映射配置必须在相同的包下
2. 持久层映射配置中 mapper 标签的 namespace 属性取值必须是持久层接口的全限定类名
3. SQL 语句的配置标签`<select>`,`<insert>`,`<delete>`,`<update>`的 id 属性必须和持久层接口的
方法名相同。

## Retrieve

**在持久层接口中添加 `findById` 方法**

```java
/**
* 根据 id 查询
* @param userId
* @return
*/
User findById(Integer userId);

```

**在用户的映射配置文件中配置**

```xml
<!-- 根据 id 查询 -->
<select id="findById" resultType="org.practice.domain.User" parameterType="int">
    select * from user where id = #{uid}
</select>
```
**resultType 属性：** 用于指定结果集的类型。
**parameterType 属性：** 用于指定传入参数的类型。
**sql 语句中使用#{}字符：** 它代表占位符，相当于原来 jdbc 部分所学的，都是用于执行语句时替换实际的数据。具体的数据是由#{}里面的内容决定的。
**#{}中内容的写法：** 由于数据类型是基本类型，所以此处可以随意写。

**在测试类添加测试**
```java
/**
*
* <p>Title: MybastisCRUDTest</p>
* <p>Description: 测试 mybatis 的 crud 操作</p>
*/
public class MybastisCRUDTest {
    private InputStream in ;
    private SqlSessionFactory factory;
    private SqlSession session;
    private IUserDao userDao;

    @Test
    public void testFindOne() {
        //6.执行操作
        User user = userDao.findById(41);
        System.out.println(user);
    }

    @Before//在测试方法执行之前执行
    public void init()throws Exception {
        //1.读取配置文件
        in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.创建构建者对象
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        //3.创建 SqlSession 工厂对象
        factory = builder.build(in);
        //4.创建 SqlSession 对象
        session = factory.openSession();
        //5.创建 Dao 的代理对象
        userDao = session.getMapper(IUserDao.class);
    }

    @After//在测试方法执行完成之后执行
    public void destroy() throws Exception{
        session.commit();
        //7.释放资源
        session.close();
        in.close();
    }
}
```

## Retrieve（模糊查询）

**在持久层接口中添加模糊查询方法**
```java
/**
* 根据名称模糊查询
* @param username
* @return
*/
List<User> findByName(String username);
```
**在用户的映射配置文件中配置**
```xml
<!-- 根据名称模糊查询 -->
<select id="findByName" resultType="org.practice.domain.User" parameterType="String">
    select * from user where username like #{username}
</select>
```
**加入模糊查询的测试方法**
```java
@Test
 public void testFindByName(){
    //5.执行查询一个方法
    List<User> users = userDao.findByName("%王%");
    for(User user : users){
        System.out.println(user);
    }
 }
```
在控制台输出的执行 SQL 语句如下：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200728230818.png)

我们在配置文件中没有加入%来作为模糊查询的条件，所以在传入字符串实参时，就需要给定模糊查询的标
识%。配置文件中的#{username}也只是一个占位符，所以 SQL 语句显示为“？”。

**模糊查询的另一种配置：**

第一步：修改 SQL 语句的配置，配置如下：
```xml
<!-- 根据名称模糊查询 -->
<select id="findByName" parameterType="string" resultType="org.practice.domain.User">
    select * from user where username like '%${value}%'
</select>
```
我们在上面将原来的#{}占位符，改成了`${value}`。注意如果用模糊查询的这种写法，那么`${value}`的写
法就是固定的，不能写成其它名字。

第二步：测试，如下：
```java
/**
* 测试模糊查询操作
 */
@Test
public void testFindByName(){
    //5.执行查询一个方法
    List<User> users = userDao.findByName("王");
    for(User user : users){
        System.out.println(user);
    }
}
```
在控制台输出的执行 SQL 语句如下：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200728231402.png)

可以发现，我们在程序代码中就不需要加入模糊查询的匹配符%了，这两种方式的实现效果是一样的，但执行
的语句是不一样的。

**#{}与${}的区别：**

**\#{}表示一个占位符号**
通过#{}可以实现 preparedStatement 向占位符中设置值，自动进行 java 类型和 jdbc 类型转换，#{}可以有效防止 sql 注入。 #{}可以接收简单类型值或 pojo 属性值。 如果 parameterType 传输单个简单类型值，#{}括号中可以是 value 或其它名称。
**\${}表示拼接 sql 串**
通\${}可以将 parameterType 传入的内容拼接在 sql 中且不进行 jdbc 类型转换， \${}可以接收简单类型值或 pojo 属性值，如果 parameterType 传输单个简单类型值，${}括号中只能是 value。

**模糊查询的${value}源码分析：**

我们一起来看 TextSqlNode 类的源码：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200729162347.png)

这就说明了源码中指定了读取的 key 的名字就是”value”，所以我们在绑定参数时就只能叫 value 的名字
了。

---

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/无标题.png)

## Retrieve（聚合函数）

**在持久层接口中添加模糊查询方法**

```java
/**
* 查询总记录条数
* @return
*/
int findTotal();
```
**在用户的映射配置文件中配置**

```xml
<!-- 查询总记录条数 -->
<select id="findTotal" resultType="int">
    select count(*) from user;
</select>
```
**加入聚合查询的测试方法**

```java
@Test
public void testFindTotal() throws Exception {
    //6.执行操作
    int res = userDao.findTotal();
    System.out.println(res);
}
```


















## Create

**在持久层接口中添加新增方法**

```java
/**
* 保存用户
* @param user
* @return 影响数据库记录的行数
*/
void saveUser(User user)
```
**在用户的映射配置文件中配置**
```xml
<!-- 保存用户-->
<insert id="saveUser" parameterType="org.practice.domain.User">
    insert into user(username,birthday,sex,address)values(#{username},#{birthday},#{sex},#{address})
</insert>
```

**parameterType 属性：** 代表参数的类型，因为我们要传入的是一个类的对象，所以类型就写类的全名称。
**sql 语句中使用#{}字符：** 它代表占位符，相当于原来 jdbc 部分所学的?，都是用于执行语句时替换实际的数据。
具体的数据是由#{}里面的内容决定的。
**#{}中内容的写法：** 由于我们保存方法的参数是 一个 User 对象，此处要写 User 对象中的属性名称。它用的是 ognl 表达式。
**ognl 表达式：** 它是 apache 提供的一种表达式语言，全称是：`Object Graphic Navigation Language` 对象图导航语言，它是按照一定的语法格式来获取数据的。语法格式就是使用 #{对象.对象}的方式 `#{user.username}`它会先去找 user 对象，然后在 user 对象中找到 username 属性，并调用getUsername()方法把值取出来。但是我们在 parameterType 属性上指定了实体类名称，所以可以省略 user，而直接写 username。

**添加测试类中的测试方法**

```java
@Test
public void testSave(){
    User user = new User();
    user.setUsername("modify User property");
    user.setAddress("北京市顺义区");
    user.setSex("男");
    user.setBirthday(new Date());
    System.out.println("保存操作之前："+user);
    //5.执行保存方法
    userDao.saveUser(user);
    System.out.println("保存操作之后："+user);
}
```
打开 Mysql 数据库发现并没有添加任何记录，原因是什么？
这一点和 jdbc 是一样的，我们在实现增删改时一定要去控制事务的提交，那么在 mybatis 中如何控制事务提交呢？
可以使用: `session.commit()` ;来实现事务提交。加入事务提交后的代码如下：
```java
@After//在测试方法执行完成之后执行
public void destroy() throws Exception{
    session.commit();
    //7.释放资源
    session.close();
    in.close();
}
```
**新增用户的ID返回值**

新增用户后，同时还要返回当前新增用户的 id 值，因为 id 是由数据库的自动增长来实现的，所以就相
当于我们要在新增后将自动增长 auto_increment 的值返回。

```xml
<insert id="saveUser" parameterType="USER">
    <!-- 配置保存时获取插入的 id -->
    <selectKey keyColumn="id" keyProperty="id" resultType="int" order = "AFTER">
        select last_insert_id();
    </selectKey>
    insert into user(username,birthday,sex,address)values(#{username},#{birthday},#{sex},#{address})
</insert>
```
## Update

**在持久层接口中添加新增方法**
```java
/**
* 保存用户
* @param user
* @return 影响数据库记录的行数
*/
void updateUser(User user);
```

**在用户的映射配置文件中配置**
```xml
<!-- 更新用户 -->
<update id="updateUser" parameterType="org.practice.domain.User">
    update user set username=#{username},birthday=#{birthday},sex=#{sex},
    address=#{address} where id=#{id}
</update>
```

**加入更新的测试方法**
```java
@Test
public void testUpdateUser()throws Exception{
    //1.根据 id 查询
    User user = userDao.findById(52); 
    //2.更新操作
    user.setAddress("北京市顺义区");
    int res = userDao.updateUser(user);
    System.out.println(res);
}
```
## Delete

**在持久层接口中添加删除方法**
```java
/**
* 根据 id 删除用户
* @param userId
* @return
*/

void deleteUser(Integer userId);
```
**在用户的映射配置文件中配置**
```xml
<!-- 删除用户 -->
<delete id="deleteUser" parameterType="java.lang.Integer">
    delete from user where id = #{uid}
</delete>
```

**加入删除的测试方法**
```java
@Test
public void testDeleteUser() throws Exception {
    //6.执行操作
    int res = userDao.deleteUser(52);
    System.out.println(res);
}
```

# Mybatis的参数深入
## Mybatis参数
### parameterType配置参数
基本类型和String我们可以直接写类型名称，也可以使用包名.类名的方式，例如：`java.lang.String`。

实体类类型，目前我们只能使用全限定类名。

究其原因，是 mybaits 在加载时已经把常用的数据类型注册了别名，从而我们在使用时可以不写包名，
而我们的是实体类并没有注册别名，所以必须写全限定类名。在今天课程的最后一个章节中将讲解如何注册实体类
的别名。
在 mybatis 的官方文档的说明(第 19 页)：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200729170746.png)

这些都是支持的默认别名。我们也可以从源码角度来看它们分别都是如何定义出来的。
可以参考 TypeAliasRegistery.class 的源码：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200729170813.png)

### 传递pojo对象

Mybatis使用ognl表达式解析对象字段的值，#{}或者${}括号中的值为pojo属性名称。

OGNL(**Object Graphic Navigation Language**)表达式：
作用是通过对象的取值方法来获取数据。在写法上把get给省略了。例如：
我们获取用户的名称，在类中的写法：`user.getUsername()`。OGNL表达式写法：`user.username`

Mybatis为什么能直接写username，而不用user呢？因为在parameterType中已经提供了属性所属的类，所以此时不需要写对象名。

### 传递pojo包装对象

开发中通过 pojo 传递查询条件 ，查询条件是综合的查询条件，不仅包括用户查询条件还包括其它的查询条件（比如将用户购买商品信息也作为查询条件），这时可以使用包装对象传递输入参数。

Pojo 类中包含 pojo。

需求：根据用户名查询用户信息，查询条件放到 QueryVo 的 user 属性中。

**编写 QueryVo**
```java
/**
*
* <p>Title: QueryVo</p>
* <p>Description: 查询条件对象</p>
*/
public class QueryVo implements Serializable {
    private User user;
    public User getUser() {
        return user;
    }
    public void setUser(User user) {
        this.user = user;
    }
}
```
**编写持久层接口**
```java
/**
*
* <p>Title: IUserDao</p>
* <p>Description: 用户的业务层接口</p>
*/
public interface IUserDao {
    /**
    * 根据 QueryVo 中的条件查询用户
    * @param vo
    * @return
    */
    List<User> findByVo(QueryVo vo);
}
```
**持久层接口的映射文件**
```xml
<!-- 根据用户名称模糊查询，参数变成一个 QueryVo 对象了 -->
<select id="findByVo" resultType="org.practice.domain.User" parameterType="org.practice.domain.QueryVo">
    select * from user where username like #{user.username};
</select>
```

**测试包装类作为参数**
```java
@Test
public void testFindByQueryVo() {
    QueryVo vo = new QueryVo();
    User user = new User();
    user.setUserName("%王%");
    vo.setUser(user);
    List<User> users = userDao.findByVo(vo);
    for(User u : users) {
        System.out.println(u);
    }
}
```

## Mybatis的输出结果封装
### resultType（输出类型）
- 基本类型
- 实体类对象类型
- 实体类列表
  
**特殊情况**：前文我们说到 

> 想要建立关系，我们需要做到：**实体类中的属性和数据库表的字段名称保持一致**

如果实体类中的属性和数据库表的字段名称不一样的话，会出现什么情况？
```java
private Integer userId;
private String userName;
private Date userBirthday;
private String userSex;
private String userAddress;
```
这里我们把实体类属性修改一下，然后执行 `testFindAll()`

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200729223102.png)

其他的值都是null可以理解，但是为什么username有值呢？

**因为：mysql 在 windows 系统中不区分大小写！**

How to solve it?

一、使用别名查询
```xml
<!-- 配置查询所有操作 -->
<select id="findAll" resultType="org.practice.domain.User">
    select id as userId,username as userName,birthday as userBirthday,sex as userSex,address as userAddress from user
</select>
```
二、 resultMap

resultMap 标签可以建立查询的列名和实体类的属性名称不一致时建立对应关系。从而实现封装。

在 select 标签中使用 resultMap 属性指定引用即可。同时 resultMap 可以实现将查询结果映射为复杂类型的 pojo，比如在查询结果映射对象中包括 pojo 和 list 实现一对一查询和一对多查询。

1. 定义 resultMap
```xml
<!-- 建立 User 实体和数据库表的对应关系
type 属性：指定实体类的全限定类名
id 属性：给定一个唯一标识，是给查询 select 标签引用用的。
-->
<resultMap type="org.practice.domain.User" id="userMap">
    <!-- 主键字段的对应 -->
    <id column="id" property="userId"/>
    <!-- 非主键字段的对应 -->
    <result column="username" property="userName"/>
    <result column="sex" property="userSex"/>
    <result column="address" property="userAddress"/>
    <result column="birthday" property="userBirthday"/>
</resultMap>
```
id 标签：用于指定主键字段
result 标签：用于指定非主键字段
column 属性：用于指定数据库列名
property 属性：用于指定实体类属性名称

2. 映射配置
```xml
<!-- 配置查询所有操作 -->
<select id="findAll" resultMap="userMap">
    select * from user
</select>
```
3. 测试结果
```java
@Test
    public void testFindAll() {
        List<User> users = userDao.findAll();
        for(User user : users) {
            System.out.println(user);
        }
    }
```

# Mybatis实现DAO层开发（了解）
使用 Mybatis 开发 Dao，通常有两个方法，即原始 Dao 开发方式和 Mapper 接口代理开发方式。而现在主流的开发方式是接口代理开发方式，这种方式总体上更加简便。我们的课程讲解也主要以接口代理开发方式为主。在第二章节已经介绍了基于代理方式的 dao 开发，现在介绍一下基于传统编写 Dao 实现类的开发方式。

[Mybatis中编写Dao实现类的使用方式-查询列表](https://www.bilibili.com/video/BV1mE411X7yp?p=30)
[Mybatis中编写Dao实现类的使用-保存操作](https://www.bilibili.com/video/BV1mE411X7yp?p=31)
[Mybatis中编写Dao实现类的使用-修改删除等其他操作](https://www.bilibili.com/video/BV1mE411X7yp?p=32)
[Mybatis中使用Dao实现类的执行过程分析-查询方法1](https://www.bilibili.com/video/BV1mE411X7yp?p=33)
[Mybatis中使用Dao实现类的执行过程分析-查询方法2](https://www.bilibili.com/video/BV1mE411X7yp?p=34)
[Mybatis中使用Dao实现类的执行过程分析-增删改方法](https://www.bilibili.com/video/BV1mE411X7yp?p=35)
[Mybatis中使用Dao实现类的执行过程分析](https://www.bilibili.com/video/BV1mE411X7yp?p=36)

**分析代理dao的执行过程**
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/非常重要的图-分析代理dao的执行过程.png)

**分析编写dao实现类Mybatis的执行过程**
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/非常重要的一张图-分析编写dao实现类Mybatis的执行过程(1).png)

# SqlMaoConfig.xml配置文件
## 配置内容
SqlMapConfig.xml 中配置的内容和顺序
```xml
-properties（属性）
    --property
-settings（全局配置参数）
    --setting
-typeAliases（类型别名）
    --typeAliase
    --package
-typeHandlers（类型处理器）
-objectFactory（对象工厂）
-plugins（插件）
-environments（环境集合属性对象）
    --environment（环境子属性对象）
        ---transactionManager（事务管理）
        ---dataSource（数据源）
-mappers（映射器）
    --mapper
    --package
```
## properties（属性）

在使用 properties 标签配置时，我们可以采用两种方式指定属性配置。

**第一种**

直接写在SqkMaoConfig.xml里面
```xml
<properties>
    <property name="driver" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql:///sample01"/>
    <property name="username" value="root"/>
    <property name="password" value="123456"/>
</properties>
```

**第二种**

在classpath 下定义 `db.properties` 文件

```
driver=com.mysql.jdbc.Driver
url=jdbc:mysql:///sample01
username=root
password=1234
```

properties 标签配置

```xml
<!-- 配置连接数据库的信息
resource 属性：用于指定 properties 配置文件的位置，要求配置文件必须在类路径下
resource="jdbcConfig.properties"
url 属性：
    URL： Uniform Resource Locator 统一资源定位符
    http://localhost:8080/mystroe/CategoryServlet 
    协议     主机     端口            URI
    URI：Uniform Resource Identifier 统一资源标识符
    /mystroe/CategoryServlet
    它是可以在 web 应用中唯一定位一个资源的路径
-->
<properties url= file:///D:/IdeaProjects/day02_eesy_01mybatisCRUD/src/main/resources/jdbcConfig.properties">
</properties>
```
此时我们的 dataSource 标签就变成了引用上面的配置
```xml
<dataSource type="POOLED">
    <property name="driver" value="${jdbc.driver}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</dataSource>
```

## typeAliases（类型别名）

在前面我们讲的 Mybatis 支持的默认别名，我们也可以采用自定义别名方式来开发。

**自定义别名**
```xml
在 SqlMapConfig.xml 中配置：
<typeAliases>
    <!-- 单个别名定义 -->
    <typeAlias alias="user" type="org.practice.domain.User"/>
    <!-- 批量别名定义，扫描整个包下的类，别名为类名（首字母大写或小写都可以） -->
    <package name="org.practice.domain"/>
    <package name="其它包"/>
</typeAliases>
```

## mappers（映射器）

`<mapper resource=" " />`

    使用相对于类路径的资源
    如：<mapper resource="com/itheima/dao/IUserDao.xml" />

`<mapper class=" " />`

    使用 mapper 接口类路径
    如：<mapper class="com.itheima.dao.UserDao"/>
    注意：此种方法要求 mapper 接口名称和 mapper 映射文件名称相同，且放在同一个目录中。 

`<package name=""/>`

    注册指定包下的所有 mapper 接口
    如：<package name="cn.itcast.mybatis.mapper"/>
    注意：此种方法要求 mapper 接口名称和 mapper 映射文件名称相同，且放在同一个目录中。 


# Mybatis连接池与事务深入

## Mybatis 的连接池技术
开始之前，首先什么是连接池？
连接池可以减少我们获取链接所消耗的时间

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/lianjiechi.png)

我们在前面的 WEB 课程中也学习过类似的连接池技术，而在 Mybatis 中也有连接池技术，但是它采用的是自己的连接池技术。

**在 Mybatis 的 `SqlMapConfig.xml` 配置文件中，通过`<dataSource type=”pooled”>`来实现 Mybatis 中连接池的配置。**


**Mybatis 连接池的分类**
在 Mybatis 中我们将它的数据源 dataSource 分为以下几类：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200816230346.png)

可以看出 Mybatis 将它自己的数据源分为三类：
|类型|描述|详细|
|-|-|-|
|UNPOOLED|不使用连接池的数据源|采用传统的javax.sql.DataSource规范中的连接池，mabytis中有针对规范的实现|
|POOLED|使用连接池的数据源|采用传统的获取连接的方式，虽然也是先Javax.sql.DataSource接口，但是并没有使用池的思想|
|JNDI|使用 JNDI 实现的数据源|采用服务器提供的JNDI技术实现来获取DataSource对象，不同的服务器能拿到的DataSource也是不一样的 注意：如果不是web或者maven的war工程，是不能使用的。 课程中使用的是tomcat服务器，采用连接池就是dpbc连接池|

具体结构如下：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200816231115.png)

相应地，MyBatis 内部分别定义了实现了 java.sql.DataSource 接口的 UnpooledDataSource，PooledDataSource 类来表示 UNPOOLED、POOLED 类型的数据源。

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200816231225.png)

在这三种数据源中，我们一般采用的是 POOLED 数据源（很多时候我们所说的数据源就是为了更好的管理数据
库连接，也就是我们所说的连接池技术）。
