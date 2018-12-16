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

