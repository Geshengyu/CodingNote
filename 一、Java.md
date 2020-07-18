## 一、Java

[TOC]

### 1.1 基础知识

#### 1.1.1 JDK与JRE的区别？

> 1. JDK——Java开发工具包，包含JRE以及编译器（javac）和工具（javadoc，jdb）。能创建和编译程序。
> 2. JRE——Java运行时环境，包含Java虚拟机，Java类库，Java命令以及其他一些基础构件。但是，它不能用来创建程序。

#### 1.1.2 抽象类和接口的区别？

> 1. 接口的方法默认是 public，所有方法在接口中不能有实现(Java 8 开始接口方法可以有默认实现），而抽象类可以有非抽象的方法。
> 2. 接口中除了static、final变量，不能有其他变量，而抽象类中则不一定。
> 3. 一个类可以实现多个接口，但只能实现一个抽象类。接口自己本身可以通过extends关键字扩展多个接口。
> 4. 接口方法默认修饰符是public，抽象方法可以有public、protected和default这些修饰符（抽象方法就是为了被重写所以不能使用private关键字修饰！）。
> 5. 从设计层面来说，抽象是对类的抽象，是一种模板设计，而接口是对行为的抽象，是一种行为的规范。
>
> 备注：在JDK8中，接口也可以定义静态方法，可以直接用接口名调用。实现类和实现是不可以调用的。如果同时实现两个接口，接口中定义了一样的默认方法，则必须重写，不然会报错。

#### 1.1.3 Java和C++的区别？

> 1. 都是面向对象的语言。
> 2. Java中不提供指针来直接访问内存，程序内存更加安全。
> 3. Java有自动内存管理机制，不需要程序员手动去释放无用内存。
> 4. Java类是单继承的，C++支持多重继承；然而Java的接口可以多继承。

#### 1.1.4 获取用键盘输入常用的两种方法

方法一：通过Scanner

```java
Scanner input = new Scanner(System.in);
String s = input.nextLine();
input.close;
```

方法二：通过BufferedReader

> ```java
> BufferedReader input = new BufferedReader(new InputStreamReader(System.in));
> String s = input.readLine();
> ```

#### 1.1.5 Servlet总结

在Java Web程序中，**Servlet**主要负责接收用户请求 `HttpServletRequest`，在`doGet()`，`doPost()`中做相应的处理，并将回应`HttpServletResponse`反馈给用户。

**Servlet** 可以设置初始化参数，供Servlet内部使用。

一个Servlet类只会有一个实例，在它初始化时调用`init()`方法，销毁时调用`destroy()`方法**。**

Servlet需要在web.xml中配置，**一个Servlet可以设置多个URL访问**。**Servlet不是线程安全**，因此要谨慎使用类变量。

### 1.2 集合

![](https://github.com/Geshengyu/CodingNote/blob/master/Java%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6.png?raw=true)

![img](https://www.runoob.com/wp-content/uploads/2014/01/2243690-9cd9c896e0d512ed.gif)

#### 1.2.1 ArrayList、Vector、LinkedList

|            | 底层实现   | 线程安全 | 扩容  |
| ---------- | :--------- | -------- | ----- |
| ArrayList  | Object数组 | 不安全   | 1.5倍 |
| Vector     | Object数组 | 安全     | 2倍   |
| LinkedList | 双向链表   | 不安全   |       |

Vector可以使用CopyOnWriteArrayList代替。

CopyOnWriteArrayList：

- 读写分离，写操作在复制的数组上，读操作在原始数组上进行
- 写操作需要加锁，写操作完成以后需要把原始数组指向新的复制数组
- 适用于读操作远大于写操作的场景

#### 1.2.2 TreeSet、HashSet、LinkedHashSet

|               | 底层             | 是否允许元素null | 线程同步   |
| ------------- | ---------------- | ---------------- | ---------- |
| TreeSet       | TreeMap（有序）  | 不可以           | 非线程同步 |
| HashSet       | HashMap（无序）  | 可以             | 非线程同步 |
| LinkedHashSet | HashMap+双向链表 | 可以             | 非线程同步 |

HashSet：重写其equals和hashCode方法，以保证放入对象的唯一性

#### 1.2.3 TreeMap、HashMap

|         | 数据结构         | Key是否可以为null | 是否有序 |                                              |
| ------- | ---------------- | ----------------- | -------- | -------------------------------------------- |
| TreeMap | 红黑树           | 不可以            | 有序     | 自然排序Comparable接口、比较器Comparator接口 |
| HashMap | 数组+链表+红黑树 | 可以              | 无序     |                                              |

#### 1.2.4 HashTable、ConCurrentHashMap

HashMap、HashTable和ConCurrentHashMap的区别？

> HashMap：线程不安全，数组+链表+红黑树
>
> HashTable：线程安全，锁住整个对象，数组+链表
>
> ConCurrentHashMap：线程安全，CAS+同步锁，数组+链表+红黑树
>
> HashMap允许Key和Value为null，其他两个不允许为null

#### 1.2.5 什么是迭代器(Iterator)？

`Iterator`接口提供了很多对集合元素进行迭代的方法。

迭代器可以在迭代的过程中删除底层集合的元素（迭代器的`remove()`方法）。

**`ListIterator`接口**：继承自`Iterator`接口，可以双向遍历集合。

#### 1.2.6 快速失败和安全失败

|           | 原理                   | 使用场景                                         |
| --------- | ---------------------- | ------------------------------------------------ |
| Fail-Fast | 一种Java集合的错误机制 | java.util包下的集合类，不能在多线程下并发修改    |
| Fail-Safe |                        | java.util.concurrent包下的容器，多线程下并发使用 |

Fail-Fast：一种Java集合的错误机制。当使用迭代器遍历集合时，集合的结构发生了变化，则抛出`ConcurrentModificationException`。

Fail-Safe：采用此机制的容器在遍历的时候是不直接在集合内容上访问的，而是先copy原有集合内容，然后在拷贝的集合上遍历。

#### 1.2.7 容器中的设计模式

迭代器模式

适配器模式：java.util.Arrays中asList()可以把数组类型转换为List类型（注意该方法只能使用包装类型的数组）

#### HashMap源码学习

构造函数：

```java
static final float DEFAULT_LOAD_FACTOR = 0.75f;
// 延迟创建
public HashMap() { 
    this.loadFactor = DEFAULT_LOAD_FACTOR;
}
```

put方法：

```java
transient Node<K,V>[] table;
int threshold;
// 若未被初始化，则进行初始化操作
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
newCap = DEFAULT_INITIAL_CAPACITY; // 16
newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY); // 12

threshold = newThr; // 12

// 对Key求Hash值，依据Hash值计算下标
p = tab[i = (n - 1) & hash] // 对长度进行取模，比取模有更高的效率
// 若未发生碰撞，则放入桶中
// 若发生碰撞，以链表形式放在后面
// 链表长度超过阈值，HashMap元素超过最低树化容量，怎将链表转换为红黑树
static final int TREEIFY_THRESHOLD = 8;
static final int MIN_TREEIFY_CAPACITY = 64;
// 若节点已经存在，则用新值替换旧值
// 若桶满了，就需要resize
if (++size > threshold)    resize();
```

get方法：



哈希算法：

面试题1：为什么初始化容量为2的倍数

- 避免内存碎片
- 计算机进行移位操作
- 提高散列度，减少冲突

面试题2：hash函数如何降低冲突

```java
// 异或操作可以提高散列度
static final int hash(Object key) {
    int h;    
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

面试题3：冲突解决方法

- 扰动函数
- final对象，并采用合适的equals()和hashCode()方法

面试题4：扩容与扩容后的内存排布

```java
newThr = oldThr << 1;
```

扩容的问题：

多线程环境下，调整大小会存在竞争条件，容易造成死锁

rehashing也是一个比较耗时的过程

#### ConCurrentHashMap源码学习

put方法逻辑：

> 1. 判断Node[]数组是否初始化，若未被初始化，则进行初始化操作
> 2. 计算hash值，定位数组索引下标。如果没有Node节点，则使用CAS进行添加（链表的头节点），添加失败则break。
> 3. 检查内部正在扩容，就帮助一块扩容
> 4. 如果f!=null，则使用synchronized锁住f元素（链表、红黑二叉树的头元素）
>
> 根据结构，链表添加或者树添加操作
>
> 保留节点（和本地缓存相关），则抛出异常
>
> 5. 判断链表长度是否达到临界值，达到则将链表转换为红黑树

比jdk1.8以前的优化：

比起Segment，锁拆分的更细

> 1. 先使用无锁操作CAS插入头结点，失败则循环尝试
> 2. 若头节点存在，则尝试获取头节点的同步锁，再进行操作

HashMap、HashTable和ConCurrentHashMap的区别？

> HashMap：线程不安全，数组+链表+红黑树
>
> HashTable：线程安全，锁住整个对象，数组+链表
>
> ConCurrentHashMap：线程安全，CAS+同步锁，数组+链表+红黑树
>
> HashMap允许Key和Value为null，其他两个不允许为null





### 1.3 序列化

#### 1.3.1 什么是 java 序列化？什么情况下需要序列化？

序列化：将 Java 对象转换成字节流的过程。

反序列化：将字节流转换成 Java 对象的过程。

适用场景：当 Java 对象需要在网络上传输 或者 持久化存储到文件中。

序列化的实现：类实现 Serializable 接口，这个接口没有需要实现的方法。实现 Serializable 接口是为了告诉 jvm 这个类的对象可以被序列化。

注意事项：

- 某个类可以被序列化，则其子类也可以被序列化
- 声明为 static 和 transient 的成员变量，不能被序列化。static 成员变量是描述类级别的属性，transient 表示临时数据
- 反序列化读取序列化对象的顺序要保持一致

### 1.4 JDBC

#### 1.4.1 访问数据库的步骤：

1. 加载驱动：`Class.forName("com.mysql.jdbc.Driver")`
2. 建立数据库连接：`Connection conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/imooc","root","gsy223")`
3. 创建Statement对象：`conn.createStatement();`
4. 执行SQL语句。
5. 访问结果集ResultSet对象。
6. 依次将ResultSet、Statement、Connection对象关闭，释放掉所占资源。

> 1、Statement对象用于执行**不带参数**的简单SQL语句。 
>
> 2、`PreparedStatement` 对象用于执行**预编译**SQL语句。 
>
> 3、`CallableStatement`对象用于执行对**存储过程的调用**。

#### 1.4.2 存储过程：

`CallableStatement`提供用来调用数据库中存储过程的接口。

```java
Connection conn = DBUtil.getConnection();
CallableStatement c = conn.prepareCall("call nameOfProcedue");
c.excute();
ResultSet rs = c.getResultSet(); 
```

#### 1.4.3 事务管理：

**事务是作为单个逻辑工作单元执行的一系列操作**

事务的特点：

> 原子性；一致性；隔离性；永久性。

JDBC对事务管理的支持（四个）：

> 提交 `commit()`
>
> 回滚 `rollback()`
>
> `setAutoCommit(false)` 禁止自动提交
>
> 保存点 `savepoint` （不终止当前事务，提交和回滚会终止）

#### 1.4.4 连接池：

`getConnection()`方法返回的Connection是原始Connection的代理，当调用close方法时，不是真正关连接，而是将代理对象放回连接池。

常用开源数据库连接池：dbcp、c3p0。

#### 1.4.5 JDBC的脏读是什么？哪种数据库隔离级别能防止脏读？

脏读：**一个事务读取到另外一个事务未提交的数据**

下面的三种个隔离级别都可以防止：

- Serializable【TRANSACTION_SERIALIZABLE】
- Repeatable read【TRANSACTION_REPEATABLE_READ】
- Read committed【TRANSACTION_READ_COMMITTED】

#### 1.4.6 JDBC中存在哪些不同类型的锁？

- **乐观锁——只有当更新数据的时候才会锁定记录**。
- **悲观锁——从查询到更新和提交整个过程都会对数据记录进行加锁。**

#### 1.4.7 升级替代产品：

1. Commons-dbutils

   `Dnutils`常规工作的工具类；`QueryRunner`简化SQL查询； `ResultSetHandler`将数据转变并处理为任何一种形式。

2. Hibernate

3. MyBatis

   sql写在xml里，便于统一管理和优化。

   解除sql与代码的耦合。

### 1.5 JVM

1. JVM如何加载.class文件？

   Class Loader：依据特定格式，加载class文件到内存

   Runtime Data Area：JVM内存结构模型

   Execution Engine：对命令进行解析

   Native Interface：融合不同开发语言的原生库为Java所用

2. 谈谈反射

   动态类或对象的获取信息，动态调用方法的功能。

   ```java
   class Robot {
   
       private String name;
   
       public void sayHello(String helloSentence) {
           System.out.println(helloSentence + " " + name);
       }
   
       private String sayHi(String tag) {
           return "hi " + tag;
       }
   }
   ```

   

   ```java
   public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException, NoSuchFieldException {
   
           /**
            * 反射的例子
            */
           Class rc = Class.forName("main.Robot");
           Robot r = (Robot) rc.newInstance();
           System.out.println(rc.getName());
           // getDeclaredMethod方法可以获取公有、私有、保护的方法或者包类型下的方法，但无法获得继承的方法以及所实现的接口的方法
           Method getHi = rc.getDeclaredMethod("sayHi", String.class);
           getHi.setAccessible(true);
           Object str = getHi.invoke(r, "Bob");
           System.out.println("The result is" + str);
   
           // getMethod方法获取公有方法，以及继承的方法和所实现接口的方法
           Method getHello = rc.getMethod("sayHello", String.class);
           getHello.invoke(r, "hello");
   
           Field name = rc.getDeclaredField("name");
           name.setAccessible(true);
           name.set(r, "Alice");
           getHello.invoke(r, "hello");
   }
   ```

3. 谈谈ClassLoader

   工作在Class装载的加载阶段，主要作用是从系统外部获得Class二进制数据流，然后交给java虚拟机进行连接、初始化操作。

   ClassLoader种类：

   1. BootStrapClassLoader：C++编写，加载核心库java.*

   2. ExtClassLoader ：Java编写，加载拓展库javax.*

   3. AppClassLoader：Java编写，加载程序所在目录

   4. 自定义ClassLoader：Java编写，自定义加载

      自定义ClassLoader的实现

      `findClass()`

      `defineClass()`

4. 谈谈类加载器的双亲委派机制

5. 你了解Java的内存模型吗？

   1. 程序计数器
   2. 虚拟机栈：递归为什么会引发StackOverflowError异常（递归过深，帧栈数超出虚拟栈深度）；虚拟机栈过多会引发OutOfMemoryError异常
   3. 本地方法栈：类似虚拟机栈，主要作用于标注了native的方法
   4. 元空间
   5. Java堆

6. 元空间（MetaSpace）与永久代（PermGen）的区别

   **元空间使用本地内存，永久代使用的是jvm内存**

   元空间的优势：

   > 字符串常量池存在在永久代中，容易出现性能问题和内存溢出；
   >
   > 类和方法的大小信息难以确定，给永久代的大小指定带来困难；
   >
   > 永久代会为GC带来不必要的复杂性；
   >
   > 方便HotSpot与其他JVM如Jrockit的集成。

7. JVM三大性能调优参数 -Xms -Xmx -Xss的含义

   -Xss：规定了每个线程虚拟机栈的大小

   -Xms：堆的初始值

   -Xmx：堆能达到的最大值

8. java内存模型中堆和栈的区别——内存分配策略

   管理方式：栈自动释放，堆需要GC；

   空间大小：栈比堆小

   碎片相关：栈产生的碎片远小于堆

   分配方式：栈支持静态和动态分配，而堆只支持动态分配

   效率：栈的效率比堆高

9. 不同版本的之间intern()方法的区别

   

10. 

    





### 1.6 垃圾回收

#### 1.6.1 标记算法

##### 1.引用计数算法

判断对象的引用数量；每个对象实例都有一个引用计数器。

优点：执行效率高，程序执行受影响小。

缺点：无法检测出循环引用的情况，导致内存泄露。

##### 2.可达性计数算法

判断对象的引用链是否可达。

可以作为GC Root的对象：

- 虚拟机栈中引用的对象（栈帧中的本地变量表）
- 方法区中常量引用的对象
- 方法区中类静态属性引用的对象
- 本地方法栈中JNI（Native）的引用对象
- 活跃线程的引用对象

#### 1.6.2 垃圾回收算法

1. 标记-清除算法：碎片化

2. 复制算法：分为对象面和空闲面，存活的对象被从对象面复制到空闲面，将对象面所有对象清除。**适用于对象存活率低的场景。**

3. 标记-整理算法：移动所有存活对象。**适用于存活率高的场景。**

4. 分代收集算法：按照对象生命周期的不同划分区域，采用不同的算法。

   年轻代——复制算法；老年代——标记-整理算法。

   GC的分类：Minor GC；Full GC。

   年轻代：Eden区；两个Survivor区。

#### 1.6.3 垃圾收集器

##### 垃圾收集器之间的关联

![](https://github.com/Geshengyu/CodingNote/blob/master/%E5%9E%83%E5%9C%BE%E6%94%B6%E9%9B%86%E5%99%A8.png?raw=true)

##### 年轻代常用收集器

Serial收集器：

单线程收集，暂停所有工作线程；简单高效，Client模式下默认的收集器。

ParNew收集器：

多线程收集，其他同Serial收集器。

Parallel Scavenge收集器：

更关注系统的吞吐量。在多核执行下才有优势，Server模式下默认的收集器。

##### 老年代常用收集器

Serial Old收集器：

标记-整理算法

Parallel Old收集器：

多线程，吞吐量有限

CMS收集器：标记-清除算法

1. *初始标记：stop-the-world*
2. 并发标记：并发追溯标记，程序不会停顿。
3. 并发预清理：查找执行并发标记阶段晋升老年代的对象
4. *重新标记：暂停虚拟机，扫描CMS堆中剩余对象*
5. 并发清理：清理垃圾对象，程序不会停顿
6. 并发重置：重置CMS收集器的数据结构

G1收集器：复制+标记-整理算法

并发和并行；分代收集；空间整合；可预测的停顿

#### 常见面试题

1. Object的finalize()方法的作用是否和C++的析构函数相同？

   不同。C++的析构函数调用确定，finalize()是不确定的。

2. Java中的强引用，软引用，弱引用，虚引用有什么用？

   强引用：最普遍的引用。

   软引用：对象处于有用但非必须的状态。内存空间不足，才会回收。可以用来实现高速缓存。

   弱引用：非必须的对象。GC时会被回收。适用于偶尔被使用且不影响垃圾收集的对象。

   虚引用：不会决定对象的生命周期。任何时候都可能被收集器回收。跟踪对象被垃圾收集器回收的活动，其哨兵作用。必须与引用队列ReferenceQueue联合使用。

   > 1、强引用：一个对象赋给一个引用就是强引用，比如new一个对象，一个对象被赋值一个对象。
   >
   > 2、软引用：用SoftReference类实现，一般不会轻易回收，只有内存不够才会回收。
   >
   > 3、弱引用：用WeekReference类实现，一旦垃圾回收已启动，就会回收。
   >
   > 4、虚引用：不能单独存在，必须和引用队列联合使用。主要作用是跟踪对象被回收的状态。

3. ReferenceQueue的作用

   GC完成之后，用来存储除强引用外的与之关联的引用对象。

### 1.7 多线程

#### 1.7.1 进程与线程

进程是资源分配的最小单位；线程是CPU调度的最小单位。

进程是抢占处理机的调度单位；线程属于某个进程，共享该资源。

#### 1.7.2 使用线程

##### 1.7.2.1 使用线程的方法

- 继承Thread类
- 实现Runnable接口
- 实现Callable接口
- 其他实现方式：匿名内部类、线程池

##### 1.7.2.2 Thread中的start和run方法的区别？

> start()方法可以创建一个线程并启动。
>
> run()方法只是Thread的一个普通方法的调用，并没有创建一个线程。

##### 1.7.2.3 如何处理线程的返回值？

主线程等待法

使用Thread类的join()阻塞当前线程以等待子线程执行完毕

通过Callable接口实现：通过FutureTask 或线程池获取

##### 1.7.2.4 线程池

> 基本概念：Executor接口管理多个异步任务的执行，无需程序员显示的管理线程的生命周期。
>
> 最大线程池大小：maxinumPoolSize
>
> 核心线程池大小：corePoolSize
>
> 状态：RUNNING、SHUTDOWN、STOP、TIDYING、TERMINATED

![img](http://mmsns.qpic.cn/mmsns/2BGWl1qPxib1uiccBviakhbhCJ6S7Nj2eicW5hm9xETCbb1uWrOoyujoFg/0?wx_lazy=1)



![img](https://mmbiz.qpic.cn/mmbiz_png/2BGWl1qPxib1uiccBviakhbhCJ6S7Nj2eicWemHnSPBE4B7CyST9wdGx8HfFQHFqszqVsl4JibUULGrSGJ0kF6ib2K0g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

ThreadPoolExecutor详解：

三个实现：

利用Executors创建不同线程池

```java
// 指定工作线程数量的线程池
newFixedThreadPool(int nThreads)
    
// 处理大量短时间工作任务的线程池
newCachedThreadPool()
    
// 创建唯一的工作线程来执行任务，如果线程异常结束，会有另一个线程取代它
newSingleThreadExecutor()
    
// 定时或周期性的工作调度
newSingleThreadScheduleExecutor()
    
// 利用working-stealing算法，并行地处理任务，不保证处理顺序
newWorkStealingPool()
```

execute执行方法：

> 运行线程少于corePoolSize，Executor首选添加新的线程；
>
> 等于或多于corePoolSize，小于maxinumPoolSize，首选将其请求加入队列，若队列已满，则创建新线程；
>
> 若超出maxinumPoolSize，这时如果队列已满，则通过handler所指定的策略来处理任务

两个关闭方法：`shutdown()`和`shutdownNow()`

#### 1.7.3 基础线程机制

##### 1.7.3.1 线程的状态（六种）

`new`

`runnable`

`waiting`

`timed_waiting`

`blocked`

`terminated`

![](https://github.com/Geshengyu/CodingNote/blob/master/%E7%BA%BF%E7%A8%8B%E7%9A%84%E7%8A%B6%E6%80%81.png?raw=true)

##### 1.7.3.2 sleep和wait的区别

> sleep是Thread类的方法，wait是Object类中定义的方法
>
> sleep()可以在任何地方使用；wait()只能在synchronized方法或者synchronized块中使用
>
> Thread.sleep只会让出CPU，不会释放锁；Object.wait不仅会让出CPU，还会释放占用的同步资源锁

##### 1.7.3.3 notify() 和 notifyAll() 有什么区别？

> notify() 方法随机唤醒对象的等待池中的一个线程，进入锁池；
>
> notifyAll() 唤醒对象的等待池中的所有线程，进入锁池。

##### 1.7.3.4 yield

声明当前线程已经完成生命周期最重要的部分，可以切换其他线程。只是对线程调度器的建议，有优先级的限制。

##### 1.7.3.5 守护线程

守护线程是运行在后台的一种特殊进程。它独立于控制终端并且周期性地执行某种任务或等待处理某些发生的事件。在 Java 中垃圾回收线程就是特殊的守护线程。

当所有非守护线程结束时，程序也就终止，同时会杀死所有的守护线程。

#### 1.7.4 中断

interrupt()通知线程应该中断了

如果线程处于被阻塞状态：立即退出，并抛异常

如果线程处于正常活动状态：该线程中断标志为设置为true，线程正常运行

#### 1.7.5 互斥同步

##### 1.7.5.1 可重入与不可重入锁

可重入：线程可以进入它已经拥有锁的代码块中

不可重入：当前线程获取到锁，在方法中再次获取锁时就获取不到

synchronized和ReentrantLock都是可重入锁

##### 1.7.5.2 synchronized与ReentrantLock比较

|               | 锁的实现 | 性能     | 可等待中断 | 公平锁                     | 可以同时绑定多个条件 |
| ------------- | -------- | -------- | ---------- | -------------------------- | -------------------- |
| synchronized  | JVM      | 大致相同 | 不可中断   | 默认非公平锁               |                      |
| ReentrantLock | JDK      | 大致相同 | 可中断     | 默认非公平锁，可实现公平锁 | 可以同时绑定多个条件 |

1.7.6 AQS







#### 同步方法与同步代码块的区别？

**同步方法**默认用this或者当前类class对象作为锁。用synchronized关键字修饰方法。

**同步代码块**可以自己选择以什么加锁，可以选择同步会发生同步问题的代码部分，相比于同步方法更加细颗粒度。synchronized (object) {代码内容}进行修饰。

#### 在监视器（Monitor）内部是如何实现同步代码块的？

监视器和锁在JVM中是一起使用的。监视器监视同步代码块，确保每次只有一个线程执行同步代码块。

#### 什么是死锁？如何避免死锁？

多个进程因竞争资源而造成的一种僵局。

避免死锁：制定获取锁的顺序，并强制线程按照制定的顺序获得锁。

#### 并发与并行

并行是在不同实体上的多个事件；

并发是在同一实体上的多个事件。

#### 创建线程有哪几种方式?

1. 继承Thread，重写run方法
2. 实现Runnable接口，重写run方法
3. 实现Callable接口通过FutureTask包装器来创建Thread线程

#### 说一下 runnable 和 callable 有什么区别？

- Runnable 接口 run 方法无返回值；Callable 接口 call 方法有返回值，支持泛型
- Runnable 接口 run 方法只能抛出运行时异常，且无法捕获处理；Callable 接口 call 方法允许抛出异常，可以获取异常信息



##### synchronized

互斥锁的特性：

互斥性（操作的原子性）；可见性

> synchronized锁的不是代码，锁的是对象

根据获取的锁的分类：获取对象锁和获取类锁

获取对象锁的两种方法：

1. 同步代码块（synchronized(this), synchronized(类实例对象)），锁是小括号中的实例对象
2. 同步非静态方法，锁是当前对象的实例对象

获取类锁的两种方法：

1. 同步代码块（synchronized(类.class)），锁是小括号中的类对象
2. 同步静态方法，锁是当前对象的类对象

底层实现原理

对象在内存中的布局：对象头、实例数据、对齐填充

Java对象头

Monitor



### 1.8 设计模式

创建型：将对象的创建与对象的使用进行了分离。

例如，工厂模式、单例模式

结构型：

例如，代理模式、适配器模式、装饰模式

行为型：

例如，观察者模式

#### 1.8.1 单例模式

实现：

> 饿汉式：全局的单例实例在类加载时构建。
>
> 懒汉式：全局的单例实例在第一次被使用时构建。

```java
// 懒汉式：内部静态类的单例模式（最佳写法）
public class Solution {
    // private访问级别的构造函数
    private Solution() {}
    // 私有静态类
    private static class LazyHolder {        
        private static final Solution INSTANCE = new Solution();    
    }
    // 显示调用getInstance方法时，显示装载LazyHolder类
    public static final Solution getInstance() {        
        return LazyHolder.INSTANCE;    
    }
}
```

#### 1.8.2 工厂模式



#### 1.8.3 代理模式



#### 8.2.生产者——消费者问题