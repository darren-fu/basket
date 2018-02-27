
## 获取对象大小
项目下载地址：http://sourceforge.net/projects/sizeof/

其实就一个jar包，放到Eclipse工程路径下，添加到classpath中。

运行前添加VM参数：-javaagent:./lib/SizeOf.jar 运行即可(将jar放在lib路径下)。

### example
```java
    int size = 1_000_000;
    List<String> list = new ArrayList<>(size);
    for (int i = 0; i < size; i++) {
        list.add("Xe21edsa2s" + i);
    }
    System.out.println(SizeOf.deepSizeOf(list));
    System.out.println(SizeOf.humanReadable(SizeOf.deepSizeOf(list)));
    System.out.println("#################");
    System.out.println(SizeOf.deepSizeOf(list));
    System.out.println(SizeOf.humanReadable(SizeOf.deepSizeOf(list)));
``` 

### result

```
JAVAGENT: call premain instrumentation for class SizeOf
Found big object:   [Ljava.lang.Object;@1490180672 size: 72.47853088378906Mb
Found big object: java.util.ArrayList@245565335 size: 72.47858428955078Mb
75999304
Found big object:   [Ljava.lang.Object;@1490180672 size: 72.47853088378906Mb
Found big object: java.util.ArrayList@245565335 size: 72.47858428955078Mb
72.47858428955078Mb
#################
Found big object:   [Ljava.lang.Object;@1490180672 size: 72.47853088378906Mb
Found big object: java.util.ArrayList@245565335 size: 72.47858428955078Mb
75999304
Found big object:   [Ljava.lang.Object;@1490180672 size: 72.47853088378906Mb
Found big object: java.util.ArrayList@245565335 size: 72.47858428955078Mb
72.47858428955078Mb
```
