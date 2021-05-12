# 创建和启动线程

## 使用Thread

~~~bash
// 创建线程对象
Thread t = new Thread() {
   public void run() {
   // 要执行的任务
  }
};
// 启动线程
t.start();
~~~ 

## Runnable配合Thread

把线程和任务（要执行的代码）分开。Thread 代表线程。Runnable可运行的任务（线程要执行的代码）。

~~~bash
Runnable runnable = new Runnable() {
  public void run(){
  // 要执行的任务
  }
};
// 创建线程对象
Thread t = new Thread( runnable );
// 启动线程
t.start();
~~~ 

## FutureTask配合Thread

FutureTask 能够接收 Callable 类型的参数，用来处理有返回结果的情况。

~~~bash
// 创建任务对象
FutureTask<Integer> task3 = new FutureTask<>(() -> {
  log.debug("hello");
  return 100;
});

// 参数1 是任务对象; 参数2 是线程名字，推荐
new Thread(task3, "t3").start();

// 主线程阻塞，同步等待 task 执行完毕的结果
Integer result = task3.get();
log.debug("结果是:{}", result);
~~~

# 查看进程线程方法

## Windows

任务管理器可以查看进程和线程数，也可以用来杀死进程

tasklist 查看进程

taskkill 杀死进程

## Linux

ps -fe 查看所有进程

ps -fT -p <PID> 查看某个进程（PID）的所有线程
  
kill 杀死进程

top 按大写 H 切换是否显示线程

top -H -p <PID> 查看某个进程（PID）的所有线程

## Java

jps 命令查看所有 Java 进程

jstack <PID> 查看某个 Java 进程（PID）的所有线程状态
  
jconsole 来查看某个 Java 进程中线程的运行情况（图形界面）

# 线程运行原理

## 栈与栈帧

栈内存由线程使用，线程启动后，JVM就会为其分配一块栈内存。

栈帧组成栈，对应方法调用所占用内存。

每个线程只能有一个活动栈帧，对应正在执行的方法。

## 线程上下文切换

CPU不再执行当前的线程，保存当前线程状态，恢复另一个线程状态，执行另一个线程的代码。

# 常见方法

## start与run

直接调用 run 是在主线程中执行了 run，没有启动新的线程

使用 start 是启动新的线程，通过新的线程间接执行 run 中的代码

## sleep与yield

### sleep

调用sleep会让当前线程从Running进入Timed Waiting状态（阻塞），不会分配时间片

其它线程可以使用interrupt方法打断正在睡眠的线程，这时sleep方法会抛出InterruptedException

睡眠结束后的线程未必会立刻得到执行

### yield

调用yield会让当前线程从Running进入Runnable就绪状态，然后调度执行其它线程

具体的实现依赖于操作系统的任务调度器

## join

等待线程运行结束。

可传入long型参数，即为最长等待时间。

### 同步

需要等待结果返回才能继续运行。

### 异步

不需要等待结果返回就能继续运行。

## interrupt

打断阻塞（sleep、wait、join）的线程。打断标记为假。

打断正在运行线程。

### 两阶段终止模式

<div align="center"> <img src="https://user-images.githubusercontent.com/37955886/117950736-16691480-b346-11eb-8325-f058c7707f58.png"/></div> 

### 打断park线程

不会清空打断状态

# 主线程与守护线程

## 守护线程

其他非守护线程运行结束，即使守护线程未执行完，也会强制结束。

线程.setDaemon()。

垃圾回收器为守护线程。























