#### 1 java内存模型

什么是java内存模型

##### 1.1 Happens-Before的7个规则

###### 1.1.1程序次序规则

在一个线程内，按照程序代码顺序，书写在前面的操作先行发生于书写在后面的操作。准确地说，应该是控制流顺序而不是程序代码顺序，因为要考虑分支、循环等结构。

###### 1.1.2  管程锁定规则

一个unlock操作先行发生于后面对同一个锁的lock操作。这里必须强调的是同一个锁，而"后面"是指时间上的先后顺序。

###### 1.1.3  volatile变量规则

对一个volatile变量的写操作先行发生于后面对这个变量的读操作，这里的"后面"同样是指时间上的先后顺序。

###### 1.1.4 线程启动规则

Thread对象的start()方法先行发生于此线程的每一个动作。

###### 1.1.5 线程终止规

线程中的所有操作都先行发生于对此线程的终止检测，我们可以通过Thread.join（）方法结束、Thread.isAlive（）的返回值等手段检测到线程已经终止执行。

###### 1.1.6 线程中断规则

对线程interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生，可以通过Thread.interrupted()方法检测到是否有中断发生。

###### 1.1.7 对象终结规则

一个对象的初始化完成(构造函数执行结束)先行发生于它的finalize()方法的开始。

##### 1.2.1 Happens-Before的1个特性：

传递性。

##### 1.2.1 Java内存模型底层怎么实现的？

主要是通过内存屏障(memory barrier)禁止重排序的，即时编译器根据具体的底层体系架构，将这些内存屏障替换成具体的 CPU 指令。对于编译器而言，内存屏障将限制它所能做的重排序优化。而对于处理器而言，内存屏障将会导致缓存的刷新操作。比如，对于volatile，编译器将在volatile字段的读写操作前后各插入一些内存屏障。