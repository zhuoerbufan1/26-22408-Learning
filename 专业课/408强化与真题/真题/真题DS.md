# 绪论

### 计算复杂的时间复杂度的方法
#### 0 母题

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251031180419.png)

假设外层执行了 k 次

当 i = 1 的时候执行了 1 次
i = 2 的时候执行了 2 次

i = 4 的时候执行了 4 次

i = 2^k 的时候执行了 2^k 次

加起来就是：

$$
\frac{1-2^k}{1-2}次
$$

k = log 2 n

所以总的复杂度是 O (n) 级别

#### 1

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250609164351.png)

i = 1 的时候内层执行 O (n) 次
i = 3 的时候内层执行 O (n) 次
...

外层一共要执行 n/2 次，每次内层都是 O (n) 级别，所以总的就是 O (n^2)

#### 2


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250526202941.png)

当 i = 1 的时候，内层两层循环执行 1 次

当 i = 2 的时候内层两层循环执行 2 次

当 i = m 的时候

内层的代码实际上是：

```cpp
for(int j = 1; j <= m; j *= 2)
	for(int k = 1; k <= j; k ++)
		count ++;
```

内层循环实际执行了 O (m) 次

所以 i = 1 内层执行了 1 次
i = m，内层执行了 k m 次（大概）

所以 i 从 1 到 n，将所有的求和起来，总的应该是 n^2 级别的复杂度

# 线性表

