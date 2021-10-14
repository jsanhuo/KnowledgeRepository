[TOC]

# MySQL

## 基础

### 数据库命令

\c终止命令执行

查看所有数据库:

```sql
show databases;
```

使用某个数据库：

```sql
use databaseName;
```

创建数据库：

```sql
create database databaseName;
```

删除数据库

```sql
drop database databaseName;
```

查看某个数据库下有那些表

```sql
show tables;
```

导入数据

```sql
source SQLpath
```

查询表中全部数据

```sql
select * from tableName;
```

查看表结构

```sql
desc(describe) tableName;
```

  查看MYSQL数据库版本号

```sql
select version();
```

查看当前使用的数据库

```sql
select database();
```

 

### SQL语句的分类：

DQL：数据查询语言（select）

DML：数据操作语言（对表中数据进行增删改的，insert,delete,update）

DDL：数据定义语言（操作表的结构而不是数据，create，drop，alter）

TCL：事务控制语言（包括事务提交commit，事务回滚rollback）

DCL：数据控制语言（授权grant，撤销权限revoke）







## 进阶

### 一，范式的理解

1. 第一范式（原子性） 
2.  第二范式（每一个非主属性都完全依赖于主键）
3. 第三范式（消除传递依赖）
4. BC范式（每个表只应该有一个候选键(不一样的键)）
5.  第四范式（消除多值依赖）

### 二，数据库的ACID特性

- 原子性(Atomic): 事务中的多个操作，不可分割，要么都成功，要么都失败
- 一致性(Consistency): 事务操作之后, 数据库所处的状态和业务规则是一致的
- 隔离性(Isolation): 多个事务之间就像是串行执行一样，不相互影响
- 持久性(Durability): 事务提交后被持久化到永久存储

### 三，MySQL的隔离级别，以及怎么设置Mysql隔离级别

| 类型                         | 脏读 | 不可重复读 | 幻读 |
| ---------------------------- | ---- | ---------- | ---- |
| 未提交读（READ UNCOMMITTED） | 是   | 是         | 是   |
| 已提交读（READ COMMITTED）   |      | 是         | 是   |
| 可重复读（REPEATABLE READ）  |      |            | 是   |
| 串行化（SERIALIZABLE）       |      |            |      |

在客户端设置

```sql
SET [SESSION|GLOBAL] TRANSACTION ISOLATION LEVEL [READ UNCOMMITTED|READ COMMITTED|REPEATABLE READ|SERIALIZABLE]
```

在配置文件中

```
[mysqld]
transaction-isolation = REPEATABLE-READ
```

MySQL默认为**REPEATABLE-READ**

#### 四，关系型数据库的特点

- 基于关系代数理论
- 缺点：表结构不直观，实现复杂，速度慢
- 优点：健壮性高，社区庞大

#### 五，乐观锁的使用

通过版本号

```sql
update product set count=20 where productid = 2 and count=21
```

此处模拟商场购物后数目减少1，此处的count=21就相当于版本号的确认

读取数据，记录timestamp或者count，version等

检查版本号 和提交数据

#### 六，事务的性能太慢怎么办？

通过乐观锁,是通过tempstamp或者版本号实现

#### 七，数据库索引是如何实现的？索引的作用

   B树

  B+树

  索引的作用：通过创建索引，可以再查询的过程中，加快查找速度，提高系统的性能

​                        : 约束数据的值，如NUIQUE INDEX,PRIMARY KEY,FOREIGN KEY,，创建唯一性索引，可以保证数据库表中每一行数据的唯一性。

​                      ：在使用分组和排序字句进行数据检索时，可以减少查询中分组和排序的时间。

#### 八，数据库索引的分类

 聚集性索引 ：Clustered_index,每个表至多有一个 ，存放的是记录，一般主键是聚集性的

非聚集性索引：指针，指向那条记录  通过指针找记录，速度慢

按照逻辑角度分类：

   1，主键索引：它是一种特殊的唯一索引，不允许有空值，一般在建表的时候同时创建主索引。

   2，普通索引：最基本的索引，没有任何限制

   3，唯一性索引：它与前面的普通索引类型，不同的就是：索引列值必须唯一，但允许有空值。

  4，复合索引（又叫多列索引，联合索引）

#### 九，数据库B树和B+树的区别

​         B树所有的节点存放值，B+树有用的值存放在叶子节点，根和非叶子节点都是存放的都是指针，通过指针

#### 十,B树和二叉树的区别，红黑树的区别

#### 十一,应该在哪些列上创建索引

   1，经常需要搜索的列上，

   2，作为主键的列上

   3，经常用在连接的列上，这些列主要是一些外键，可以加快连接的速度。

  4，经常需要根据范围搜索的列上

  5，经常需要排序的列上

  6，经常使用在where字句上面的列上

