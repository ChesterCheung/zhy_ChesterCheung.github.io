---
layout: post
title:  "Operating System 1"
date: 2019-10-22 18:00:00
categories: OS
tags: 
  - OS 
  - Linux
cover: >- 
  http://csuzhang.info/photos/OS-1.png
---



## OS基本概念、系统调用、中断

### 一、操作系统特征

1. 并发：多个事件在同一时间间隔内同时发生，在宏观上是同时发生的，在微观上是交替发生的；
**区分：并行（多个事件在同一时刻同时发生，同一时刻运行多个指令，需要硬件支持：比如多处理器）**






2. 共享：系统中的资源被多个并发的进程共同使用，分为互斥共享和同时共享；

互斥共享：互斥共享的资源被称为临界资源，即同一时间只允许一个进程访问，需要同步机制来实现

同时共享：同时共享比如有对硬件资源的访问

3. 虚拟：将一个物理实体转换为多个逻辑实体，分为时分复用和时分复用；

时分复用：多进程在同一处理器上并发执行（类似于轮询的方式）

空分复用：将物理内存抽象为地址空间，每个进程有各自的地址空间

4. 异步：进程不是一次执行完毕，而是走走停停。

#### Question

试述多道程序设计技术的基本思想。为什么采用多道程序设计技术可以提高资源利用率？

> 多批道程序设计技术的**基本思想**是，在主存同时保持多道程序，主机以交替的方式同时处理多道程序。从宏观上看，主机内同时保持和处理若干道已开始运行但尚未结束的程序。从微观上看，某一时刻处理机只运行某道程序。 可以提高资源利用率的原因：由于任何一道作业的运行总是交替地串行使用CPU、外设等资源，即使用一段时间的CPU，然后使用一段时间的I/O设备，由于采用多道程序设计技术，加之对多道程序实施合理的运行调度，则可以实现CPU和I/O设备的高度并行，可以大大提高CPU与外设的利用率。

什么是分时系统？其主要特征是什么？适用于哪些应用？

> 分时系统是以多批道程序设计技术为基础的交互式系统，在此系统中，一台计算机与多台终端相连接，用户通过各自的终端和终端命令以交互的方式使用计算机系统。每个用户都感觉到好像是自己在独占计算机系统，而在系统内部则由操作系统以时间片轮转的方式负责协调多个用户分享CPU。 主要特征是： 
**并行性**：系统能协调多个终端用户同时使用计算机系统，能控制多道程序同时运行。
**共享性**：对资源而言，系统在宏观上使各终端用户共享计算机系统中的各种资源，而在微观上它们则分时使用这些资源。
**交互性**：人与计算机以交互的方式进行工作。 
**独占性**：使用户感觉到他在独占使用计算机。现在的系统大部分都是分时系统，主要应用于人机交互的方面。

### 二、系统调用

Definition: 如果一个进程在用户态需要使用内核态的功能，就进行系统调用从而陷入内核，由操作系统代为完成 。

即为操作系统的作用，作为用户和操作系统你底层硬件之间的接口，系统调用是操作系统为上层提供接口，包括有**命令接口**和**程序接口**

通过汇编语言可以进行系统调用，而汇编语言又为C语言向上提供了接口；C语言经过封装，又为上层的高级语言提供了接口，最终形成了我们使用的高级计算机语言，比如java、phthon等。**这种系统调用->汇编语言->C语言->高级语言->应用程序**的模式就是硬件和用户使用的应用程序之间的交互模式，是不是很有意思呢？haha

### 三、操作系统的体系结构

![OS-1](http://csuzhang.info/photos/OS-1.png)

> 通过程序状态寄存器PSW中的某个标记位来标记当前处理器的状态

**OS的内核程序是操作系统的管理者，运行在核心态。**

大内核、微内核：大内核相比之下多了系统资源管理部分的功能（不同操作系统，对于内核功能的划分可能不太一样）

大内核：是将操作系统功能作为一个紧密结合的整体放到内核。由于各模块共享信息，因此有很高的性能。

微内核：操作系统被划分成小的、定义良好的模块，只有微内核这一个模块运行在内核态，其余模块运行在用户态。**因为需要频繁地在用户态和核心态之间进行切换，所以会有一定的性能损失。**

### 四、中断异常处理

Definition：

中断:是指来自CPU执行指令以外的事件发生后，处理机暂停正在运行的程序，转去执行处理该事件的程序的过程。

异常:是指源自CPU执行指令内部的事件发生后，处理机暂停正在执行的程序，转去处理该事件的过程。

区别：广义的中断包括中断和异常，统一称为中断。狭义的中断(外中断，平常说的中断)和异常的区别在于是否与正在执行的指令有关，中断可以屏蔽，而异常不可屏蔽。

#### 中断

产生：为了实现多道程序并发执行而引入的技术

作用：发生中断后，CPU会进入核心态

> 中断是CPU从用户态进入核心态的唯一途径

分类：外中断: 由 CPU 执行指令以外的事件引起，如I/O完成中断，表示设备输入/输出处理已经完成，处理器能够发送下一个输入/输出请求。此外还有时钟中断、控制台中断等。

内中断: ①异常: 由 CPU 执行指令的内部事件引起，如非法操作码、地址越界、算术溢出等。②陷入: 在用户程序中使用系统调用。

> **如何判断内外中断**
中断信号来自内部还是外部

#### 总结中断、异常和系统调用: 

|类型|源头|响应方式|处理机制|
|--|--|--|--|
|中断(外中断)|外设|异步|持续、对用户应用程序是透明的|
|异常(内中断)|应用程序未知的行为|同步|杀死或重新执行这些未知的应用程序指令|
|系统调用|应用程序请求操作系统提供服务|异步或者同步|等待和持续|


#### Question

1. 什么是中断向量？其内容是什么？试述中断的处理过程。

**中断向量**：为处理方便，一般为系统中每个中断信号编制一个相应的中断处理程序，并把这些程序的入口地址放在特定的主存单元中。通常将这一片存放中断处理程序入口地址的主存单元称为中断向量。 **中断向量的内容**：对不同的系统，中断向量中的内容也不尽相同。一般每一个中断信号占用连续的两个单元：一个用来存放中断处理程序的入口地址，另一个用来保存在处理中断时CPU应具有的状态。 **中断的处理过程**：一般包括保存现场，分析中断原因，进入相应的中断处理程序，最后重新选择程序运行，恢复现场等过程。

2. 为什么要把中断分级？如何设定中断的优先级？试述多级中断的处理原则。 (有关中断优先级)

**为什么要把中断分级**：在计算机系统中，不同的中断源可能在同一时刻向CPU发出不同的中断信号，也可能前一中断尚未处理完，紧接着又发生了新的中断。此时，存在谁先被响应和谁先被处理的优先次序问题。为了使系统能及时地响应和处理所发生的紧急中断，根据中断的轻重缓急，对各类中断规定了高低不同的响应级别。 **如何设定中断的优先级**：中断分级的原则是根据中断的轻重缓急来排序，把紧迫程度大致相当的中断源归并在同一级，而把紧迫程度差别较大的中断源放在不同的级别。一般来说，高速设备的中断优先级高，慢速设备的中断优先级低。 **多级中断的处理原则**：当多级中断同时发生时，CPU按照由高到低的顺序响应。高级中断可以打断低级中断处理程序的运行，转而执行高级中断处理程序。当同级中断同时到时，则按位响应。
