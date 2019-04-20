# jmap
用于生成heap dump文件，如果不使用这个命令，还可以使用-XX:+HeapDumpOnOutOfMemoryError 参数来让虚拟机出现OOM的时候自动生成dump文件。 jmap不仅能生成dump文件，还可以查询finalize执行队 列、Java堆和永久代的详细信息，如当前使用率、当前使用的是哪种收集器等
```
Usage:
    jmap [option] <pid>
        (to connect to running process)
    jmap [option] <executable <core>
        (to connect to a core file)
    jmap [option] [server_id@]<remote server IP or hostname>
        (to connect to remote debug server)

where <option> is one of:
    <none>               to print same info as Solaris pmap
    -heap                to print java heap summary
    -histo[:live]        to print histogram of java object heap; if the "live"
                         suboption is specified, only count live objects
    -permstat            to print permanent generation statistics
    -finalizerinfo       to print information on objects awaiting finalization
    -dump:<dump-options> to dump java heap in hprof binary format
                         dump-options:
                           live         dump only live objects; if not specified,
                                        all objects in the heap are dumped.
                           format=b     binary format
                           file=<file>  dump heap to <file>
                         Example: jmap -dump:live,format=b,file=heap.bin <pid>
    -F                   force. Use with -dump:<dump-options> <pid> or -histo
                         to force a heap dump or histogram when <pid> does not
                         respond. The "live" suboption is not supported
                         in this mode.
    -h | -help           to print this help message
    -J<flag>             to pass <flag> directly to the runtime system

```
命令选项 | 描述
--- | ---
-dump | 生成Java堆快照。格式:-dump:[live, ]format=b, file=<filename>，live为是否 只生成存活的对象
-histo | 显示堆中对象的统计信息，包括类、有多少个实例，合计容量等
-permstat | 显示永久代内存状态。只在Linux/Solaris平台下有效
-heap | 显示堆详细信息，如使用哪种回收器、参数配置、分代状况等。只在Linux/Solaris 平台下有效
-finalization | 显示在F-Queue中等待Finalizer线程执行finalize方法的对象。只在Linux/Solaris 平台下有效
-F | 当虚拟机进程多-dump没有响应时，可以使用这个选项强制生成dump快照。只在 Linux/Solaris平台下有效
