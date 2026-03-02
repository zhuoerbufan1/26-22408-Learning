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

##### **全 0**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302194506.png)

##### **全 1**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302195244.png)

##### **未初始化**

这里只是指定了形状，没有指定具体的值，这个函数每次执行都会生成一个指定形状的不同的各种矩阵

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302195359.png)

##### **full 初始化**


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302201124.png)


##### **指定矩阵形状初始化**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302201446.png)

下面的代码创建的就是指定与 arr（三行四列）相同形状的矩阵
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302201501.png)

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
