---
title: Java中Comparable和Comparator比较
date: 2016-08-12 14:53:59
tags:
- Java
categories:
- 业余学习
---
总结Java中Comparable和Comparator两个接口

# Comparable接口
Comparable是排序接口，实现该接口的类支持“排序”，可以通过**Collections.sort**或者**Arrays类**来进行排序。<br>
## 接口定义
```java
public interface Comparable<T>{
  public int compareTo(T o);
}
```
* 返回负数， 表示小于<br>
* 返回零，表示等于<br>
* 返回正数， 表示大于<br>

# Comparator接口
**Comparator**是比较器接口<br>
我们可以实现**Comparator**类来新建一个比较器，然后通过比较器来进行对类排序。<br>
## 接口定义
```java
public interface Comparator<T> {
  int compare(T o1, T o2);
  boolean equals(Object obj);
}
```
若一个类实现了Comparator接口，则必须实现compareTo函数，但可以不实现equals函数，因为任何类默认都是已经实现了equals方法的。<br>
* 返回负数， 表示小于<br>
* 返回零，表示等于<br>
* 返回正数， 表示大于<br>

# 两者区别
1. Comparable是排序接口，是一个对象就已经支持自比较所需要实现的接口。若一个类实现了Comparable接口，就意味着“该类支持排序”。比如，String，Integer，在用Collections类的sort方法排序时若不指定Comparator，就以自然顺序排序。
2. Comparator是比较器，当这个对象不支持自比较或者自比较方法不能满足要求时使用。我们若需要控制某个类的次序，可以建立一个“该类的比较器”来进行排序。<br>

所以Comparable相当于“内部比较器”，而Comparator相当于“外部比较器”。
