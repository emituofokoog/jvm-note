# jinfo
可以用来查看正在运行的Java应用程序的扩展参数，甚至支持在运行时，修改部分参数

```
Usage:
    jinfo [option] <pid>
        (to connect to running process)
    jinfo [option] <executable <core>
        (to connect to a core file)
    jinfo [option] [server_id@]<remote server IP or hostname>
        (to connect to remote debug server)

where <option> is one of:
    -flag <name>         to print the value of the named VM flag        输出vm参数值
    -flag [+|-]<name>    to enable or disable the named VM flag         启用禁用vm参数
    -flag <name>=<value> to set the named VM flag to the given value    设置vm参数
    -flags               to print VM flags                              打印所有vm参数
    -sysprops            to print Java system properties                打印系统properties参数
    <no option>          to print both of the above                     打印system和vm参数
    -h | -help           to print this help message

```

示例子
```
jinfo -flag +PrintGC pid
jinfo -flag +PrintGCDetails pid 
jinfo -flag +PrintGCTimestamp pid
jinfo -flag -PrintGC pid
jinfo -flag -PrintGCDetails pid 
jinfo -flag -PrintGCTimestamp pid

jinfo -flags pid

jinfo pid
jinfo -sysprops pid
```
