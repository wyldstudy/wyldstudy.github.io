---
title: java集合
date: 2016-12-05 20:56:26
tags: 
- java集合
- work
---

<img src="/img/java.jpg" height="300px" width="700px" style="margin:0px outo; ">
由于在平时经常用到java的集合类，所以我决定把这次的东西记录下来，顺便研读一下集合类的源码（虽然网上已经有很多了）
  <!--more-->

java的集合类可以说是在我的生活和工作中用的最多，也是最早看源码的集合类。其中很重要要的就是hashCode和equals，所以在我们使用集合类的很多属性时都要先重写hashCode和equals。但前几天在工作中发现，重写的hashCode有问题，但在使用ArrayList的contains时却没有报错。

所以可见在调用ArrayList的contains的方法时并没有调用hashCode，跟踪源码可见：
```
 /**
     * Returns <tt>true</tt> if this list contains the specified element.
     * More formally, returns <tt>true</tt> if and only if this list contains
     * at least one element <tt>e</tt> such that
     * <tt>(o==null&nbsp;?&nbsp;e==null&nbsp;:&nbsp;o.equals(e))</tt>.
     *
     * @param o element whose presence in this list is to be tested
     * @return <tt>true</tt> if this list contains the specified element
     */
    public boolean contains(Object o) {
        return indexOf(o) >= 0;
    }
     /**
     * Returns the index of the first occurrence of the specified element
     * in this list, or -1 if this list does not contain the element.
     * More formally, returns the lowest index <tt>i</tt> such that
     * <tt>(o==null&nbsp;?&nbsp;get(i)==null&nbsp;:&nbsp;o.equals(get(i)))</tt>,
     * or -1 if there is no such index.
     */
    public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = 0; i < size; i++)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }
```
所以，ArrayList的contains的方法时并没有调用hashCode。

但这并不是说写代码的时候可以只重写equals，在平时的编程中还是要养成良好的编码规范。在重写equals的时候同事重写hashCode。并且遵循Java语言规范要求equals方法具有以下特性：
&nbsp; &nbsp;&nbsp; &nbsp; 自反性：对于任何非空引用x，x.equals(x)返回true；
&nbsp; &nbsp;&nbsp; &nbsp; 对称性：对于任何引用x和y，如果y.equals(x)返回true，x.equals(y)也应该返回true；
&nbsp; &nbsp;&nbsp; &nbsp;传递性：对于任何引用x，y，和z，如果x.equals(y)返回true，y.equals(z)返回true，x.equals(z)也应该返回true。
&nbsp; &nbsp;&nbsp; &nbsp; 一致性：如果x和y引用对象没有发生变化，x.equals(y)应该返回同样结果。
&nbsp; &nbsp;&nbsp; &nbsp;任意非空引用x，x.equals(null)返回false。
在上述问题中，如果将list换成set则会有不同的结果。所以在平时使用非常频繁的东西，有时有些东西了解的也不是很透彻，还是好好梳理一下java的集合类。
<h3>java常用集合类关系</h3>

<img src="/img/java集合类.png" >

List集合：
有序可重复，线性，无固定大小。
LinkedList底层实现是链表，对于频繁的插入删除效率好一些。
ArrayList底层实现是数组，读取效率高。
Vector的底层实现和ArrayList一样，只是vector是线程同步的，所以它也是线程安全的，而arraylist是线程异步的，是不安全的。如果不考虑到线程的安全因素，一般用arraylist效率比较高。

Set集合：
无序不可重复，长度可变。
HashSet是不可排序的。
TreeSet可实现caparable接口，实现对对象的排序。

Map：
存放键值对，无序，Key值不可重复。
Hashtable是线程安全的，也就是说是同步的，而HashMap是线程序不安全的，不是同步的。

先记录到这里，没有什么很深的东西，但有时还是会有遗忘。
