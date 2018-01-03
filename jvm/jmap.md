
- jmap -heap pid 

使用jmap -heap pid查看进程堆内存使用情况，包括使用的GC算法、堆配置参数和各代中堆内存使用情况。
```
#  解决 Can't attach to the process
echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
```
- jmap -histo[:live] pid 
查看堆内存中的对象数目、大小统计直方图，如果带上live则只统计活对象 

name|type
---|---- 
B | byte 
C | char 
D | double
F | float
I | int
J | long
Z | boolean
[ | 数组，如[I表示int[]
[L+类名 | 其他对象

- jmap -dump:format=b,file=/tmp/dump.dat 21711 
生成dump文件
```shell
#jmap -dump:format=b,file=/tmp/dump.dat 21711     
Dumping heap to /tmp/dump.dat ...
Heap dump file created
```
dump出来的文件可以用MAT、VisualVM等工具查看，这里用jhat查看：
```shell
#注意如果Dump文件太大，可能需要加上-J-Xmx512m这种参数指定最大堆内存，
#即jhat -J-Xmx512m -port 9998 /tmp/dump.dat
# jhat -port 9998 /tmp/dump.dat
Reading from /tmp/dump.dat...
Dump file created Tue Jan 28 17:46:14 CST 2014
Snapshot read, resolving...
Resolving 132207 objects...
Chasing references, expect 26 dots..........................
Eliminating duplicate references..........................
Snapshot resolved.
Started HTTP server on port 9998
Server is ready.
```
然后就可以在浏览器中输入主机地址:9998查看了
