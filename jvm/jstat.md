[TOC]

## 参数
### class
统计class loader行为信息。命令例子：jstat -class pid 1000 10 （每隔1秒监控一次，一共做10次），输出内容，含义如下：

|Name|Desc|
|----|------|
|Loaded	|Number of classes loaded.|
|Bytes	|Number of Kbytes loaded.|
|Unloaded|	Number of classes unloaded.|
|Bytes	|Number of Kbytes unloaded.|
|Time	|Time spent performing class load and unload operations.|

 
### compile
统计编译行为信息。
### gc
输出每个堆区域的当前可用空间以及已用空间（伊甸园，幸存者等等），GC执行的总次数，GC操作累计所花费的时间。
### gccapactiy
输出每个堆区域的最小空间限制（ms）/最大空间限制（mx），当前大小，每个区域之上执行GC的次数。（不输出当前已用空间以及GC执行时间）。
### gccause
输出-gcutil提供的信息以及最后一次执行GC的发生原因和当前所执行的GC的发生原因
### gcnew
输出新生代空间的GC性能数据
### gcnewcapacity
输出新生代空间的大小的统计数据。
### gcold
输出老年代空间的GC性能数据。
### gcoldcapacity
输出老年代空间的大小的统计数据。
### gcpermcapacity
输出持久带空间的大小的统计数据。
### gcutil
输出每个堆区域使用占比，以及GC执行的总次数和GC操作所花费的事件。
