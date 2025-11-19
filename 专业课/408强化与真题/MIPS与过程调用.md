
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

（3）Q 保存 P 的现场中，有可能 Q 需要使用 P 中保存老值的寄存器，比如 s 0 ~ s 7 寄存器，这个时候必须将 s 0 ~ s 7 压栈保存后过程 Q 才能使用

（5）一般是放到 v 0 ~ v 1 的通用寄存器中
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026175531.png)


执行这个过程调用的指令如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026085746.png)

这里是 PC + 4，意思是 MIPS 一条指令是 4 B，并且主存是按字节编址

### MIPS 的栈顶寄存器是什么？它指向什么位置？入栈和出栈操作是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026175221.png)

（1）$sp 指针指向栈顶元素，sp 寄存器中存放的是栈顶元素的地址
（2）入栈一个字（32 bit），sp 先往下减小 4 个存储单元单位，然后再用指令 sw 存数
（3）每出栈一个字（32 bit）, 先 lw 指令将字取出，然后sp 再往上增大 4 个存储单元单位
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

### 一个函数的 MISP 指令具体例子（能分析即可，不要求写出汇编）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251119113448.png)

第一行：`sll` 是逻辑左移指令，意思是将寄存器 a 1 中的内容左移两位，然后放在寄存器 t 1 中，这个函数中要求出 v[k]对应的元素，一个元素 4 个存储单元（4 B），所以在取元素的时候是 v + 4 * k 拿到 v[k]所在的地址，因此这里 $a 1 寄存器存放的就是参数 k，这里让 k * 4 然后放到寄存器 t 1 中，拿到 v[k]相对于数组起始地址 v 偏移 4 * k

第二行：这里 a 0 + t 1 再放到 t 1 中，根据第一行的分析，显然此时 a 0 存放的就是数组 v 的起始地址，然后加上 4 * k 的偏移，得到 v[k]的地址了

第三行：t 1 中保存的就是 v[k]的有效地址了，这里 lw 指令就是取出元素 v[k]，放入到 t 0 中

第四行：t 1 中保存的就是 v[k]的有效地址了，那么 t 1 + 4，这个就是 v[k + 1]的有效地址，此时将其取出放到 t 2 中

第五行：此时 t 0 中存放的是 v[k]，t 2 中存放的是 v[k + 1]，如果要交换的话，就将 t 0 的值放入 v[k + 1]位置处，将 t 2 的值放入到 v[k]处即可，所以这一行的指令功能就是将 t 2 中存放的原来 v[k + 1]的值放入到 v[k]处

第六行：这一步是将 t 0 中存放的原来 v[k]的值存放到 v[k + 1]处，此时完成了 v[k]和 v[k + 1]原来两个元素的互换

### 一个复杂的过程调用例子（袁书）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251119115724.png)

假设从主函数开始调用了函数 set_array，右侧是函数 set_array 的函数调用过程的完整汇编 MIPS 指令

第一行：这里先开辟 set_array 的函数栈，这个栈的最开始的大小可以直接计算出来，set_array 这个函数中有 10 个 array 数组元素，所以是 40 B，再加上函数调用开头保存的 4 个寄存器的 4 B 的值，因此最开始的栈空间是 40 B + 16 B = 56 B，所以先让 sp 指针直接指向这个函数栈的开头了，即 sp - 56 的位置处：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251119150120.png)


第二行：这里需要保存 ra 中完成 set_array 函数之后的返回地址，它保存的地方是栈底，此时已经开辟了一个 56 B 的栈，并且 sp 指向了栈顶，所以栈底 4 B 的位置就是 sp - 52 处，因此将 ra 保存到这里

第三行：同理，保存函数调用之前的 fp 指针内容到 ra 保存位置的下面（靠近栈顶方向），因此是 sp - 48 的位置处

