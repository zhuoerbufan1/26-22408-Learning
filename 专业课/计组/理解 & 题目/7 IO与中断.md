## 理解

## 题目

### 1 磁盘驱动器与磁盘控制器的概念
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250627202542.png)


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250627202524.png)

磁盘控制器才是磁盘的 IO 接口，其结构如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250627202753.png)

它负责将来做 CPU 的并行数据转换成串型数据，然后发送给磁盘驱动器，磁盘驱动器其实就是磁头和盘片这些实际物理组件

磁盘驱动器将来自磁盘控制器的串型数据写入到磁盘中

### 2 IO 指令的概念理解

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250627203239.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250627210141.png)

IO 指令是 IO 独立编址中专门用来访问 IO 设备的指令，比如 X 86 中的 IN 和 OUT 指令

A:
系统指令，更准确的说法是 **特权指令**。这些是 CPU 指令集中只能在最高特权级别（通常是内核态或 Ring 0）下才能成功执行的指令，IO 指令，比如 IN 或者 OUT 指令；因为 IO 设备可能连接一些非常关键的东西，比如持久化到磁盘的数据，比如向网络发送一些包，比如显示器显示的东西；这些东西如果可以随意被修改的化将会造成比较严重的后果，比如磁盘数据篡改导致系统崩溃，比如伪造数据包发送到网络上进行黑客攻击，比如瞎写显示器导致无法使用计算机，所以 IO 指令也是特权指令的一部分

B：
显然正确

C：
显然正确

D:
IO 指令的格式一般比较简单，不会像访存指令一样支持很多的寻址格式，所以不一定与通用指令的格式一样

==第二题==

IO 指令作用就是将 CPU 的指令传递给 IO 设备，但是实际上是先传递给 IO 接口中的 IO 端口，然后 IO 端口再将数据写到 IO 设备中
### 3 

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250627204459.png)


### 4 通信总线的概念和三总线结构

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250627204547.png)
三总线结构如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250627205527.png)

根据三总线结构，IO 总线是与 IO 接口相连

这里通信总线是连接计算机系统之间的总线，一个具体的例子是手机通过数据线连接到 USB 接口上，USB 接口就是 IO 接口，其通过数据线这样一个通信总线连接另外一个计算机设备：手机

