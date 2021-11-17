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

String在进行修改后会生成新的对象，其内部char[]通过finalStringBuffer和StringBuilder可以通过方法进行字符串修改，并且不会生成新的对象

StringBuffer是线程安全的

StringBuilder线程不安全,适合在单线程下运行

String ,Stringbugffer,stringbuilder都适合进行字符串的拼接。String字符串拼接原理是在内部使用了stringbuilder对象 Stringbuffer是sychnonize的，适合多线程。

String，stringbuilder,Stringbuffer都可以进行字符串的拼接。效率Stringbulider<Stringbuffer<"+"

 因为Stringbuffer要处理线程同步问题，所以时间比较长，“+”是因为在拼接的时候，会new Stringbuilder()对象，在for循环里，每次都要new一个，所以，效率比较低下。

### 三，什么是序列化和反序列化

https://blog.csdn.net/qq_27093465/article/details/78544505
序列化：把对象转换为字节序列的过程

把堆内存的java对象数据，通过某种方式把对象存储到磁盘或者其他网络节点上，这个过程就称为序列化。（通俗来说，就是将树结构或对象转成二进制串的过程）

反序列化：把字节序列转化为对象的过程
使用情况：

1. 需要将对象持久化，存储在数据库或者文件中的时候
2. 需要用Socket套接字传输对象的时候

需要注意的是序列化的类必须实现Serializable接口，并且被transient修饰的 字段不参与序列化

### 四，Java的动态代理

JDK自带的动态代理只能代理接口，并且需要一个实现了InvocationHandler的处理类,然后通过**Proxy.newProxyInstance**来创建代理对象。（注意JDK自带的只能代理接口，如果需要代理类，需要使用第三方的代理如CGlib等）

1.创建接口

```java
public interface Subject {
    public String getName();
}

```

2.创建真实类

```java
public class RealSubject implements Subject {
    @Override
    public String getName() {
        return "RealSubject";
    }
}

```

3.创建处理类

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class Handler implements InvocationHandler {
    private RealSubject realSubject;

    public Handler(RealSubject realSubject) {
        this.realSubject = realSubject;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("before");
        Object result = null;
        try {
            result = method.invoke(realSubject, args);
        }catch (Exception e){
            System.out.println("Exception");
            throw e;
        }finally {
            System.out.println("after");
        }
        return result;
    }
}


```

4.创建代理对象

```java
Subject o = (Subject)
                Proxy.newProxyInstance(Main.class.getClassLoader(), 
                        new Class[]{Subject.class}, 
                        new Handler(new RealSubject()));
System.out.println(o.getName());
```

5.执行

before
after
RealSubject

可以看到，在真实类的执行前后，加入了代理处理类的逻辑，通过这种机制，可以实现AOP的切面编程。

### 五，抽象类和接口的区别

关于抽象类：

1）如果一个类中有抽象方法，那么这个类一定是抽象类，抽象类不能实例化。

2）抽象类中不一定都是抽象方法。

3）抽象类中的抽象方法必须在子类中被重写。

4）抽象类中成员变量：可以是变量，也可以是常量 也可以包含静态成员常量

5）抽象类中的构造方法：可以无参，可以有参（作用：实例化对象）

接口：

1）成员变量：是一个常量，不能被修改。并且默认修饰符为(public static final)

2)成员方法：都是抽象方法。

3）默认修饰符 ：public abstract

4)和子类的关系：实现关系

java只支持单继承，多接口

### 六，final

1)final是java中的修饰符，可以修饰类，方法，属性（属性是一个常量）

2）final修饰类，类不可以被继承

3）final修饰常量，常量不可以被修改

4final修饰方法，不可以被子类重写。

### 七，构造函数

1）子类不能继承父类的构造函数

2）子类可以再自己的构造函数里面使用spuer关键字来调用父类的含参构造函数，但这个调用语句必须是子类构造函数的第一个可执行语句

3）在创建子类对象时，若不含带参构造函数，将限制性父类的无参构造函数，然后再执行自己的无参构造函数。

### 八，集合

![1560260007538](../../%E5%AD%A6%E4%B9%A0%E8%B5%84%E6%96%99/KnowledgeRepository/assets/1560260007538.png)

Collection为集合层级的根接口。一个集合代表一组对象，这些对象即为它的元素。

java集合中，主要分为以下几个 Set  List Map Queue 等几大类。

Collection 主要为 Set List Queue 的父级接口。

Set接口有两个常用的实现类， HashSet 和TreeSet   List接口有ArrayList和 Vector两个实现类。

Map是一个将key映射到value的对象，一个Map不可能包含重复的key,每个key最多只能映射一个value。

#### 1) 各集合的特点

​     Set集合，无序的，不允许重复。

​     List集合，有序，可以允许重复，可以通过索引来访问，List更像长度动态变幻的数组。

#### 2）为何Collection不从Cloneable和Serializable接口继承？

​    collection接口指定一组对象，对象即为它的元素。如何维护这些元素由Collection的具体实现决定。例如，一些如List的Collection实现允许重复的元素，而其它的如Set就不允许。很多Collection实现有一个公有的clone方法。然而，把它放到集合的所有实现中也是没有意义的。这是因为Collection是一个抽象表现。重要的是实现。

当与具体实现打交道的时候，克隆或序列化的语义和含义才发挥作用。所以，具体实现应该决定如何对它进行克隆或序列化，或它是否可以被克隆或序列化。

在所有的实现中授权克隆和序列化，最终导致更少的灵活性和更多的限制。特定的实现应该决定它是否可以被克隆和序列化。

#### 3)ArrayList和LinkedList有何区别？

​      1、ArrayList可以动态增长和减缩的索引序列，它是基于数组实现的List类。数据元素类型为Object类，即可以存放所有类型。它提供对元素的索引访问，复杂度为O(1)。但LinkedList存储一系列的节点数据，每个节点都与前  一个和下一个节点相连接。所以，尽管有使用索引获取元素的方法，内部实现是从起始点开始遍历，遍历到索引的节点然后返回元素，时间复杂度为O(n)，比ArrayList要慢。 ArrayList是线程不安全的，当多条线程访问同一个ArrayList集合时，程序需要手动保证该集合的同步性。都可以存放空值，都是线程不安全。

​    2、与ArrayList相比，在LinkedList中插入、添加和删除一个元素会更快，因为在一个元素被插入到中间的时候，不会涉及改变数组的大小，或更新索引。

​    3、LinkedList比ArrayList消耗更多的内存，因为LinkedList中的每个节点存储了前后节点的引用。

​          1）arrayList可以存放null。
​         2）arrayList本质上就是一个elementData数组。
​         3）arrayList区别于数组的地方在于能够自动扩展大小，其中关键的方法就是gorw()方法。
​         4）arrayList中removeAll(collection c)和clear()的区别就是removeAll可以删除批量指定的元素，而clear是        全是删除集合中的元素。
​        5）arrayList由于本质是数组，所以它在数据的查询方面会很快，而在插入删除这些方面，性能下降很多，有   移动很多数据才能达到应有的效果
​       6）arrayList实现了RandomAccess，所以在遍历它的时候推荐使用for循环。

#### 4)HashMap是如何工作的？ 

​      1)hashMap是一个散列桶，数组+链表的形式。 （非线程安全）

​      2）它具备了数组线性查找和链表的寻址修改

  HashMap是基于hashing的原理，使用put(key,value)的形式，将对象存储到HashMAp中，取的时候，使用get(key)取出对象。当我们给Put()方法传递键值的时候，我们先对键进行计算，让它返回、hashcode是用于找到Map数组的对象bucket位置来存储Node对象，其实就是Entry对象。Hashmap是在bucket中存储键值独享，作为Map.Node

​      put过程 

​     1,对key求Hash值，然后计算下标。

​     2，如果没有碰撞，直接放入到桶中，（碰撞的意思是，计算得到的hash相同，需要放到同一个bucklet中）

​    3，如果碰撞了，以链表的方式连接到后面。

​    4，如果链表的长度超过过阈值（8）就把链表转成红黑树，链表长度低于6，就把红黑树转回链表。

   5，如果节点已经存在，就替换旧值。

   6，如果桶装满了，就需要resize(),扩容两倍后，重新排列。

 get过程  

​    通过计算算出hash值，找到数组下标索引，然后在便利链表。

#### 5）hashMap和hashTable的区别

​          hashMap允许空键值(null ,key),非线程安全，HashTable不允许空的key value,hashTable是线程安全的。但是两者采用的hash算法都是一样的，所以性能不会有很大的差别。

### 九，什么是装箱，什么是拆箱

​    装箱就是 自动将基本数据类型转换为包装器类型；拆箱就是 自动将包装器类型转换为基本数据类型。在装箱的时候自动调用的是Integer的valueOf(int)方法。而在拆箱的时候自动调用的是Integer的intValue方法。

   

### Comparable接口

```
int compareTo(Object o)
```

a.compareTo(b)

如果a等于b，则返回值 0；

如果a小于b，则返回一个小于 0 的值；

如果a大于b，则返回一个大于 0 的值。

等同于 a-b 

## 排序



![1545921563405](../../%E5%AD%A6%E4%B9%A0%E8%B5%84%E6%96%99/KnowledgeRepository/assets/1545921563405.png)

https://www.cnblogs.com/onepixel/articles/7674659.html

### 一，快速排序

递归代码

```java
/**
 * 快速排序主函数
 * @param array
 */
public static void quickSort(int[] array)
{
    //第一次调用快排
    Quick(array, 0, array.length-1);
}


/**
 * 返回基准下标,相当于每次为一个元素找到其位置，将数组分为两部分
 * @param array 数组
 * @param start 开始下标
 * @param end   结束下标
 * @return      基准下标
 */
public static int partion(int[] array,int low,int hight)
{   
    //找到一个基准
    int temp = array[low];  
    //遍历直到所有遍历完
    while(low<hight)
    {   
        //从右向左找比temp小的
        while(low<hight&&array[hight]>=temp)
        {
            --hight;
        }
        //如果相遇了，或交错了，直接跳出
        if(low>=hight)
        {
            break;
        }
        //如果符合条件，将小的放到当前low的位置。
        else
        {
            array[low] = array[hight];
        }
        //从左向右找比temp大的
        while(low<hight&&array[low]<=temp)
        {
            ++low;
        }
        //如果遍历完了，跳出
        if(low>=hight)
        {
            break;
        }
        //如果符合条件，将大的放到当前hight位置
        else
        {
            array[hight] = array[low];
        }
    }
    //将基准放入low
    array[low] = temp;
    //返回low
    return low;
}



/**
 * 快速排序递归
 * @param array 数组
 * @param start 开始下标
 * @param end   结束下标
 */
public static void Quick(int[] array,int start,int end)
{   //先找到基准下标
    int par = partion(array, start, end);
    //判断基准的左边是不是还有1个以上元素，如果是再调用快排
    if(par>start+1)
    {
        Quick(array, start, par-1);
    }
    //判断基准的右边是不是还有1个以上元素，如果是再调用快排
    if(par<end-1)
    {
        Quick(array, par+1, end);
    }
}
```

非递归

```java
/**
 * 快速排序非递归
 * @param array 目标数组
 */
public static void quickSort1(int[] array)
{
    LinkStack a = new LinkStack();
    int low = 0;
    int hight = array.length-1;
    int par = partion(array, low, hight);
    if(par>low+1)
    {
        a.push(low);
        a.push(par-1);
    }
    if(par<hight-1)
    {
        a.push(par+1);
        a.push(hight);
    }
    //出栈
    while(!a.isEmpty())
    {
        hight = a.pop();
        low = a.pop();
        par = partion(array, low, hight);
        if(par>low+1)
        {
            a.push(low);
            a.push(par-1);
        }
        if(par<hight-1)
        {
            a.push(par+1);
            a.push(hight);
        }
    }
}


/**
 * 返回基准下标,相当于每次为一个元素找到其位置，将数组分为两部分
 * @param array 数组
 * @param start 开始下标
 * @param end   结束下标
 * @return      基准下标
 */
public static int partion(int[] array,int low,int hight)
{   
    //找到一个基准
    int temp = array[low];  
    //遍历直到所有遍历完
    while(low<hight)
    {   
        //从右向左找比temp小的
        while(low<hight&&array[hight]>=temp)
        {
            --hight;
        }
        //如果相遇了，或交错了，直接跳出
        if(low>=hight)
        {
            break;
        }
        //如果符合条件，将小的放到当前low的位置。
        else
        {
            array[low] = array[hight];
        }
        //从左向右找比temp大的
        while(low<hight&&array[low]<=temp)
        {
            ++low;
        }
        //如果遍历完了，跳出
        if(low>=hight)
        {
            break;
        }
        //如果符合条件，将大的放到当前hight位置
        else
        {
            array[hight] = array[low];
        }
    }
    //将基准放入low
    array[low] = temp;
    //返回low
    return low;
}
```



### 二，归并排序

```java
public static void mergeSort(int a[],int left,int right){
        if(left<right) {
            int mid = (left + right) / 2;

            mergeSort(a, left, mid);  //将原来的数组划分为两个数组
            mergeSort(a,mid+1,right);
            merge(a,left,mid,right); //将划分排序好的两个数组合并成原来的数组

        }
    }public static void merge(int[] arr, int L, int mid, int R) {
        int[] temp = new int[R - L + 1];
       // System.out.println(" 长度： "+temp.length);
        int i = 0;
        int p1 = L;
        int p2 = mid + 1;
        // 比较左右两部分的元素，哪个小，把那个元素填入temp中
        while(p1 <= mid && p2 <= R) {
            temp[i++] = arr[p1] < arr[p2] ? arr[p1++] : arr[p2++];
        }
        // 上面的循环退出后，把剩余的元素依次填入到temp中
        // 以下两个while只有一个会执行
        while(p1 <= mid) {
            temp[i++] = arr[p1++];
        }
        while(p2 <= R) {
            temp[i++] = arr[p2++];
        }
        // 把最终的排序的结果复制给原数组
        for(i = 0; i < temp.length; i++) {
            arr[L + i] = temp[i];
        }
    }
    public static void printarr(int a[]){
        for (int i = 0; i <a.length;i++) {
            System.out.print(a[i]+" ");

        }
    }
    
```



### 三，冒泡排序

冒泡排序：是一种简单的排序算法，它重复的走访过要排序的序列，一次比较两个元素，如果他们的顺序错误。则把他们的交换过来，重复的进行，直到在没有需要进行交换，也就是说该数列已经排序完成。

` `    

```java
public static int[] bubbleSort(int[] arr) {
    for (int i = 0; i <arr.length-1;i++){
        for(int j=0;j<arr.length-1-i;j++){
            if(arr[j]>arr[j+1]){
                int temp = arr[j];
                arr[j]=arr[j+1];
                arr[j+1]=temp;

            }

        }

    }
    return  arr;

优化：加标志，如果某趟不用比较，则退出
public static int[] bubbleSort(int[] arr){
        boolean flag =false;
        for (int i = 0; i <arr.length-1;i++){
            for(int j=0;j<arr.length-1-i;j++){
                if(arr[j]>arr[j+1]){
                    int temp = arr[j];
                    arr[j]=arr[j+1];
                    arr[j+1]=temp;
                    flag=true;

                }

            }
            if(flag=false){
                break;
            }

        }
        return  arr;

    }
    
```

时间复杂度：情况坏的时候O（n2）,情况好的时候如这样{2,3,4,5,6,10,9}只需要一次排序，其他几次都不需要，此时，加标志为最好的情况O（n）  稳定性：稳定

### 四，插入排序

插入排序：把n个待排序的元素看成一个有序表和一个无序表。开始时有序表中只含有一个元素，无序表中有n-1个元素。每次讲无序表中取出一个元素，将它插入到有序表中的适当位置，使之成为新的有序序列，重复n-1次可以完成排序序列，每次排序完后，都是部分有序。

`

```java
public static void insertsort(int array[]) {
    int length = array.length;

    for (int i = 1; i < length; i++) {   //外层循环控制的是待排序的序列
        int temp = array[i];
        int j = i;
        // 依次从后向前进行比较，如果前面的元素比他大，则前面的元素向后移动，直到比它小在替换
        for (;j > 0 && array[j - 1] > temp; j--) { //
            array[j] = array[j - 1];
        }

        array[j] = temp;
    }
}
public
```

在处理近乎有序的数据的时候，性能极好，在处理完全有序的列表的时候，时间复杂度为O(n)，因为在有序的数组中，只会运行外部循环，内部循环会直接break掉。



### 五，选择排序（×）

### 六，堆排序（×）

堆排序(Heapsort)是指利用堆积树（堆）这种数据结构所设计的一种排序算法，它是选择排序的一种。可以利用数组的特点快速定位指定索引的元素。堆分为大根堆和小根堆，是完全二叉树。大根堆的要求是每个节点的值都不大于其父节点的值，即A[PARENT[i]] >= A[i]。在数组的非降序排序中，需要使用的就是大根堆，因为根据大根堆的要求可知，最大的值一定在堆顶。



### 七，希尔排序（×）

### 八，桶排序（×）

### 九，基数排序（×）

### 十，计数排序（×）

## 多线程

### 一，线程池是什么？

线程池是一种多线程的处理形式，处理过程中将任务添加到队列，然后在创建线程后自动启动这些任务。线程池线程都是后台线程。每个线程都使用默认的堆栈大小，以默认的优先级运行。也可以传入自己的线程工厂。

### 二，run()和start()的区别

run只是调用了普通方法，其内部的是线程的线程体

start启动的是线程

启动一个线程是调用start方法，使得线程处于就绪状态，在获取cpu时间片后，就可以直接执行了。

###   1）线程的5种基本状态

​         创建   就绪  运行  终止   阻塞

​         创建状态：在程序中用new创建了一个Thread类或子类实例化后，新的线程对象处于创建状态，此时，它已经有了相应的内存空间，但还未对线程分配任何资源所以还处于不可运行状态。创建一个线程对象，可采用线程构造的方法来实现。Thread  thread = new Thread();

​          就绪状态：线程创建后，若要执行它，就要为他分配资源。调用线程的start()方法就可以完成此项工作。当线程启动时，线程就进入就绪状态。此时，线程进入就绪队列排队，等待cpu服务，这表明它已经具备运行条件了，一旦她获得cpu等资源时，就可以脱离创建它的主线程来运行。

​    运行状态：就绪状态线程被调用并获得cpu等资源，便进入了运行状态。此时自动调用线程对象的run()方法。

​          阻塞状态：一个线程在某些特殊的环境下，被人们人为挂起或需要执行耗时的输入输出操作时，将让出cpu并暂时停止自己的执行，以后还可以恢复运行的状态称为阻塞状态。在可执行的状态下，如果发生以下几种情况的一种，说明线程进入了阻塞状态：

​         调用了线程的sleep()方法。

​         调用了线程的wait()方法。 

​         调用了现成的suspend()休眠方法。

​         该线程正在等待i/o六操作来完成。

阻塞时，线程不能进入排队队列，只有当引起阻塞的原因被消除后，线程才可以转入就绪状态。

终止状态：

​      如果一个线程在可执行的状态下，运行、run()方法完成后或调用stop()或、destory()方法，就说明线程进入了终止状态（死亡状态），处于这种状态的线程不具有继续运行的能力。   

sleep()是线程类的一个方法，导致此线程暂停执行指定的时间，将执行的机会给其他线程，但是监控状态依然保持，到时候会自动恢复。调用sleep不会释放锁。

wait()是Object类的方法，此对象调用wait方法导致本线程放弃对象锁，进入等待此对象的线程池。只有针对此对象发出notify方法，本线程才会进入对象锁定池准备获得独享锁进入运行状态。

### 2）创建线程的2中方式

   分别是继承Thread类，与实现Runnable接口。

  实现Runnable接口除了拥有和继承Thread类一样的功能外，还具有以下功能。

​    适合多个相同程序代码的线程处理同一资源的情况，可以把线程同程序中的数据有效分离，较好的体现了面向   对象的思想。适合多个线程共享代码

   增强了代码的鲁棒性，代码能够被多个线程共同访问，代码与数据是独立的。多个线程可以操作相同的数据，与     他们的代码无关。·

### 3）进程和线程的区别

   进程是系统进行资源分配和处理机调度的基本单位。

  线程是CPU调度和分派的基本单位。（线程是在程序执行过程中，程序中的一条执行路径。）

   同样作为基本单元，线程是比进程更小的执行单位。‘

   每个进程都有一段专用的内存区域。一个进程崩溃后，在保护模式下，不会对其他进程产生影响，线程是进城内部单一的一个顺序控制流。与此相反，线程之间没有单独的地址空间，一个线程死掉，就等于一个进程死掉。线程共享内存单元（包括代码和数据),通过共享的内存单元实现数据交换，实时通讯和必要的操作。

###   4）线程同步

​       由于同一进程的多个线程共享一片存储空间，在带来方便的同时，也带来了访问冲突这个问题。java中使用同步方法和同步代码块来实现资源共享。  

### 三，线程池中Submit和execute的区别

```java
	/**
     * @throws RejectedExecutionException {@inheritDoc}
     * @throws NullPointerException       {@inheritDoc}
     */
    public Future<?> submit(Runnable task) {
        if (task == null) throw new NullPointerException();
        RunnableFuture<Void> ftask = newTaskFor(task, null);
        execute(ftask);
        return ftask;
    }

    /**
     * @throws RejectedExecutionException {@inheritDoc}
     * @throws NullPointerException       {@inheritDoc}
     */
    public <T> Future<T> submit(Runnable task, T result) {
        if (task == null) throw new NullPointerException();
        RunnableFuture<T> ftask = newTaskFor(task, result);
        execute(ftask);
        return ftask;
    }

    /**
     * @throws RejectedExecutionException {@inheritDoc}
     * @throws NullPointerException       {@inheritDoc}
     */
    public <T> Future<T> submit(Callable<T> task) {
        if (task == null) throw new NullPointerException();
        RunnableFuture<T> ftask = newTaskFor(task);
        execute(ftask);
        return ftask;
    }
```

可以看出，submit直接将执行体丢给了execute执行

所以当你不需要一个结果，那么就老老实实使用execute，如果你需要的是一个空结果，那么 submit(yourRunnable) 与 submit(yourRunnable,null) 是等价的

### 四，多线程使用的场景

对于单核CPU，如果是CPU密集型的任务，如解压文件，多线程的性能反而不如单线程性能，因为解压文件需要一直占用CPU资源，如果采用多线程，线程切换导致的开销反而会使得性能下降，但是对于比如交互类型的任务，肯定需要使用多线程。

对于多核CPU，对于解压文件来说，多线程肯定优于单线程，因为多个线程能够更加充分利用每个核的资源，虽然多线程能够提高程序性能，但是相对于单线程来说，它的编程要复杂的多，要考虑线程安全问题，因此在实际编程过程中，要根据实际情况具体选择。

### 五，基本线程池化模式

1.从池的空闲线程列表中选择一个 Thread，并且指派它去运行一个已提交的任务（一个 Runnable的实现） 

2.当任务完成时，将该Thread返回给该列表，使其可被重用

### 六，线程池的好处

1. 创建和销毁线程会有系统的开销，频繁的进行操作会大大影响系统的处理效率，线程池可以使得线程重复利用，减少创建和销毁的系统消耗。
2. 可以根据系统的承受能力，调整线程池中工作线程的数量，防止过多线程造成的宕机。

### 七，常见的几种线程池（×）



### 八，线程间通信的几种方式

1. wait(),notify(),notifyall()
2. 通过synchronized 进行同步
3. while轮询
4. 管道通信





## JVM

### 一，Java类为什么要采用双亲委派模型 

为了防止不可信的类扮演被信任的类：例如类java.lang.Object，它存放在 rt.jar中，无论哪个类加载器(Application,Extension)要加载这个类，最终都会 委派给启动类(BootStrap)加载器进行加载，因此Object类在程序的各种类加载 器环境中都是同一个类。相反，如果用户自己写了一个名为java.lang.Object的 类，并放在程序的Classpath中，那系统中将会出现多个不同的Object类，java 类型体系中最基础的行为也无法保证，应用程序也会变得一片混乱。

### 二，什么是双亲委派模型

如果一个类加载器收到了加载某个类的请求,则该类加载器并不会去加载该类,而是把这个请求委派给父类加载器,每一个层次的类加载器都是如此,因此所有的类加载请求最终都会传送到顶端的启动类加载器;只有当父类加载器在其搜索范围内无法找到所需的类,并将该结果反馈给子类加载器,子类加载器会尝试去自己加载.

### 三，如何实现自定义类加载器

继承**ClassLoader**然后重写其方法

如果想打破双亲委派模型 重写findClass，否则重写loadClass

### 四，Java垃圾回收如何确认垃圾

引用计数（Java不采用），原因：内存开销，引用环

可达性分析：从一定有用的东西（暂停虚拟机：局部变量，静态变量，Native所引用的对象，活动线程）出发，看能否访问

### 五，Java的垃圾回收算法

基础假设：大部分对象只存在很短的时间

将内存分为新生代，老生代

新生代分为Eden，Survivor1，Survivor2

新生代的GC叫 MinorGC

每次清理

Eden+Survivor1 ->Survivor2

Eden和Survivor1清理

下一次

Eden+Survivor2->Survivor1

Eden和Survivor2清理

当Survivor区存不下了或者超过年龄阈值（JVM默认15）就将年龄大的存放到老生代

当老生代满了则进行Major/Full GC

老生代采用Compact算法

### 六，Java虚拟机的配置

-XX:NewRatio  老生代/新生代比，默认2

-XX:SurvivorRatio Eden/Survivor比例，默认8

-XX:MaxTenuringThreshold 新生代转至老生代阈值，默认15

### 七，内存持久代中包含什么

1. ClassLoader读进来的Class，除系统CLass外
2. 放置String.intern后的结果

易出现OutOfMemoryError:PermGen Space

### 八，解决OutOfMemoryError:PermGen Space

-XX:MaxPermSize 调整大小

### 九，Java1.8Metaspace和PermGen的区别

1. Java1.8使用Metaspace，取消了PermGen Space
2. String.intern的结果被放入堆
3. Metaspace默认不设限制，使用系统内存

### 十，谈谈Java垃圾回收机制

1. 垃圾回收在什么时候运行

   内存分配失败，还有调用`System.gc()`的时候

2. 垃圾回收对什么对象进行回收

   根据可达性分析来查找不用的对象

3. 垃圾回收算法对内存划分了那些区域

   新生代（Eden，Survivor1，Survivor2   copy算法），老生代（compact算法），持久代（Class）

### 十一，对happens-after的理解

- 操作执行顺序具有先后性
- 后执行的操作能够看到先执行操作（在内存）的结果

规定以下操作必须保证happens-after

1. UnLock发生在Lock之前

2. 写volatile发生在读volatile之前

3. 线程start发生在线程所有动作之前

4. 线程中所有操作发生在join之前

5. 构造函数发生在finalizer开始之前

6. 传递性：happens-after关系满足传递性

   ### 十二、java异常处理

   #### checked Exception 和 Uncheckd Exception

   RuntimeException极其子类：Unckecked

   IllegalArgumentException ,ClassCastException,IllegalStateException,NullPointException,....

   其他Exception :Checked

   IoException ,InterrupteException,PraseException,.....

   #### 如何处理异常

     写日志

     执行相关逻辑

   ####  如何不处理Checked Exception

      如果能，添加throws定义

      否则，将Checked Exception转换成 Unchecked Exception，再抛出

      转换时，必须带上Case;

   ​    throw  new  RuntimeException(checkedException);

## 并发

### 一，volatile关键字的底层实现，volatile是不是原子性的

底层保证缓存的一致性，在汇编代码之前加了lock指令，保证了在其他线程修改此变量之后，其他线程是可见的

不是原子性，它保证了顺序性和可见性。对任意单个volatile变量的读/写具有原子性，但类似于volatile++这种复合操作不具有原子性。

### 二，volatile与synchronized的区别（×）





### 三，Synchronized的作用

能够保证在同一时刻只有一个线程执行该代码，以达到保证并发安全的效果。

### 四，Synchronized的两个用法

- 对象锁：包括方法锁（默认锁对象为this当前实例对象）和同步代码块锁（自己制定锁对象）
- 类锁：修饰静态方法或指定锁为Class对象

### 五，多线程访问同步方法的7种情况

1. 两个线程同时访问一个对象的同步方法

   一次只能执行一个

2. 两个线程访问的是两个对象的同步方法

   相互不影响，因为锁定的是不同的实例

3. 两个线程访问的是synchronized的静态方法

   一个一个的执行，因为Synchronized的静态方法获取的是类锁

4. 同时访问同步方法和非同步方法

   同时执行

5. 访问同一个对象的不同的普通同步方法

   同一个实例来讲，逐个执行，因为获取的是当前实例的对象锁

6. 同时访问静态Synchronized和非静态Synchronized

   同时运行，因为一个加的是对象锁，一个是类锁

7. 方法抛出异常后会释放锁

   Lock中不会释放，synchronized会自动释放

总结：

- 一把锁只能同时被一个线程获取，没有拿到锁的必须等待
- 每个实例都对应有一个锁，不同实例直接不影响，锁对象为.class或者synchronized修饰的static方法时，所有对象共用一个类锁
- 无论方法正常执行或者抛出异常，都会释放锁

### 六，synchronized的性质

- 可重入：同一线程的外层函数获取锁以后，内层函数可以直接再次获取该锁。

  好处：避免死锁，提升封装性，原理在于：**加锁次数计数器**

- 不可中断：如果锁被别人获取，只能选择等待或者阻塞

### 七，synchronized使用的注意点

1. 锁对象不能为空
2. 作用域不能过大
3. 避免死锁
4. 在处理多把锁的时候，要注意加锁和解锁顺序要一样

### 八，死锁的必要条件

- 互斥条件：当一个资源被占用的时候，其他的线程不能使用
- 请求与保持：一个进程因请求资源被阻塞的时候，其原来就拥有的资源不进行释放
- 不剥夺：进程已经获得的资源，在未使用之前，不能强行剥夺
- 循环等待：若干进程之间形成了头尾相接的循环等待资源关系

## IO

## NIO

### 一，JavaNIO的工作原理

1. 通过Selector来进行事件的分发
2. 事件到的时候触发，而不是监视事件

### 二，NIO的核心

1. Selector：通过调用select方法可以从所有Channel中找到需要服务的实例
2. Channel：代表一个通道可以进行读和写
3. Buffer：提供读写数据的缓存

### 三，NIO和IO相比的好处

1.使用较少的线程便可以处理许多连接，因此也减少了内存管理和上下文切换所造成的开销

2.当没有IO操作需要处理的时候，线程也可以用于其他任务。

## 设计模式

### 1.工厂模式

1. 简单工厂 =》 Factory  getProduct（参数）  开闭原则

2. 工厂方法 =》 可以改善上面简单工厂的缺点

3. 抽象工厂 =》  把具有相关联的产品放在一个工厂中创建

   

   SpringContext上下文加载起来  context.getBean

### 2.单例模式

TomcatServlet

SpringBean

类加载器

### 3.代理模式

Proxy（） 静态代理，动态代理MyBatis   Spring AOP





### 4.Java IO所用的设计模式

装饰器：DataInputStream  DataOutPutStream

适配器 InputStreamReader，OutputStreamWriter