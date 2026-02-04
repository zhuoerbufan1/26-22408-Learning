
### 反向传播机制

简单来说，就是计算最终输出变量对中间各个变量的导数值

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260115192351.png)

这里 a = A (x)，b = B (a)，y = C (b)，x 经过一系列函数复合得到 y，A, B, C 都是中间的函数，比如对于 a -> B -> b，这里的含义是 b 作为函数关系 B 的因变量，a 作为自变量，即 b = B (a)

反向传播就是在 x = x 0 的情况下，计算输出变量 y 对于中间变量 b, a, x 的导数值，即计算：

$$
\frac{dy}{db}|_{x = x_{0}}; \frac{dy}{da}|_{x = x_{0}}; \frac{dy}{dx}|_{x = x_{0}}
$$

根据复合函数链式法则得到：

$$
\frac{dy}{dx} = \frac{dy}{dy} \frac{dy}{db} \frac{db}{da} \frac{da}{dx}
$$

这里将 dy/db 记作 C' (b)，db/da 记作 B' (a)，da/dx 记作 A' (x)

可以得到：

$$
\frac{dy}{dx} = \frac{dy}{dy} C'(b) B'(a) A'(x)
$$

同时可以得到 $\frac{dy}{dy}* C'(b) = \frac{dy}{db}$，按照这个顺序可以得到下面的图：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260115192927.png)

这里 dy/dy 分别乘中间各个函数的导数，**就得到了因变量 y 对中间各个变量的微分，也就是变量 y 对各个变量的导数，从输出变量从右向左传播，即反向传播**


这样反向传播的意义在于，大多数机器学习我们需要找到输出变量对中间各个变量的导数，这样的话只用从右到左传播一次就计算出输出变量 y 对中间各个变量的导数了

这里举一个例子说明反向传播计算输出变量对中间各个变量的导数的过程

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260201114627.png)

这里计算 y 对 b 或者 y 对 a 的导数，一定是在一个确定的 x 值 x 0 的情况下，计算 dy/db，也就是计算：

$$
\frac{dy}{db}|_{b=b_{0}}或者说 \frac{dy}{db}|_{x = x_{0}}
$$

想要计算 $\frac{dy}{db}|_{b=b_{0}}$，按照公式，其实就是 $C'(b)|_{b = b_{0}}或者就是C'(b_{0})$，也就是 $2 * b_{0}$，即 $2 * b_{0} * 1$ 就是输出变量 y 对中间变量 b 在 b = b 0 情况下的导数

而 b 0 是输入变量 x 0 经过 A，B 两个函数作用后正项传播得到的（正项传播就是普通的函数符合运算过程）

所以想要计算反向传播（输出变量 y 对各个中间变量在 x = x 0 处的导数），那就得先进行一次正项传播将各个中间变量在 x = x 0 处的值求出来才行

同理想要计算 $\frac{dy}{da}|_{a = a_{0}}或者 \frac{dy}{da}|_{x = x_{0}}$

得先计算出来 $C'(b_{0})$，即 $\frac{dy}{db}|_{b = b_{0}}$，然后根据下面得到结果：

$$
\frac{db}{da}|_{a = a_{0}} * \frac{dy}{db}|_{b = b_{0}} = \frac{dy}{da}|_{a = a_{0}}
$$
所以得知道 $\frac{db}{da}|_{a = a_{0}}$，也就是 $B'(a_{0})$，所以得知道 a 0，这个同样可以经过一次正项传播得到
对于 $\frac{dy}{db}|_{b = b_{0}}$，它在反向传播的过程中已经计算出来了，所以同样知道了结果

因此对于反向传播计算输出变量对于各个中间变量在输入变量 x = x 0 情况下导数的计算过程就是：
（1）经过一次正项传播，计算各个中间变量在 x = x 0 导数下的值
（2）再经过一次反向传播，从而将输出变量对各个中间变量的导数值计算出来

### 建立连接

也就是在正向传播的过程中，让函数记住它的输入变量和输出变量；同时让输出变量记住生成它的函数

这样做的好处在于：
（1）进行一次正向传播，得到了输出变量 y
（2）通过输出变量可以获得上一步生成输出变量 y 的函数 C
（3）通过输出函数 C 可以获得 C 的输入变量 b
（4）直接进行计算输出变量 y 对 b 的导数：`b.grad = C.backward(y.grad)`

得到了 b.grad 之后，可以通过 b 获得 b 的生成函数 B，于是通过 B 得到 B 的输入变量 a，从而计算 `a.grad = B.backward(b.grad)`

就这样利用递归一步步得到输出变量 y 对各个中间变量的导数了

### $x_{1}^{2} + x_{2}^{2} = y$ 的反向传播实现

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260204205045.png)


```python
x1 = Variable(np.array(2.0))
x2 = Variable(np.array(3.0))
a1 = square(x1)
a2 = square(x2)
y = add(a1, a2)
y.backward()
print(y.data)
print(x1.grad)
print(x2.grad)
```

首先是正向传播：

```
a1 = square(x1)
a2 = square(x2)
y = add(a1, a2)
```

这里的 `square()` 函数实际是一个封装：

```python
def square(x):  
    f = Square()  
    return f(x)
```

他创建了一个函数类 `Square() `，这个函数类接受输入 x，返回输出变量 f (x)，在这个过程中中间输出变量 a 1 记住了生成函数 f（第一个 Square() 类函数），同时这个 Square () 类函数记住了输入变量 x 1 和输出变量 a 1

同理中间输出变量 a 2 记住了生成函数 f (第二个 Square () 类函数)，同时这个 Square () 类函数记住了输入变量 x 2 和中间输出变量 a 2

然后调用代码 `y = add(a1, a2)`

add () 仍然是自定义的函数，类似上文的 `Square()` 类函数，在其中创建了一个 Add () 的函数类

在计算得到输出变量 y 的过程，让生成函数 Add () 记住了输入变量列表 `[a 1, a 2]`

同时记住了输出变量 y

当然，输出变量 y 也记住了自己的生成函数 Add ()

以上就是正向传播的过程

然后调用输出变量 y 中的成员函数 backward ()，在这个函数中迭代计算 y 分别对中间变量 a 1, a 2, 以及输入变量 x 1, x 2 的导数

```python
class Variable:  
    def __init__(self, data):  
        if data is not None:  
            if not isinstance(data, np.ndarray):  
                raise TypeError('{} is not supported'.format(type(data)))  
  
        self.data = data  
        self.grad = None  
        self.creator = None  
  
    def set_creator(self, func):  
        self.creator = func  
  
    def backward(self):  
    # 这里调用y的backward方法, y自己一开始的导数是NULL
    # 那么这里就让y.grad一开始是1，作为反向传播计算的起始值
        if self.grad is None:  
            self.grad = np.ones_like(self.data)  
  		# 获得y的生成函数Add()
        funcs = [self.creator]  
        while funcs:  
            f = funcs.pop()  
            # 对于一个函数f来说，获取它的所有中间输出变量的grad  
            # 也就是获取输出变量b1, b2的导数，即输出变量y对b1和b2的导数  
            # 将输出变量y对函数f的所有中间输出变量b1, b2的导数打包成一个列表gys  
            gys = [output.grad for output in f.outputs]  
  
            # 将这个列表解包，传递给f的backward()函数。来计算所有输入变量a1, a2的导数  
            gxs = f.backward(*gys)  
            if not isinstance(gxs, tuple):  
                gxs = (gxs,)  
  
            # 将所有的输入变量的grad设置成上面计算得到的导数  
            for x, gx in zip(f.inputs, gxs):  
                x.grad = gx  
  
                if x.creator is not None:  
                    funcs.append(x.creator)
```

（1）y.grad 初始化为 1，表示输出变量 y 对自己的导数
（2）获得 y 的生成函数，这里是 Add （）
（3）获得 Add () 的所有输出变量，获得输出变量 y 对他们的导数，这里仅仅是一个 1
（4）调用 Add () 类的 backward () 方法，传入（3）中获得的 y 对中间输出变量的导数，计算 y 对 Add () 函数的所有输入变量的导数，这里计算得到了 y 对输入变量 a 1 和 a 2 的导数
（5）将 a 1 和 a 2 的生成函数分别假如到生成函数队列中，重复上面的步骤
（6）首先是 a 1 的生成函数第一个Square () 函数
（7）获得 Square () 类的所有输出变量，或者 y 对他们的导数，这里就是 y 对 a 1 的导数
（8）调用 Square () 类的 backward () 方法，传入 (7) 中获得的 y 对 a 1 的导数，计算 y 对 Square () 类输入变量 x 1 的导数，这里计算得到了 y 对 x 1 的导数
（9）x 1 没有生成函数，于是接下来处理生成函数队列中的下一个 Square () 类函数
（10）同理计算出来了 y 对 x 2 的导数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260204211014.png)

这样反向传播过程就结束了