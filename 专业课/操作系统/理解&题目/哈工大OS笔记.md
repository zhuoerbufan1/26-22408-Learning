
### 系统调用的本质是什么？为什么应用程序不能直接执行系统调用的程序？

以下面的这份代码为例：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250712142435.png)

其实本质上 main () 这些用户的代码与 printf () 这个函数的实现代码都在内存中，但是用户调用 printf () 这些实现代码的时候就是不能直接调用，就是不能是普通的函数调用

因为 printf () 的函数实现的指令中包含对硬件显示器的操控，用户程序就是不能直接使用这段对显示器操控的代码，因为一旦有恶意的用户程序能够直接操控计算机的硬件关键资源的话是一件非常危险的事情，如果应用程序可以随意执行 jmp 和 mov 指令的话，就可以随意查看计算机的密码和别人的硬盘内容了

本质上应用程序的代码和操作系统的代码全部都在内存中，应用程序只能通过请求操作系统来完成 printf () 对显示器的操控，这个就是系统调用要完成的工作了

### CPU 是如何实现指令访问隔离的？

==CPL 的概念==

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250712144805.png)


==DPL 的概念==

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250712145006.png)

**GDT（Global Descriptor Table，全局描述符表）表项**是构成 GDT 的核心元素，通常被称为**段描述符**。它们是存储在 GDT 中的数据结构，每个描述符占用 **8 字节**，用于向 CPU 精确描述一个内存段的关键属性和访问控制信息。

==RPL 的概念==

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250712145342.png)


==指令的访问隔离：==

当用户程序想要通过 mov 或者 jmp 指令访问内存中的其他段的数据的时候，CPU 会进行检查当前指令的特权级，即 CS: IP 指向的指令的 CPL 与目标段的特权级即 DPL 的大小

只有 CPL <= DPL 的时候，表明当前指令的特权级比要访问的目标段的特权级更高，此时才能进行访问

通常只用考虑 CPL 就行了，RPL 也可以显示设置，不过一般设置为 CPL 的值


那么再看下面的这段代码：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250712142435.png)

在执行用户程序的时候此时 CPL = 3, 而 printf () 函数的实现在内核中，其 DPL = 0，当用户程序试图访问其实现的时候 CPU 就会进行检查，发现 CPL > DPL，此时就拒绝访问了

但是操作系统还是提供了一个访问目标端优先级更高的方法，那就是中断