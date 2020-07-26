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
## 编写持久层接口的映射文件

**创建位置**：必须和持久层接口在相同的包中。
**名称**：必须以持久层接口名称命名文件名，扩展名是`.xml`

## 编写SqlMapConfig.xml配置文件
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
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/ee50"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

<!-- 告知 mybatis 映射配置的位置 -->
    <mappers>
        <mapper resource="com/itheima/dao/IUserDao.xml"/>
    </mappers>
</configuration>

```