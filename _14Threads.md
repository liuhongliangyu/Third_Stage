# 多线程

## 多线程的概念

打开电脑，我们可以一边听音乐，一边下载文件，一边浏览网页，三项工作可以同时进行。操作系统用三个应用程序完成这三项工作，每个应用程序都可以被看做一条连续的指令，CPU一条一条地执行这些指令。然后再单核CPU的计算机中，一个时刻只能执行一条指令，如何实现三项工作同时进行呢？
原来操作系统以“时间片轮转”的方式实现这一目标。操作系统以进程（Process）的方式运行应用程序，进程不但包括应用程序的指令流，也包括运行程序所需的内存、寄存器等资源。你只需要把进程理解为一条执行路线就行了，一般情况下，开启三个应用程序，系统里创建了三个进程，就增加了三条执行路线。
操作系统轮流执行每个进程，每个进程执行一小段时间。比如先执行几十毫秒播放音乐的进程，接着执行几十毫秒下载文件的进程，然后再执行几十毫秒浏览网页的进程，三个进程就这样循环往复，交替进行。因为交替时间很短（一般只有几十毫秒），人们根本感觉不到如此短暂的停顿，所以在表面上看来就像三个工作同时进行似的。

* 因此进程在宏观上是并发进行的，在微观上是交替进行的。

我们可以考虑这样一个例子，一个网页有一段Flash动画，还有一个等待用户输入的文本框。
程序必须在播放动画的同时不停滴检测有无用户输入，以便及时响应用户的操作。在最早期的操作系统中，我们需要编写一段非常复杂的代码来实现这一目标，现在由于有了多线程技术（Multi-threading），这类问题就变得很简单了，我们可以通过在一个进程中创建两个线程（Threading）来实现我们的目标。线程非常类似于进程，它相当于在一个进程中创建了若干条并行的线路，比如一个线程播放动画，一个线程检测用户输入，操作系统将自动以“时间片轮转”的方式交替执行这两个线程中的指令，这样我们的目标就实现了。

## 多线程介绍

C#通过多线程支持并行执行的代码。一个线程是一个独立执行的路径，可以同时与其他线程一起运行。一个C#客户端程序(Console,WPF,Winows Forms)开始于一个单独的线程，该线程由CLR和操作系统自动地创建，我们称它为主线程，而且可以通过创建附加的线程来实现多线程。

### 创建进程

#### Thread类

一般情况下,每开启一个应用程序，系统就会创建一个与该程序相关的进程，紧接着进程就会
创建一个主线程（Main Thread），然后从主函数中的代码开始执行。当然，我们可以在一个应用程序中创建任意多个线程，每个线程完成一项任务。
假设我们要进行一个非常耗时的图片处理工作，由于消耗时间非常长，程序界面就会处于
锁定状，用户不能进行其他操作。对于这一种情况，我们可以创建一个单独的线程来处理图片，让它和其他线程并发进行，这样我们就可以在处理图片的同时进行其操作。这中执行特定任务的线程被称为工作线程（Work Thread）.

在C#中，线程由System.Threading命名空间中的Thread类实现。声明线程的语法如下：

```C#
Thread WorkThread = new Thread(entryPoint); 
```

每一个线程都需要一个入口函数，因为线程使用入口函数中的代码开始执行的。所以我们需要事先编写一个入口函数，然后把它传递给线程。这一点可以通过委托机制完成。

在C#中，与入口函数相关的委托是ThreadStart。

```C#
public delegate void ThreadStart();
```

从ThreadStart委托的定义我们可以看出，传递给线程的如果扣函数必须是没有参数和返回值的函数。

综上所述，创建一个线程一般需要三个步骤：

```C#
//第一步：编写入口函数
private void EntryPointMethod(){}
//第二步：创建入口的委托
ThreadStart entryPoint= new ThreadStart(EntryPointMethod);
//第三步：创建线程
Thread WorkThread = new Thread(entryPoint);
```

Thread类为我们提供了丰富的属性和方法，可以非常方便地控制线程。Tread类的部分属性和方法如下图所示：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/%E5%A4%9A%E7%BA%BF%E7%A8%8B/%E7%BA%BF%E7%A8%8B%E7%9A%84%E6%A6%82%E5%BF%B5/images/Image1.png)

【例子】 创建线程

```C#
using System;
using System.Threading;
class Program
{
            static void Main(string[] args)
            {
                Thread thread = new Thread(WriteY);//创建一个线程
                thread.Start();//开始一个线程

                for (int i = 0; i < 1000; i++)//主线程执行循环
                {
                    Console.Write("A");
                }

                Console.ReadLine();
            }
            static void ThreadA()
            {
                for (int i = 0; i < 1000; i++)
                {
                    Console.Write("B");
                }
            }
}
```

#### 创建线程的方法

```C#
static void CreateThread1()  //第一种创建线程方法   逆向思维创建线程
{
    //逆向思维：因为我们要创建线程，那么我们就要创建Thread WorkThread = new Thread();
    //但是，Thread内的参数要一个ThreadStart类型的入口委托，所以创建ThreadStart entryPoint = new ThreadStart()
    //但是，TreadStart内的参数要一个无返回值的函数，所以创建static void ThreadA() 方法
    ThreadStart entryPoint = new ThreadStart(ThreadA);   //创建线程入口委托  第二步
    Thread WorkThread = new Thread(entryPoint);    //创建线程  第三步
    WorkThread.Start();   //开启线程
}

static void ThreadA()    //   线程执行循环   第一步
{
    for (int i = 0; i < 1000; i++)
    {
        Console.Write("B");
    }
}

static void CreateThread2()          //第二种创建线程方法  匿名委托
{
    Thread thread = new Thread(delegate()    //匿名入口委托
    {
        for (int i = 0; i < 1000; i++)
        {
            Console.Write("C");
        }
    });
    thread.Start();
}

static void CreateThread3()   //第三种创建线程方法   拉姆达表达式
{

    //拉姆达表达式
    Thread thread = new Thread(() =>
    {
        for (int i = 0; i < 1000; i++)
        {
            Console.Write("D");
        }
    });
    thread.Start();
}

static void CreateThread4()   //第四种创建线程的方法   直接传函数
{
    Thread thread = new Thread(ThreadE);
    thread.Start();
}

static void ThreadE()
{
    for (int i = 0; i < 1000; i++)
    {
        Console.Write("E");
    }
}
```

#### 线程的优先级

```
计算机中经常会多个任务同时运行，其中总有一些看起来更紧急，更需要优先完成。比如我们现在有两个任务，一个任务是下载一部电影，另一个任务是检测用户的输入。显然及时响应用户操作应具有更高的优先级，因为我们不能让用户等得太久。
线程的优先级可以通过Thread类Priority属性设置，Priority属性是一个ThreadPriority型枚举，
```

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/%E5%A4%9A%E7%BA%BF%E7%A8%8B/%E7%BA%BF%E7%A8%8B%E7%9A%84%E6%A6%82%E5%BF%B5/images/Image2.png)

普通线程的优先级默认认为Normal(正常的)；如果想有更高的优先级，可设置为AboveNormal（高级的）或Highest;（最高级的）如果想有较低的优先级，可设置为BelowNormal（低级的）或Lowest（最低级的）.

```C#
class Program
{
    static void Main(string[] args)
    {

        //1.线程A
        ThreadStart threadPoint = new ThreadStart(TestA);
        Thread threadA = new Thread(threadPoint);


        //线程B
        Thread threadB = new Thread(delegate ()
        {
            for (int i = 0; i < 500000000; i++)
            {
                if (i % 100000 == 0)
                {
                    Console.Write('B');
                }
            }

        });

        //3.线程的优先级
        //  threadA.Priority = ThreadPriority.Highest;
        //  threadB.Priority = ThreadPriority.Lowest;

        //启动线程
        threadA.Start();
        threadB.Start();

        //2.工作线程和主线程的并发运行
        for (int i = 0; i <= 500000000; i++)
        {
            if (i % 100000 == 0)
            {
                Console.Write('M');
            }

        }
        Console.ReadKey();

    }
    static void TestA()
    {
        for (int i = 0; i < 500000000; i++)
        {
            if (i % 100000 == 0)
            {
                Console.Write('A');
            }
        }
    }
}
```

#### 线程的插入

Thread类的Join()方法能够将两个交易执行的线程合并为顺序执行的线程，比如在线程B中调用了线程A的Join()方法，线程A将插入线程B之前，直到线程A执行完毕后，才会继续执行线程B,

```C#
 //4.插入线程A
 threadA.Join();
 for (int i = 0; i < 500000000; i++)
 {
     if (i % 100000 == 0)
     {
         Console.Write('b');
     }
 }
```
当我们在线程B中调用ThreadA.Join()时，该方法只有在线程ThreadA执行完成之后才返回。Join()方法还可以接受一个表示毫秒数的参数，
当达到指定时间后，如果线程A还没运行完毕，那么Join方法将返回，这时线程A和线程B再次处于交替运行状态中。

【例子】

```C#
线程的插入
Thread threadB = new Thread(ThreadA);  //创建一个线程
threadB.Start();  //开始线程

Thread threadC = new Thread(delegate ()    //匿名入口委托
{
    for (int i = 0; i < 1000; i++)
    {
        Console.Write("C");
    }
});
threadC.Start();

//拉姆达表达式
Thread threadD = new Thread(() =>
{
    threadB.Join();  //插入线程，执行完插入的线程再执行此线程
    for (int i = 0; i < 1000; i++)
    {
        Console.Write("D");
    }
});
threadD.Start();

ThreadStart entryPoint = new ThreadStart(ThreadE);
Thread threadE = new Thread(entryPoint);
threadE.Start();
```
#### 线程状态

在线程的生命周期中，线程的状态并非是一成不变的，它可能经历多种状态。线程的状态由Thread类的ThreadState属性表示。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/%E5%A4%9A%E7%BA%BF%E7%A8%8B/%E7%BA%BF%E7%A8%8B%E7%9A%84%E6%A6%82%E5%BF%B5/images/Image3.png)

一旦线程被创建，它就处于某一个状态中，而且可以同时处于多个状态中。比如线程在WaitSleepJoin状态时收到Abrted()请求时，将同时处于WaitSleepJoin状态和AbortRequested状态。
下图演示了线程状态间的转换规律。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/%E5%A4%9A%E7%BA%BF%E7%A8%8B/%E7%BA%BF%E7%A8%8B%E7%9A%84%E6%A6%82%E5%BF%B5/images/Image4.png)

当一个线程被创建后，它就处于Unstarted状态，直到调用了 StartO方法为止。但处于 Running状态的线程也不是一定正在被CPU执行，可能该线程的时间片刚刚用完，CPU正 在处理其他线程，过一段时间后才会处理它。

有三种方法使线程由Running状态变为WaitSleepJoin状态。第一种情况是为了保持线 程间的同步而使之处于等待状态，这一点将在21.6节讲到。第二种情况是线程调用了 SleepO 方法而处于睡眠状态，当达到指定的睡眠时间以后，线程将会回到Running状态。第三种 情况是调用了 J〇in()方法，比如在线程A的代码中调用了线程B Join〇方法，线程A将处 于WaitSleepJoin状态，直到线程B结束，开始继续执行线程A时为止。

如果当线程处于Running状态时调用线程的SuspendO函数，线程将由Running状态变 为SuspendedRequested状态（请求挂起状态），线程一般会再继续执行几个指令，当确保 线程在安全的状态下时，挂起线程，这时线程变为Suspended状态（挂起状态）。调用线程 的Resume()方法,可使线程回到Running状态。

线程的状态是由操作系统的调度程序决定的，所以除了在一些调试方案中，我们一般 不使用线程的状态。但线程的Background状态除外，我们可以通过Thread类的 IsBackground属性把线程设置为Background状态。那么后台运行的线程有什么特別的地方 呢？其实后台线程跟前台线程只有一个区别，那就是后台线程不妨碍程序的终止。一旦 一个应用程序的所有前台线程都终止，CLR就通过调用任意一个存活中的后台线程的 AbortO方法来彻底终止应用程序。

另外Thread类还有一个IsAHve属性，这是一个只读属性，用来说明线程己经启动， 还没有结束。

#### 现成的同步

在大多数时候，计算机中都会有多个线程并发运行。有些线程之间是没有任何联系的， 它们各自独立运行，互不干扰，这类线程称为无关线程。而有些线程之间则是有联系的， 比如一个线程等待另一个线程的运算结果，两个线程共享一个资源等，这种线程称为相关 线程。
我们在网上观看在线视频，一个线程下载视频，另一个线程播放视频，这两个线程相 互合作，我们才能看到精彩的节目。由此可以看出线程的相关性集中体现在对同一资源的 访问上。我们把这种多个线程共享的资源称为临界资源（Critical Resource),临界资源可 以是内存中的一个变量，也可以是一个文件，也可以是一台打印机等。访问临界资源的代 码称为临界区（Critical Region )。

下面我来看一个如下图所示的例子，线程Writer不停地向缓冲区写入字符，线程 Reader则从缓冲区中读取相应的字符。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/%E5%A4%9A%E7%BA%BF%E7%A8%8B/%E7%BA%BF%E7%A8%8B%E7%9A%84%E6%A6%82%E5%BF%B5/images/Image5.png)

为了简单，我们假设缓冲区只能存放一个字符，当线程Writer第二次向缓冲区写入符后，第一次写入的字符就被抹掉了，所以线程Reader一定要及时读取字符。

```C#
using System.Threading;

   class Program
   {

       static void Main(string[] args)
       {
	      char buffer = null;
           //线程：写入者
           Thread Writer = new Thread(delegate ()
          {
              string sentence = "无可奈何花落去，似曾相识燕归来，校园香径独徘徊。";
                for(int i=0;i<24;i++)
				{
				   buffer = sentence[i];    //向缓冲区写入数据
                  Thread.Sleep(26);
				}
                 
          });
           //线程：读出者
           Thread Reader = new Thread(delegate ()
           {
               for (int i = 0; i < 24; i++)
               {

                   char ch = buffer;       //从缓冲区读取数据
                   Console.Write(ch);
                   Thread.Sleep(24);

               }
           });
           //启动线程
           Writer.Start();
           Reader.Start();
           Console.ReadKey();
      }
```

程序中，每次for循环都让线程睡眠一段时间，为什么要这样呢？这个因为两个线程的代码都很短，很可能在一个时间片就被执行完毕，为了确保出线交替，我们添加了睡眠语句后，以便能体现要说明的问题，输出的结果出现混乱。为什么会这样呢？其实原因很简单，线程Writer每写一个字 符，线程Reader就要读一个字符，两个线程步调一致，配合默契，才能得出正确的结果。 然而系统中往往有多个线程交替执行，它们被执行的时间是不确定的。可能线程Writer连 续写入好儿次字符，线程Reader才读取一次;也可能线程Reader读取了好几次，线程Writer 才写入一次。这样都得不到正确的结果。像这种需要两个线程协同T作才能共同完成一项 任务的情况叫做线程同步（Synchronization)。

如何才能保证两个线程同步呢？ .NET框架为我们提供了一系列的冋步类，最常用的 包括Interlocked (互锁）、Monitor (管程）和Mutex (互斥体）这里就以互锁举例：、

##### 互锁（Interlocked 类）

为了实现同步，线程Writer在写入字符之前应检查缓 冲区是否已满，若缓冲区为己满，就进行等待，直到缓冲区中的数据被进程Reader读取为 止：若缓冲区为空就写入字符，然后把缓冲区标记为满。线程Reader在读取字符时先检查 缓冲区是杏为空，如果缓冲区为空，就进行等待，直到进程Writer向缓冲区中写入数据为 止：若缓冲区为满，就读取字符，然后把缓冲区标记为空。
为了标记缓冲区是否已满，我们设计一个名为mimbetOfUsedSpace的计数器来记录缓 冲区中的己用空间。假设缓冲区的总大小为10,则mimberOfUsedSpace为0时表明缓冲区 为空，numberOfUsedSpace为10时表示缓冲区已满，numberOfUsedSpace为6时表示已经 用，6个空间，还有4个空间可用。当然为了使例子更加简单易懂，突出主要问题.我们 假设缓冲区只能容纳一个字符，这时numberOfUsedSpace为0时表明缓冲区为空， numberOfUsedSpace为1时表示缓冲区已满。

我们可以通过System.Threading命名空间的Interlocked类控制计数器，从而实现进程 的同步。Iterlocked类的部分方法如下表

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/%E5%A4%9A%E7%BA%BF%E7%A8%8B/%E7%BA%BF%E7%A8%8B%E7%9A%84%E6%A6%82%E5%BF%B5/images/Image6.png)

```C#
using System.Threading;

    class Program
    {

        //缓冲区，只能容纳一个字符
        private static char buffer;
        //标识量（缓冲区中已使用的空间，初始值为0）
        private static long numberOfUsedSpace = 0;
        static void Main(string[] args)
        {
            //线程：写入者
            Thread Writer = new Thread(delegate ()
           {
               string sentence = "无可奈何花落去，似曾相识燕归来，校园香径独徘徊。";
               for (int i = 0; i < 24; i++)
               {
                   //写入数据前检查缓冲区是否已满
                   //如果已满，就进行等待，直到缓冲区中的数据被进程Reader读取为止
                   while(Interlocked.Read(ref numberOfUsedSpace) == 1)
                   {
                       Thread.Sleep(50);
                   }
                   buffer = sentence[i];    //向缓冲区写入数据
                   //Thread.Sleep(26);
                   //写入数据后把缓冲区标记为满（由0变为1）
                   Interlocked.Increment(ref numberOfUsedSpace);
               }
           });
            //线程：读出者
            Thread Reader = new Thread(delegate ()
            {
                for (int i = 0; i < 24; i++)
                {
                    //读取数据前检查缓冲区是否为空
                    //如果为空，就进行等待，直到进程Writer向缓冲区中写入数据为止
                    while (Interlocked.Read(ref numberOfUsedSpace) == 0)
                    {
                        Thread.Sleep(50);
                    }

                    char ch = buffer;       //从缓冲区读取数据
                    Console.Write(ch);
                    //   Thread.Sleep(24);
                    Interlocked.Decrement(ref numberOfUsedSpace);

                }
            });

            //启动线程
            Writer.Start();
            Reader.Start();
            Console.ReadKey();
          }

}
```

作现代处理器中，Interlocked类的操作经常可以由单个指令实现，闵此它提供了性能 非常高的同步。即使那些不是由单个指令构成的操作，操作系统也保证这些操作是“原子” 的，即这些操作是不能被中断的，以确保同步的安全性。


# 练习

1：编写一个程序，开启3个线程，这3个线程的ID分别为A、B、C，每个线程将自己的ID在屏幕上打印10遍，要求输出结果必须按ABC的顺序显示；如：ABC0ABC1….依次递推。

2：有四个线程1、2、3、4。线程1的功能就是输出1，线程2的功能就是输出2，以此类推………现在有四个文件ABCD。初始都为空。现要让四个文件呈如下格式：

A：1 2 3 4 1 2….

B：2 3 4 1 2 3….

C：3 4 1 2 3 4….

D：4 1 2 3 4 1….







