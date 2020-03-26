# [Java并发编程：volatile关键字解析](https://www.cnblogs.com/dolphin0520/p/3920373.html)





## 并发编程中的三个概念



**1.原子性**

由于synchronized和Lock能够保证任一时刻只有一个线程执行该代码块，那么自然就不存在原子性问题了，从而保证了原子性。

**2.可见性**

Java提供了volatile关键字来保证可见性。

另外，通过synchronized和Lock也能够保证可见性，synchronized和Lock能保证同一时刻只有一个线程获取锁然后执行同步代码，并且在释放锁之前会将对变量的修改刷新到主存当中。因此可以保证可见性。

**3.有序性**