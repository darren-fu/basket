
- 查找java进程
JPS
- 获取最耗CPU的线程
  * ps -Lfp pid
  * ps -mp pid -o THREAD,tid,time
  * top -Hp pid
- 获取线程ID的16进制
  ```shell
    printf "%x\n" 21742
    #得到21742的十六进制值为54ee
  ```
- 使用Jstack
  ```shell
    # 查找Java进程中指定线程的堆栈
    jstack 21711 | grep 54ee
    # or 输出堆栈道文本，然后查找指定线程54ee的内容
    jstack 21711 > stack.txt
    
  ```
  
