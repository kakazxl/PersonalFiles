### Synchronize和Lock锁的区别

**首先，java通过Synchronize关键字实现同步，为什么还要提供Lock？**要先从Synchronize的缺陷说起

#### Synchronize的缺陷

​	释放对象的锁有两种情况：

- 程序执行完同步代码块会释放代码块
- 程序执行同步代码块时出现异常，JVM会自动释放锁去处理 异常

​      如果获取锁的线程需要等待I/O或者调用了sleep()方法被阻塞，但是仍然持有锁，其他线程只能干巴巴的等着，这样就会很影响效率。因此需要一种机制，可以不让等待的线程无限等待下去，比如只等待一段时间或者响应中断，lock锁就可以办到。

​		再比如，当有多个线程读写文件时，读操作和写操作会发生冲突现象，写操作和写操作会发生冲突，但是读和读不会冲突。但是采用synchronize关键字来实现同步的好的话，就会导致一个问题：如果多个线程都只是进行读操作，所以当一个线程在进行读操作时，其他线程都只能等待无法进行读操作。

​		再比如，Lock可以获知线程有没有得到锁，而synchronize不能。

#### 底层实现方式

##### 	synchronize实现

​	是一种悲观锁，java是用字节码指令来控制程序的，在存在synchronize的代码块中，字节码会行程2段流程

```java
public class SyncDemo {
    public void sync(){
        synchronized(SyncDemo.class) {
            //
        }
    }
}
//在生成的字节码中，会出现1次monitorenter，2cimonitorexit指令，
// 当一个线程执行代码，遇到monitorenter指令的时候，会尝试获取锁，如果获取锁，锁计数 +1,
// 如果没有获得所，线程阻塞
// 两次monitorexit ，如上文描述，JVM会在程序发生异常时，自行释放锁
```

##### Lock实现

​	Lock实现，是基于volatile和CAS操作，本质上是一种乐观锁的体现，如果拿不到锁，会阻塞在队列中，重新尝试获取锁。

具体原理可以读AQS源码，[**Lock原理**](https://blog.csdn.net/Luxia_24/article/details/52403033)

#### 区别总结

1. Lock是一个接口，而Synchronize是关键字
2. Sync会自动释放锁，而Lock必须手动释放
3. Lock可以让等待锁的线程响应中断，而Sync不会，线程会一直等待
4. 通过Lock可以知道线程有没有拿到锁，而Sync不能
5. Lock可以提高多个线程的读效率
6. Sync可以锁类、方法和代码块，而Lock是块方位内的

[详解Lock与Synchronize](https://blog.csdn.net/u012403290/article/details/64910926)

