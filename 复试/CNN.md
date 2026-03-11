### 图像在计算机中的表示

对于一个单通道的灰度图像来说，它的图像其实就是一个矩阵，每个矩阵都是 0 ~ 2 55 的数字大小，越靠近黑色数字就越小，越靠近 255 数字就越大

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309090652.png)

对于彩色图像来说它是三通道的，每个通道都是一个矩阵，每个矩阵表示红，绿，蓝三种颜色的深浅（因为任意一种颜色都由这三个颜色构成），由 3 个矩阵叠加就可以表示出这个彩色图像了：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309090838.png)

### 全连接神经网络存在的问题

全连接神经网络的输入是一个行向量（见 Dezero 中的分析），它始终是一维的，而图像是二维的，如果使用全连接神经网络的话则必须将二维的数据拆成一维的进行训练拟合，这样其实丢失打散了原来矩阵图像中的信息（前后上下之前的信息）

而 CNN 的输入直接就是一个矩阵
### 卷积运算

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309095812.png)
![](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309095854.png)
实际上这里的过滤器就类似于全连接神经网络中的 x * W = y 中的参数 W，整个运算相当于代替了全连接神经网络中的线性运算过程

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309095725.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309095854.png)
### 卷积运算步幅

实际上就是用核与输入进行卷积运算的时候滑动的幅度，比如步幅为 2 的卷积运算：
当然这里的步幅会导致最后的输出维度出现变化
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309100216.png)

### 填充操作

根据上文的运算过程，实际上进行卷积之后会导致输出维度相比输入来说不断下降，如果进行卷积操作之后输出维度太小，这就会导致无法继续进行卷积了，这时就需要进行填充，比如下图就将原本的 3 * 3 的输出填充成 5 * 5 作为下一次卷积运算的输入：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309100841.png)

步幅和填充都会导致卷积运算的输出维度进行变化

### 根据输入维度，步幅，填充之后的维度计算输出维度的公式

![image.png|299](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309101319.png)

这里 OH 是输出的高，H 是原来输入的高，P 是填充的圈数，下图是填充了一圈的 0，FH 是核的高，S 是步幅

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309101345.png)

### 多通道数据卷积

实际上还是加法运算

如果输入是 3 个通道，那么核也会是 3 个通道，然后将核与输入进行逐元素叠加得到输出数据：

![image.png|627](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309104501.png)

这里的输出数据的通道数与输入的通道数是无关的

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309104557.png)

如果想要得到输出特征图是多通道的，那么就得多个卷积核才行：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309104815.png)


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309163157.png)

这里的 OC 是卷积核的个数，C 是通道的个数
### 池化运算

池化实际上就是找输入数据中某个区域的最大值或者平均值，池化操作是没有参数的，没有池化核

比如下面的平均池化操作：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309105737.png)

上图就是计算小区域中的平均值

池化操作也是有步幅和填充操作，输入特征值和输出特征图大小仍然符合下面的公式：

$$
OH = \frac{H + 2P - KH}{S} + 1
$$

不过这里的 KH 不是核的高度，而是上图中的小区域的高度：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309105953.png)

### 小批量数据处理

神经网络的训练过程中一般是多个数据合并起来进行处理的，比如全连接神经网络中 30 个 1 *  2 的数据形成了 30 * 2 的矩阵 X，通过 X * W = Y 进行训练和参数调整

在这里也是一样，可能有多个 3 通道的数据参与到神经网络中的训练，这就形成了 4 阶张量：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309163935.png)

### img 2 col 算法

（1）卷积核的通道数与输入特征图的通道数保持一致
（2）卷积核的个数与输出特征图的通道数保持一致

这个算法的本质是将卷积滑动变成两个大矩阵的运算，最后将结果还原成输出特征图

设输入特征图的形状是 `(N, C, IH, IW)`

卷积核的形状是 `(KN, C, KH, KW)`

那么输出特征图的形状就是 `(OC, OH, OW)`


#### 单通道情况

![image.png|477](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311091600.png)
![image.png|479](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311091614.png)

只要将左边的输入矩阵的单个卷积核的窗口多排列几次就会得到：

![image.png|597](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311091838.png)

上图左侧是输入特征图按照卷积核的窗口拆成的一个个行向量，右侧是单个卷积核拆成的一个列向量，两者相乘得到了一个列向量，这个列向量按顺序还原成输出特征图即可

#### 多通道情况

假设输入特征图和卷积核都是 3 通道的，此时输出特征图仍然是单通道的，因为只有一个卷积核

这个时候对于输入特征图，它每个卷积核窗口中的 3 个通道会形成一个行向量：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311092851.png)


对于卷积核本身它这三个通道也会形成一个列向量
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311093005.png)
![image.png|396](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311093152.png)

所以他们形成的还是这样的结构，只不过是在横向或者竖向上增加了几个通道的数据：


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311093525.png)

#### 多通道多卷积核情况

如果是多个卷积核，实际上本质是每个卷积核都与输入特征图进行相同的运算结构得到一个通道的输出特征图

所以多个卷积核就是在上图基础上多加几个竖条即可：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311094221.png)

上图蓝色的卷积核与左侧的输入特征图运算得到右侧的蓝色竖条，表示一个通道的输出特征图

#### 多输入多通道多卷积核情况

对于多个输入的情况，只需要将每个输入特征图拆成矩阵然后往下叠加成一个大的输入矩阵即可：

![image.png|650](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311094857.png)

只需要最后将获得的结果的大矩阵按照 OW * OH 为单位还原成一个个输入所对应的 3 通道输出特征图即可

#### img 2 col 函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311095100.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311095201.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311095212.png)

这里的输入是单个数据，3 通道，宽和高都是 7 的特征图

卷积核是 3 通道，宽和高都是 5 的形状

所以它的结果就是将这个输入特征图变成了上文所述的一个 9 * (3 * 5 * 5) 的矩阵

同样，如果是 10 个输入，则将每个输入都拆成9 * (3 * 5 * 5) 的矩阵然后向下叠加成一个大的矩阵：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311095612.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311095617.png)

### img 2 col 的反向传播

其实这个算法的本质是先对输入特征图，和卷积核进行一次形状变换，然后进行矩阵乘法得到了输出矩阵

所以只要得到了对卷积核矩阵的导数之后，然后再变换回去就是反向传播了，本质还是矩阵乘法的反向传播



