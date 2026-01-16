
### 反向传播机制

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260115192351.png)

这里 a = A (x)，b = B (a)，y = C (b)，x 经过一系列函数复合得到 y

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

这里 dy/dy 分别乘各个函数的导数，就得到了 y 对各个变量的微分，也就是变量 y 对各个变量的导数，从右向左传播，即反向传播

当然，这里的传播顺序（即复合函数求导乘法顺序）是从 dy/dy 到 dy/dx

其实也可以从 dx/dx 到 dy/dx：

$$
\frac{dy}{dx} =  \frac{dy}{db} \frac{db}{da} \frac{da}{dx} \frac{dx}{dx}\left( 这里从 \frac{dx}{dx} 开始依次乘各个函数导数从右向左计算\right)
$$
这里从 dx/dx 出发依次得到的是 da/dx, db/dx, dy/dx，这实际上也是一种传播方式，但是一般机器学习都是反向传播，从输出 y 到 x

