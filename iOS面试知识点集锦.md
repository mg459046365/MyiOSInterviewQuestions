---
typora-root-url: ../MyiOSInterviewQuestions
---

### 聊一聊 TCP 的滑动窗口协议？

TCP 引入了一些技术和设计来做网络流控，Sliding Window 是其中一个技术。前面我们说过，TCP 头里有一个字段叫 Window，又叫 Advertised-Window，这个字段是接收端告诉发送端自己还有多少缓冲区可以接收数据。于是发送端就可以根据这个接收端的处理能力来发送数据，而不会导致接收端处理不过来。

### 聊一聊TCP的拥塞控制相关过程？

TCP 的拥塞控制主要是四个算法：1）慢启动；2）拥塞避免；3）拥塞发生；4）快速恢复。

### 堆和栈的区别

1. **栈区(stack)**:由编译器自动分配释放 ,存放方法(函数)的参数值, 局部变量的值等，栈是向低地址扩展的数据结构，是一块连续的内存的区域。即栈顶的地址和栈的最大容量是系统预先规定好的。
2. **堆区(heap)**:一般由程序员分配释放, 若程序员不释放,程序结束时由OS回收，向高地址扩展的数据结构，是不连续的内存区域，从而堆获得的空间比较灵活。
3. **碎片问题**：对于堆来讲，频繁的new/delete势必会造成内存空间的不连续，从而造成大量的碎片，使程序效率降低。对于栈来讲，则不会存在这个问题，因为栈是先进后出的队列，他们是如此的一一对应，以至于永远都不可能有一个内存块从栈中间弹出.
4. **分配方式**：堆都是动态分配的，没有静态分配的堆。栈有2种分配方式：静态分配和动态分配。静态分配是编译器完成的，比如局部变量的分配。动态分配由alloca函数进行分配，但是栈的动态分配和堆是不同的，他的动态分配是由编译器进行释放，无需我们手工实现。
5. **分配效率**：栈是机器系统提供的数据结构，计算机会在底层对栈提供支持：分配专门的寄存器存放栈的地址，压栈出栈都有专门的指令执行，这就决定了栈的效率比较高。堆则是C/C++函数库提供的，它的机制是很复杂的。
6. **全局区(静态区)(static)**:全局变量和静态变量的存储是放在一块的,初始化的全局变量和静态变量在一块区域, 未初始化的全局变量和未初始化的静态变量在相邻的另一块区域。程序结束后有系统释放。
  文字常量区—常量字符串就是放在这里的。程序结束后由系统释放。
7. **程序代码区**:存放函数体的二进制代码

### 线程与进程的区别和联系

1. 一个程序至少要有一个进程,一个进程至少要有一个线程.
2. 进程:资源分配的最小独立单元,进程是具有一定独立功能的程序关于某个数据集合上的一次运行活动,进程是系统进行资源分配和调度的一个独立单位.
3. 线程:进程下的一个分支,是进程的实体,是CPU调度和分派(任务调度和执行)的基本单元,它是比进程更小的能独立运行的基本单位,线程自己基本不拥 有系统资源,只拥有一点在运行中必不可少的资源(程序计数器、一组寄存器、栈)，但是它可与同属一个进程的其他线程共享进程所拥有的全部资源。
4. 进程和线程都是由操作系统所体会的程序运行的基本单元，系统利用该基本单元实现系统对应用的并发性。
5. 进程和线程的主要差别在于它们是不同的操作系统资源管理方式。进程有独立的地址空间，一个进程崩溃后，在保护模式下不会对其它进程产生影响，而线程只是一个进程中的不同执行路径。线程有自己的堆栈和局部变量，但线程之间没有单独的地址空间，一个线程死掉就等于整个进程死掉，所以多进程的程序要比多线程的程序健壮，但在进程切换时，耗费资源较大，效率要差一些。
6. 但对于一些要求同时进行并且又要共享某些变量的并发操作，只能用线程，不能用进程。
7. 与线程"绑定"的是栈，用于存储自动变量。每一个线程建立的时候，都会新建一个默认栈与之配合。堆则是通常与进程有关，用于存储全局变量，进程建立的时候，会建立默认堆。于是，每一个线程都有自己的栈，然后访问所处的进程的堆。
8. 堆分为全局堆和局部堆。全局堆就是所有没有分配的空间，局部堆就是用户分配的空间。堆在操作系统对进程初始化的时候分配，运行过程中也可以向系统要额外的堆，但是记得用完了要记得还给操作系统(释放申请的内存空间)，不然会出现内存泄漏。
9. 栈是线程独有的，保存期运行状态和局部自动变量。栈在线程开始的时候初始化，每个线程的栈互相独立，因此，栈是thread safe(线程安全)的。操作系统在切换线程的时候自动的切换栈，就是切换SS/SEP寄存器。栈空间不需要在高级语言里面显示的分配和释放。

### 如何用HTTP实现长连接？

轮询：隔一段时间访问服务器，服务器不管有没有新消息都立刻返回。
 设置HTTP长连接，有过期时间：

在首部字段中设置Connection:keep-alive 和Keep-Alive: timeout=60，表明连接建立之后，空闲时间超过60秒之后，就会失效。如果在空闲第58秒时，再次使用此连接，则连接仍然有效，使用完之后，重新计数，空闲60秒之后过期。

设置HTTP长连接，无过期时间：
在首部字段中只设置Connection:keep-alive，表明连接永久有效。

### 数据库建表的时候索引有什么用？

创建索引可以大大提高系统的性能。

1. 通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性。
2. 可以大大加快数据的检索速度，这也是创建索引的最主要的原因。
3.  可以加速表和表之间的连接，特别是在实现数据的参考完整性方面特别有意义。
4. 在使用分组和排序子句进行数据检索时，同样可以显著减少查询中分组和排序的时间。
5.  通过使用索引，可以在查询的过程中，使用优化隐藏器，提高系统的性能.   

### 谈下Objective C都有哪些锁机制，你一般用哪个？

1. NSLock

iOS中对于资源抢占的问题可以使用<font color=red size=4>同步锁NSLock</font>来解决，使用时把需要加锁的代码（以后暂时称这段代码为”加锁代码“）放到NSLock的lock和unlock之间，一个线程A进入加锁代码之后由于已经加锁，另一个线程B就无法访问，只有等待前一个线程A执行完加锁代码后解锁，B线程才能访问加锁代码。

2. @synchronized互斥锁

使用@synchronized解决线程同步问题相比较NSLock要简单一些，日常开发中也更推荐使用此方法。

3. 使用GCD解决资源抢占问题

在GCD中提供了一种信号机制，也可以解决资源抢占问题（和同步锁的机制并不一样）。

```objective-c
@property (nonatomic,strong) dispatch_queue_t syncQueue;

//_syncQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
/* 这里应该使用自己创建的并发队列，因为苹果文档中指出，如果使用的是全局队列或者创建的不是并发队列，
则dispatch_barrier_async实际上就相当于dispatch_async，就达不到我们想要的效果了 */
_syncQueue = dispatch_queue_create("com.effetiveobjectivec.syncQueue", DISPATCH_QUEUE_CONCURRENT);

- (NSString *)someString {
    __block NSString *localSomeString;
    dispatch_sync(_syncQueue, ^{
        localSomeString = _someString;
    });
    return localSomeString;
}

- (void)setSomeString:(NSString *)someString {
    dispatch_barrier_async(_syncQueue, ^{
        _someString = someString;
    });
}

/**
 * 售卖火车票(线程安全),加锁方式一 dispatch_semaphore 自旋锁
 */
- (void)saleTicketSafe {
    while (1) {
        // 相当于加锁
        dispatch_semaphore_wait(semaphoreLock, DISPATCH_TIME_FOREVER);
        
        if (self.ticketSurplusCount > 0) {  //如果还有票，继续售卖
            self.ticketSurplusCount--;
            NSLog(@"%@", [NSString stringWithFormat:@"剩余票数：%d 窗口：%@", self.ticketSurplusCount, [NSThread currentThread]]);
            [NSThread sleepForTimeInterval:0.2];
        } else { //如果已卖完，关闭售票窗口
            NSLog(@"所有火车票均已售完");
            
            // 相当于解锁
            dispatch_semaphore_signal(semaphoreLock);
            break;
        }
        
        // 相当于解锁
        dispatch_semaphore_signal(semaphoreLock);
    }
}

```

4. 扩展--控制线程通信

由于线程的调度是透明的，程序有时候很难对它进行有效的控制，为了解决这个问题iOS提供了NSCondition来控制线程通信(同前面GCD的信号机制类似)。

5. iOS中的其他锁

NSRecursiveLock ：递归锁，有时候“加锁代码”中存在递归调用，递归开始前加锁，递归调用开始后会重复执行此方法以至于反复执行加锁代码最终造成死锁，这个时候可以使用递归锁来解决。使用递归锁可以在一个线程中反复获取锁而不造成死锁，这个过程中会记录获取锁和释放锁的次数，只有最后两者平衡锁才被最终释放。

NSDistributedLock：分布锁，它本身是一个互斥锁，基于文件方式实现锁机制，可以跨进程访问。

pthread_mutex_t：同步锁，基于C语言的同步锁机制，使用方法与其他同步锁机制类似。

### 聊下HTTP post的body体使用form-urlencoded和multipart/form-data的区别。

1. application/x-www-form-urlencoded：
    窗体数据被编码为名称/值对，这是标准且默认的编码格式。当action为get时候，客户端把form数据转换成一个字串append到url后面，用?分割。当action为post时候，浏览器把form数据封装到http body中，然后发送到server。

2. multipart/form-data：
    multipart表示的意思是单个消息头包含多个消息体的解决方案。multipart媒体类型对发送非文本的各媒体类型是有用的。一般多用于文件上传。
   multipart/form-data只是multipart的一种。目前常用的有以下这些类型(注：任何一种执行时无法识别的multipart子类型都被视为子类型"mixed")

### dSYM你是如何分析的

**方法一 使用XCode**

这种方法可能是最容易的方法了。

1. 要使用Xcode符号化 crash log，你需要下面所列的3个文件：
2. crash报告（.crash文件）
3. 符号文件 (.dsymb文件)
4. 应用程序文件 (appName.app文件，把IPA文件后缀改为zip，然后解压，Payload目录下的appName.app文件), 这里的appName是你的应用程序的名称。
5. 把这3个文件放到同一个目录下，打开Xcode的Window菜单下的organizer，然后点击Devices tab，然后选中左边的Device Logs。
6. 然后把.crash文件拖到Device Logs或者选择下面的import导入.crash文件。
7. 这样你就可以看到crash的详细log了。

**方法二 使用命令行工具symbolicatecrash**

1. 有时候Xcode不能够很好的符号化crash文件。我们这里介绍如何通过symbolicatecrash来手动符号化crash log。
2. 在处理之前，请依然将“.app“, “.dSYM”和 ".crash"文件放到同一个目录下。现在打开终端(Terminal)然后输入如下的命令：
3. export DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer
4. 然后输入命令：
5. /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/PrivateFrameworks/DTDeviceKitBase.framework/Versions/A/Resources/symbolicatecrash appName.crash appName.app > appName.log
6. 现在，符号化的crash log就保存在appName.log中了。

**方法三 使用命令行工具atos**

1. 如果你有多个“.ipa”文件，多个".dSYMB"文件，你并不太确定到底“dSYMB”文件对应哪个".ipa"文件，那么，这个方法就非常适合你。
2. 特别当你的应用发布到多个渠道的时候，你需要对不同渠道的crash文件，写一个自动化的分析脚本的时候，这个方法就极其有用。
3. 具体方法 请百度

### 怎么防止反编译？

1. 本地数据加密。

​       iOS应用防反编译加密技术之一：对NSUserDefaults，sqlite存储文件数据加密，保护帐号和关键信息

2. URL编码加密。

​    iOS应用防反编译加密技术之二：对程序中出现的URL进行编码加密，防止URL被静态分析

3. 网络传输数据加密。

​    iOS应用防反编译加密技术之三：对客户端传输数据提供加密方案，有效防止通过网络接口的拦截获取数据

4. 方法体，方法名高级混淆。

​    iOS应用防反编译加密技术之四：对应用程序的方法名和方法体进行混淆，保证源码被逆向后无法解析代码

5. 程序结构混排加密。

​      iOS应用防反编译加密技术之五：对应用程序逻辑结构进行打乱混排，保证源码可读性降到最低

### 为什么说TCP是可靠连接

超时重发，丢弃重复数据，校验数据，流量控制

### Scoket连接和HTTP连接的区别

HTTP协议是基于TCP连接的，是应用层协议，主要解决如何包装数据。Socket是对TCP/IP协议的封装，Socket本身并不是协议，而是一个调用接口（API），通过Socket，我们才能使用TCP/IP协议。

**HTTP连接**：短连接，客户端向服务器发送一次请求，服务器响应后连接断开，节省资源。服务器不能主动给客户端响应（除非采用HTTP长连接技术），iPhone主要使用类NSURLConnection。

**Socket连接**：长连接，客户端跟服务器端直接使用Socket进行连接，没有规定连接后断开，因此客户端和服务器段保持连接通道，双方可以主动发送数据，一般多用于游戏.Socket默认连接超时时间是30秒，默认大小是8K（理解为一个数据包大小）。

### 基于CTMediator的组件化方案，有哪些核心组成？

假如主APP调用某业务A，那么需要以下组成部分：

- CTMediator类，该类提供了函数 - (id)performTarget:(NSString *)targetName action:(NSString *)actionName params:(NSDictionary *)params shouldCacheTarget:(BOOL)shouldCacheTarget;
   这个函数可以根据targetName生成对象，根据actionName构造selector，然后可以利用performSelector:withObject:方法，在目标上执行动作。
- 业务A的实现代码，另外要加一个专门的类，用于执行Target Action
   类的名字的格式：Target_%@，这里就是Target_A。
   这个类里面的方法，名字都以Action_开头，需要传参数时，都统一以NSDictionary*的形式传入。
   CTMediator类会创建Target类的对象，并在对象上执行方法。
- 业务A的CTMediator扩展
   扩展里声明了所有A业务的对外接口，参数明确，这样外部调用者可以很容易理解如何调用接口。
   在扩展的实现里，对Target, Action需要通过硬编码进行指定。由于扩展的负责方和业务的负责方是相同的，所以这个不是问题。

### HTTPS的加密原理

1. 服务器端用非对称加密(RSA)生成公钥和私钥

2. 然后把公钥发给客户端, 服务器则保存私钥

3. 客户端拿到公钥后, 会生成一个密钥, 这个密钥就是将来客户端和服务器用来通信的钥匙

4. 然后客户端用公钥对密钥进行加密, 再发给服务器

5. 服务器拿到客户端发来的加密后的密钥后, 再使用私钥解密密钥, 到此双方都获得通信的钥匙

### 哈希原理

散列表（Hash table，也叫哈希表），是根据关键码值(Key value)而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做散列函数，存放记录的数组叫做散列表。

给定表M，存在函数f(key)，对任意给定的关键字值key，代入函数后若能得到包含该关键字的记录在表中的地址，则称表M为哈希(Hash）表，函数f(key)为哈希(Hash) 函数。

**哈希概念**:哈希表的本质是一个数组，数组中每一个元素称为一个箱子(bin)，箱子中存放的是键值对。

### 编程中的六大设计原则？

##### 1.单一职责原则

通俗地讲就是一个类只做一件事

-  `CALayer`：动画和视图的显示。
-  `UIView`：只负责事件传递、事件响应。

##### 2.开闭原则

对修改关闭，对扩展开放。 要考虑到后续的扩展性，而不是在原有的基础上来回修改

##### 3.接口隔离原则

使用多个专门的协议、而不是一个庞大臃肿的协议

- `UITableviewDelegate`
- `UITableViewDataSource`

##### 4.依赖倒置原则

抽象不应该依赖于具体实现、具体实现可以依赖于抽象。 调用接口感觉不到内部是如何操作的

##### 5.里氏替换原则

父类可以被子类无缝替换，且原有的功能不受任何影响

例如 KVO

##### 6.迪米特法则

一个对象应当对其他对象尽可能少的了解，实现高聚合、低耦合

### 并行和并发的区别

并发行和并行性的区别可以用馒头做比喻。前者相当于一个人同时吃三个馒头和三个人同时吃一个馒头。

**并发性（Concurrence）**：指两个或两个以上的事件或活动在同一时间间隔内发生。并发的实质是一个物理CPU(也可以多个物理CPU) 在若干道程序之间多路复用，并发性是对有限物理资源强制行使多用户共享以提高效率。
 **并行性（parallelism）**指两个或两个以上事件或活动在同一时刻发生。在多道程序环境下，并行性使多个程序同一时刻可在不同CPU上同时执行。
 **区别**：一个处理器同时处理多个任务和多个处理器或者是多核的处理器同时处理多个不同的任务。
 前者是逻辑上的同时发生（simultaneous），而后者是物理上的同时发生。
 **两者的联系**：并行的事件或活动一定是并发的，但反之并发的事件或活动未必是并行的。并行性是并发性的特例，而并发性是并行性的扩展。

### AFNetworking 底层原理分析

AFNetworking主要是对NSURLSession和NSURLConnection(iOS9.0废弃)的封装,其中主要有以下类:

1. AFHTTPRequestOperationManager：内部封装的是 NSURLConnection, 负责发送网络请求, 使用最多的一个类。(3.0废弃)
2. AFHTTPSessionManager：内部封装是 NSURLSession, 负责发送网络请求,使用最多的一个类。
3.  AFNetworkReachabilityManager：实时监测网络状态的工具类。当前的网络环境发生改变之后,这个工具类就可以检测到。
4. AFSecurityPolicy：网络安全的工具类, 主要是针对 HTTPS 服务。
5.  AFURLRequestSerialization：序列化工具类,基类。上传的数据转换成JSON格式
    (AFJSONRequestSerializer).使用不多。
6.  AFURLResponseSerialization：反序列化工具类;基类.使用比较多:
7.  AFJSONResponseSerializer; JSON解析器,默认的解析器.
8.  AFHTTPResponseSerializer; 万能解析器; JSON和XML之外的数据类型,直接返回二进制数据.对服务器返回的数据不做任何处理.
9. AFXMLParserResponseSerializer; XML解析器;

### SDWebImage如何解决tableView的复用时出现图片错乱问题的呢？

解决tableView复用错乱问题：每次都会调UIImageView+WebCache文件中的 [self sd_cancelCurrentImageLoad];

### 与 NSURLConnection 相比，NSURLsession 改进哪些?

1. 可以配置每个 session 的缓存，协议，cookie，以及证书策略（credential policy），甚至跨程序共享这些信息

2. session task，它负责处理数据的加载以及文件和数据在客户端与服务端之间的上传和下载。
3. NSURLSessionTask 与 NSURLConnection 最大的相似之处在于它也负责数据的加载，最大的不同之处在于所有的 task 共享其创造者 NSURLSession 这一公共委托者（common delegate）

### 对称加密（AES）和非对称加密(RSA)

1. 对称加密加密与解密使用的是同样的密钥，所以速度快，但由于需要将密钥在网络传输，所以安全性不高。
2. 非对称加密使用了一对密钥，公钥与私钥，所以安全性高，但加密与解密速度慢。
3. 解决的办法是将对称加密的密钥使用非对称加密的公钥进行加密，然后发送出去，接收方使用私钥进行解密得到对称加密的密钥，然后双方可以使用对称加密来进行沟通。

### Bugly检查卡顿的依据和上报时机是什么?

iOS 卡顿检查的依据是监控主线程 Runloop 的执行，观察执行耗时是否超过预定阀值(默认阀值为3000ms) 在监控到卡顿时会立即记录线程堆栈到本地，在App从后台切换到前台时，执行上报。

#### Runloop逻辑

一个线程的的消息事件处理都是依赖于NSRunLoop来驱动，所以要知道线程在调用什么方法就需要从Runloop入手，其核心方法CFRunLoopRun简化后的主要逻辑大概如下：

```objective-c
int32_t __CFRunLoopRun()
{
    //通知即将进入runloop
    __CFRunLoopDoObservers(KCFRunLoopEntry);

    do
    {
        // 通知将要处理timer和source
        __CFRunLoopDoObservers(kCFRunLoopBeforeTimers);
        __CFRunLoopDoObservers(kCFRunLoopBeforeSources);

        __CFRunLoopDoBlocks();  //处理非延迟的主线程调用
        __CFRunLoopDoSource0(); //处理UIEvent事件

        //GCD dispatch main queue
        CheckIfExistMessagesInMainDispatchQueue();

        // 即将进入休眠
        __CFRunLoopDoObservers(kCFRunLoopBeforeWaiting);

        // 等待内核mach_msg事件
        mach_port_t wakeUpPort = SleepAndWaitForWakingUpPorts();

        // Zzz...

        // 从等待中醒来
        __CFRunLoopDoObservers(kCFRunLoopAfterWaiting);

        // 处理因timer的唤醒
        if (wakeUpPort == timerPort)
            __CFRunLoopDoTimers();

        // 处理异步方法唤醒,如dispatch_async
        else if (wakeUpPort == mainDispatchQueuePort)
            __CFRUNLOOP_IS_SERVICING_THE_MAIN_DISPATCH_QUEUE__()

        // UI刷新,动画显示
        else
            __CFRunLoopDoSource1();

        // 再次确保是否有同步的方法需要调用
        __CFRunLoopDoBlocks();

    } while (!stop && !timeout);

    //通知即将退出runloop
    __CFRunLoopDoObservers(CFRunLoopExit);
}
```

NSRunloop调用方法主要就是在KCFRunLoopBeforeSources和KCFRunLoopBeforeWaiting之间，还有KCFRunloopAfterWaiting之后，也就是如果我们发现这两个时间耗时太长，那么就可以判定主线程卡顿。

#### 量化卡顿的程度

要监控NSRunloop的状态，我们需要使用到CFRunloopObserverRef，通过它可以实时获的这些状态值得变化，具体使用如下：

```objective-c
static void runLoopObserverCallBack(CFRunLoopObserverRef observer, CFRunLoopActivity activity, void *info)
{
    MyClass *object = (__bridge MyClass*)info;
    object->activity = activity;
}

- (void)registerObserver
{
    CFRunLoopObserverContext context = {0,(__bridge void*)self,NULL,NULL};
    CFRunLoopObserverRef observer = CFRunLoopObserverCreate(kCFAllocatorDefault,
                                                            kCFRunLoopAllActivities,
                                                            YES,
                                                            0,
                                                            &runLoopObserverCallBack,
                                                            &context);
    CFRunLoopAddObserver(CFRunLoopGetMain(), observer, kCFRunLoopCommonModes);
}
```

只要另外开启在开启一个县城，实时计算这两个状态区域之间的耗时是否到达某个阈值，就能揪出这些性能杀手。

为了让计算更加精确，需要让子线程更及时的获取主线程NSRunloop状态变化，所以dispatch_semaphore_t是个不错的选择，另外卡顿需要覆盖到多次连续小卡顿和单次长时间卡顿两种情况，所以判断条件也需要做适当优化，将上面两个方法添加计算的逻辑如下：

```objective-c
static void runLoopObserverCallBack(CFRunLoopObserverRef observer, CFRunLoopActivity activity, void *info)
{
    MyClass *object = (__bridge MyClass*)info;

    // 记录状态值
    object->activity = activity;

    // 发送信号
    dispatch_semaphore_t semaphore = moniotr->semaphore;
    dispatch_semaphore_signal(semaphore);
}

- (void)registerObserver
{
    CFRunLoopObserverContext context = {0,(__bridge void*)self,NULL,NULL};
    CFRunLoopObserverRef observer = CFRunLoopObserverCreate(kCFAllocatorDefault,
                                                            kCFRunLoopAllActivities,
                                                            YES,
                                                            0,
                                                            &runLoopObserverCallBack,
                                                            &context);
    CFRunLoopAddObserver(CFRunLoopGetMain(), observer, kCFRunLoopCommonModes);

    // 创建信号
    semaphore = dispatch_semaphore_create(0);

    // 在子线程监控时长
    dispatch_async(dispatch_get_global_queue(0, 0), ^{
        while (YES)
        {
            // 假定连续5次超时50ms认为卡顿(当然也包含了单次超时250ms)
            long st = dispatch_semaphore_wait(semaphore, dispatch_time(DISPATCH_TIME_NOW, 50*NSEC_PER_MSEC));
            if (st != 0)
            {
                if (activity==kCFRunLoopBeforeSources || activity==kCFRunLoopAfterWaiting)
                {
                    if (++timeoutCount < 5)
                        continue;

                    NSLog(@"好像有点儿卡哦");
                }
            }
            timeoutCount = 0;
        }
    });
}
```

#### 记录卡顿的函数调用

监控到了卡顿现场，当然下一步便是记录此时的函数调用信息，此处可以使用一个第三方的Crash收集组件PLCrashReporter，他不仅可以收集Crash信息也可以用书实时获取各线程的调用堆栈信息，如下

```objective-c
PLCrashReporterConfig *config = [[PLCrashReporterConfig alloc] initWithSignalHandlerType:PLCrashReporterSignalHandlerTypeBSD
                                                                   symbolicationStrategy:PLCrashReporterSymbolicationStrategyAll];
PLCrashReporter *crashReporter = [[PLCrashReporter alloc] initWithConfiguration:config];

NSData *data = [crashReporter generateLiveReport];
PLCrashReport *reporter = [[PLCrashReport alloc] initWithData:data error:NULL];
NSString *report = [PLCrashReportTextFormatter stringValueForCrashReport:reporter
                                                          withTextFormat:PLCrashReportTextFormatiOS];

NSLog(@"------------\n%@\n------------", report);
```

当检测到卡顿时，抓取堆栈信息，然后客户端做一些过滤处理，便可以上报服务器，收集一定量的卡顿数据后经过分析便能准确定位需要优化的逻辑。

### 移动端网络常见问题及优化对策

#### 问题

通过对数据的分析以及调研、用户反馈，发现移动端网络常常存在如下的问题：

- 网络成功率低，经常请求失败
- 用户反馈 DNS 劫持，数据被篡改，出现广告和请求超时等情况
- 网络延迟较长，且存在较多的长尾数据
- 经过数据分析，发现长连的时间明显比短连的时间少 100ms 左右（短连指的是，经过DNS解析、 TCP 握手、 SSL 握手等一系列的过程建立连接，长连指的是直接复用前者的连接通道）
- 网络经常出现抖动，本来大部分请求都是 100ms 左右，突然冒出来一两千毫秒的，甚至有10、20秒的延迟情况
- `HTTP 1.1`的`head of blocking（排头阻塞）` 情况存在，一个网络抖动，很容易影响后续的请求，导致一连串的延迟较高请求（`head of blocking`：指的是在 HTTP 1.1 中，如果你发出1、2、3 三个网络请求，那么 `Response` 的顺序 2、3 要在第一个网络请求之后）
- 传输的 `Payload` 太大，延迟高，易超时
- 苹果要求`HTTPS` ，此时加入的 `SSL` 握手较耗时

#### 解决方案

* 对于DNS劫持，使用HTTPDNS或者内置SeverIP列表。

  客户端直接访问HttpDNS接口，获取业务的域名配置管理系统上配置的访问延迟最优的IP，获取到IP后就直接往此IP发送业务协议请求，不需要使用本地运营商解析域名，所以从根本上避免了劫持问题，同时可以降低网络延迟，提高连接成功率。而建立Server IP列表，是在本地缓存一个IP的映射表，此表在App启动时动态下发更新，访问服务器时直接拿出IP发出请求。

* 对Payload(有效数据或者关键数据，类似与请求返回的data字段对应的内容)进行数据压缩， 使用ProtoBuf协议,减少数据量

* 使用HTTP2.0, HTTP2.0通过头部压缩的方式也帮你减少了传输的Payload

* 域名合并,HTTP的通道复用是基于域名划分的，如果域名只有几个多数请求都可以在长连接通道进行，这样可以降低延迟、增加成功率。

* 长连接，业务请求可以复用长连接通道，加快访问速度。对于建立长连接的时机，可以考虑冷启动，前后台切换、网络切换

* 接入HTTP2.0， 解决了HTTP1.1的`head of blocking(排头阻塞)`，降低了网络延迟，提供了更强大的多路复用技术(**将多个请求复用同一个tcp链接中**)，加入了流量控制、新的二进制格式、Server Push、请求优先级和依赖等特性。

* 建立多通道

* 加入CDN加速，动静支援分配

* 对于买点的数据，合并请求，减少流量

* App网络诊断

* 根据网络情况，动态设置超时时间

### 图片渲染到屏幕的过程

1. 读取文件
2. 计算frame
3. 将压缩的图片数据解码成未压缩的位图形式，默认在主线程，解码非常耗时。SDWebImage和YYImage，都是在子线程提前对图片进行强制解压缩，得到一个新的解压缩后的位图。其中用到的最核心的函数就是**`CGBitmapContextCreate`**
   * 使用CGBitmapContextCreate函数创建一个位图上下文
   * 使用CGContextDrawImage函数将原始位图绘制到上下文中
   * 使用CGBitmapContextCreateImage函数创建一张新的解压缩后的位图
   * 返回图片

4. 解码后纹理图片位图数据通过数据总线交给GPU
5. GPU获取图片Frame
6. 顶点变换计算
7. 光栅化
8. 根据纹理坐标获取每个像素点的颜色值(如果出现透明值需要将每个像素点的颜色*透明度值)
9. 渲染到帧缓冲区
10. 渲染到屏幕

### HTTP报头信息有哪些

![Http请求报头信息](/相关图片/Http请求报头信息.png)

请求报文和响应报文都是以下4部分组成的

1. 请求行
2. 请求头
3. 空行
4. 消息主体

**请求行**

格式: Method Request-URI HTTP-Version 结尾符

结尾符一般用\r\n

**请求头**

**通用报头**

既可以出现在请求报头，也可以出现在响应报头

Date: 表示消息产生的日期和时间

Connection: 允许发送指定连接的选项，例如指定连接是连续的，或者指定"close"选项，通知服务器，在响应完成后，关闭连接

Cache-Control: 用于指定缓存指令，缓存指令是单向的(响应中出现的缓存指令在请求中未必会出现)，且是独立一的(一个消息的缓存指令不会影响另一个消息处理的缓存机制)

**请求报头**

请求报头通知服务器关于客户端请求的信息，典型的请求有：

Host：请求的主机名，允许多个域名同处一个IP地址，即虚拟主机

User-Agent:  发送请求的浏览器类型、操作系统等信息

Accept: 客户端可识别的内容类型列表，用户指定客户端接收哪些类型的消息

Accept-Encoding: 客户端可识别的数据编码

Accept-Language: 表示浏览器所支持的语言类型

Connection: 允许客户端和服务器指定与请求/响应连接有关的选项，例如Keep-Alive表示保持连接

Transfer-Encoding: 告知接收端为了保证报文的可靠传输，对报文采用什么编码方式

**响应报头**

用于服务器传递自身信息的响应，常见的响应报头：

Location: 用于重定向接收者到的一个新的位置，常用在更换域名的时候

Server: 包含服务器可用来处理请求的系统信息，与User-Agent请求报头是相对应的

**实体报头**

实体报头用来定于被传送资源的信息，既可以用于请求也可用于响应。请求和响应消息都可以传送一个实体，常见的实体报头为：

Content-Type：发送给接收者的实体正文的媒体类型

Content-Lenght：实体正文的长度

Content-Language：描述资源所用的自然语言，没有设置则该选项则认为实体内容将提供给所有的语言阅读

Content-Encoding：实体报头被用作媒体类型的修饰符，它的值指示了已经被应用到实体正文的附加内容的编码，因而要获得Content-Type报头域中所引用的媒体类型，必须采用相应的解码机制。

Last-Modified：实体报头用于指示资源的最后修改日期和时间

Expires：实体报头给出响应过期的日期和时间

#### **空行**

http协议规定的格式，一般采用\r\n

#### **消息主体**

一般用于http的post method。通过实体报头规定消息主体的格式内容、

例如 Content-Type=text/plain

该实体报头规定了消息主体的数据是纯文本格式

常见的还有

Content-Type=application/x-www-form-urlencoded，定义为Key=value格式

Content-Type=application/json，定义为序列化为的json字符串

Content-Type= multipart/form-data，定义为表单数据提交，该格式比较复杂，详细解释一下。

**multipart/form-data**

1. 该格式是post的常见提交方式，也就是说是由post方法来组合实现的

2. 使用该提交方法需要规定一个内容分割符用于分割请求体中的多个post的内容，如文件内容和文本内容自然需要分割开来，不然接收方就无法正常解析和还原这个文件了。具体的头信息如下：

Content-Type: multipart/form-data; boundary=${bound} 

其中${bound}是自定义的分隔符，一般情况用一长串不会和业务数据重复的字符串表示 ，例如9431149156168

3. 分割符前面需要加上--

4. 最后的分割符后面也需要加上—

5. 所有的数据请求头和数据之间都用\r\n\r\n分开，两个数据间用 --${bound}\r\n分开

### 分类和扩展有什么区别？可以分别用来做什么？分类有哪些局限性？分类的结构体里面有哪些成员？

分类优点：

1. 分类原则上只能添加方法(能添加属性的原因只是通过runtime的object_setAssociatedObject和objc_getAssociatedObject方法添加setter/getter方法)

2. 分类中方法没有被实现编译器不会有任何警告，因为**分类是在运行时添加到类中**的

3. 分类可以减少单个文件的体积

4. 分类可以把不同功能组织到不同的Category里面

5. 分类可以由多个开发者共同完成一个类

6. 分类可以按需加载想要的category

7. 分类可以对私有方法向前声明

   

分类缺点：

1. 无法向分类中添加新的实例变量
2. 名称冲突，即当类别中的方法与原始类方法名称冲突时，类别具有更高级的优先级，覆盖掉本类中的方法
3. 如果多个分类中都有和原有类中同名的方法，那么调用该方法的时候执行谁由编译器决定，编译器会执行最后一个参与编译的分类中的方法



扩展优点：

1. 不仅可以增加方法，还可以增加实例变量(或者属性)，只是该实例变量默认是@private类型的(使用范围只能在自身类，而不是子类或者其他地方)
2. 声明的方法没被实现，编译器会报警，因为**扩展是在编译阶段被添加到类中**
3. 定义在.m文件中的扩展方法为私有的，定义在.h文件中的扩展方法为公有的。扩展是在.m文件中声明私有方法的非常好的方式

[Category的原理](https://www.jianshu.com/p/fa66c8be42a2)

分类(类别)**Category**结构体如下图：

```objective-c
struct category_t {

        const char *name; //分类的名字

        classref_t cls;  //分类对应的类

        struct method_list_t *instanceMethods; // 分类中添加的实例方法列表

        struct method_list_t *classMethods; // 分类中添加的类方法列表

        struct protocol_list_t *protocols; // 分类中添加的协议列表

        struct property_list_t *instanceProperties; // 分类中添加的属性列表

        // Fields below this point are not always present on disk.

        struct property_list_t *_classProperties;

        

        method_list_t *methodsForMeta(bool isMeta) {

            if (isMeta) return classMethods;

            else return instanceMethods;

        }

        

        property_list_t *propertiesForMeta(bool isMeta, struct header_info *hi);

    };
```

### Copy与MutableCopy知识点

在非集合类对象中，对不可变对象进行copy操作，是指针复制(浅复制)，mutableCopy操作时内容复制（深复制）。对可变对象进行操作时copy和mutableCopy都是内容复制(深复制)。

### 在有了自动合成属性实例变量之后，@synthesize还有哪些使用场景？

如果使用了属性，那么编译器就会自动编写访问属性所需的方法，此过程叫"自动合成"。

什么时候不会自动合成呢？

1. 同时重写了setter/getter方法
2. 重写了只读的getter方法
3. 使用了@dynamic
4. 在@protocol中定义的属性
5. 在category定义的属性
6. 重载的属性

### 讲一下atomic的实现机制；为什么不能保证绝对的线程安全（最好可以结合场景来说）？

atomic是安全的, 而且是绝对安全的。线程是不一定安全的。atomic所说的线程安全只是保证了getter和setter存取方法的线程安全，并不能保证整个对象是线程安全的。
例如：当线程A进行写操作，这时其他线程的读或者写操作会因为该操作而等待。当A线程的写操作结束后，B线程进行写操作，然后当A线程需要读操作时，却获得了在B线程中的值，这就破坏了线程安全，如果有线程C在A线程读操作前release了该属性，那么还会导致程序崩溃。所以仅仅使用atomic并不会使得线程安全，我们还要为线程添加lock来确保线程的安全。
又例如：如果一个线程循环的读数据，一个线程循环写数据，那么肯定会产生内存问题，因为这和setter、getter没有关系。如使用[self.arr objectAtIndex:index]就不是线程安全的。好的解决方案就是加锁。

### 如何手动触发一个value的KVO

自动触发是指：在注册KVO之前设置一个初始值，注册之后，设置一个不一样的值，就可以触发了。 

 手动触发的原理是：
 键值观察者通知依赖于两个方法：willChangeValueForKey:和 didChangeValueForKey：。在一个被观察属性发生改变之前，willChangeValueForKey：一定会被调用，这就会记录旧的值。而当改变发生后，didChangeValueForKey：会被调用，继而observeValueForKey：ofObject：change：context：也会被调用。如果可以手动实现这些调用，就可以实现“手动触发”了。
 那么手动触发的使用场景是什么？一般我们只希望能控制“回调的调用时机”时才会这么做。比如手动触发self.now：
 [self willChangeValueForKey:@"now"]; “手动触发self.now的KVO”，必写。 [self didChangeValueForKey:@"now"];  “手动触发self.now的KVO”，必写。

### iOS系统架构以及常用框架

iOS的系统架构分为四层，由上到下依次为：可触摸层(Cocoa Touch layer)、媒体层(Media layer)、核心服务层(Core Services layer)、核心操作系统层(Core OS layer)

#### 触摸层(Cocoa Touch layer)

为应用程序开发提供了各种常用的框架并且大部分框架与界面有关，本质上来说它负责用户在iOS设备上的触摸交互操作。包含以下组件：Multi-Touch Events、Core Motion、Camera、View Hierarchy、Localization 、Alert

WebViews、ImagePicker 、Multi-Touch Controls、NotificationCenter本地和远程通知、iAd广告框架、消息UI框架、地图框架、自动适配等。

#### 媒体层(Media layer)

提供应用中视听，图形音视频相关的技术，如图形相关的CoreGraphics, CoreImage, GLKit, OpenGL, OpenGL ES,CoreText, ImageIO等等。声音技术相关的CoreAudio,OpenAL,AVFoundation,视频相关的CoreMedia,Media Player框架，音视频传输的AirPlay框架，Core Animation, Quartz,PDF。

#### 核心服务层(Core Services layer)

提供给应用所需的基础的系统服务。如Accounts账户框架AddressBook，广告框架，数据存储框架SQLite，网络链接框架Networking，地理位置框架CoreLocation, Net Services，运动框架等。这些服务中最核心的是CoreFoundation和Foundation框架。定义了所有应用使用的数据类型。CoreFoundation是基于C的一组接口，Foundation是对CoreFoundation的OC封装。

#### 核心操作系统层(Core OS layer)

包含大多数低级别接近硬件的功能，它所包含的框架常常被其他框架所使用。Accelerate框架包含数字信号，线性代数，图像处理的接口。针对所有的iOS设备硬件之间的差异做优化，保证写一次代码在所有iOS设备上高效运行。CoreBluetooth框架利用蓝牙和外设交互，包括扫描连接蓝牙设备，保存连接状态，断开连接，获取外设的数据或者给外设传输数据等等。Security框架提供管理证书，公钥和私钥信任策略，keychain,hash认证数字签名等等与安全相关的解决方案。

![iOS系统架构以及常用框架](/相关图片/iOS系统架构以及常用框架.png)

### Objective-C的内省

内省(Introspection)是面向对象语言和环境的一个强大特性，Objective-C和Cocoa在这个方面尤其的丰富。**内省是对象揭示自己作为一个运行时对象的详细信息的一种能力。**这些详细信息包括**对象在继承树上的位置**，**对象是否遵循特定的协议**，以及**是否响应特定的消息**。NSObject协议和类定义了很多内省方法，用户查询运行时信息，以便根据对象的特征进行识别。

明智地使用内省可以使面向对象的程序更加高效和强壮。它有助于避免错误的进行消息派发、错误的假设对象相等、以及类似的问题。

列举几个内省方法：

1. isKindOfClass: 

   检查对象是否是类某个类或者其子类的实例。

2. isMemberOfClass:

   检查是否是某个类(不包含其子类)的实例。

3. respondToSelector:

   检查对象是否包含这个方法

4. ConformsToProtocol

   检查对象是否符合协议，是否实现了协议中所有的必选方方法 

   

### 面向对象的SLOID原则

**单一功能原则(The Single Responsibility Principle, 简称SRP)**

当需要修改某个类的时候原因有且只有一个。换句话说就是让一个类只做一种类型的责任，当这个类需要承担其他类型的责任的时候，就需要分解这个类。类被修改的几率很大，因此应该专注于单一的功能，不要在一个类里添加很多功能。一句话：**一个对象只承担一种责任，所有服务接口只通过它来执行这种任务**

**开闭原则(The Open Closed Principle, 简称OCP)**

软件实体应该是可扩展，不可修改的。也就是说，对扩展应该是开放的，对修改应该是封闭的。一句话：**程序实体，比如类和对象，向扩展行为开放，向修改想行为关闭。**

1. 通过增加代码来扩展功能，而不是修改已经存在的代码
2. 若客户模块和服务模块遵循同一个接口设计，则客户模块可以不关心服务模块的类型，服务模块可以方便扩展服务(代码)
3. 开闭原则支持替换的服务，而不用修改客户模块

**里氏替换原则(The Liskov Substitution Principle, 简称LSP)**

当一个子类的实例应该能够替换任何其超类的实例时，他们之间才具有is-A关系。客户模块不应该关心服务模块是如何工作的；同样的接口模块之间，可以在不知道服务模块代码的情况下，进行替换。即接口或父类出现的地方，实现接口的类或子类可以代入。也就是说子类对象是可以替代基类对象的。一句话：**子类应该可以用来替代它所继承的类**

**接口隔离原则(The Interface Segregation Principle, 简称ISP)**

接口的功能要单一，不要在一个接口上根据不同参数实现多个功能。也就是说，使用多个功能单一的接口比使单一的总接口要好。一句话：**一个类对另一个类的依赖应该限制在最小化的接口上**

**依赖倒置原则(The Dependency Inversion Principle, 简称DIP)**

一句话:**依赖抽象层(接口)，而不是具体类**

1. 方法应该依赖抽象，不要依赖实例。
2. 高层模块不应该依赖于底层模块，二则都应该依赖于抽象。iOS开发中一般依赖于协议。
3. 抽象不应该依赖于具体实现，具体实现应该依赖于抽象。
4. 抽象和接口使模块之间的依赖分离。

### 面向对象编程(OOP)和面向协议编程(POP)

面向对象编程有点： 可重用性，继承，可维护性，对复杂性的隐藏(封装), 抽象性，多态性，对于一个类的属性和方法的访问权限控制。

OOP和POP都拥有大部分上述的特点，主要一点不同在于：类只能继承自其他一个类，但协议可以继承自多个协议。POP，swift中除了类，值类型也可以遵循协议，比如struct和enum。

### lldb(gdb)常用的控制台调试命令

1. p 输出基本类型，是打印命令，print的简写
2. po 打印对象，对调用description方法，是print-object的简写。
3. Expr 可以在调试时动态执行制定表达式，并将结果打印出来。常用于在调试过程中修改变量的值。
4. bt 打印调用堆栈，是thread backtrace的简写，加all可打印所有的thread的堆栈
5. br l 是breakpoint list 的简写

### 一些特殊的整数的二进制形式

0xaaaaaaaa = 10101010101010101010101010101010 (偶数位为1，奇数位为0）

0x55555555 = 1010101010101010101010101010101 (偶数位为0，奇数位为1）

0x33333333 = 110011001100110011001100110011 (1和0每隔两位交替出现)

0xcccccccc = 11001100110011001100110011001100 (0和1每隔两位交替出现)

0x0f0f0f0f = 00001111000011110000111100001111 (1和0每隔四位交替出现)

0xf0f0f0f0 = 11110000111100001111000011110000 (0和1每隔四位交替出现)

利用上述具有特殊二进制的整数，可以很方便进行位操作，而且该整数的十六进制形式比较好记，也不用写那么多的0，1。

### 自动释放池的实现原理

1. 在主线程的NSRunLoop对象(在系统级别的线程中应该也是如此，比如通过dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0)获取到的线程)的每个event loop开始前，并在event loop 结束时drain.每一个线程都会维护自己的autoreleasepool堆栈，换句话说autoreleasepool是与线程紧密相关的，每一个autoreleasepool只对应一个线程。
2. 只有在自动释放池中调用了对象的autorelease方法,这个对象才会被存储到这个自动释放池之中.
    如果只是将对象的创建代码写在自动释放之中,而没有调用对象的autorelease方法.是不会将这个对象存储到这个自动释放池之中的.
3.  对象的创建可以在自动释放池的外面,在自动释放池之中,调用对象的autorelease方法,就可以将这个对象存储到这个自动释放池之中.将1个对象存储到自动释放池中,最重要的步骤**调用对象的autorelease方法,这句代码一定要放在池子中**.
4. 当自动释放池结束的时候.仅仅是对存储在自动释放池中的对象发送1条release消息 而不是销毁对象.
5. 如果在自动释放池中,调用同1个对象的autorelease方法多次.就会将对象存储多次到自动释放池之中.在自动释放池结束的时候.会为对象发送多条release消息.那么这个时候就会出现僵尸对象错误.所以,1个自动释放池之中,只autorelease1次,只将这个对象放1次, 否则就会出现僵尸对象错误.
6. 如果在自动释放池中,调用了存储到自动释放中的对象的release方法.在自动释放池结束的时候,还会再调用对象的release方法.这个时候就有有可能会造成野指针操作.也可以调用存储在自动释放池中的对象的retain方法.
7. 将对象存储到自动释放池,并不会使对象的引用计数器+1.所以其好处就是:创建对象将对象存储在自动释放池,就不需要在写个release了.
8. 自动释放池可以嵌套.调用对象的autorelease方法,会讲对象加入到当前自动释放池之中.只有在当前自动释放池结束的时候才会像对象发送release消息.

### 自动释放池是什么？如何工作的？

自动释放池是cocoa提供的帮助我们管理对象内存的一个工具。当我们像一个对象发送autorelease消息时，这个对象就自动加入到最新的自动释放池中，当自动释放池被销毁的时候，会自动向自动释放池中的所有对象发送一条release消息。也就是说我们不再需要手动向每一个对象发送release消息以释放对象，而是将其加入到自动释放池中最后统一释放。使用自动释放池也可以避免一些人为原因导致的内存泄漏。

**第一次创建： 启动runloop时候**

**最后一次销毁：runloop退出的时候**

**其他时候的创建和销毁：当runloop即将睡眠时销毁之前的释放池，重新创建一个新的。** 

autorelease对象出了作用域之后，会被添加到最近一次创建的自动释放池中，并会在当前的runloop迭代结束时释放。

### Objective-C对象模型

NSObject实现了一个NSObject的protocol，里面包含一个isa的指针指向一个objc_class的结构体；而id其实就是objc_object的结构指针，所以它可以指向任何对象。实际上NSObject就是一个包含isa指针的结构体，如下图：

```c
// NSObject在Objective-C中的定义如下。
@interface NSObject <NSObject> {
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wobjc-interface-ivars"
    Class isa  OBJC_ISA_AVAILABILITY;
#pragma clang diagnostic pop
}

//借助xcrun命令生成的文件，实际上NSObject的定义最终是包含一个Class类型名为isa的变量的结构体。如下：
typedef struct objc_class *Class;
struct NSObject_IMPL {
    Class isa; // 8个字节
}
//OC中id类型底层实现如下，一个objc_object类型的结构体，里面只包含了一个objc_class类型的变量isa指针。
typedef struct objc_object {
  // 里面就一个class类型的isa指针
  Class isa;
} *id;
```

```c
//objc_class的底层实现
struct objc_class {
   Class isa                                                //isa指针
   #if !_OBJC2_ 
     Class supper_class  OBJC2_UNAVAILABLE;                 //父类指针
     const char *name    OBJC2_UNAVAILABLE;                 //类名
     long version        OBJC2_UNAVAILABLE;                 //
     long info           OBJC2_UNAVAILABLE;
     long instance_size  OBJC2_UNAVAILABLE;                 //size
     struct objc_ivar_list *ivars OBJC2_UNAVAILABLE;        //成员变量列表
     struct objc_method_list  **methodLists OBJC2_UNAVAILABLE；//方法列表(指针的指针)
     struct objc_cache  *cache  OBJC2_UNAVAILABLE;            //缓存
     struct objc_protocol_list *protocols OBJC2_UNAVAILABLE;  //协议列表
   #endif
} OBJC2_UNAVAILABLE;
```

类也是对象，它是元类(metaclass)的一个实例。元类保存了类方法的列表。当一个类方法被调用时，元类会首先查找它本身是否有该类方法的实现。如果没有，则该元类会向它的父类查找该方法，直到一直找到继承链的头。元类也是一个对象，所有元类的isa指针会指向一个根元类。根元类的isa指针，指向自己。

```c
//METHOD 代表类中某个方法的类型
typedef struct objc_method *Method;
struct objc_method {
  SEL method_name     OBJC2_UNAVAILABLE;   //方法名
  char *method_types  OBJC2_UNAVAILABLE;   //存储方法的参数类型和返回值类型
  //typedef id (*IMP)(id, SEL, ...);
  IMP method_imp      OBJC2_UNAVAILABLE;   //指向方法的实现，本质上是一个函数指针
}
```

### Foundation对象与Core Foundation 对象有什么区别

Foundation对象是Objective-C对象，使用Objective-C语言实现；而Core Foundation 对象是C对象，使用C语言实现。两者之间可以通过`__bridge`、`__bridge_transfer`、`__bridge_retained`等关键字转换(桥接)。

Foundation对象和CoreFoundation对象更重要的区别是ARC下的内存管理问题。在非ARC下两者都需要开发者手动管理内存，没有区别。但在ARC下，系统只会自动管理Foundation对象的释放，而不支持对Core Foundation对象的管理。因此，在ARC下两者进行转换后，必须要确定转换后的对象是由开发者手动管理，还有由ARC系统继续管理，否则可能导致内存泄漏。

下面以NSString对象(Foundation对象)和CFStringRef对象(Core Foundation)对象为例，介绍两者的转换和内存管理权移交问题。

1）在非ARC下，NSString对象和CFStringRef对象可以直接进行强制转换，都是手动管理内存，无须关心内存管理权限的移交问题。

2）在ARC下，NSString对象和CFStringRef对象在相互转换时，需要选择使用`__bridge`、`__bridge_transfer`、 `__bridge_retained` 来确定对象的管理权限转移问题，三者的作用语义分别如下：

1. `__bridge`关键词最常用，它的含义是不改变对象的管理权所有者，本来由ARC管理的Foundation对象，转换成Core Foundation对象后依然由ARC管理；本来由开发者手动管理的Core Foundation 对象转换成Foundation对象后继续由开发者手动管理。示例代码如下：

   ```objective-c
   //ARC管理的Foundation对象
   NSString *s1 = @"string";
   //转换后依然由ARC管理释放
   CFStringRef cfstr = (__bridge CFStringRef)s1;
   //开发者手动管理的CoreFoundation对象
   CFStringRef s2 = CFStringCreateWithCString(NULL, "string", kCFStringEncodingASCII);
   //转换后仍需开发者手动管理释放
   NSString *fs = (__bridge NSString*)s2;
   ```

2. `__bridge_transfer`用在将CoreFoundation 对象转换成Foundation对象时，用于进行内存管理权的移交，即本来需要由开发者手动管理释放的CoreFoundation对象在转换成Foundation对象后，交由ARC来管理对象的释放，开发者不用关心对象释放的问题，因为不会发生内存泄漏。示例代码如下：

   ```objective-c
   //开发者手动管理的Core Foundation对象
   CFStringRef s2 = CFStringCreateWithCString(NULL, "string", kCFStringEncodingASCII);
   //转换后交由ARC管理对象的释放，不用担心内存泄漏
   NSString *str = (__bridge_transfer NSString*)s2;
   //另一种等效写法
   NSString *str1 = (NSString*)CFBridgingRelease(s2);
   ```

3. `__bridge_retained`用在将Foundation对象转换成Core Foundation对象时，进行ARC内存管理权的剥夺，即本来由ARC管理的Foundation对象在转换成CoreFoundation对象后，ARC不在继续管理该对象，需要开发者自己进行手动释放对象，否则会发生内存泄漏。示例代码如下：

   ```objective-c
   //ARC管理的Foundation对象
   NSString *s1 = @"string";
   //转换后ARC不再继续管理，需要手动释放
   CFStringRef cfstring = (__bridge_retained CFStringRef)s1;
   //另一种等效写法
   CFStringRef cfstring1 = (CFStringRef)CFBridgingRetain(s1);
   ```




###只有实现了的方法才能被runtime找到，只声明的方法不能被找到。

### Dealloc相关

当一个对象要释放时，会自动调用dealloc，接下来的调用轨迹是

1. dealloc

2. _objc_rootDealloc

3. rootDealloc

4. object_dispose

5. objc_destructInstance、free

   ```objective-c
   void *objc_destructInstance(id obj) {
     if (obj) {
       //Read all of the flags at once for perfomance
       bool cxx = obj -> hasCxxDtor();
       bool assoc = obj -> hasAssociatedObjects();
       
       //This order is important
       if (cxx) object_cxxDestruct(obj); //清除成员变量
       if (assoc) _object_remove_assocations(obj);//移除关联对象
       obj->clearDeallocating();// 将指向当前对象的弱指针置为nil
     }
     return obj;
   }
   ```

   

































































