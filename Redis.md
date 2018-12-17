[TOC]

# Redis

https://www.nowcoder.com/discuss/92610?type=2&order=0&pos=29&page=1

## 基础

### 一，基础数据类型

基础数据类型有5个

- String：字符串 最常规的get/set操作 value为数字的时候还可以进行计数
- List：列表 底层为链表，可以用来当做消息队列和基于redis的分页
- Set：集合 不重复的集合
- Hash：哈希表 类似Java里面的Map 可以存放结构化的对象，可以模拟session
- Sorted set：有序集合 和set的区别是有权值 

### 二，为什么要使用Redis？

1. 一是为了性能，在碰到执行耗时长，且SQL变动不频繁的时候，就适合将结果放入缓存，如果后面有相同的请求，可以直接在缓存中读取。
2. 在高并发的时候，所有请求直接访问数据库，数据库会出现连接异常。此时可以用redis来作为一个缓冲，就如同CPU中的cache和内存的关系一样

