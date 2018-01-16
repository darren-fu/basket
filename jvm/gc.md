# GC算法
名称|算法-类型|参数-说明
--|--|--
Serial收集器|复制算法，单线程，新生代|-UseSerialGC参数<br>使用Serial+Serial Old的串行收集器组合
Serial Old| ”标记－整理“算法，单线程，老年代|-
ParNew收集器|复制算法，多线程，新生代|-XX:ParallelGCThreads 限制垃圾收集的线程数<br> -UseParNewGC:使用ParNew + Serial Old的收集器组合|
Parallel Scavenge收集器|复制算法，多线程，新生代|关注的目标是吞吐量(Throughput)。<br>吞吐量=运行用户代码时间 / (运行用户代码时间 + 垃圾收集时间)。<br>停顿时间越短就越适合需要与用户交互的程序，良好的响应速度能够提升用户的体验；<br>而高吞吐量则可以最高效率地利用CPU时间，尽快地完成程序的运算任务，主要适合在后台运算而不需要太多交互的任务。|
Parallel Old收集器|”标记－整理”算法,多线程，老年代|-UseParallelGC:使用Parallel Scavenge + Serial Old的收集器组合<br>-UseParallelOldGC:使用Parallel Scavenge + Parallel Old的收集器组合|
CMS收集器|标记清除算法，多线程，老年代|以获取最短回收停顿时间为目标<br>-XX:+UseConcMarkSweepGC选项启用CMS收集器<br>-XX:+UseParNewGC 新生代使用ParNew<br>-XX:+UseCMSCompactAtFullCollection收集开关参数（默认开启的)，CMS进行FullGC完开启内存碎片的合并整理<br>-XX:CMSFullGCsBeforeCompaction参数用于设置执行多少次不压缩的FULL GC后跟着来一次带压缩的（默认值为0，表示每次进入Full GC时都进行碎片整理<br>-XX:CMSInitiatingOccupancyFraction=65 指定启动gc时old的使用率<br>-XX:UseCMSInitatingOccupancyOnly：表示只在到达阈值的时候，才进行 CMS 回收。<br>-XX:+CMSParallelRemarkEnabled开启并行remark，减少第二次暂停的时间。<br>-XX:+CMSScavengeBeforeRemark选项，强制remark之前开启一次minor gc，减少remark的暂停时间，但是在remark之后也立即开始一次minor gc。<br>-XX:+ParallelCMSThreads CMS启动的回收线程数目,默认是(ParallelGCThreads + 3)/4<br>-XX:+ParallelGCThread年轻代的并发收集线程数目。<br>-XX:CMSClassUnloadingEnabled： 允许对类元数据进行回收。<br>-XX:CMSInitatingPermOccupancyFraction：当永久区占用率达到这一百分比后，启动 CMS 回收 (前提是-XX:+CMSClassUnloadingEnabled 激活了)。
G1收集器|-|-
## CMS
 - 初始标记(initial mark) (stop the world)
 - 并发标记(concurrent mark) 
 - 重新标记(remark) (stop the world)
 - 并发清除(concurrent sweep)
 
 ## 其它参数
 - -Xmx: 设置堆内存的最大值。
 - -Xms: 设置堆内存的初始值。
 - -Xmn: 设置新生代的大小。
 - -Xss: 设置栈的大小。
 - -PretenureSizeThreshold: 直接晋升到老年代的对象大小，设置这个参数后，大于这个参数的对象将直接在老年代分配。
 - -MaxTenuringThrehold: 晋升到老年代的对象年龄。每个对象在坚持过一次Minor GC之后，年龄就会加1，当超过这个参数值时就进入老年代。
 - -UseAdaptiveSizePolicy: 在这种模式下，新生代的大小、eden 和 survivor 的比例、晋升老年代的对象年龄等参数会被自动调整，以达到在堆大小、吞吐量和停顿时间之间的平衡点。在手工调优比较困难的场合，可以直接使用这种自适应的方式，仅指定虚拟机的最大堆、目标的吞吐量 (GCTimeRatio) 和停顿时间 (MaxGCPauseMills)，让虚拟机自己完成调优工作。
 - -SurvivorRattio: 新生代Eden区域与Survivor区域的容量比值，默认为8，代表Eden: Suvivor= 8: 1。
 - -XX:ParallelGCThreads：设置用于垃圾回收的线程数。通常情况下可以和 CPU 数量相等。但在 CPU 数量比较多的情况下，设置相对较小的数值也是合理的。
 - -XX:MaxGCPauseMills：设置最大垃圾收集停顿时间。它的值是一个大于 0 的整数。收集器在工作时，会调整 Java 堆大小或者其他一些参数，尽可能地把停顿时间控制在 MaxGCPauseMills 以内。
 - -XX:GCTimeRatio:设置吞吐量大小，它的值是一个 0-100 之间的整数。假设 GCTimeRatio 的值为 n，那么系统将花费不超过 1/(1+n) 的时间用于垃圾收集。
