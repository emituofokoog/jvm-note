## Thread

1.1、基本属性

- name：线程名称，可以重复，若没有指定会自动生成。
- id：线程ID，一个正long值，创建线程时指定，终生不变，线程终结时ID可以复用。
- priority：线程优先级，取值为1到10，线程优先级越高，执行的可能越大。
- state：线程状态，Thread.State枚举类型，有NEW、RUNNABLE、BLOCKED、WAITING、TIMED_WAITING、TERMINATED 6种。
- ThreadGroup：所属线程组，一个线程必然有所属线程组。

线程组ThreadGroup表示一组线程的集合,一旦一个线程归属到一个线程组之中后，就不能再更换其所在的线程组。那么为什么要使用线程组呢？主要有以下的好处：方便统一管理，线程组可以进行复制，统一进行异常处理、后台线程设置、中断、最大优先级设置等。ThreadGroup它其实并不属于Java并发包中的内容，它是java.lang中的内容。

![img](https://qqadapt.qpic.cn/txdocpic/0/abd950c371d0a0476bc6763d1caa6bb2/0)

**Thread层级图**：

![img](https://qqadapt.qpic.cn/txdocpic/0/fd0f806b63232115336296ba804b97c3/0)



- UncaughtExceptionHandler：未捕获异常时的处理器，默认没有，线程出现错误后会立即终止当前线程运行，并打印错误。



1.2、构造方法

- public Thread（） 
- public Thread（Runnable target） 
- public Thread（Runnable target,AccessControlContext acc）
- public Thread（ThreadGroup group,Runnable target） 
- public Thread（String name）
- public Thread（ThreadGroup group,String name）
- public Thread（Runnable target,String name）
- public Thread（ThreadGroup group, Runnable target, String name）
- public Thread（ThreadGroup group, Runnable target, String name, long stackSize）

入参：

- group：指定当前线程的线程组，未指定时线程组为创建该线程所属的线程组。
- target：指定运行其中的Runnable，一般都需要指定，不指定的线程没有意义，或者可以通过创建Thread的子类并重新run方法。
- name：线程的名称，不指定自动生成。
- stackSize：预期栈大小，不指定默认为0，0代表忽略这个属性根据jvm指定的大小创建。
- acc：系统资源访问权限

它们最终都是调用同一个方法



- public void init（ThreadGroup g, Runnable target, String name, long stackSize, AccessControlContext acc, boolean inheritThreadLocals）

1.3、方法

- Thread Thread.currentThread() ：获得当前线程的引用。获得当前线程后对其进行操作。
- Thread.UncaughtExceptionHandler getDefaultUncaughtExceptionHandler() ：返回线程由于未捕获到异常而突然终止时调用的默认处理程序。
- int Thread.activeCount()：当前线程所在线程组中活动线程的数目。
- void dumpStack() ：将当前线程的堆栈跟踪打印至标准错误流。
- int enumerate(Thread[] tarray) ：将当前线程的线程组及其子组中的每一个活动线程复制到指定的数组中。
- Map<Thread,StackTraceElement[]> getAllStackTraces() ：返回所有活动线程的堆栈跟踪的一个映射。
- boolean holdsLock(Object obj) ：当且仅当当前线程在指定的对象上保持监视器锁时，才返回 true。
- boolean interrupted() ：测试当前线程是否已经中断。
- void setDefaultUncaughtExceptionHandler(Thread.UncaughtExceptionHandler eh) ：设置当线程由于未捕获到异常而突然终止，并且没有为该线程定义其他处理程序时所调用的默认处理程序。
- void sleep(long millis) ：休眠指定时间
- void sleep(long millis, int nanos) ：休眠指定时间
- void yield() ：暂停当前正在执行的线程对象，并执行其他线程。
- void checkAccess() ：判定当前运行的线程是否有权修改该线程。
- ClassLoader getContextClassLoader() ：返回该线程的上下文 ClassLoader。
- long getId() ：返回该线程的标识符。
- String getName() ：返回该线程的名称。
- int getPriority() ：返回线程的优先级。
- StackTraceElement[] getStackTrace() ：返回一个表示该线程栈转储的栈跟踪元素数组。
- Thread.State getState() ：返回该线程的状态。
- ThreadGroup getThreadGroup() ：返回该线程所属的线程组。
- Thread.UncaughtExceptionHandler getUncaughtExceptionHandler() ：返回该线程由于未捕获到异常而突然终止时调用的处理程序。
- void interrupt() ：中断线程。
- boolean isAlive() ：测试线程是否处于活动状态。
- boolean isDaemon() ：测试该线程是否为守护线程。
- boolean isInterrupted()：测试线程是否已经中断。
- void join() ：等待该线程终止。
- void join(long millis) ：等待该线程终止的时间最长为 millis 毫秒。
- void join(long millis, int nanos) ：等待该线程终止的时间最长为 millis 毫秒 + nanos 纳秒。
- void run() ：线程启动后执行的方法。
- void setContextClassLoader(ClassLoader cl) ：设置该线程的上下文 ClassLoader。
- void setDaemon(boolean on) ：将该线程标记为守护线程或用户线程。
- void start()：使该线程开始执行；Java 虚拟机调用该线程的 run 方法。
- String toString()：返回该线程的字符串表示形式，包括线程名称、优先级和线程组。



1. **sleep方法**

2.1、Thread类中的sleep方法

1. public static native void sleep(long millis) throws InterruptedException;
2. public static void sleep(long millis, int nanos) throws InterruptedException



2.2、sleep(long millis, int nanos)

sleep最小休眠单位仍为millis

if (nanos >= 500000 || (nanos != 0 && millis == 0)) {

millis++;

}

2.3、要点

- 在调用sleep()方法的过程中，线程不会释放对象锁。
- 推荐使用TimeUnit的sleep方法代替原生sleep方法，TimeUnit提供的可读性更好。

例，线程休眠一天代码：

TimeUnit.DAYS.sleep(1)；

Thread.sleep(1000*60*24*8)；



1. **yield方法**

3.1、Thread类中的yield方法

public static native void yield();



yield当前正在运行的线程让出CPU使用权，以允许具有相同优先级的其他线程获得运行的机会。因此，

使用 yield()的目的是让具有相同优先级的线程之间能够适当的轮换执行。但是，实际中无法保证yield()达

到让步的目的，因为让步的线程可能被线程调度程序再次选中。



3.2、要点

**sleep和yield相同点：**



（1）两者都是Thread类的静态方法，暂停处于运行态的线程。



（2）在线程执行同步代码块或同步方法中(synchronized)，调用Thread.sleep或者Thread.yield，都不会释放同步监视器(锁)



**sleep和yield区别：**



（1）优先级：sleep暂停线程后，会给其他线程执行机会，不考虑线程的优先级问题；yield暂停后只有优先级高于或等于当前线程的线程才有执行机会。



（2）状态：sleep当前线程由运行态进入TIME_WAITING；yield使当前线程由运行态到RUNNABLE。



（3）异常：sleep方法在声明时抛出InterruptedException异常，所以在使用时要么try捕获要么throws抛出；而yield没有声明异常。



（4）移植性：sleep的移植性更好。



1. isAlive**方法**

**Thread类中的isAlive方法：**

public final native boolean isAlive();



isAlive的功能是判断当前的线程是否处于活动状态。活动状态就是指线程已经启动且尚未终止，即

Thread6个状态中除了NEW和TERMINATED外都是活动状态。

1. join方法

5.1、概念

join()的作用是：“等待该线程终止”，这里需要理解的就是该线程是指的主线程等待子线程的终止。也就

是在子线程调用了join()方法后面的代码，只有等到子线程结束了才能执行。

5.2、Thread类中的join方法：

1. public final **synchronized** void join(long millis) throws InterruptedException;
2. public final **synchronized** void join(long millis, int nanos) throws InterruptedException;
3. public final void join() throws InterruptedException;

5.3、要点：

- join底层是wait方法，所以它是会释放对象锁的，而sleep在同步的方法中是不释放对象锁的，只有同步方法执行完毕，其他线程才可以执行。
- 只有当线程启动后，join()才会生效。
```java

public final synchronized void join(long millis) throws InterruptedException {
    long base = System.currentTimeMillis();
    long now = 0;
    if (millis < 0) {
      throw new IllegalArgumentException("timeout value is negative");
    }
    if (millis == 0) {
        while (isAlive()) {
          wait(0);
        }
    } else {
      while (isAlive()) {
          long delay = millis - now;
          if (delay <= 0) {
            break;
          }
          wait(delay);
          now = System.currentTimeMillis() - base;
    	}
    }
}
```