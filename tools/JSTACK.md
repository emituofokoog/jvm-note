# jstack
用于生成虚拟机当前时刻的线程快照，以便可以进一步定位线程出现长时间停顿的原因，如线程间死锁、 死循环、 请求外部资源导致的长时间等待等。
```

Usage:
    jstack [-l] <pid>
        (to connect to running process)
    jstack -F [-m] [-l] <pid>
        (to connect to a hung process)
    jstack [-m] [-l] <executable> <core>
        (to connect to a core file)
    jstack [-m] [-l] [server_id@]<remote server IP or hostname>
        (to connect to a remote debug server)

Options:
    -F  to force a thread dump. Use when jstack <pid> does not respond (process is hung)        当输出请求不被响应时，强制输出线程堆栈信息
    -m  to print both java and native frames (mixed mode)                                       除堆栈信息外，附加显示关于锁的信息
    -l  long listing. Prints additional information about locks                                 如果涉及本地方法调用，则显示C/C++的堆栈
    -h or -help to print this help message

```
