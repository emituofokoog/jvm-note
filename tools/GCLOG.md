## GC参数

|                 参数                  |                       含义     |
| :----------------------------------: | :-------------------------: |
|-XX:PrintGC					 |打印GC日志                    |
|-XX:+PrintGCDetails   | 打印详细的GC日志。还会在退出前打印堆的详细信息。|
|-XX:+PrintHeapAtGC    |              每次GC前后打印堆信息。         |
|-XX:+PrintGCTimeStamps|             打印GC发生的时间。             |
| -XX:+PrintGCApplicationConcurrentTime |打印应用程序的执行时间              |
|-XX:+PrintGCApplicationStoppedTime |打印应用由于GC而产生的停顿时间          |
|-XX:+PrintReferenceGC |跟踪软引用、弱引用、虚引用和Finallize队列。    |
|-XLoggc               |             将GC日志以文件形式输出。        |

## GC日志分析工具

GCViewer



