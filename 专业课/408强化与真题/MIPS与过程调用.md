
### R 型指令的基本格式是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026080627.png)

一些常见的操作如下：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026080647.png)

### I 型指令的基本格式是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026080840.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026080847.png)

### J 型指令的基本格式是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026080907.png)

jal 在这里就是过程调用

### MIPS 提供了多少个寄存器？

32 个寄存器，根据上文的 MIPS 指令的格式也可以看出来

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026081355.png)

v 0 ~ v 1 用于函数返回值保存的寄存器

a 0 ~ a 3 是函数传递的形参保存的寄存器

t 0 ~ t 7 是函数执行过程中保存的临时变量，当这个函数调用另一个函数的时候这些寄存器中的值可以不用保存

a 0 ~ a 7 是函数执行过程中保存的临时变量，当这个函数调用另一个函数的时候这些寄存器中的值应该被保存

### MIPS 是如何开辟内存栈的？

MIPS 在内存中开辟一个栈，有栈底和栈顶，并且地址是从高到低的，栈底在高地址地方

如果想要开辟一个新的栈，则会在地址更低的地方开辟一个新的栈

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026081728.png)

### MIPS 一些常用的指令

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026083912.png)


### 循环语句如何用 MIPS 来表示？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026084206.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026084216.png)

这里代码中：

第一行：是 while 循环中的条件判断，如果不满足，则直接跳到 exit，跳出循环

第二行：s 2 中存放的是 i，这一行是将 2 * i 放入到 s 7 中

第三行：这一行是将 4 * i 放入到 s 7 中，第二行和第三行的目的是让 i 扩大四倍

第四行：这一行是将 A + 4 * i 的结果放入 s 7 中，这一步是得到数组中第 i 个元素的首地址，这里 i 之所以要扩大四倍是因为数组中一个元素占 4 个字节，第 0 个元素地址是 A，第 1 个元素地址就是 A + 4，第 2 个元素地址就是 A + 4 * 2，因此第 i 个元素地址就是 A + 4 * i

第五行：这一行是从内存中在地址 A + 4 * i 的位置取出数组元素，即取出第 i 个元素，将 A[i]取出放到 s 6 中

第六行：这一行就是执行加法操作，x + A[i] 的值再放到 s 1，x 寄存器中

第七行：让 i + 1，取下一个 A[i]元素

第八行：跳转到循环最开始的条件判断

### 过程（函数）的栈空间是什么？

其实就是上文过程开辟的栈，比如 P 如果调用 Q 过程，就会在 P 的栈下面再开辟一个栈空间这个栈空间的起始结束地址由编译器决定：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026090004.png)

### 过程调用的基本步骤是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026085521.png)

（1）P 过程一般就是将参数放到 a 0 ~ a 3 这 3 类寄存器中

（2）P 过程将返回地址放到 ra 寄存器中，并且这里存放的是调用 Q 指令的下一条指令，即 PC + 4 的指令地址，因为 P 调用 Q 如上图右边所示，Q 返回的时候显然是返回到调用 Q 的下一条指令中

（3）Q 保存 P 的现场中，有可能 Q 需要使用 P 中保存老值得寄存器，比如 s 0 ~ s 7 寄存器，这个时候必须将 s 0 ~ s 7 压栈保存后过程 Q 才能使用


（5）一般是放到 v 0 ~ v 1 的通用寄存器中
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026175531.png)


执行这个过程调用的指令如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026085746.png)

这里是 PC + 4，意思是 MIPS 一条指令是 4 B，并且主存是按字节编址

### MIPS 的栈顶寄存器是什么？它指向什么位置？入栈和出栈操作是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026175221.png)

### 栈空间中栈顶寄存器和栈底寄存器分别指向哪里？

栈顶指针寄存器 sp 指向栈顶元素的地址

栈底指针寄存器 fp 指向栈底元素的地址

注意栈是从高地址向低地址进行增长的，所以从图像上来看 fp 会指向图像中栈底往下一点点

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026175939.png)

### 过程调用栈顶指针寄存器 sp 和栈底指针寄存器 fp 的变化过程是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026180333.png)

### 过程 P 在调用过程 Q 之前保存返回地址 ra 和参数的具体过程是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026180434.png)

右侧的指令：

第一条：先将过程 P 的栈顶指针 sp 往下移动两个元素单位，即 sp - 8，栈顶往下增长两个单位
第二条：将地址保存到 sp 指向位置的下一个位置上
第三条：将参数保存到 sp 指向的位置上