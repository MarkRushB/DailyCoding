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

## 编写测试类

文件结构：
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200726152630.png)

```java
public class MybatisTest {
    public static void main(String[] args)throws Exception {
        //1.读取配置文件
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");

        //2.创建 SqlSessionFactory 的构建者对象
        //方便下面利用这个builder.nuild去构建工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();

        //3.使用构建者创建工厂对象 SqlSessionFactory
        //注意这个SqlSessionFactory是一个Interface
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
**注意事项**：不要忘记在映射配置中告知mybatis要封装到哪个实体类中，配置的方式：指定实体类的全限定类名。

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