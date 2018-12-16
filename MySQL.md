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

MySQL默认为==REPEATABLE-READ==