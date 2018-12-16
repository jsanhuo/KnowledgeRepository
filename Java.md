[TOC]



# Java

## Java基础

### 一，== 和equals的区别

https://blog.csdn.net/javazejian/article/details/51348320
对于基础类型，如int，double...只能采用== 进行比较，且比较的是数值
对于引用类型，可以采用和equals进行比较，比较的是两个引用的引用地址， 及两个引用引用的是否为一个对象，而equals则是Object方法进行提供的，如果不进行重写，其比较的也是引用
Object中equals

```java
public boolean equals(Object obj) {
        return (this == obj);
    }
```

其和==相同
当我们重写其以后，就可以按照我们的要求进行比较
和比较有关系的还有个方法叫做hashcode
equals和hashcode有以下的关系

1.equal()相等的两个对象他们的hashCode()肯定相等，也就是用equal()对比是 绝对可靠的。

2.hashCode()相等的两个对象他们的equal()不一定相等，也就是hashCode()不 是绝对可靠的。

因为hashcode可能存在碰撞



在使用HashMap的时候在重写equals方法的时候，一定要重写hashCode方 法。

有这个要求的症结在于，要考虑到类似HashMap、HashTable、HashSet的这 种散列的数据类型的运用。

### 二，String，StringBuffer，StringBuilder的区别



## 排序

https://www.cnblogs.com/onepixel/articles/7674659.html

### 一，快速排序

### 二，归并排序

### 三，冒泡排序

### 四，插入排序

### 五，选择排序

### 六，堆排序

### 七，希尔排序

### 八，桶排序

### 九，基数排序

### 十，计数排序

## 多线程

### 一，volatile关键字的底层实现，volatile是不是原子性的



## JVM

### 一，Java类为什么要采用双亲委派模型 

为了防止不可信的类扮演被信任的类：例如类java.lang.Object，它存放在 rt.jar中，无论哪个类加载器(Application,Extension)要加载这个类，最终都会 委派给启动类(BootStrap)加载器进行加载，因此Object类在程序的各种类加载 器环境中都是同一个类。相反，如果用户自己写了一个名为java.lang.Object的 类，并放在程序的Classpath中，那系统中将会出现多个不同的Object类，java 类型体系中最基础的行为也无法保证，应用程序也会变得一片混乱。

### 二，什么是双亲委派模型





## IO



## NIO





### 	