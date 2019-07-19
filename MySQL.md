[TOC]

# MySQL

## 基础知识

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

  索引的作用：加快查找速度

​                         约束数据的值，如NUIQUE INDEX,PRIMARY KEY,FOREIGN KEY

#### 八，数据库索引的分类

 聚集性索引 ：Clustered_index,每个表至多有一个 ，存放的是记录，一般主键是聚集性的

非聚集性索引：指针，指向那条记录  通过指针找记录，速度慢

#### 九，数据库B树和B+树的区别

​         B树所有的节点存放值，B+树有用的值存放在叶子节点，根和非叶子节点都是存放的都是指针，通过指针

#### 十,B树和二叉树的区别，红黑树的区别

#### 十一,

