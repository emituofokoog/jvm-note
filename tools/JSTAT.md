# jstat
JVM统计监测工具(JVM Statistics Monitoring Tool)，主要用于监测并显示JVM的性能统计信息。

命令选项 | 描述
--- | ---
-class | 类加载、卸载数量、总空间及类装载所耗费的时间
-compiler | 显示JIT编译器编译过的方法、耗时等信息
-gc | 统计Java堆，包括Eden、Survivor、老年代、永久代的容量，已用空间、 GC时间等信息
-gccapacity | 显示Java堆各个区域使用到的最大、最小空间
-gcutil | 显示已使用空间占总空间的百分比
-gccause | 垃圾收集统计概述(同-gcutil)，附加最近两次垃圾回收事件的原因
-gcnew | 新生代行为统计
-gcnewcapacity | 新生代使用到的最大、最小空间统计
-gcold | 统计老年代GC状况
-gcoldcapacity | 年老代行为统计(同-gcoldcapacity)，主要关注使用到的最大、最小空间
-gcpermcapacity | 显示永久代使用到的最大、最小空间(-gcmetacapacity)
-printcompilation | 显示已经被JIT编译的方法 



> -class (监视类装载、卸载数量、总空间以及耗费的时间)

字段  |  说明
--- | ---
Loaded | 加载class的数量
Bytest | class字节大小
Unloaded| 未加载class的数量
Bytes | 未加载class的字节大小
Time | 加载时间


> -compiler pid（输出JIT编译过的方法数量耗时等）

字段 | 说明
--- | ---
Compiled | 编译数量
Failed | 编译失败数量
Invalid | 无效数量
Time | 编译耗时
FailedType | 失败类型
FailedMethod| 失败方法的全限定名



