# HashMap底层实现和原理

[TOC]

### HashMap介绍

#### 概述

HashMap基于Map接口实现，以键值对方式存储，并且允许使用null，以为key是唯一的，因此只能有一个key为null，它是无序的。

#### 继承关系

```java
public class HashMap<K,V>extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable
```

#### 基本属性

```java
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; //默认初始化容量 16 
static final float DEFAULT_LOAD_FACTOR = 0.75f;     //负载因子0.75
static final Entry<?,?>[] EMPTY_TABLE = {};         //初始化的默认数组
int threshold;          //临界值（阀值）HashMap扩容判断的条件
```
### HashMap自身做的操作

#### 计算hash

##### **Jdk1.7**

```java
int hash = hash(key.hashCode());
int i = indexFor(hash, table.length); 
static int hash(int h) {      
        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
 } 
 static int indexFor(int h, int length) {
        return h & (length-1);
 }
```

这个方法返回值就是数组下标。我们平时用map大多数情况下map里面的数据不是很多。这里与（length-1）相&,**但由于绝大多数情况下length一般都小于2^16即小于65536。**所以return h & (length-1);结果始终是h的低16位与（length-1）进行&运算。

**当length=2的N次方， 下标运算结果取决于哈希值的低N位。**

##### **Jdk1.8**

```java
 static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
 }
```

##### **为什么重构代码**

由于和（length-1）运算，length 绝大多数情况小于2的16次方。所以始终是hashcode 的低16位（甚至更低）参与运算。要是高16位也参与运算，会让得到的下标更加散列。

所以这样高16位是用不到的，如何让高16也参与运算呢。所以才有hash(Object key)方法。让他的hashCode()和自己的高16位^运算。所以(h >>> 16)得到他的高16位与hashCode()进行^运算。

#### 初始容量与扩容

HashMap的初始容量为16，Hashtable初始容量为11，两者的填充因子默认都是0.75。

HashMap扩容时是当前容量翻倍即:capacity*2，Hashtable扩容时是容量翻倍+1即:capacity*2+1。

##### HashMap什么时候扩容，什么时候初始化容量

我们需要注意这三个名词：

- 负载因子loadFactor
- 临界值 threshold
- 容量 capacity
- DK并不会直接拿用户传进来的数字当做默认容量，而是会进行一番运算，最终得到一个2的幂

```java
    /**
     * 实际数组容量
     * 对于给定的目标容量，返回两倍大小的幂
     * 就是找到一个离目标容量 最接近的2的n次方的数字
     * 
     */
    static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
```

存放数据

```java

    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
    
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        
       //  第一次put元素会执行resize()
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
            
      //  这边也可以关注下 HashMap 是怎么存值的 n是tab的容量-1 hash是key的hash后的值  2个数值相与后 得到存放的位置
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
            
        else {
            Node<K,V> e; K k;
            //如果当前算出的位置的元素的hash值和key的hash值相同 
          	//如果Hash 相同 并且 数值相同 这里的判断 用的是== 或者equals方法  
            //相同  就直接替换
            if (p.hash == hash && ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
                
            //这边判断的 是如果P是一个红黑树 的处理   
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            //如果不是树结构  那就是链表结构  执行的逻辑是 循环链表 如果hash值和key数值相同 就替换链表中的元素  
            //如果匹配不到 就放入 链表的尾部节点  
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {// next=null 说明已经循环到了链表的尾部
                        p.next = newNode(hash, key, value, null);//负责 就加入的链表尾部
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;//匹配到相等的 就替换
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }

```

 关注下resize()方法

```java
 final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;//第一次插入的时候 oldTab 是null oldCap值就是0
        int oldThr = threshold;//临界值threshold 这个值 我们上面说过  是初始化的时候 给赋值的
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        
        //第一次的时候 应该进入这个判断里面  下面的因为注释也说了 这个是 initial capacity 的
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;//此时的newCap值 就是我们初始化的时候 临界值threshold
            
        else {               //使用默认容量
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        //newThr 没有被赋值 此时是0 进入下面的判断
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
      //下面的等式 就是得到新的临界值newThr   就是 我们的初始值算出的capacity*负载因子 
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;//赋值 新的临界值 
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];//初始化 Node数组
        table = newTab;        
        //后面有一段是HashMap 扩容的处理  我就不细说了 具体讲的是 怎么把oldTab 元素 放入新的tab 中
        return newTab;
    }

```

#### HashMap中初始容量的合理值

在阿里巴巴Java开发手册有以下建议

> **initialCapacity=（需要存储的元素个数/负载因子）+1。注意负载因子（即loaderfactor）默认为0.75，如果暂时无法确定初始值大小，请设置为16（即默认值）**

##### 总结：

1. HashMap 的容量计算是在第一次存放元素的时候执行的
2. HashMap的扩容条件就是当HashMap中的元素个数（size）超过临界值（threshold）时就会自动扩容
3. threshold 临界值（阀值）= loadFactor负载因子 * capacity容量

### **JDK1.8使用红黑树的改进** 

摘自[Java中HashMap底层实现原理(JDK1.8)源码分析](https://www.cnblogs.com/little-fly/p/7344285.html)

在[Java ]()jdk8中对HashMap的源码进行了优化，在jdk7中，HashMap处理“碰撞”的时候，都是采用链表来存储，当碰撞的结点很多时，查询时间是O（n）。
在jdk8中，**HashMap处理“碰撞”增加了红黑树这种数据结构，当碰撞结点较少时，采用链表存储，当较大时（>8个），采用红黑树（特点是查询时间是O（logn））存储（有一个阀值控制，大于阀值(8个)，将链表存储转换成红黑树存储）**

**问题分析：**

你可能还知道哈希碰撞会对hashMap的性能带来灾难性的影响。如果多个hashCode()的值落到同一个桶内的时候，这些值是存储到一个链表中的。最坏的情况下，所有的key都映射到同一个桶中，这样hashmap就退化成了一个链表——查找时间从O(1)到O(n)。

 

随着HashMap的大小的增长，get()方法的开销也越来越大。由于所有的记录都在同一个桶里的超长链表内，平均查询一条记录就需要遍历一半的列表。

**JDK1.8HashMap的红黑树是这样解决的**：

​     **如果某个桶中的记录过大的话（当前是TREEIFY_THRESHOLD = 8），HashMap会动态的使用一个专门的treemap实现来替换掉它。这样做的结果会更好，是O(logn)，而不是糟糕的O(n)。**

​    它是如何工作的？前面产生冲突的那些KEY对应的记录只是简单的追加到一个链表后面，这些记录只能通过遍历来进行查找。但是超过这个阈值后HashMap开始将列表升级成一个二叉树，**使用哈希值作为树的分支变量，如果两个哈希值不等，但指向同一个桶的话，较大的那个会插入到右子树里。如果哈希值相等，HashMap希望key值最好是实现了Comparable接口的，这样它可以按照顺序来进行插入**。这对HashMap的key来说并不是必须的，不过如果实现了当然最好。如果没有实现这个接口，在出现严重的哈希碰撞的时候，你就并别指望能获得性能提升了。

