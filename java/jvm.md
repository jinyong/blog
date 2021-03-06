# 概述
Java虚拟机有自己完善的硬件架构，如处理器、堆栈、寄存器等，还具有相应的指令系统。

Java虚拟机本质是就是一个程序，当它在命令行上启动的时候，就开始执行保存在某字节码文件中的指令。Java语言的可移植性正是建立在Java虚拟机的基础上。任何平台只要装有针对于该平台的Java虚拟机，字节码文件（.class）就可以在该平台上运行。这就是“一次编译，多次运行”。

Java虚拟机不仅是一种跨平台的语言，而且是一种新的网络计算平台。该平台包括许多相关的技术，如符合开放接口标准的各种API、优化技术等。Java技术使同一种应用可以运行在不同的平台上。Java平台可分为两部分，即Java虚拟机（Java virtual machine，JVM）和Java API类库。

# 体系结构
在Java虚拟机规范中，分别用子系统、内存区、数据类型以及指令这几个术语来描述的。这些组成部分一起展示出一个抽象化的虚拟机内部的抽象体系结构。

Java虚拟机主要分为五大模块：类装载器子系统、运行时数据区、执行引擎、本地方法接口和垃圾收集模块。其中垃圾收集模块在Java虚拟机规范中并没有要求Java虚拟机垃圾收集，但是在没有发明无限的内存之前，大多数JVM实现都是有垃圾收集的。
而运行时数据区都会以某种形式存在于每一个JAVA虚拟机实例中，但是Java虚拟机规范对它的描述却是相当抽象。这些运行时数据结构上的细节，大多数都由具体实现的设计者决定。

Java虚拟机不是真实的物理机，它没有寄存器，所以指令集是使用Java栈来存储中间数据，这样做的目的就是为了保持Java虚拟机的指令集尽量的紧凑，同时也便于JAVA虚拟机在那些只有很少通用寄存器的平台上实现。另外，JAVA虚拟机的这种基于栈的体系结构，有助于运行时某些虚拟机实现的动态编译器和即时编译器的代码优化。

# 内存管理
<ol>
  <li>
    对于Java运行时涉及到的存储区域主要包括程序计数器、Java虚拟机栈、本地方法栈、java堆、方法区以及直接内存等等。对于每个部分，都有其使用的条件。程序计数器主要是取下一条指令，在Java里面主要是取下一条指令的字节码文件；Java虚拟机栈主要是利用栈先进后出的特性存储局部变量表，动态链接等，主要包括堆内存和栈内存，对于程序员内存分析而言是特别重要的。本地方法栈与上边的栈基本作用差不多，只不过这里是为Java方法而服务。Java堆是内存管理中最大的一块，所有的线程共享这一块内容，同时该部分也是垃圾收集器的主要区域。
  </li>
  <li>
    虚拟机的垃圾回收机制是完善的，动态内存分配和回收是比较成熟的，在内存管理机制中，大部分都不需要我们考虑内存回收，只有Java堆和方法区需要我们考虑处理内存问题。一般的对于内存回收首先就是判断某一个部分是生存还是死亡，主要是通过下面二种算法：
    
    其一是引用计数算法，本算法实现简单，判定的效率也是比较高的，很多的软件都使用了该算法，但是主流的Java并没有选择该算法，核心的问题是该算法难以处理对象之间相互调用的问题。
    
    其二是称可达性分析算法，该算法核心思想是依靠判断对象是否存活来实现的，本算法是通过一系列的GC ROOTS的对象作为起始点，采用搜索的算法遍历引用链，如果搜索过程中没有发现该节点，则认为该节点是不可达的，即可回收的，在现在的主流Java里面，一般可以使用该算法处理问题。
  </li>
</ol>

# 特性
<ol>
  <li>
    移植性
    
    无论是GC还是Hotspot都可以用在任何Java可用的地方。比方说，JRuby可以运行在其他平台上，Rails应用就可以运行在IBM主机上的JRuby上，而且这台IBM主机运行的是CP/CMS.实际上，由于Java和OpenJDK项目的开源，我们正在看到越来越多的平台的衍生，因此JVM的移植性也将越来越棒。
  </li>
  <li>
    成熟
    
    JVM已有多年的历史，在过去的这些年里，许多开发者为它做出了许多贡献，使得它的性能一次又一次地提升，让JVM变得更加稳定、快速和广泛。
  </li>
  <li>
    覆盖面
    
    JRuby和JVM上的其他语言项目已经被承认，一个例子是invokedynamic specification（akaJSR292）。JSR越来越配合新的语言，JVM已不再是Java一个人定制规则。JVM正在构建成为类如JRuby等项目的优良平台。还有一个MLVM（multiple languageVM）项目，好比是新特性的清算机构，是一个许多企业应用的开发者试图添加应用的地方，而这些应用正是他们想在JVM中看到的。而且JVM开发者互相协作、彼此影响，无疑这有利于JVM新特性的诞生。这些细节都可以看到JVM正在关注开发者的需求，扩大他的覆盖面。
  </li>
</ol>

# GC垃圾回收
## 二、 发现虚拟机频繁full GC时应该怎么办？
（full GC指的是清理整个堆空间，包括年轻代和永久代）

（1） 首先用命令查看触发GC的原因是什么 jstat –gccause 进程id

（2） 如果是System.gc()，则看下代码哪里调用了这个方法

（3） 如果是heap inspection(内存检查)，可能是哪里执行jmap –histo[:live]命令

（4） 如果是GC locker，可能是程序依赖的JNI库的原因

## 常见的垃圾回收算法
### 1、Mark-Sweep（标记-清除算法）
（1）思想：标记清除算法分为两个阶段，标记阶段和清除阶段。标记阶段任务是标记出所有需要回收的对象，清除阶段就是清除被标记对象的空间。

（2）优缺点：实现简单，容易产生内存碎片
### 2、Copying（复制清除算法）
（1）思想：将可用内存划分为大小相等的两块，每次只使用其中的一块。当进行垃圾回收的时候了，把其中存活对象全部复制到另外一块中，然后把已使用的内存空间一次清空掉。

（2）优缺点：不容易产生内存碎片；可用内存空间少；存活对象多的话，效率低下。
### 3、Mark-Compact（标记-整理算法）
（1）思想：先标记存活对象，然后把存活对象向一边移动，然后清理掉端边界以外的内存。

（2）优缺点：不容易产生内存碎片；内存利用率高；存活对象多并且分散的时候，移动次数多，效率低下
### 4、分代收集算法：（目前大部分JVM的垃圾收集器所采用的算法）
思想：把堆分成新生代和老年代。（永久代指的是方法区）

（1） 因为新生代每次垃圾回收都要回收大部分对象，所以新生代采用Copying算法。新生代里面分成一份较大的Eden空间和两份较小的Survivor空间。每次只使用Eden和其中一块Survivor空间，然后垃圾回收的时候，把存活对象放到未使用的Survivor（划分出from、to）空间中，清空Eden和刚才使用过的Survivor空间。

（2） 由于老年代每次只回收少量的对象，因此采用mark-compact算法。

（3） 在堆区外有一个永久代。对永久代的回收主要是无效的类和常量
### 5、GC使用时对程序的影响
垃圾回收会影响程序的性能，Java虚拟机必须要追踪运行程序中的有用对象，然后释放没用对象，这个过程消耗处理器时间
### 6、几种不同的垃圾回收类型
（1）Minor GC：从年轻代（包括Eden、Survivor区）回收内存。
<ol>
  <li>
    A、当JVM无法为一个新的对象分配内存的时候，越容易触发Minor GC。所以分配率越高，内存越来越少，越频繁执行Minor GC
  </li>
  <li>
    B、执行Minor GC操作的时候，不会影响到永久代（Tenured）。从永久代到年轻代的引用，被当成GC Roots，从年轻代到老年代的引用在标记阶段直接被忽略掉。
  </li>
</ol>
（2）Major GC：清理整个老年代，当eden区内存不足时触发。

（3）Full GC：清理整个堆空间，包括年轻代和老年代。当老年代内存不足时触发
