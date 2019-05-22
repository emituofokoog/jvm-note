

![Java虚拟机运行时数据区](../images/Java虚拟机运行时数据区.png)

### 堆内存溢出

```java
package com.oom;

import java.util.ArrayList;
import java.util.List;

/**
 * VM Args:-Xms20m -Xmx20m -XX:+HeapDumpOnOutOfMemoryError
 *
 * @author zzm
 */
public class HeapOOM {
    static class OOMObject {

    }

    public static void main(String[] args) {
        List<OOMObject> oomObjects = new ArrayList<OOMObject>();
        int i = 0;
        while (i++ < Integer.MAX_VALUE) {
            oomObjects.add(new OOMObject());
        }
    }
}

```

运行结果：

> java.lang.OutOfMemoryError: Java heap space
> Dumping heap to java_pid74046.hprof ...
> Heap dump file created [27590212 bytes in 0.208 secs]
> Exception in thread "main" java.lang.OutOfMemoryError: Java heap space

### 虚拟机栈和本地方法栈溢出

1. 虚拟机栈StackOverflowError

```java
package com.oom;

/**
 * VM Args:-Xss128k
 */
public class JavaVMStackSOF {
    private int stackLength = 1;

    public void stackLeak() {
        stackLength++;
        stackLeak();
    }

    public static void main(String[] args) throws Throwable {
        JavaVMStackSOF javaVMStackSOF = new JavaVMStackSOF();
        try {
            javaVMStackSOF.stackLeak();
        } catch (Throwable throwable) {
            System.out.println("stack length:" + javaVMStackSOF.stackLength);
            throw throwable;
        }
    }
}
```
运行结果：
> Exception in thread "main" java.lang.StackOverflowError
> stack length:10828
> 	at com.oom.JavaVMStackSOF.stackLeak(JavaVMStackSOF.java:10)
> 	at com.oom.JavaVMStackSOF.stackLeak(JavaVMStackSOF.java:11)
> 	at com.oom.JavaVMStackSOF.stackLeak(JavaVMStackSOF.java:11)



2. **创建线程导致OOM**

```java
package com.oom;

/**
 * VM Args: -Xss2M
 */
public class JavaVMStackOOM {
    //让当前线程休眠 Integer.MAX_VALUE ms
    private void dontStop() {
        try {
            Thread.sleep(Integer.MAX_VALUE);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    //大量创建线程
    public void stackLeakByThread() {
        int i = 0;
        while (i++ < Integer.MAX_VALUE) {
            Thread thread = new Thread(new Runnable() {
                @Override
                public void run() {
                    dontStop();
                }
            }, "thread-for-leak");
            thread.start();
        }
    }

    public static void main(String[] args) {
        JavaVMStackOOM javaVMStackOOM = new JavaVMStackOOM();
        javaVMStackOOM.stackLeakByThread();
    }
}

```

   

运行结果：

> Exception in thread "main" java.lang.OutOfMemoryError: unable to create new native thread

### 方法区和运行时常量池溢出



方法区用于存放Class的相关信息，如类名、访问修饰符、常量池、字段描述符、方法描述符等。对于这些区域的测试，基本思路是运行时产生大量的类去填满方法区，直到溢出。



1. 运行时常量池导致的内存溢出异常

```java
package com.oom;

import java.util.ArrayList;
import java.util.List;

/**
 * VM Args: -XX:PermSize=10m -XX:MaxPermSize=10m
 * JDK1.6会抛出:PermGen Space
 */
public class RuntimeConstantPoolOOM {
    public static void main(String[] args) {
        //使用List保持常量池引用，避免FullGC回收常量池行为
        List<String> list = new ArrayList<String>();

        //10M的PermSize在Integer范围内足够产生OOM
        int i = 0;

        while (true) {
            list.add(String.valueOf(i++).intern());
        }
    }

}

```

2. 借助CGLib使方法区出现内存溢出异常

```java
package com.oom;

import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

/**
 * VM Args: -XX:PermSize=10m -XX:MaxPermSize=10m
 */
public class JavaMethodAreaOOM {
    public static void main(final String[] args) {
        long i = 0;
        while (i++ < Long.MAX_VALUE) {
            Enhancer enhancer = new Enhancer();
            enhancer.setSuperclass(OOMObject.class);
            enhancer.setUseCache(false);
            enhancer.setCallback(new MethodInterceptor() {
                @Override
                public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
                    return methodProxy.invokeSuper(objects, args);
                }
            });

            enhancer.create();
        }
    }

    static class OOMObject {
    }
}

```

运行结果：

> Exception in thread "main" 
> Exception: java.lang.OutOfMemoryError thrown from the UncaughtExceptionHandler in thread "main"



### 本机直接内存溢出

```java
package com.oom;

import sun.misc.Unsafe;

import java.lang.reflect.Field;

/**
 * VM Args:-Xmx20M -XX:MaxDirectMemorySize=10M
 */
public class DirectMemoryOOM {
    private static final int _1MB = 1024 * 1024;

    public static void main(String[] args) throws NoSuchFieldException, \IllegalAccessException {
        Field unsafeField = Unsafe.class.getDeclaredFields()[0];
        unsafeField.setAccessible(true);
        Unsafe unsafe = (Unsafe) unsafeField.get(null);

        int i = 0;
        while (i++ < Integer.MAX_VALUE) {
            unsafe.allocateMemory(_1MB * 100);
        }

    }
}

```

运行结果：

> Exception in thread "main" java.lang.OutOfMemoryError
> 	at sun.misc.Unsafe.allocateMemory(Native Method)
> 	at com.oom.DirectMemoryOOM.main(DirectMemoryOOM.java:20)