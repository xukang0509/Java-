# JVM

视频链接：[黑马程序员JVM完整教程，Java虚拟机快速入门，全程干货不拖沓](https://www.bilibili.com/video/BV1yE411Z7AP?vd_source=6e6b2286ee9a603d7bdb2bc5ba80e449)

+++

## *引言*

1. 什么是 JVM ?

   Java Virtual Machine --- java程序的运行环境（java二进制字节码的运行环境）

   好处：

   - 一次编写，到处运行
   - 自动内存管理，垃圾回收功能
   - 数组下标越界检查
   - 多态

2. 学习 JVM 有什么用 ?

   - 面试
   - 理解底层的实现原理
   - 中高级程序员的必备技能

   比较 JVM JRE JDK 的区别：

   ![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150422.png)

3. 常见的 JVM

   ![image-20230308124745090](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230308124745090.png)

   > [维基百科参考](https://en.wikipedia.org/wiki/Comparison_of_Java_virtual_machines)

4. 学习路线

   ![image-20230308124839496](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230308124839496.png)

   - ClassLoader：Java代码编译成二进制后，会经过类加载器，这样才能加载到 JVM 中运行。
   - Method Area：类是放在方法区中。
   - Heap：类的实例对象。
   - 当类调用方法时，会用到 JVM Stack、PC Register、本地方法栈。
   - 方法执行时的每行代码是有执行引擎中的解释器逐行执行，方法中的热点代码频繁调用的方法，由 JIT 编译器优化后执行，GC 会对堆中不用的对象进行回收。需要和操作系统打交道就需要使用到本地方法接口。
   
   > [参考资料](https://www.javainterviewpoint.com/java-virtual-machine-architecture-in-java/)

+++

## *内存结构*

### 1 程序计数器

![image-20230310161231181](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230310161231181.png)

Program Counter Register 程序计数器（寄存器）

**作用**：

用于保存JVM中下一条所要执行的指令的地址

**特点**：

- 线程私有
  - CPU会为每个线程分配时间片，当当前线程的时间片使用完以后，CPU就会去执行另一个线程中的代码
  - 程序计数器是**每个线程**所**私有**的，当另一个线程的时间片用完，又返回来执行当前线程的代码时，通过程序计数器可以知道应该执行哪一句指令
- 不会存在内存溢出



### 2 虚拟机栈

![image-20230310161405077](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230310161405077.png)

Java Virtual Machine Stacks（Java 虚拟机栈）

**定义**：

- 每个**线程**运行需要的内存空间，称为**虚拟机栈**
- 每个栈由多个**栈帧**(Frame)组成，对应着每次调用方法时所占用的内存
- 每个线程只能有**一个活动栈帧**，对应着**当前正在执行的方法**
- 不涉及垃圾回收（方法入栈必出栈！）

**代码演示**：

```java
public class Main {
	public static void main(String[] args) {
		method1();
	}

	private static void method1() {
		method2(1, 2);
	}

	private static int method2(int a, int b) {
		int c = a + b;
		return c;
	}
}
```

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150534.png)

在控制台中可以看到，主类中的方法在进入虚拟机栈的时候，符合栈的特点

**问题辨析**：

- 垃圾回收是否涉及栈内存？
  
  **不需要**。因为虚拟机栈中是由一个个栈帧组成的，在方法执行完毕后，对应的栈帧就会被弹出栈。所以无需通过垃圾回收机制去回收内存。
- 栈内存的分配越大越好吗？
  
  不是。因为**物理内存是一定的**，栈内存越大，可以支持更多的递归调用，但是可执行的线程数就会越少。
- 方法内的局部变量是否是线程安全的？
  - 如果方法内**局部变量没有逃离方法的作用范围**，则是**线程安全**的
  - 如果如果**局部变量引用了对象**，并**逃离了方法的作用范围**，则需要考虑线程安全问题

**栈内存溢出**：

**Java.lang.stackOverflowError** 栈内存溢出

发生原因

- 虚拟机栈中，**栈帧过多**（无限递归）
- 每个栈帧**所占用过大**

**线程运行诊断**：CPU占用过高

- Linux环境下运行某些程序的时候，可能导致CPU的占用过高，这时需要定位占用CPU过高的线程
  1. `top`命令，查看是哪个**进程**占用CPU过高
  2. `ps H -eo pid, tid（线程id）, %cpu | grep 刚才通过top查到的进程号` 通过ps命令进一步查看是哪个线程占用CPU过高
  3. `jstack 进程id` 通过查看进程中的线程的nid，刚才通过ps命令看到的tid来**对比定位**，注意jstack查找出的线程id是**16进制的**，**需要转换**



### 3 本地方法栈

![image-20230310161529884](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230310161529884.png)

一些带有**native关键字**的方法就是需要JAVA去调用本地的C或者C++方法，因为JAVA有时候没法直接和操作系统底层交互，所以需要用到本地方法

- 非常类似于虚拟机栈
- 其中方法为本地native方法
- 例如：Object类中的wait()、clone()等



### 4 堆

![image-20230310161547332](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230310161547332.png)

Heap 堆

**定义**：

通过new关键字**创建的对象**都会被放在堆内存

**特点**：

- **所有线程共享**，堆内存中的对象都需要**考虑线程安全问题**
- 有垃圾回收机制

**堆内存溢出**：

`java.lang.OutofMemoryError：java heap space.` 堆内存溢出

可以使用 `-Xmx8m` 来指定堆内存大小。

**堆内存诊断**：

1. jps：查看当前系统中有哪些 java 进程
2. jmap：查看堆内存占用情况 jmap -heap 进程id
3. jconsole：图形界面的，多功能的监测工具，可以连续监测
4. jvirsalvm



### 5 方法区

![image-20230310161711218](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230310161711218.png)

#### 5.1 方法区简介

Method Area 方法区

**定义**：

Java 虚拟机有一个在所有 Java 虚拟机线程之间共享的方法区域。方法区域类似于用于传统语言的编译代码的存储区域，或者类似于操作系统进程中的“文本”段。它存储每个类的结构，例如**运行时常量池**、**字段和方法数据**，以及**方法和构造函数的代码**，包括**特殊方法**，用于类和实例初始化以及接口初始化，方法区域是在虚拟机启动时创建的。尽管方法区域在逻辑上是堆的一部分，但简单的实现可能不会选择垃圾收集或压缩它。此规范不强制指定方法区的位置或用于管理已编译代码的策略。方法区域可以具有固定的大小，或者可以根据计算的需要进行扩展，并且如果不需要更大的方法区域，则可以收缩。方法区域的内存不需要是连续的！

**组成**：Hotspot 虚拟机 jdk1.6 1.8 内存结构图

![image-20230310170213626](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230310170213626.png)

![image-20230310170224716](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230310170224716.png)

**特点**：

- 线程共享
- 逻辑上属于堆得一部分，但别名：Non-Heap
- jdk1.6 StringTable 位置是在永久代中，1.8 StringTable 位置是在堆中。
- 方法区的实现（HotSpot虚拟机）：
  - jdk1.8以前：用**永久代**（GC分代）实现
  - jdk1.8以后：用**元空间**（直接内存）实现

**内存溢出**：

- 1.8以前会导致**永久代**内存溢出
- 1.8以后会导致**元空间**内存溢出



#### 5.2 常量池

二进制字节码的组成：类的基本信息、常量池、类的方法定义（包含了虚拟机指令）

**通过反编译来查看类的信息**

- 获得对应类的.class文件

  - 在JDK对应的bin目录下运行cmd，**也可以在IDEA控制台输入**

    [![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150602.png)](https://nyimapicture.oss-cn-beijing.aliyuncs.com/img/20200608150602.png)

  - 输入 **javac 对应类的绝对路径**

    ```
    F:\JAVA\JDK8.0\bin>javac F:\Thread_study\src\com\nyima\JVM\day01\Main.javaCopy
    ```

    输入完成后，对应的目录下就会出现类的.class文件

- 在控制台输入 javap -v 类的绝对路径

  ```
  javap -v F:\Thread_study\src\com\nyima\JVM\day01\Main.classCopy
  ```

- 然后能在控制台看到反编译以后类的信息了

  - 类的基本信息

    ![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150618.png)

  - 常量池

    ![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150630.png)

    ![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150641.png)

  - 虚拟机中执行编译的方法（框内的是真正编译执行的内容，**#号的内容需要在常量池中查找**）

    ![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150653.png)

**运行时常量池**：

- 常量池

  就是一张表（如上图中的constant pool），虚拟机指令根据这张常量表找到要执行的类名、方法名、参数类型、字面量信息

- 运行时常量池

  常量池是`*.class`文件中的，当该**类被加载以后**，它的常量池信息就会**放入运行时常量池**，并把里面的**符号地址变为真实地址**



#### 5.3 常量池与串池的关系

##### **串池**StringTable

**特征**

- 常量池中的字符串是符号，只有**在被用到时才会转化为对象**
- 利用串池的机制，来避免重复创建字符串对象
- 字符串**变量**拼接的原理是**StringBuilder**
- 字符串**常量**拼接的原理是**编译器优化**
- 可以使用**intern方法**，主动将串池中还没有的字符串对象放入串池中
- 存放位置：
  - jdk1.6 位置是在永久代中（fullgc才能触发垃圾回收）
  - jdk1.8 位置是在堆中（方便垃圾回收）
- **注意**：无论是串池还是堆里面的字符串，都是对象

用来放字符串对象且里面的**元素不重复**

```java
public class StringTableStudy {
	public static void main(String[] args) {
		String a = "a"; 
		String b = "b";
		String ab = "ab";
	}
}
```

常量池中的信息，都会被加载到运行时常量池中，但这是 a b ab 仅是常量池中的符号，**还没有成为java字符串**

```bash
0: ldc           #2                  // String a
2: astore_1
3: ldc           #3                  // String b
5: astore_2
6: ldc           #4                  // String ab
8: astore_3
9: returnCopy
```

当执行到 ldc #2 时，会把符号 a 变为 “a” 字符串对象，**并放入串池中**（hashtable结构 不可扩容）

当执行到 ldc #3 时，会把符号 b 变为 “b” 字符串对象，并放入串池中

当执行到 ldc #4 时，会把符号 ab 变为 “ab” 字符串对象，并放入串池中

最终**StringTable [“a”, “b”, “ab”]**

**注意**：字符串对象的创建都是**懒惰的**，只有当运行到那一行字符串且在串池中不存在的时候（如 ldc #2）时，该字符串才会被创建并放入串池中。

使用拼接**字符串变量对象**创建字符串的过程

```java
public class StringTableStudy {
	public static void main(String[] args) {
		String a = "a";
		String b = "b";
		String ab = "ab";
		//拼接字符串对象来创建新的字符串
		String ab2 = a + b;
	}
}
```

反编译后的结果

```bash
	 Code:
      stack=2, locals=5, args_size=1
         0: ldc           #2                  // String a
         2: astore_1
         3: ldc           #3                  // String b
         5: astore_2
         6: ldc           #4                  // String ab
         8: astore_3
         9: new           #5                  // class java/lang/StringBuilder
        12: dup
        13: invokespecial #6                  // Method java/lang/StringBuilder."<init>":()V
        16: aload_1
        17: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        20: aload_2
        21: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        24: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
        27: astore        4
        29: returnCopy
```

通过拼接的方式来创建字符串的**过程**是：`StringBuilder().append(“a”).append(“b”).toString()`

最后的toString()方法的返回值是一个**新的字符串**，但字符串的**值**和拼接的字符串一致，但是两个不同的字符串，**一个存在于串池之中，一个存在于堆内存之中**

```java
String ab = "ab";
String ab2 = a + b;
//结果为false,因为ab是存在于串池之中，ab2是由StringBuffer的toString方法所返回的一个对象，存在于堆内存之中
System.out.println(ab == ab2);
```

使用**拼接字符串常量对象**的方法创建字符串

```java
public class StringTableStudy {
	public static void main(String[] args) {
		String a = "a";
		String b = "b";
		String ab = "ab";
		String ab2 = a + b;
		//使用拼接字符串的方法创建字符串
		String ab3 = "a" + "b";
	}
}
```

反编译后的结果

```bash
 	  Code:
      stack=2, locals=6, args_size=1
         0: ldc           #2                  // String a
         2: astore_1
         3: ldc           #3                  // String b
         5: astore_2
         6: ldc           #4                  // String ab
         8: astore_3
         9: new           #5                  // class java/lang/StringBuilder
        12: dup
        13: invokespecial #6                  // Method java/lang/StringBuilder."<init>":()V
        16: aload_1
        17: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        20: aload_2
        21: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        24: invokevirtual #8                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
        27: astore        4
        //ab3初始化时直接从串池中获取字符串
        29: ldc           #4                  // String ab
        31: astore        5
        33: returnCopy
```

- 使用**拼接字符串常量**的方法来创建新的字符串时，因为**内容是常量，javac在编译期会进行优化，结果已在编译期确定为ab**，而创建ab的时候已经在串池中放入了“ab”，所以ab3直接从串池中获取值，所以进行的操作和 ab = “ab” 一致。
- 使用**拼接字符串变量**的方法来创建新的字符串时，因为内容是变量，只能**在运行期确定它的值，所以需要使用StringBuilder来创建**



##### intern方法 1.8

调用字符串对象的intern方法，会将该字符串对象尝试放入到串池中

- 如果串池中没有该字符串对象，则放入成功
- 如果有该字符串对象，则放入失败

无论放入是否成功，都会返回**串池中**的字符串对象

**注意**：此时如果调用intern方法成功，堆内存与串池中的字符串对象是同一个对象；如果失败，则不是同一个对象

**例1**

```java
public class Main {
	public static void main(String[] args) {
		//"a" "b" 被放入串池中，str则存在于堆内存之中
		String str = new String("a") + new String("b");
		//调用str的intern方法，这时串池中没有"ab"，则会将该字符串对象放入到串池中，此时堆内存与串池中的"ab"是同一个对象
		String st2 = str.intern();
		//给str3赋值，因为此时串池中已有"ab"，则直接将串池中的内容返回
		String str3 = "ab";
		//因为堆内存与串池中的"ab"是同一个对象，所以以下两条语句打印的都为true
		System.out.println(str == st2);
		System.out.println(str == str3);
	}
}
```

**例2**

```java
public class Main {
	public static void main(String[] args) {
        //此处创建字符串对象"ab"，因为串池中还没有"ab"，所以将其放入串池中
		String str3 = "ab";
        //"a" "b" 被放入串池中，str则存在于堆内存之中
		String str = new String("a") + new String("b");
        //此时因为在创建str3时，"ab"已存在与串池中，所以放入失败，但是会返回串池中的"ab"
		String str2 = str.intern();
        //false
		System.out.println(str == str2);
        //false
		System.out.println(str == str3);
        //true
		System.out.println(str2 == str3);
	}
}
```



##### intern方法 1.6

调用字符串对象的intern方法，会将该字符串对象尝试放入到串池中

- 如果串池中没有该字符串对象，会将该字符串对象复制一份，再放入到串池中
- 如果有该字符串对象，则放入失败

无论放入是否成功，都会返回**串池中**的字符串对象

**注意**：此时无论调用intern方法成功与否，串池中的字符串对象和堆内存中的字符串对象**都不是同一个对象**



##### StringTable的位置

jdk1.6 StringTable 位置是在永久代中，1.8 StringTable 位置是在堆中



##### StringTable 垃圾回收

StringTable在内存紧张时，会发生垃圾回收

- `-Xmx10m` 指定堆内存大小
- `-XX:+PrintStringTableStatistics` 打印字符串常量池信息
- `-XX:+PrintGCDetails`
- `-verbose:gc` 打印 gc 的次数，耗费时间等信息



##### StringTable调优

- 因为StringTable是由HashTable实现的，所以可以**适当增加HashTable桶的个数**，来减少字符串放入串池所需要的时间

  ```
  -XX:StringTableSize=桶个数(至少1009及以上)
  ```

- 考虑是否需要将字符串对象入池

  可以通过**intern方法减少重复入池**



### 6 直接内存

**定义**：

Direct Memory 直接内存

- 属于操作系统，常见于NIO操作时，**用于数据缓冲区**
- 分配回收成本较高，但读写性能高
- 不受JVM内存回收管理



#### 6.1 文件读写流程

**普通IO**：

因为 java 不能直接操作文件管理，需要切换到内核态，使用本地方法进行操作，然后读取磁盘文件，会在系统内存中创建一个缓冲区，将数据读到系统缓冲区， 然后在将系统缓冲区数据复制到 java 堆内存中。缺点是数据存储了两份，在系统内存中有一份，java 堆中有一份，造成了不必要的复制。

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150715.png)

**使用了DirectBuffer**：

直接内存是操作系统和 Java 代码**都可以访问的一块区域**，无需将代码从系统内存复制到 Java 堆内存，从而提高了效率。

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150736.png)



#### 6.2 释放原理

直接内存的回收不是通过JVM的垃圾回收来释放的，而是通过**unsafe.freeMemory**来手动释放

通过

```JAVA
//通过ByteBuffer申请1M的直接内存
ByteBuffer byteBuffer = ByteBuffer.allocateDirect(_1M);
```

申请直接内存，但JVM并不能回收直接内存中的内容，它是如何实现回收的呢？

**allocateDirect的实现**

```java
public static ByteBuffer allocateDirect(int capacity) {
    return new DirectByteBuffer(capacity);
}
```

**DirectByteBuffer类**

```java
DirectByteBuffer(int cap) {   // package-private
   
    super(-1, 0, cap, cap);
    boolean pa = VM.isDirectMemoryPageAligned();
    int ps = Bits.pageSize();
    long size = Math.max(1L, (long)cap + (pa ? ps : 0));
    Bits.reserveMemory(size, cap);

    long base = 0;
    try {
        base = unsafe.allocateMemory(size); //申请内存
    } catch (OutOfMemoryError x) {
        Bits.unreserveMemory(size, cap);
        throw x;
    }
    unsafe.setMemory(base, size, (byte) 0);
    if (pa && (base % ps != 0)) {
        // Round up to page boundary
        address = base + ps - (base & (ps - 1));
    } else {
        address = base;
    }
    cleaner = Cleaner.create(this, new Deallocator(base, size, cap)); //通过虚引用，来实现直接内存的释放，this为虚引用的实际对象
    att = null;
}
```

这里调用了一个Cleaner的create方法，且后台线程还会对虚引用的对象监测，如果虚引用的实际对象（这里是DirectByteBuffer）被回收以后，就会调用Cleaner的clean方法，来清除直接内存中占用的内存

```java
public void clean() {
       if (remove(this)) {
           try {
               this.thunk.run(); //调用run方法 释放内存
           } catch (final Throwable var2) {
               AccessController.doPrivileged(new PrivilegedAction<Void>() {
                   public Void run() {
                       if (System.err != null) {
                           (new Error("Cleaner terminated abnormally", var2)).printStackTrace();
                       }

                       System.exit(1);
                       return null;
                   }
               });
           }
       }
}
```

对应对象的run方法

```java
public void run() {
    if (address == 0) {
        // Paranoia
        return;
    }
    unsafe.freeMemory(address); //释放直接内存中占用的内存
    address = 0;
    Bits.unreserveMemory(size, capacity);
}
```

**直接内存的回收机制总结**

- 使用了Unsafe类来完成直接内存的分配回收，回收需要主动调用freeMemory方法
- ByteBuffer的实现内部使用了Cleaner（虚引用）来检测ByteBuffer。一旦ByteBuffer被垃圾回收，那么会由ReferenceHandler来调用Cleaner的clean方法调用freeMemory来释放内存

> 注意：
>
> ```java
> 	/**
>      * -XX:+DisableExplicitGC 显示的
>      */
>     private static void method() throws IOException {
>         ByteBuffer byteBuffer = ByteBuffer.allocateDirect(_1GB);
>         System.out.println("分配完毕");
>         System.in.read();
>         System.out.println("开始释放");
>         byteBuffer = null;
>         System.gc(); // 手动 gc 失效
>         System.in.read();
>     }
> ```
>
> 一般用 jvm 调优时，会加上下面的参数：
>
> ```bash
> -XX:+DisableExplicitGC  # 静止显示的 GC
> ```
>
> 意思就是禁止我们手动的 GC，比如手动 System.gc() 无效，它是一种 full gc，会回收新生代、老年代，会造成程序执行的时间比较长。所以我们就通过 unsafe 对象调用 freeMemory 的方式释放内存。

+++

## *垃圾回收*

### 1 如何判断对象可以回收

#### 1.1 引用计数法

当一个对象被引用时，就当引用对象的值加 1，当值为 0 时，就表示该对象不被引用，可以被垃圾收集器回收。

弊端：循环引用时，两个对象的计数都为1，导致两个对象都无法被释放

![image-20230311163901693](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311163901693.png)



#### 1.2 可达性分析算法

- JVM中的垃圾回收器通过**可达性分析**来探索所有存活的对象
- 扫描堆中的对象，看能否沿着GC Root对象为起点的引用链找到该对象，如果**找不到，则表示可以回收**
- 可以作为GC Root的对象
  - 虚拟机栈（栈帧中的本地变量表）中引用的对象。　
  - 方法区中类静态属性引用的对象
  - 方法区中常量引用的对象
  - 本地方法栈中JNI（即一般说的Native方法）引用的对象



#### 1.3 四种引用

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150800.png)

##### 强引用

只有GC Root**都不引用**该对象时，才会回收**强引用**对象

- 如上图B、C对象都不引用A1对象时，A1对象才会被回收

##### 软引用

当GC Root指向软引用对象时，在**内存不足时**，会**回收软引用所引用的对象**

- 如上图如果B对象不再引用A2对象且内存不足时，软引用所引用的A2对象就会被回收

**软引用的使用**：

```java
public class Demo1 {
	public static void main(String[] args) {
		final int _4M = 4*1024*1024;
		//使用软引用对象 list和SoftReference是强引用，而SoftReference和byte数组则是软引用
		List<SoftReference<byte[]>> list = new ArrayList<>();
		SoftReference<byte[]> ref= new SoftReference<>(new byte[_4M]);
	}
}
```

如果在垃圾回收时发现内存不足，在回收软引用所指向的对象时，**软引用本身不会被清理**

如果想要**清理软引用**，需要使用**引用队列**

```java
public class Demo1 {
	public static void main(String[] args) {
		final int _4M = 4*1024*1024;
		//使用引用队列，用于移除引用为空的软引用对象
		ReferenceQueue<byte[]> queue = new ReferenceQueue<>();
		//使用软引用对象 list和SoftReference是强引用，而SoftReference和byte数组则是软引用
		List<SoftReference<byte[]>> list = new ArrayList<>();
        for(int i = 0; i < 5; i++) {
            //关联了引用队列，当软引用所关联的 byte[] 被回收时，软引用自己会加入到 queue 中去
		   SoftReference<byte[]> ref= new SoftReference<>(new byte[_4M], queue);
            list.add(ref);
        }
        
		//遍历引用队列，如果有元素，则移除
		Reference<? extends byte[]> poll = queue.poll();
		while(poll != null) {
			//引用队列不为空，则从集合中移除该元素
			list.remove(poll);
			//移动到引用队列中的下一个元素
			poll = queue.poll();
		}
	}
}
```

**大概思路为：**查看引用队列中有无软引用，如果有，则将该软引用从存放它的集合中移除（这里为一个list集合

##### 弱引用

只有弱引用引用该对象时，在垃圾回收时，**无论内存是否充足**，都会回收弱引用所引用的对象

- 如上图如果B对象不再引用A3对象，则A3对象会被回收
- 可以配合引用队列来释放弱引用自身

**弱引用的使用和软引用类似**，只是将 **SoftReference 换为了 WeakReference**

##### 虚引用

当虚引用对象所引用的对象被回收以后，虚引用对象就会被放入引用队列中，调用虚引用的方法

- 虚引用的一个体现是**释放直接内存所分配的内存**，当引用的对象ByteBuffer被垃圾回收以后，虚引用对象Cleaner就会被放入引用队列中，然后调用Cleaner的clean方法来释放直接内存
- 如上图，B对象不再引用ByteBuffer对象，ByteBuffer就会被回收。但是直接内存中的内存还未被回收。这时需要将虚引用对象Cleaner放入引用队列中，然后调用它的clean方法来释放直接内存

#####  终结器引用

所有的类都继承自Object类，Object类有一个finalize方法。当某个对象不再被其他的对象所引用时，会先将终结器引用对象放入引用队列中，然后根据终结器引用对象找到它所引用的对象，然后调用该对象的finalize方法。调用以后，该对象就可以被垃圾回收了

- 如上图，B对象不再引用A4对象。这是终结器对象就会被放入引用队列中，引用队列会根据它，找到它所引用的对象。然后调用被引用对象的finalize方法。调用以后，该对象就可以被垃圾回收了

##### 引用队列

- 软引用和弱引用**可以配合**引用队列

  在**软引用**和**弱引用**所引用的对象被回收以后，会将这些引用放入引用队列中，方便一起回收这些软/弱引用对象

- 虚引用和终结器引用**必须配合**引用队列

  虚引用和终结器引用在使用时会关联一个引用队列



### 2 垃圾回收算法

####  2.1 标记-清除

![image-20230311165853137](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311165853137.png)

**定义**：标记清除算法顾名思义，是指在虚拟机执行垃圾回收的过程中，先采用标记算法确定可回收对象，然后垃圾收集器根据标识清除相应的内容，给堆内存腾出相应的空间

- 这里的腾出内存空间并不是将内存空间的字节清0，而是记录下这段内存的起始结束地址，下次分配内存的时候，会直接**覆盖**这段内存

**缺点**：**容易产生大量的内存碎片**，可能无法满足大对象的内存分配，一旦导致无法分配对象，那就会导致jvm启动gc，一旦启动gc，我们的应用程序就会暂停，这就导致应用的响应速度变慢



#### 2.2 标记-整理

![image-20230311170020788](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311170020788.png)

标记-整理 会将不被GC Root引用的对象回收，清楚其占用的内存空间。然后整理剩余的对象，可以有效避免因内存碎片而导致的问题，但是因为整体需要消耗一定的时间，所以效率较低。



#### 2.3 复制

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150842.png)

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150907.png)

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150919.png)

将内存分为等大小的两个区域，FROM和TO（TO中为空）。先将被GC Root引用的对象从FROM放入TO中，再回收不被GC Root引用的对象。然后交换FROM和TO。这样也可以避免内存碎片的问题，但是会占用双倍的内存空间。



### 3 分代回收

![image-20230311170520193](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311170520193.png)

#### 3.1 回收流程

- 对象首先分配在伊甸园区域
- 新生代空间不足时，触发 minor gc，伊甸园和 from 存活的对象使用 copy 复制到 to 中，存活的对象年龄加 1并且交换 from to
- minor gc 会引发 stop the world，暂停其它用户的线程，等垃圾回收结束，用户线程才恢复运行
- 当对象寿命超过阈值时，会晋升至老年代，最大寿命是15（4bit）
- 当老年代空间不足，会先尝试触发 minor gc，如果之后空间仍不足，那么触发 full gc，STW的时间更长



新创建的对象都被放在了**新生代的伊甸园**中

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150939.png)

当伊甸园中的内存不足时，就会进行一次垃圾回收，这时的回收叫做 **Minor GC**

Minor GC 会将**伊甸园和幸存区FROM**存活的对象**先**复制到 **幸存区 TO**中， 并让其**寿命加1**，再**交换两个幸存区**

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150946.png)

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608150955.png)

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608151002.png)

再次创建对象，若新生代的伊甸园又满了，则会**再次触发 Minor GC**（会触发 **stop the world**， 暂停其他用户线程，只让垃圾回收线程工作），这时不仅会回收伊甸园中的垃圾，**还会回收幸存区中的垃圾**，再将活跃对象复制到幸存区TO中。回收以后会交换两个幸存区，并让幸存区中的对象**寿命加1**

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608151010.png)

如果幸存区中的对象的**寿命超过某个阈值**（最大为15，4bit），就会被**放入老年代**中

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608151018.png)

如果新生代老年代中的内存都满了，就会先触发Minor GC，再触发**Full GC**，扫描**新生代和老年代中**所有不再使用的对象并回收



#### 3.2 GC分析

- **大对象处理策略**

  当遇到一个**较大的对象**时，就算新生代的**伊甸园**为空，也**无法容纳该对象**时，会将该对象**直接晋升为老年代**

-  **线程内存溢出**

  某个线程的内存溢出了而抛异常（out of memory），不会让其他的线程结束运行

  这是因为当一个线程**抛出OOM异常后**，**它所占据的内存资源会全部被释放掉**，从而不会影响其他线程的运行，**进程依然正常**



#### 3.3 相关 VM 参数

| **含义**           | **参数**                                                     |
| ------------------ | ------------------------------------------------------------ |
| 堆初始大小         | -Xms                                                         |
| 堆最大大小         | -Xmx 或 -XX:MaxHeapSize=size                                 |
| 新生代大小         | -Xmn 或 (-XX:NewSize=size + -XX:MaxNewSize=size )            |
| 幸存区比例（动态） | -XX:InitialSurvivorRatio=ratio 和 -XX:+UseAdaptiveSizePolicy |
| 幸存区比例         | -XX:SurvivorRatio=ratio                                      |
| 晋升阈值           | -XX:MaxTenuringThreshold=threshold                           |
| 晋升详情           | -XX:+PrintTenuringDistribution                               |
| GC详情             | -XX:+PrintGCDetails -verbose:gc                              |
| FullGC 前 MinorGC  | -XX:+ScavengeBeforeFullGC                                    |



### 4 垃圾回收器

#### 4.1 相关概念

- **并行收集**：指多条垃圾收集线程并行工作，但此时**用户线程仍处于等待状态**。
- **并发收集**：指用户线程与垃圾收集线程**同时工作**（不一定是并行的可能会交替执行）。**用户程序在继续运行**，而垃圾收集程序运行在另一个CPU上
- **吞吐量**：即CPU用于**运行用户代码的时间**与CPU**总消耗时间**的比值(吞吐量 = 运行用户代码时间 / (运行用户代码时间 + 垃圾收集时间))，也就是。例如：虚拟机共运行100分钟，垃圾收集器花掉1分钟，那么吞吐量就是99%



#### 4.2 串行

- 单线程
- 内存较小，个人电脑（CPU核数较少）

![image-20230311203036922](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311203036922.png)

**安全点**：让其他线程都在这个点停下来，以免垃圾回收时移动对象地址，使得其他线程找不到被移动的对象

因为是串行的，所以只有一个垃圾回收线程。且在该线程执行回收工作时，其他线程进入**阻塞**状态

##### Serial收集器

Serial收集器是最基本的、发展历史最悠久的收集器

**特点：**单线程、简单高效（与其他收集器的单线程相比），采用**复制算法**。对于限定单个CPU的环境来说，Serial收集器由于没有线程交互的开销，专心做垃圾收集自然可以获得最高的单线程手机效率。收集器进行垃圾回收时，必须暂停其他所有的工作线程，直到它结束（Stop The World）

##### ParNew收集器

ParNew收集器其实就是Serial收集器的多线程版本

**特点**：多线程、ParNew收集器默认开启的收集线程数与CPU的数量相同，在CPU非常多的环境中，可以使用`-XX:ParallelGCThreads`参数来限制垃圾收集的线程数。和Serial收集器一样存在Stop The World问题

##### Serial Old收集器

Serial Old是Serial收集器的老年代版本

**特点**：同样是单线程收集器，采用**标记-整理算法**



#### 4.3 吞吐量优先

- 多线程
- 堆内存较大，多核CPU
- 单位时间内，STW（stop the world，停掉其他所有工作线程）时间最短
- **JDK1.8默认使用**的垃圾回收器

![image-20230311203424657](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311203424657.png)

##### Parallel Scavenge收集器

与吞吐量关系密切，故也称为吞吐量优先收集器

**特点**：属于新生代收集器也是采用**复制算法**的收集器（用到了新生代的幸存区），又是并行的多线程收集器（与ParNew收集器类似）

该收集器的目标是达到一个可控制的吞吐量。还有一个值得关注的点是：**GC自适应调节策略**（与ParNew收集器最重要的一个区别）

**GC自适应调节策略**：Parallel Scavenge收集器可设置`-XX:+UseAdptiveSizePolicy`参数。当开关打开时**不需要**手动指定新生代的大小（-Xmn）、Eden与Survivor区的比例（-XX:SurvivorRation）、晋升老年代的对象年龄（-XX:PretenureSizeThreshold）等，虚拟机会根据系统的运行状况收集性能监控信息，动态设置这些参数以提供最优的停顿时间和最高的吞吐量，这种调节方式称为GC的自适应调节策略。

Parallel Scavenge收集器使用两个参数控制吞吐量：

- XX:MaxGCPauseMillis 控制最大的垃圾收集停顿时间
- XX:GCRatio 直接设置吞吐量的大小

##### **Parallel Old收集器**

是Parallel Scavenge收集器的老年代版本

**特点**：多线程，采用**标记-整理算法**（老年代没有幸存区）



#### 4.4 响应时间优先

- 多线程
- 堆内存较大，多核CPU
- 尽可能让单次STW时间变短（尽量不影响其他线程运行）

![image-20230311203718437](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311203718437.png)

##### CMS收集器

Concurrent Mark Sweep，一种以获取**最短回收停顿时间**为目标的**老年代**收集器

- **特点**：基于**标记-清除算法**实现。并发收集、低停顿，但是会产生内存碎片
- **应用场景**：适用于注重服务的响应速度，希望系统停顿时间最短，给用户带来更好的体验等场景下。如web程序、b/s服务
- **CMS收集器的运行过程分为下列4步：**
  1. **初始标记**：标记GC Roots能直接到的对象。速度很快但是**仍存在Stop The World问题**
  2. **并发标记**：进行GC Roots Tracing的过程，找出存活对象且用户线程可并发执行
  3. **重新标记**：为了**修正并发标记期间**因用户程序继续运行而导致标记产生变动的那一部分对象的标记记录。仍然存在Stop The World问题
  4. **并发清除**：对标记的对象进行清除回收，清除的过程中，可能任然会有新的垃圾产生，这些垃圾就叫浮动垃圾，如果当用户需要存入一个很大的对象时，新生代放不下去，老年代由于浮动垃圾过多，就会退化为 serial Old 收集器，将老年代垃圾进行标记-整理，当然这也是很耗费时间的！

CMS 收集器的内存回收过程是与用户线程一起**并发执行**的，可以搭配 ParNew 收集器（多线程，新生代，复制算法）与 Serial Old 收集器（单线程，老年代，标记-整理算法）使用。



#### 4.5 G1

##### 定义

G1：Garbage First

JDK 9以后默认使用，而且替代了CMS收集器

![img](./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200909201212.png)

##### 适用场景

- 同时注重吞吐量和低延迟（响应时间）
- 超大堆内存（内存大的），会将堆内存划分为多个**大小相等**的区域
- 整体上是**标记-整理**算法，两个区域之间是**复制**算法

**相关参数**：JDK8 并不是默认开启的，所需要参数开启

`-XX:+UseG1GC`
`-XX:G1HeapRegionSize=size`
`-XX:MaxGCPauseMillis=time`

##### G1垃圾回收阶段

![image-20230311204247691](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311204247691.png)

- Young Collection：对新生代垃圾收集
- Young Collection + Concurrent Mark：如果老年代内存到达一定的阈值了，新生代垃圾收集同时会执行一些并发的标记。
- Mixed Collection：会对新生代 + 老年代 + 幸存区等进行混合收集，然后收集结束，会重新进入新生代收集。

新生代伊甸园垃圾回收—–>内存不足，新生代回收+并发标记—–>回收新生代伊甸园、幸存区、老年代内存——>新生代伊甸园垃圾回收(重新开始)

##### Young Collection

**分区算法region**

分代是按对象的生命周期划分，分区则是将堆空间划分连续几个不同小区间，每一个小区间独立回收，可以控制一次回收多少个小区间，方便控制 GC 产生的停顿时间

E：伊甸园 S：幸存区 O：老年代

- **新生代存在 STW**

  ![image-20230311204532007](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311204532007.png)

  ![image-20230311204539570](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311204539570.png)

  ![image-20230311204546963](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311204546963.png)

##### Young Collection + CM

CM：并发标记

- 在 Young GC 时会**对 GC Root 进行初始标记**

- 在老年代**占用堆内存的比例**达到阈值时，对进行并发标记（不会STW），阈值可以根据用户来进行设定

  `-XX:InitiatingHeapOccupancyPercent=percent`（默认45%）

![image-20230311204716538](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311204716538.png)

##### Mixed Collection

会对 E S O 进行**全面的回收**

- 最终标记会 STW
- **拷贝**存活会 STW

`-XX:MaxGCPauseMills:xxx` 用于指定最长的停顿时间

**问**：为什么有的老年代被拷贝了，有的没拷贝？

因为指定了最大停顿时间，如果对所有老年代都进行回收，耗时可能过高。为了保证时间不超过设定的停顿时间，会**回收最有价值的老年代**（回收后，能够得到更多内存）

![image-20230311204818022](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311204818022.png)

##### Full GC

G1在老年代内存不足时（老年代所占内存超过阈值）

- 如果垃圾产生速度慢于垃圾回收速度，不会触发Full GC，还是并发地进行清理
- 如果垃圾产生速度快于垃圾回收速度，便会触发Full GC

> - SerialGC
>   - 新生代内存不足发生的垃圾收集 - minor gc
>   - 老年代内存不足发生的垃圾收集 - full gc
> - ParallelGC
>   - 新生代内存不足发生的垃圾收集 - minor gc
>   - 老年代内存不足发生的垃圾收集 - full gc
> - CMS
>   - 新生代内存不足发生的垃圾收集 - minor gc
>   - 老年代内存不足
> - G1
>   - 新生代内存不足发生的垃圾收集 - minor gc
>   - 老年代内存不足

##### Young Collection 跨代引用

- 新生代回收的跨代引用（老年代引用新生代）问题

  ![image-20230311205106177](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311205106177.png)

- 卡表与Remembered Set

  - Remembered Set 存在于E中，用于保存新生代对象对应的脏卡
    - 脏卡：O被划分为多个区域（一个区域512K），如果该区域引用了新生代对象，则该区域被称为脏卡

- 在引用变更时通过post-write barried + dirty card queue

- concurrent refinement threads 更新 Remembered Set

  ![image-20230311205213174](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311205213174.png)

##### Remark

重新标记阶段

在垃圾回收时，收集器处理对象的过程中

黑色：已被处理，需要保留的 灰色：正在处理中的 白色：还未处理的

![image-20230311205310519](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230311205310519.png)

但是在**并发标记过程中**，有可能A被处理了以后未引用C，但该处理过程还未结束，在处理过程结束之前A引用了C，这时就会用到remark

过程如下

- 之前C未被引用，这时A引用了C，就会给C加一个写屏障，写屏障的指令会被执行，将C放入一个队列当中，并将C变为 处理中 状态
- 在**并发标记**阶段结束以后，重新标记阶段会STW，然后将放在该队列中的对象重新处理，发现有强引用引用它，就会处理它

<img src="./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608151239.png" alt="img" style="zoom: 67%;" />

##### JDK 8u20 字符串去重

`-XX:+UseStringDeduplication`

```java
String s1 = new String("hello"); // char[]{'h','e','l','l','o'}
String s2 = new String("hello"); // char[]{'h','e','l','l','o'}
```

过程

- 将所有新分配的字符串（底层是char[]）放入一个队列
- 当新生代回收时，G1并发检查是否有重复的字符串
- 如果字符串的值一样，就让他们**引用同一个字符串对象**
- 注意，其与String.intern()的区别
  - intern关注的是字符串对象
  - 字符串去重关注的是char[]
  - 在JVM内部，使用了不同的字符串标

优点与缺点

- 节省了大量内存
- 新生代回收时间略微增加，导致略微多占用CPU

##### JDK 8u40 并发标记类卸载

在并发标记阶段结束以后，就能知道哪些类不再被使用。如果一个类加载器的所有类都不在使用，则卸载它所加载的所有类

`-XX:+ClassUnloadingWithConcurrentMark` 默认启用

##### JDK 8u60 回收巨型对象

- 一个对象大于region的一半时，就称为巨型对象
- G1不会对巨型对象进行拷贝
- 回收时被优先考虑
- G1会跟踪老年代所有incoming引用，如果老年代incoming引用为0的巨型对象就可以在新生代垃圾回收时处理掉

<img src="./02-JVM-%E9%BB%91%E9%A9%AC.assets/20200608151249.png" alt="img" style="zoom:67%;" />

##### JDK 9并发标记起始时间的调整

- 并发标记必须在堆空间占满前完成，否则退化为 FulGC
- JDK 9 之前需要使用 -XX:InitiatingHeapOccupancyPercent
- JDK 9 可以动态调整
  - -XX:InitiatingHeapOccupancyPercent 用来设置初始值
  - 进行数据采样并动态调整
  - 总会添加一个安全的空挡空间

##### JDK 9更高效的回收

- 250+增强
- 180+bug修复
- https://docs.oracle.com/en/java/javase/12/gctuning



### 5 垃圾回收调优

预备知识

- 掌握 GC 相关的 VM 参数，会基本的空间调整
- 掌握相关工具
- 明白一点：调优跟应用、环境有关，没有放之四海而皆准的法则

查看虚拟机参数命令

```bash
"X:\newJava\JDK8\bin\java" -XX:+PrintFlagsFinal -version | findstr "GC"
```

可以根据参数去查询具体的信息



#### 5.1 调优领域

- 内存
- 锁竞争
- CPU占用
- IO
- GC



####  5.2 确定目标

低延迟/高吞吐量？ 选择合适的GC

- CMS G1 ZGC
- ParallelGC
- Zing



#### 5.3 最快的GC是不发生GC

首先排除减少因为自身编写的代码而引发的内存问题

- 查看Full GC前后的内存占用，考虑以下几个问题
  - 数据是不是太多？
    - resultSet = statement.executeQuery(“select * from 大表 limit n”)
  - 数据表示是否太臃肿
    - 对象图
    - 对象大小 16 Integer 24 int 4
  - 是否存在内存泄漏
    - static Map map …
    - 软
    - 弱
    - 第三方缓存实现



#### 5.4 新生代调优

- 新生代的特点
  - 所有的new操作分配内存都是非常廉价的
    - TLAB(thread-lcoal allocation buffer)
  - 死亡对象回收零代价
  - 大部分对象用过即死（朝生夕死）
  - MInor GC 所用时间远小于Full GC
- 新生代内存越大越好么？
  - 不是
    - 新生代内存太小：频繁触发Minor GC，会STW，会使得吞吐量下降
    - 新生代内存太大：老年代内存占比有所降低，会更频繁地触发Full GC。而且触发Minor GC时，清理新生代所花费的时间会更长
  - 新生代内存设置为内容纳[`并发量 * (请求 - 响应)`]的数据为宜



#### 5.5 幸存区调优

- 幸存区需要能够保存 **当前活跃对象**+**需要晋升的对象**
- 晋升阈值配置得当，让长时间存活的对象尽快晋升

```bash
-XX:MaxTenuringThreshold=threshold
-XX:+PrintTenuringDistrubution
```



#### 5.6 老年代调优

以 CMS 为例：

- CMS 的老年代内存越大越好
- 先尝试不做调优，如果没有 Full GC 那么已经...，否者先尝试调优新生代
- 观察发现 Full GC 时老年代内存占用，将老年代内存预设调大 1/4 ~ 1/3

```bash
-XX:CMSInitiatingOccupancyFraction=percent
```



#### 5.7 案例

- 案例1：Full GC 和 Minor GC 频繁	*加大新生代内存，提高晋升阈值*
- 案例2：请求高峰期发生 Full GC，单次暂停时间特别长(CMS)    *在重新标记前在新生代先回收一次*
- 案例3：老年代充裕情况下，发生 Full GC(jdk1.7)    *加大永久代*

+++

## *类加载与字节码技术*

![image-20230313203455058](./02-JVM-%E9%BB%91%E9%A9%AC.assets/image-20230313203455058.png)























































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































