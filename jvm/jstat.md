
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

## 列说明
|列	|说明|	Jstat参数|
|--|--|--|
S0C |	输出Survivor0空间的大小。单位KB。|	-gc -gccapacity -gcnew -gcnewcapacity |
S1C|	输出Survivor1空间的大小。单位KB。|	-gc -gccapacity -gcnew -gcnewcapacity|
S0U|		输出Survivor0已用空间的大小。单位KB。|		-gc -gcnew
S1U|		输出Survivor1已用空间的大小。单位KB。|		-gc -gcnew
EC|		输出Eden空间的大小。单位KB。|		-gc -gccapacity -gcnew -gcnewcapacity
EU|		输出Eden已用空间的大小。单位KB。|		-gc -gcnew
OC|		输出老年代空间的大小。单位KB。	|	-gc -gccapacity -gcold -gcoldcapacity
OU|		输出老年代已用空间的大小。单位KB。|		-gc -gcold
PC|		输出持久代空间的大小。单位KB。	|	-gc -gccapacity -gcold -gcoldcapacity -gcpermcapacity
PU|		输出持久代已用空间的大小。单位KB。|		-gc -gcold
YGC|		新生代空间GC时间发生的次数。|		-gc -gccapacity -gcnew -gcnewcapacity -gcold -gcoldcapacity -gcpermcapacity -gcutil -gccause
YGCT|		新生代GC处理花费的时间。|		-gc -gcnew -gcutil -gccause 
FGC|		full GC发生的次数。|		-gc -gccapacity -gcnew -gcnewcapacity -gcold -gcoldcapacity -gcpermcapacity -gcutil -gccause
FGCT|		full GC操作花费的时间	|	-gc -gcold -gcoldcapacity -gcpermcapacity -gcutil -gccause
GCT	|	GC操作花费的总时间。|		-gc -gcold -gcoldcapacity -gcpermcapacity -gcutil -gccause
NGCMN|		新生代最小空间容量，单位KB。|		-gccapacity -gcnewcapacity
NGCMX|		新生代最大空间容量，单位KB。|		-gccapacity -gcnewcapacity
NGC	|	新生代当前空间容量，单位KB。|		-gccapacity -gcnewcapacity
OGCMN|		老年代最小空间容量，单位KB。|		-gccapacity -gcoldcapacity
OGCMX	|	老年代最大空间容量，单位KB。|		-gccapacity -gcoldcapacity
OGC	|	老年代当前空间容量制，单位KB。|		-gccapacity -gcoldcapacity
PGCMN|		持久代最小空间容量，单位KB。|		-gccapacity -gcpermcapacity
PGCMX|		持久代最大空间容量，单位KB。|		-gccapacity -gcpermcapacity
PGC	|	持久代当前空间容量，单位KB。|		-gccapacity -gcpermcapacity
PC	|	持久代当前空间大小，单位KB|		-gccapacity -gcpermcapacity
PU	|	持久代当前已用空间大小，单位KB	|	-gc -gcold
LGCC|		最后一次GC发生的原因	|	-gccause
GCC|		当前GC发生的原因	|	-gccause
TT	|	老年化阈值。被移动到老年代之前，在新生代空存活的次数。	|	-gcnew
MTT	|	最大老年化阈值。被移动到老年代之前，在新生代空存活的次数。	|	-gcnew
DSS	|	Adequate size of survivor in KB幸存者区所需空间大小，单位KB。	|	-gcnew

