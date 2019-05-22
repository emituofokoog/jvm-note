![Java虚拟机运行时数据区](../images/Java虚拟机运行时数据区.png)

Java虚拟机栈也是线程私有的。

虚拟机描述的是Java方法执行的内存模型：每个方法执行时都会创建一个栈帧(Stack Frame)用于存储：

- 局部表量表
- 操作数栈
- 动态链接
- 方法出口

等。



局部变量表存放了：

- 编译期可知的各种基本数据类型（boolean、byte、char、short、int、float、long、double）
- 对象引用（reference类型，它不等同于对象本身，可能是一个对象起始地址的指针，对象的句柄或其它与此对象相关的位置）
- returnAddress类型（指向了一条字节码指令的地址）



规定的异常情况：

1. 线程请求栈深度大雨虚拟机所允许的深度-抛出StackOverflowError
2. 动态扩展情况下，若无法申请到足够的内存就会抛出OutOfMemoryError异常