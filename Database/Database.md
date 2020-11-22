
Comparison - NewSQL, SQL, and NoSQL Databases

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200912134730.png)

Data Universal Framework
![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200912135936.png)

# NoSQL

NoSQL is a category of relatively new technologies and products.

## NoSQL Characteristics
**Non-Relational Database**

**Big Data**

**Distributed Storage & Processing**

**Open Source**

**Less expensive hardware**

**Batch Processing**
  - Google Map Reduce

**Interactive and Stream Processing**
  - Apache TezFramework
  - Apache Spark
  - Facebook Presto

## NoSQL Characteristics -continued

- Denormalizationat ingestion to speed up query
- Append instead of update to improve performance
- Schema-agnostic

## Facebook Presto

- Open source distributed SQL query engine
- Run interactive analytic queries against data sources of all sizes ranging from gigabytes to petabytes
- Designed for interactive analytics

## Three V’s of Big Data
- Volume: Ranges from terabytes to petabytes of data
- Variety: Includes data from a wide range of sources and formats (e.g. web logs, social media interactions, transactions, etc)
- Velocity: data needs to be collected, stored, processed, and analyzed within relatively short windows –ranging from daily to real-time

## NoSQL Databases
**Key Value**
- Dynamo, Riak, Basho

**Columnar**
- Google’s Bigtable, Apache’s HBase(part of Hadoop)
- Column Family/Columns

**Document**
- MongoDB
- JSON/XML

**Graph and Triple Store**
- Neo4j

**Analytics and Data Warehousing**
 - Hive
 - Redshift (Amazon)
 - Presto (Facebook)
 - Airpal(Airbnb)

## NoSQL Database Use Cases

**Key‐value stores** 
- Simple binary values, lists, maps, and strings

**Columnar stores**
- Related information values can be grouped in column families

**Document stores** 
- Highly complex parent‐child hierarchal structures

**Triple and Graph stores** 
- A web of interrelated information

## NoSQL Database Application

Key-value stores 
- provide easy and fast storage of simple data through use of key

Columnar stores 
- support very wide tables but not relationships between tables

Document stores
- keep JSON and/or XML hierarchical structures
  
Triple and graph stores 
- store complex relationships

## Key-value Store vs Columnar Store

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930170355.png)

## NoSQL Databases Operations

- Memory Cache
- Distributed
- Proprietary Interface
- SQL-like Interface


# Relational Data Model

Introduced in early ‘70s and gradually implemented beginning ~1980

ELIMINATED (almost) all previous problems
- Records linked together by ALL relationships
  - each record type has **no explicit** owner

## Advantages
- Efficiency in data storage
- ‘Guaranteed’ data integrity
- VERY affordable
- Easy to customize initially as well as modify later
- Standards allow platform-independence (SQL)
- ScalabilityRelatively easy to understand
- Able to capture complex relationships
- Easy access to data

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930170823.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930170840.png)





# Entity Relationship Modeling

## Entity Relationship Diagram (ERD)

### Concepts of the ER Model
- Entities
- Atributes
- Relationshipst

### Types of Keys
|    Key    | Description                                                                                                                                                                                                                                                                                          |
| :-------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Candidate | A field or combination of fields that uniquely identifies each record in a table. A table can have many candidate keys. Any one of the candidate keys can be chosen as the primary key of the table. Once the primary key is chosen, other candidate keys become just key fields, or alternate keys. |
|  Primary  | The field that would always accept unique value and never hold a blank value is identified as primary key. The combination of two fields as a primary key is called composite primary key.                                                                                                           |
|  Foreign  | A field that matches the primary key column of another table.                                                                                                                                                                                                                                        |

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200919130727.png)

Alternate Keys are the candidate keys we didn’t choose as the primary key

### Types of Data Integrity
Domain Integrity (Columns) - Data TypesSQL CHECK Constraints
Entity Integrity (Rows) - Primary Key
Referential Integrity (Between Tables or Columns in different rows of the same table) -Primary Key andForeign Key Pair

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930172823.png)

### UML Notation

**Multiplicity:**
 - Cardinality: How many tuples from each entity can participate in the relationship
 - Participation: Whether a tuple from an entity is required to participate in the relationship 

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930173044.png)

**Meanings of UML Notation:**

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930173225.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930173243.png)

### Crow's Foot Notation

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930173515.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930173536.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930173546.png)

## Enhanced Entity Relationship Diagram (EERD)

Generalization - Super Type (Superclass)
Specialization - Sub Type (Subclass)

AllStaffentity holding  details of all staff:

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930174217.png)

    This doesn’t provide enough semantic meaning of the design. It also wastes storage.

### EER -Supertype/Subtype (UML)

Disjoint (Or) -Can be a member of only one subclass

Disjoint (And, Overlapping, Nondisjoint) -Can be a member of more than one subclass

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930174330.png)

### EER -Supertype/Subtype (Crow’s Foot)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930174512.png)

## Identifying Relationship vs Non-Identifying Relationship

### Identifying Relationship
Identifying Relationship enforcesif there is a child tuple, there mustbe a corresponding parent tuple

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930174819.png)

### Non-Identifying Relationship
Non-Identifying Relationship does notenforceif there is a child tuple, theremustbe a corresponding parent tuple

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200930174845.png)

下面这个说的很详细

    The identifying relationship makes sure when a tuple exists in a child entity,there is a matching tuple in the parent entity. The non-identifying relationshipdoes not enforce it.

    An identifying relationship means that the child table cannot be uniquely identified without the parent. For example, you have this situation in the intersection table used to resolve a many-to-many relationship where the intersecting table's Primary Key is a composite of the left and right (parents) table's Primary Keys.


    identifying关系意思是: 如果没有父表, 子表无法独立定义, 举个例子: 有这么个情况,你建立了一个关系表, 用于解决多对多的关系, 而且这张关系表的主键是由左表和右表(父表)的主键复合组成 .

    Example... 
    Account (AccountID, AccountNum, AccountTypeID) 
    PersonAccount (AccountID, PersonID, Balance)   
    Person(PersonID, Name)  

    The Account to PersonAccount relationship and the Person to PersonAccount relationship are identifying because the child row (PersonAccount) cannot exist without having been defined in the parent (Account or Person). In other words: there is no personaccount when there is no Person or when there is no Account.

    Account 和 PersonAccount 的关系, 以及 Person 和 PersonAccount 的关系, 都是identifying关系, 因为(Account or Person)中未定义的数据不可能在PersonAccount中存在.换句话说: 如果没有Person或者没有Account,  就不会有personaccount .

    A non-identifying relationship is one where the child can be identified independently of the parent ( Account - AccountType)

    non-identifying关系: 字表可以独立定义, 与父表无关 .

    Example... 
    Account( AccountID, AccountNum, AccountTypeID ) 
    AccountType( AccountTypeID, Code, Name, Description )

    The relationship between Account and AccountType is non-identifying because each AccountType can be identified without having to exist in the parent table.

    Account 和 AccountType之间的关系是non-identifying关系, 每一条定义在AccountType中数据不需要必须存在父表中 .

## Surrogate Key vs Natural Key

关于代理键和自然键，请看教授视频：

[![Surrogate Key vs Natural Key](https://res.cloudinary.com/marcomontalbano/image/upload/v1601503167/video_to_markdown/images/youtube--utVee8EU1o0-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=utVee8EU1o0&list=PLDT2VyyU52opF2g7ZT3MJvFYvVMv-0ZNI&index=5&ab_channel=DatabaseDesign "Surrogate Key vs Natural Key")

## Surrogate Key and Business Rule

[![Surrogate Key and Business Rule](https://res.cloudinary.com/marcomontalbano/image/upload/v1601503253/video_to_markdown/images/youtube--JFdNR_bGGNM-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=JFdNR_bGGNM&list=PLDT2VyyU52opF2g7ZT3MJvFYvVMv-0ZNI&index=6&ab_channel=DatabaseDesign "Surrogate Key and Business Rule")

## Associative Entity and Primary Key

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/AssociativeEntityandPrimaryKey.PNG)

视频解释：

[![](https://res.cloudinary.com/marcomontalbano/image/upload/v1601505213/video_to_markdown/images/youtube--lrHNUagk6O0-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=lrHNUagk6O0&list=PLDT2VyyU52opF2g7ZT3MJvFYvVMv-0ZNI&index=7&ab_channel=DatabaseDesign "")
















# Normalization

Purpose of Normalization
- Produce a set of suitable entities that support the data requirements of an enterprise
- Minimize data redundancy
- Provide reporting flexibility
- Maintain data integrity

Database Design Approach

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926130006.png)

Functional Dependency

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926130037.png)

**Lossless-join and Dependency Preservation Properties**
Two important properties of decomposition
- Lossless-join propertyenables us to find any instance of the original relation from corresponding instances in the smaller relations
- Dependency  preservation  propertyenables us to enforce a constraint on the original relation by enforcing some constraint on each of the smaller relations

The Normalization Process

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926130309.png)

什么是范式：简言之就是，数据库设计对数据的存储性能，还有开发人员对数据的操作都有莫大的关系。所以建立科学的，规范的的数据库是需要满足一些规范的来优化数据数据存储方式。在关系型数据库中这些规范就可以称为范式。

## First Normal Form
确保每列保持原子性
- Remove multi-valued attributes
- Remove composite attributes
- Remove repeating groups
- Remove many-to-many relationship

What are Multi-Valued Attributes?

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926131920.png)

What are Composite Attributes?

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926132134.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926132057.png)

What are Repeating Groups?

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926132656.png)

What are Many-to-Many Relationships?

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926133742.png)

对于一个不符合要求的数据进行改进

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926135008.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926135021.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926135034.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926135054.png)

## Second Normal Form
确保表中的每列都和主键相关
- Remove partial dependencies

第二范式在第一范式的基础之上更进一层。第二范式需要确保数据库表中的每一列都和主键相关，而不能只与主键的某一部分相关（主要针对联合主键而言）。也就是说在一个数据库表中，一个表中只能保存一种数据，不可以把多种数据保存在同一张数据库表中。

What is Partial Dependency?

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926135758.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926135831.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926135838.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926135846.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926135901.png)

## Third Normal Form
- Remove transitive dependencies

What is Transitive Dependency?

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926140809.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926140818.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926140830.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926140840.png)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200926140852.png)

# Introduce to SQL

## Role of SQL

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201020210703.png)

- SQl commands are sent from front-end programs to back-end server
- Back end server
  - Parses command to determine action to be taken
  - Completes the action
  - Return results

## SQL Commmand Categories

**DDL：Data Definition Language**

- DDL允许用户定义数据，也就是创建表、删除表、修改表结构这些操作。通常，DDL由数据库管理员执行。

**DML：Data Manipulation Language**

- DML为用户提供添加、删除、更新数据的能力，这些是应用程序对数据库的日常操作。

**DQL：Data Query Language**

- DQL允许用户查询数据，这也是通常最频繁的数据库日常操作。

**DCL：Data Control Language**
- Control user's permisssions


|  DML   |     DDL      |  DCL   |
| :----: | :----------: | :----: |
| SELECT | CREATE TABLE | GRANT  |
| INSERT | ALTER TABLE  | REVOKE |
| UPDATE |  DROP TABLE  |  DENY  |
| DELETE | CREATE VIEW  |        |
|        |  ALTER VIEW  |        |
|        |  DROP VIEW   |        |

## SELECT (Transact-SQL)

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201020214241.png)

### 基本查询
```sql
SELECT * FROM <表名>
```
SELECT语句其实并不要求一定要有FROM子句。

```sql
SELECT 100+200;
```
上述查询会直接计算出表达式的结果。虽然`SELECT`可以用作计算，但它并不是`SQL`的强项。但是，不带`FROM`子句的`SELECT`语句有一个有用的用途，就是用来判断当前到数据库的连接是否有效。许多检测工具会执行一条`SELECT 1`;来测试数据库连接。

### 条件查询

使用`SELECT * FROM <表名>`可以查询到一张表的所有记录。但是，很多时候，我们并不希望获得所有记录，而是根据条件选择性地获取指定条件的记录，例如，查询分数在80分以上的学生记录。在一张表有数百万记录的情况下，获取所有记录不仅费时，还费内存和网络带宽。

`SELECT`语句可以通过`WHERE`条件来设定查询条件，查询结果是满足查询条件的记录。例如，要指定条件“分数在80分或以上的学生”，写成`WHERE`条件就是`SELECT * FROM students WHERE score >= 80`。

其中，WHERE关键字后面的`score >= 80`就是条件。`score`是列名，该列存储了学生的成绩，因此，`score >= 80`就筛选出了指定条件的记录

```sql
SELECT * FROM <表名> WHERE <条件表达式>
```

条件表达式可以用`<条件1> AND <条件2>`表达满足条件1并且满足条件2。例如，符合条件“分数在80分或以上”，并且还符合条件“男生”，把这两个条件写出来：

- 条件1：根据`score`列的数据判断：`score >= 80`；
- 条件2：根据`gender`列的数据判断：`gender = 'M'`，注意`gender`列存储的是字符串，需要用单引号括起来。

就可以写出`WHERE`条件：`score >= 80 AND gender = 'M'`：

第二种条件是`<条件1> OR <条件2>`，表示满足条件1或者满足条件2。例如，把上述`AND`查询的两个条件改为`OR`，查询结果就是“分数在80分或以上”或者“男生”，满足任意之一的条件即选出该记录。

第三种条件是`NOT <条件>`，表示“不符合该条件”的记录。例如，写一个“不是2班的学生”这个条件，可以先写出“是2班的学生”：`class_id = 2`，再加上NOT：`NOT class_id = 2`

要组合三个或者更多的条件，就需要用小括号()表示如何进行条件运算。例如，编写一个复杂的条件：分数在80以下或者90以上，并且是男生: `SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';`

SQL Logical Operators

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201020215323.png)

常用的条件表达式：

![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20201020215148.png)

# How to set database context

## Problem with wrong database context
- An error will occur for any sql command that uses a table that does not exist in the current context.
- Data will be retrieved from the wrong table if table exists in both the current context and the intended context.
- New tables, etc. will be created in wrong database

## Accessing Table in a different database
- Use a three-part name to access a table that exists in a different database than the context being applied in the current Query Editor window:
  - `Databasename.schemaname.tablename`
- Use a four-part name to access a table that exists in a different database than the context being applied in the current Query Editor window:
  - `SQLservername.Databasename.schemaname.tablename`
