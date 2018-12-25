<!-- GFM-TOC -->
* [一、线程安全性](#线程安全性)
    * [1 什么是线程安全](#什么是线程安全)
    * [2 原子性](#原子性)
    * [3 加锁机制](#加锁机制)
* [二、对象的共享](#对象的共享)
	* [1 可见性](#可见性)
	* [2 发布与逸出](#发布与逸出)
	* [3 线程封闭](#线程封闭)

----------


# 线程安全性

## 什么是线程安全
线程安全性：当多个线程访问某个类时，这个类始终都能表现出正常的行为，那么就称这个类是线程安全的。（无状态的对象一定是线程安全的）

## 原子性
原子性指的是整个程序中的所有操作，要么全部完成，要么全部不完成，不可能停滞在中间的某个环节
```java
++count;  // 非原子操作
```
实际上，++count包含了3个独立的操作：读取count的值，将值+1，然后将计算结果写入count，这是一个“读取——修改——写入”的操作序列，并且其结果状态依赖于之前的状态。

要保持状态的一致性，就需要在单个原子操作中更新所有相关的状态变量。

## 竞态条件
由于不恰当的执行时序而出现的不正确的结果叫作**竞态条件**

## 加锁机制
### 内置锁
Java提供了一种内置的锁机制来支持原子性：同步代码块(Synchronized Block)，其包含两个部分
- 一个作为锁的对象引用
- 一个作为由这个锁保护的代码块，其中该同步代码块的锁就是方法调用所在的对象。
静态的synchronize方法以Class对象作为锁。
``` java
synchronized (lock){
// 访问或修改锁保护的共享状态
}
```
每一个Java对象都可以用作一个实现同步的锁，这些锁被称为**内置锁(Intrinsic Lock)**或**监视器锁(Monitor Locl)**。
线程在进入同步代码块之前会自动获得锁，并且在退出同步代码块时会自动释放锁，而无论是通过正常的路径退出，还是通过从代码块中抛出异常退出。

**获得内置锁的唯一途径**就是进入由这个锁保护的同步代码块或者方法。
Java的内置锁相当于一种**互斥体(或互斥锁)**，这意味着同时最多只有一个线程能持有这种锁。当线程A尝试获得由线程B持有的锁时，线程A必须等待或者阻塞，等到B释放锁之后才有可能获得这个锁，如果B永远不释放锁，则A永远等待下去。

### 重入
当某个线程请求一个由其他线程持有的锁时，发出请求的线程就会阻塞。然而，由于内置锁是可以**重入**的，因此某个线程试图获得一个已由它自己持有的锁，那么这个请求就会成功。

重入进一步提升了加锁行为的封装性，因此简化了面向对象并发代码的开发
```java
public class Widget{
    public synchronized void doSomething(){
        ...
    }
}
public class LoggingWidget extends Widget{
    public synchronized void doSomething(){
        System.out.println(toString() + ": calling doSomething");
        super.doSomething();
    }
}
```
上述程序清单中，子类重写了父类的synchronized方法，然后调用父类中的方法，由于Widget和loggingWidget中的doSomething方法在执行前都会获得Widget上的锁，此时如果没有可重入的锁，子类调用父类的doSomething方法时，将永远无法获得Widget上的锁，因此这段代码将产生死锁。

### 用锁来保护状态
如果在复合操作的执行过程中持有一个锁，则会使复合操作变成原子操作。对于可能被多个线程同时访问的可变状态变量，在访问它们的时候都需要持有一个锁，我们称这个状态变量是由这个锁保护的。

一种常见的加锁约定是，把所有可变的状态都封装到对象的内部。

如果只是将每个方法作为同步方法，例如Vector，那么并不足以确保Vector上的复合操作都是原子的，例如：
```java
if(!vector.contains(element))
	vector.add(element);
```
虽然contains和add 方法都是原子的，假设contains方法由A线程占有，add方法由B线程占有，但是在执行完if判断条件之后，被其它线程C抢先执行了一次add方法，之后线程B再执行add方法，也就是if之后执行了2次add，显然与目标程序设计不符。

虽然synchronized方法可以确保单个操作的原子性，但如果要把多个操作合并为一个复合操作，还需要额外的加锁机制(了解如何在线程安全对象中添加原子操作的方法)，否则仍然会产生**竞态条件**。

----------

# 对象的共享

## 可见性
为了确保多个线程之间对内存写入操作的可见性，必须使用同步机制。

### 加锁与可见性
内置锁可以用于确保某个线程以一种可预测的方式来查看另一个线程的执行结果。
<div align="center"> <img src="../pics//1545703873(1).png" width=""/> </div><br>
访问某个共享且可变的变量时，要求所有的线程都在同一个锁上同步，就是为了确保某个线程写入该变量的值对于其它线程来说是可见的。否则，如果一个线程在未持有正确的锁的情况下读取某个变量，则读取到的值可能是实效值。

加锁的含义不仅仅局限于互斥行为，还包括内存可见性。为了确保所有的线程都能看到共享变量的最新值，所有执行读操作或者写操作的线程都必须在同一个锁上同步。

### Volatile变量
Java语言提供了一种稍弱的同步机制，即volatile变量，用来确保将变量的更新操作通知其它线程。当把一个变量声明为volatile类型后，编译器与运行时都会注意到这个变量是共享的，因此不会将该变量上的操作与其它内存操作一起重排序。volatile不会被缓存在寄存器或者对其它处理器不可见的地方，因此在读取volatile类型的变量时，总会返回最新写入的值。

volatile变量是一种比synchronized关键字更轻量级的同步机制。

volatile变量的一种经典用法：检查某个状态变量以标记是否退出循环。
```java
volatile boolean asleep;
...
while(!asleep)
	countSomeSheep();
```
为了使这个实例能正确执行，asleep必须设置为volatile类型，否则其它线程修改了asleep后，执行判断的线程缺发现不了。


**局限性：**volatile变量通常用作某个操作完成、发生或中断的状态标志。volatile的语义不足以确保递增操作(count++)的原子性。

**当且仅当**满足以下条件时，才应该使用volatile关键字：
- 对变量的写入操作不依赖于变量的当前值，或者你能确保只有单个线程对变量进行更新。
- 该变量不会与其它状态变量一起纳入不变性条件中。
- 在访问变量时不需要加锁。

## 发布与逸出
**发布(Publish)**一个对象的意思是指，使对象能够在当前作用域之外的代码中使用。当某个不应该发布的对象被发布时，这种情况叫作**逸出(Escape)**。

发布对象对简单的方法是将对象的引用保存到一个公有的静态变量中。
```java
public static Set<secret> knowScrets;
	
public void initialize(){
	knowScrets = new HashSet<>();
}
```
当发布一个对象时，该对象的非私有域中引用的所有对象同样会被发布。

发布对象或者其内部状态的机制就是发布一个内部的类实例，例如：
```java
public class ThisEscape{
	public ThisEscape(EventSource source){
		source.registerListener(new EventListener(){
			public void onEvent(Event e){
				doSomething(e);
			}
		})
	}
}
```
ThisEscape发布EventListener时，也隐含的发布了ThisEscape本身，因为在这个内部类实例中包含了对ThisEscape实例的隐含引用。但是不推荐这么做。

## 线程封闭
访问共享的可变数据时，通常需要使用同步。一种避免使用同步的方式就是不共享数据。如果仅在单线程内访问数据，就不需要同步。这种技术称为**线程封闭(Thread Confinemwnt)**，它是实现线程安全性的最简单方式之一。常见的应用有JDBC(Java Database Connectivity)的Connection对象。

### Ad-hoc线程封闭
Ad-hoc线程封闭是指，维护线程封闭性的职责完全由程序来实现。由于Ad-hoc线程封闭技术的脆弱性，因此在程序中要尽量少使用。

### 栈封闭
栈封闭是线程封闭的一种特例，在栈封闭中，只能通过局部变量才能访问到对象。局部变量(在JVM虚拟机栈中，线程私有)的固有属性之一就是封闭在执行线程中。

### ThreadLocal类
ThreadLocal类能使线程中的某个值与保存值的对象关联起来。ThreadLocal提供了get和set等访问接口或方法这些方法为每个使用该变量的线程都存有一份独立的副本，因此get总是返回由当前线程执行在调用set时设置的最新值。

ThreadLocal对象通常用于防止对可变的单实例变量(Singleton)或全局变量进行共享。

由于JDBC连接对象不一定是线程安全的，通过将JDBC的连接保存到ThreadLocal对象中，每个线程都会有自己的连接。
```java
private static ThreadLocal<Connection> connectionHolder = new ThreadLocal<Connection>(){
	public Connection initialValue(){
		return DriverManager.getConnection(DB_URL);
	}
};
public static Connection getConnection(){
	return connectionHolder.get();
}
```
当某个频繁执行的操作需要一个临时对象，例如一个缓冲区，而同时又希望避免在每次执行时都重新分配该临时对象，就可以使用该技术。

## 不变性
如果某个对象在创建后，其状态就不能被修改，那么这个对象就称为**不可变对象**，不可变对象一定是线程安全的。
当满足以下条件时，对象才是不可变的
- 对象创建后其状态就不能修改
- 对象的所有域都是final类型
- 对象是正确创建(在创建对象期间，this引用没有逸出)

在不可变对象的内部，仍可以用可变对象来管理其状态。
```java
public final class ThreeStooges{
	private final Set<String> stooges = new HashSet<>();

	public ThreeStooges(){
		stooges.add("Moe");
		stooges.add("A");
		stooges.add("B");
	}

	public boolean isStooge(String name){
		return stooges.contains(name);
	}
}
```
尽管保存姓名的set对象可变，但是从ThreeStooges的设计中可以看到，在set对象构造完成之后无法改变。
