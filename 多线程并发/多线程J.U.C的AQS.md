[TOC]

# J.U.C的AQS

## 1.JUC的由来

[synchronized](https://mp.weixin.qq.com/s?__biz=MzI4NjI1OTI4Nw==&mid=2247488320&idx=1&sn=6fd5cddf2a0ff68fe00ccc834e90521b&scene=21#wechat_redirect) 关键字是JDK官方人员用C++代码写的，在JDK6以前是重量级锁。Java大牛 **Doug Lea**对 [synchronized](https://mp.weixin.qq.com/s?__biz=MzI4NjI1OTI4Nw==&mid=2247488320&idx=1&sn=6fd5cddf2a0ff68fe00ccc834e90521b&scene=21#wechat_redirect) 在并发编程条件下的性能表现不满意就自己写了个JUC，以此来提升并发性能，本文要讲的就是JUC并发包下的**AbstractQueuedSynchronizer**。



在JUC中 CountDownLatch、ReentrantLock、ThreadPoolExecutor、ReentrantReadWriteLock 等底层用的都是AQS，**AQS几乎占据了JUC并发包里的半壁江山，**如果想要获取锁可以被中断、超时获取锁、尝试获取锁那就用AQS吧**。**

**AQS原理**
AQS：AbstractQuenedSynchronizer抽象的队列式同步器。是除了java自带的synchronized关键字之外的锁机制。
AQS的全称为（AbstractQueuedSynchronizer），这个类在java.util.concurrent.locks包

**AQS的核心思想**是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并将共享资源设置为锁定状态，如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制AQS是用CLH队列锁实现的，即将暂时获取不到锁的线程加入到队列中。
CLH（Craig，Landin，and Hagersten）队列是一个虚拟的双向队列，虚拟的双向队列即不存在队列实例，仅存在节点之间的关联关系。
**AQS是将每一条请求共享资源的线程封装成一个CLH锁队列的一个结点（Node），来实现锁的分配。**

用大白话来说，AQS就是基于CLH队列，用volatile修饰共享变量state，线程通过CAS去改变状态符，成功则获取锁成功，失败则进入等待队列，等待被唤醒。

**注意：AQS是自旋锁：**在等待唤醒的时候，经常会使用自旋（while(!cas())）的方式，不停地尝试获取锁，直到被其他线程获取成功

**实现了AQS的锁有：自旋锁、互斥锁、读锁写锁、条件产量、信号量、栅栏都是AQS的衍生物**

## 2.AQS前置知识点

### 2.1 模板方法

`AbstractQueuedSynchronizer`是个**抽象类**，所有用到方法的类都要继承此类的若干方法，对应的设计模式就是**模版模式**。

> **模版模式定义**：一个抽象类公开定义了执行它的方法的方式/模板。
>
> 它的子类可以按需要重写方法实现，但调用将以抽象类中定义的方式进行。
>
> 这种类型的设计模式属于行为型模式。

**抽象类**

```java
/**
 * @author beifeng
 * @date 2021/1/5
 */
public abstract class SendCustom {
    public abstract void to();
    public abstract void from();
    public abstract void send();
  
    public void date(){
        System.out.println(new Date());
    }
    //注意此处  模板方法
    public void sendMessage(){
        to();
        from();
        date();
        send();
    }
}
```

**模板方法派生类**

```java
/**
 * @author beifeng
 * @date 2021/1/5
 */
public class SendSms extends SendCustom {
    @Override
    public void to() {
        System.out.println("to");
    }
    @Override
    public void from() {
        System.out.println("from");
    }
    @Override
    public void send() {
        System.out.println("send message");
    }
    public static void main(String[] args) {
        SendCustom send=new SendSms();
        send.sendMessage();
    }
}
```

### 2.2 LockSupport

**LockSupport** 是一个线程阻塞工具类，所有的方法都是静态方法，可以让线程在任意位置阻塞，当然阻塞之后肯定得有唤醒的方法。常用方法如下：

```java
public static void park(Object blocker); // 暂停当前线程
public static void parkNanos(Object blocker, long nanos); // 暂停当前线程，不过有超时时间的限制
public static void parkUntil(Object blocker, long deadline); // 暂停当前线程，直到某个时间
public static void park(); // 无期限暂停当前线程
public static void parkNanos(long nanos); // 暂停当前线程，不过有超时时间的限制
public static void parkUntil(long deadline); // 暂停当前线程，直到某个时间
public static void unpark(Thread thread); // 恢复当前线程
public static Object getBlocker(Thread t);
```

与Object类的wait/notify机制相比，park/unpark有两个优点：

> 1. 以**thread**为操作对象更符合阻塞线程的**直观定义**
> 2. **操作更精准**，可以准确地唤醒某一个线程（notify随机唤醒一个线程，notifyAll 唤醒所有等待的线程），增加了灵活性。

park/unpark调用的是 **Unsafe**(提供CAS操作) 中的 **native**代码。

park/unpark 功能在Linux系统下是用的**Posix**线程库**pthread**中的**mutex**(互斥量)，**condition**(条件变量)来实现的。**mutex**和**condition**保护了一个 **_counter** 的变量，当 **park** 时，这个变量被设置为0。当**unpark**时，这个变量被设置为1。

### 2.3 CAS

[CAS](https://mp.weixin.qq.com/s?__biz=MzI4NjI1OTI4Nw==&mid=2247488426&idx=1&sn=705cace6ce7fbc2d6f141e8b03623fff&scene=21#wechat_redirect) 是 CPU指令级别实现了原子性的比较和交换(Conmpare And Swap)操作，注意CAS不是锁只是CPU提供的一个原子性操作指令。

![image-20210105165837359](/Users/beifeng/Documents/笔记/笔记图片/image-20210105165837359.png)

CAS在语言层面不进行任何处理，直接将原则操作实现在**硬件级别实现**，之所以可以实现硬件级别的操作核心是因为CAS操作类中有个核心类**UnSafe**类。

关于CAS引发的ABA问题、性能开销问题、只能保证一个共享变量之间的原则性操作问题，以前[CAS](https://mp.weixin.qq.com/s?__biz=MzI4NjI1OTI4Nw==&mid=2247488426&idx=1&sn=705cace6ce7fbc2d6f141e8b03623fff&scene=21#wechat_redirect)中写过，在此不再重复讲解。

**注意**：并不是说 CAS 一定比SYN好，如果高并发执行时间久 ，用SYN好， 因为SYN底层用了wait() 阻塞后是不消耗CPU资源的。如果锁竞争不激烈说明自旋不严重，此时用CAS。

## 3.AQS重要方法

模版方法分为`独占式`跟`共享式`，子类根据需要不同调用不同的模版方法(讲解有点多，想看底层可直接下滑到第四章节)。

### 3.1 模板方法

#### 3.1.1 独占式获取

##### 3.1.1 acquire

不可中断获取锁`acquire`是获取独占锁方法，`acquire`尝试获取资源，成功则直接返回，不成功则进入等待队列，这个过程不会被线程中断，被外部中断也不响应，获取资源后才再进行自我中断`selfInterrupt()`。

```java
public final void acquire(int arg) {
        if (!tryAcquire(arg) &&
            acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
            selfInterrupt();
}
```

1. acquire(arg) tryAcquire(arg) 顾名思义，它就是尝试获取锁，需要我们自己实现具体细节，一般要求是：

> 如果该锁没有被另一个线程保持，则获取该锁并立即返回，将锁的保持计数设置为 1。
>
> 如果当前线程已经保持该锁，则将保持计数加 1，并且该方法立即返回。
>
> 如果该锁被另一个线程保持，则出于线程调度的目的，禁用当前线程，并且在获得锁之前，该线程将一直处于休眠状态，此时锁保持计数被设置为 1。

1. addWaiter(Node.EXCLUSIVE)

> 主要功能是 一旦尝试获取锁未成功，就要使用该方法将其加入同步队列**尾部**，由于可能有多个线程并发加入队尾产生竞争，因此采用**compareAndSetTail**锁方法来保证同步

1. acquireQueued(addWaiter(Node.EXCLUSIVE), arg)

> 一旦加入同步队列，就需要使用该方法，**自旋阻塞** 唤醒来不断的尝试获取锁，直到被中断或获取到锁。

##### 3.1.1.2 acquireInterruptibly

可中断获取锁`acquireInterruptibly`相比于`acquire`支持响应中断。

