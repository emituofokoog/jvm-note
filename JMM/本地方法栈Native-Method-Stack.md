![Java虚拟机运行时数据区](../images/Java虚拟机运行时数据区.png)



本地方法栈为虚拟机使用到的Native方法服务。

虚拟机规范中堆中对本地方法栈中方法使用的语言、使用方式与数据结构并没有强制规定。

本地方法栈会抛出StackOverflowError和OutOfMemoryError

