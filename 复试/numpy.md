### numpy 中的数据维度

#### ndim 属性和 shape 属性

`ndim` 这个属性表示了 ` ndarry ` 中数据的维度，`shape` 属性表示了 ndarry 的形状，`ndim` 实际上等价于 `len(ndarry.shape)`

```python
import numpy as np


# 0维数组（标量）
a = np.array(42)
print(a.ndim)        # 输出: 0
print(a.shape)       # 输出: ()
  
# 1维数组（向量）
b = np.array([1, 2, 3])
print(b.ndim)        # 输出: 1
print(b.shape)       # 输出: (3,)


# 2维数组（矩阵）
c = np.array([[1, 2], [3, 4]])
print(c.ndim)        # 输出: 2
print(c.shape)       # 输出: (2, 2)


# 3维数组（例如：batch × height × width）
d = np.random.rand(2, 3, 4)
print(d.ndim)        # 输出: 3
print(d.shape)       # 输出: (2, 3, 4)
```

#### 其他属性

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302192309.png)
转置：arr. T，转置只对二维有效，对一维或者 0 维是无效的

### ndarry 数据的创建

#### 基础创建

```python
# 手动指定一个数组或者一个python的列表
list = [2, 3, 4, 5]
arr1 = np.array([1, 2, 3, 4])
arr2 = np.array(list, np.float64);
print(arr1.ndim) # 结果是1
print(arr2) # 结果是[2., 3., 4., 5.]
```

#### np.copy (arr)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302194243.png)

#### 预定义（全 0，全 1，未初始化，指定数初始化）

##### 全 0

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302194506.png)

##### 全 1

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302195244.png)

##### 未初始化

这里只是指定了形状，没有指定具体的值，这个函数每次执行都会生成一个指定形状的不同的各种矩阵

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302195359.png)

##### full 初始化


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302201124.png)


##### 指定与某个矩阵矩阵形状初始化

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302201446.png)

下面的代码创建的就是指定与 arr（三行四列）相同形状的矩阵
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302201501.png)


##### `np.arange` 生成等差数列

这里是左闭右开范围内的数据，即 `[1, 10)` 中，从 1 开始步长为 1 的数据：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303200847.png)

或者从 2 开始步长为 2 的数据，仍然保证范围是在 `[2, 10)` 中：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303200930.png)

##### `np.linspace()` 生成等差数列

它生成的就是 `[0, 10]` 中的三个数，这三个数之间的间隔相同

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303201247.png)

##### 创建单位矩阵

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303202329.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303202336.png)

##### 创建对角矩阵

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303202412.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303202430.png)

##### 创建均匀分布的矩阵

这里的均匀分部是指，它生成的 2 * 3 的矩阵，这个矩阵的每个元素都是一个服从 U~(0, 1)均匀分布的随机变量的一个具体取值，矩阵的各个元素所对应的随机变量之间互相独立

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303203031.png)


##### 创建标准正态分布随机数

同上均匀分布的解释

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303203203.png)

##### 创建整数随机数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303203322.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303203330.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303203348.png)

### numpy 访问数组元素

这里创建一个二维数组
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304192307.png)

#### 基本访问方式

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304192330.png)

#### 行和列的切片

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304192804.png)

#### 连续切片

这里分析的时候按照取交集的思想，首先是取哪些行，然后是哪些列，然后取这些交集的元素
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304193001.png)

#### slice 函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304193249.png)

#### bool 索引

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304193322.png)

### numpy 中的运算

#### 数组之间的加减乘除

numpy 中的加减乘除都是逐元素进行的，不论这里是一维的数组还是二维的数组

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309153252.png)

#### 数组与标量之间的运算

这里运算也是逐元素进行的

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309153620.png)

#### 矩阵运算


### numpy 中的轴

一个二维数组的轴方向如下所示：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301174114.png)

按照轴的方向进行求和如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301174129.png)


### `sum` 求和函数

**基本用法**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304195122.png)

**axis 参数**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304195150.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304195705.png)

**keepdims 参数**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304195740.png)

### 矩阵操作

#### flatten 函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309120523.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309120531.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309120550.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309120612.png)

### numpy 中的广播机制

也就是自动扩展较小的数组，让其可以与不同形状的数组进行兼容，从而可以逐元素进行计算

#### 广播可行原理

（1）将矩阵的形状进行右对齐，对于维度较少的，则形状左边补 1
（2）从右到左进行比较，只有每一位对应相等或者某个为 1，这样才能进行广播

##### 一维数组与标量

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302205351.png)

```python
import numpy as np 
a = np.array([1, 2, 3]) # (3,) 
b = 10 # () 
print(a + b) # [11, 12, 13]
```

##### 二维数组与一维数组

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302205504.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302205553.png)

##### 两个二维数组进行广播

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302205627.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302205638.png)

##### 两个二维数组无法进行广播

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302205731.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302205807.png)

##### 三维数组与二维数组进行广播

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302205902.png)
