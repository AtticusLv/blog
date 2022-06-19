---
title: Java需要掌握的基本知识
date: 2017-01-17 10:15:47
tags:
- Java
- backend
- Spring
categories:
- 技术资料
---
# Java需要掌握的基本知识总结
## Java基础
### 什么是多态？
### final、finally和finalize区别？
### GC的基本原理？ 老年代和新生代一些概念？什么时候会回收？
### abstract和interface区别？
### Java只能单继承，不能多重继承？
可以创造一个描述这类行为的接口
### Overload和Override区别
### ArrayList和Vector区别
### HashMap与Hashtable区别
#### HashMap
HashMap是基于哈希表的Map接口实现，允许使用null值和null键，它不保证映射的顺序，不是同步的，如果多个线程同时访问一个HashMap，而其中至少一个线程从结构上（添加或者删除一个或多个映射关系的操作）修改了，则必须保持外部同步，以防止对映射进行恶意的非同步访问。
HashMap实际上是数组和链表结合的数据结构
{% asset_img hashmap1.jpg hashmap %}
HashMap底层是一个数组结构，数组中每一项又是一个链表，当新建一个HashMap时，就会初始化一个数组。JDK中HashMap源码：
```java
public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);

        // Find a power of 2 >= initialCapacity
        int capacity = 1;
        while (capacity < initialCapacity)
            capacity <<= 1;

        this.loadFactor = loadFactor;
        threshold = (int)Math.min(capacity * loadFactor, MAXIMUM_CAPACITY + 1);
        table = new Entry[capacity];
        useAltHashing = sun.misc.VM.isBooted() &&
                (capacity >= Holder.ALTERNATIVE_HASHING_THRESHOLD);
        init();
}
```
可以注意到`table = new Entry[capacity];`在构造函数中，它创建了一个Entry数组，其大小为capacity，我们再来看下Entry的源码：
```java
static class Entry<K,V> implements Map.Entry<K,V> {
    final K key;
    V value;
    Entry<K,V> next;
    final int hash;
    ……
}
```
`Entry`是一个static类，其中包含了`key`和`value`键值对，另外还包含了一个next的Entry指针。Entry 就是数组中的元素，每个 Entry 其实就是一个 key-value 对，它持有一个指向下一个元素的引用，这就构成了链表。

简单总结，HashMap在底层将`key-value`对当成一个整理进行处理，这个整体就是一个 Entry 对象。HashMap 底层采用一个 Entry[] 数组来保存所有的 key-value 对，当需要存储一个 Entry 对象时，会根据 hash 算法来决定其在数组中的存储位置，在根据 equals 方法决定其在该数组位置上的链表中的存储位置；当需要取出一个Entry 时，也会根据 hash 算法找到其在数组中的存储位置，再根据 equals 方法从该位置上的链表中取出该Entry。

**HashMap的resize**

当HashMap中的元素越来越多时
#### Hashtable

### 集合类接口有哪些?具体实现有哪些类？
总共有两大接口：Collection和Map。一个是元素集合，一个是键值对集合。
[I] - interface
[C] - class
```
Collection
├── List[I]
|   ├── ArrayList[C]
|   ├── LinkedList[C]
|   ├── Vector[C]
|   ├── Stack[C]
├── Set[I]
|   ├── HashSet[C]
|   ├── SortedSet[I]
|   ├── TreeSet[C]
```
```
Map
├── SortedMap[I]
├── TreeMap[C]
├── Hashtable[C]
├── HashMap[C]
├── LinkedHashMap[C]
├── WeakHashMap[C]
```
### TreeMap如何实现Key排序
TreeMap是一个有序的key-value集合，它是通过红黑树实现的，该映射根据其键的自然顺序进行排序，或者根据创建映射时提供的Comparator进行排序，具体取决于使用的构造方法。它继承于AbstractMap，所以它是一个key-value集合。
大致结构是这样的：
{% asset_img tree-map.jpg tree-map %}

TreeMap如果不指定排序器，默认将按照key值进行升序排序，如果指定了排序器，则按照指定的排序器进行排序，如果指定了排序器，则按照指定的排序器来进行排序；具体排序规则可以通过在`int compare()`方法中进行指定

### Java里有哪些并发库

### ConcurrentHashMap是什么？
ConcurrentHashMap是util.cconcurrent包中一员，由Segment数组结构和HashEntry数组结构组成。Segment是一种可重入锁ReentrantLock，在ConcurrentHashMap里扮演锁的角色，HashEntry则用于存储键值对数据
#### HashEntry类
```java
static final class HashEntry<K,V> {
       final K key;                       // 声明 key 为 final 型
       final int hash;                   // 声明 hash 值为 final 型
       volatile V value;                 // 声明 value 为 volatile 型
       final HashEntry<K,V> next;      // 声明 next 为 final 型

       HashEntry(K key, int hash, HashEntry<K,V> next, V value) {
           this.key = key;
           this.hash = hash;
           this.next = next;
           this.value = value;
       }
}
```
## Spring基础
### 什么是Spring AOP？

### Spring AOP实现机制
### 什么是Spring IOC/控制反转
### Hibernate，iBatis和JPA？
### Spring依赖注入
平常的java开发中，程序员在某个类中需要依赖其它类的方法，则通常是new一个依赖类再调用类实例的方法，这种开发存在的问题是new的类实例不好统一管理，spring提出了依赖注入的思想，即依赖类不由程序员实例化，而是通过spring容器帮我们new指定实例并且将实例注入到需要该对象的类中。依赖注入的另一种说法是“控制反转”，通俗的理解是：平常我们new一个实例，这个实例的控制权是我们程序员，而控制反转是指new实例工作不由我们程序员来做而是交给spring容器来做。

## 数据库
### mySQL索引结构原理
索引是对记录按照多个字段进行排序的一种方式。对表中某个字段建立索引会创建另一种数据结构，其中保存着字段的值，每个值又指向与它相关的记录。这种索引的数据结构是经过排序的，因而可以对其执行二分查找。
索引的缺点是占用额外的磁盘空间，因为索引保存在MyISAM数据库中，所以如果为同一表中的很多字段都建立索引，那这个文件可能会很快膨胀到文件系统规定的上限。
**唯一索引**
索引值必须唯一
创建索引： create unique index 索引名 on 表名(列名); alter table 表名 add unique index 索引名 (列名);
删除索引： drop index 索引名 on 表名; alter table 表名 drop index 索引名;
**主键**
主键是唯一索引的一种，主键要求建表时指定，一般用auto_increment列，关键字是primary key
主键创建：creat table test2 (id int not null primary key auto_increment);
**全文索引**
InnoDB不支持，MyISAM支持性能比较好，一般在 CHAR、VARCHAR 或 TEXT 列上创建。 Create table 表名( id int not null primary key anto_increment, title varchar(100),FULLTEXT(title) )type=MyISAM;
**单列索引与多列索引**

# 参考
