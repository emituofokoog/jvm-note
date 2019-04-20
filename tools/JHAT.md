# jhat
jhat也是jdk内置的工具之一。主要是用来分析java堆的命令，可以将堆中的对象以html的形式显示出来，包括对象的数量，大小等等，并支持对象查询语言。

#### 第一步：导出堆

`jmap -dump:live,file=a.log pid`

除了使用jmap命令，还可以通过以下方式：
1. 使用 jconsole 选项通过 HotSpotDiagnosticMXBean 从运行时获得堆转储（生成dump文件）
2. 虚拟机启动时如果指定了 -XX:+HeapDumpOnOutOfMemoryError 选项, 则在抛出 OutOfMemoryError 时, 会自动执行堆转储.
3. 使用 hprof 命令


#### 第二步：分析堆

`jhat -J-Xmx512M a1.log`

说明：有时dump出来的堆很大，在启动时会报堆空间不足的错误，可加参数：jhat -J-Xmx512m <heap dump file>。这个内存大小可根据自己电脑进行设置。

#### 第三步：查看html
