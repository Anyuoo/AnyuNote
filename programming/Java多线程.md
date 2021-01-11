## Java多线程

### 1.1进程，线程简介

> 什么是进程？

进程是程序的一次执行，是动态改变的

> 什么是线程？

一个进程有若干个线程，是CPU调度和执行的单位，程序运行的不同路径

### 1.2 线程创建

1. ==继承Thread类==

   ```JAVA
   //继承Thread类，重写run方法
   class MyThread1 extends Thread{
       @Override
       public void run() {
           System.out.println("继承Thread类");
       }
   }
   //调用
    MyThread1 myThread1 = new MyThread1();
           myThread1.start();
   ```

2. ==实现Runnable接口(推荐)==

   ```java
   //实现Runnable接口你，并实现run方法
   class MyThread2 implements Runnable {
       @Override
       public void run() {
           System.out.println("实现Runnable接口");
       }
   }
   //调用
   MyThread2 myThread2 = new MyThread2();
   new Thread(myThread2).start();
   ```

3. ==实现Callable接口==

   ```java
   //实现Callable接口,实现call方法
   class MyThread3 implements Callable {
       @Override
       public Object call() throws Exception {
           return "实现Callable接口";
       }
   }
   ```

   

> run方法和start方法区别？

**run方法**是普通的方法，调用run方法，主线程执行run方法后，再继续执行，不会开启多线程，本质还是只有一条执行路径。

**start方法**调用后程序会开启子线程，子线程在执行run方法，同时主线程不会停止并继续执行，本质程序开启了另一条执行路径。

![](C:\Users\Administrator\Desktop\note\Java多线程\1.PNG)

### 1.3 线程停止

- 不推荐使用jdk自带的stop方法和destroy方法
- 推荐线程自己停下来
- 建议使用标志位进行终止变量，flag=false，线程终止

### 1.4 Java线程的五个状态

**操作系统：** 新生 -> 就绪 -> 运行 -> 阻塞 -> 终止

```java
Thread.State.NEW //创建 new Thread()
Thread.State.RUNNABLE //运行 .start()
Thread.State.WAITING //等待
Thread.State.TIMED_WAITING //超时等待
Thread.State.TERMINATED //终止死亡
```

### 1.5 Java线程优先级

**==优先级高不一定先执行，根据CPU调度==**

```JAVA
   /**
     * The minimum priority that a thread can have.
     */
    public static final int MIN_PRIORITY = 1;

   /**
     * The default priority that is assigned to a thread.
     */
    public static final int NORM_PRIORITY = 5;

    /**
     * The maximum priority that a thread can have.
     */
    public static final int MAX_PRIORITY = 10;
```

### 1.6 守护(daemon)线程

- 线程分为守护线程和用户线程
- 虚拟机不需要等待守护线程执行完毕，GC垃圾回收

### 1.7 线程同步

#### **1.7.1 死锁**

> 什么是死锁？

==多个线程互相占有对方需要的资源，以至于都不能继续执行==

- ​	**条件**
  1. 互斥条件： 一个资源只能被一个线程使用
  2. 循环等待条件：请求资源的进程形成首尾相连循环等待资源的关系
  3. 不可剥夺条件：对于已持有资源的线程，不能剥夺该进程资源
  4. 请求与保持条件：一个线程因请求资源而阻塞时，对已持有的资源保持不放

> 如何避免死锁？

==破坏请求与保持条件==：释放持有资源

#### 1.7.2 同步

**每一个类都有一把锁**

- **==synchronized==**  `隐式锁`  可以对方法和代码块 锁的是临界资源
  1. 默认锁自己（this）
  2. 浪费资源，效率低

- ==ReentrantLock== `可重入锁` 实现Lock 可以显式的加锁



### 1.8线程池

![]()