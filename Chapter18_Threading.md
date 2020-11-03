# 第 18 章：多线程

1. **线程**（Thread）概念：一个程序就是一个进程，相当于工厂，这个工厂会有很多工人在协同工作，每个工人就类似线程的概念
2. 使用线程：创建一个 `Thread` 类的实例对象，然后里面写上多线程要执行的操作（无返回值的方法，以委托形式传入）
3. `Thread.Sleep`：线程休息指定的毫秒数
4. **前台**（Foreground）线程和**后台**（Background）线程：
    1. 前台线程（前面讲的都是前台线程）：线程一旦执行就不受程序自身控制，而且创建会消耗大量性能（虽然你感觉不到）；后台线程在主程序关闭后自动终止并消失，有两种创建方式，第一种是通过前台线程的创建模式，使用 `Thread` 类创建；另外一个则是用**线程池**（Thread Pool）的资源
    2. 线程池：存储后台线程资源的一个逻辑集合
    3. 使用：`ThreadPool.QueueUserWorkItem` 方法
5. 线程内抛出异常跟之前委托抛出异常的执行效果类似，它并不会在线程内抛出，而是终止线程后，退回线程外部抛出该异常；线程报错时会把错误信息包裹存储起来，待退出线程后直接抛出
6. 线程同步
    1. 为什么要线程同步：生产者消费者模型

    ```csharp
    public class Program
    {
        const int Times = int.MaxValue;
        int count = 0;

        static void Main()
        {
            Thread thread = new Thread(new ThreadStart(Decrement));

            Increment();
        }

        static void Increment()
        {
            for (int i = 0; i < Times; i++)
                count++;
        }

        static void Decrement()
        {
            for (int i = 0; i < Times; i++)
                count--;
        }
    }
    ```

    最后看到的结果可能是一个随意的数值（不可再现性）

    2. 使用 `Monitor.Enter` 和 `Monitor.Exit` 达到同步要求：创建一个 `object` 对象，直接实例化出来（`new object()`），然后来锁住它
    3. 锁住的对象不建议使用值类型，建议使用私有的、静态的引用类型对象；不建议使用 `this`，如果程序挂了，那么 `this` 无法解锁，更容易出问题；不建议使用 `typeof(string)` 和 `string` 之类的东西，因为 `string` 在 C# 里被特殊处理过，是不变的，就很类似于值类型赋值的复制副本，因此不建议
    4. `volatile` 修饰符：保证可被多个线程读写
    5. `System.Threading.Interlocked` 类：提供一些原子性操作，例如交换（`CompareExchange`）、增大一个单位（`Increment`）、减小一个单位（`Decrement`）等
    6. 事件在增减的时候发生的问题：请尽量建议在事件增减方法的时候使用 `lock` 语句防止多个线程读写而报错
    7. **死锁**（Deadlock）：
        1. 四大特性：**互斥**（Mutual Exclusion）、**占有和保持**（Hold & Wait）、**不可抢占**（No preemption）和**循环等待**（Circular wait），四大特性缺一不可，都满足时必然死锁；参考文章：https://user.qzone.qq.com/747507738/blog/1598518585 的 Part 5（死锁）
        2. 避免不必要的锁定，以减少甚至消除死锁的可能性
    8. 可用于同步的模型：互斥量 `Mutex`
    9. 其他东西
        1. `ThreadStaticAttribute`
        2. `STAThreadAttribute`
        3. `MethodImplAttribute` 实现同步方法（每次只能由一个线程调用，即方法是原子性的）