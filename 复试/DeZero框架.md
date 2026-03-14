## 利用反向传播来求导
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

### 一个稍稍复杂的多层函数的反向传播计算过程

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260205165448.png)

在这个图中，需要计算的是输出变量 y 对变量 b, c, a, x 的导数，也就是反向传播过程要做的事情

这里的 D 函数不一定是 b 和 c 两个变量的加法，其实可以看成 y = D (b, c)

显然，在正向传播的过程中，函数 D 记住了两个输入变量 b 0, c 0

反向传播的时候设置 y.grad = 1，那么输出变量 y 对变量 b 和 c 在 b 0, 和 c 0 的情况下的导数（也可以说是在输入 x 0 情况下的导数）就是：

$$
\frac{\partial D(b, c)}{\partial b}|_{b_{0},c_{0}} * 1以及\frac{\partial D(b, c)}{\partial c}|_{b_{0},c_{0}} * 1
$$

这里利用 b 0 和 c 0 来计算 $\frac{\partial D(b, c)}{\partial b}|_{b_{0},c_{0}} * 1以及\frac{\partial D(b, c)}{\partial c}|_{b_{0},c_{0}} * 1$ 就放在函数 D 的 backward () 的方法中，他返回的就是对输入变量的导数，b.grad 和 c.grad

接着要计算的就是输出变量 y 对变量 a 的导数

这里可以看成这样的复合关系：

$$
y = D(b, c); b = B(a); c = C(a)
$$
链式法则如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260205170152.png)

所以 y 对 a 的导数就是：

$$
\frac{dy}{da} = \frac{\partial y}{\partial b} \frac{db}{da} + \frac{\partial y}{\partial c} \frac{dc}{da}
$$

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260205165448.png)

因此在这个计算图中，得到了 b.grad 之后继续向前（左）反向传播得到的是 $\frac{\partial y}{\partial b} \frac{db}{da}$，同理得到了 c.grad 之后继续向前反向传播得到的是 $\frac{\partial y}{\partial c} \frac{dc}{da}$

所以我们需要先计算函数 B.backward 方法，然后计算 C.backward 方法，这两个方法作用于同一个输入变量 a 上，所以 a.grad 应该将 B 和 C 的 backward 方法结果加起来，也就是代码中的：

```python
for x, gx in zip(f.inputs, gxs):  
  # 首先计算一次B函数的backward，此时就得到了一个a.grad
  # 下一次计算C函数的backward的时候，此时是相同的输入变量，a.grad不为空，此时应该加上函数C的backward结果，也就是上文中的链式法则异路相加的过程
    if x.grad is None:  
        x.grad = gx   
    else:  
        x.grad = x.grad + gx  
  
    if x.creator is not None:  
        funcs.append(x.creator)
```

### 反向传播函数计算的优先级

在迭代从后往前反向传播的时候是利用一个栈结构进行的

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220094900.png)

这里初始化设置 y.grad = 1，然后利用函数 D，调用 D.backward () 计算输入遍历 b, c 的导数（y 对他们的导数），计算完成之后，将 b 和 c 的生成函数 B，C 放入 funcs 这个栈中

然后取出函数 C，根据 c.grad 来计算 y 从 c 路径对 a 的偏导，下一步应该是计算 y 从 b 路径对 a 的偏导，并将两者加起来得到 y 对 a 的偏导；但是按照代码的逻辑，下一步取出的函数是 A，而不是 B，下面是实现这个过程的代码（Variable 类中的 backward() 成员函数）：

```python
def backward(self):  
    if self.grad is None:  
        self.grad = np.ones_like(self.data)  
  
    funcs = [self.creator]  
    while funcs:  
    # 这里存在缺陷，当y从变量c路径到达变量a之后下一步取出的是函数A
    # 而不是函数B
        f = funcs.pop()  
        gys = [output.grad for output in f.outputs]  
        gxs = f.backward(*gys)  
        if not isinstance(gxs, tuple):  
            gxs = (gxs,)  
  
        for x, gx in zip(f.inputs, gxs):  
  
            if x.grad is None:  
                x.grad = gx  
            else:  
                x.grad = x.grad + gx  
  
            if x.creator is not None:  
                funcs.append(x.creator)
```

为了解决这个问题，引入了辈分机制，给每个 Func 类和 Variable 类加入了一个 generation 属性，并保证：

（1）变量的辈分是其生成函数辈分 + 1，这一步在变量设置自己的生成函数的时候执行
（2）生成函数辈分与输入变量辈分的最大值保持相同，这一步在正向传播调用函数的时候执行

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220100116.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220100123.png)


修改之后的 Variable 类中的 backward () 函数代码：

```python

def backward(self):  
    if self.grad is None:  
        self.grad = np.ones_like(self.data)  
  
    funcs = []  
    seen_set = set()  
    # 定义在一个函数中的函数  
    # add_func()函数在此处定义之后，只能被父函数调用(backward())  
    # 只能访问父函数中的参数  
    def add_func(f):  
        # 这里的seen_set是为了去重，防止一个函数被多次加入到funcs中  
        if f not in seen_set:  
            funcs.append(f)  
            seen_set.add(f)  
            # 让funcs进行排序，让辈分更高的更先取出计算输入变量的导数  
            funcs.sort(key=lambda x: x.generation)  
  
    add_func(self.creator)  
  
    while funcs:  
        f = funcs.pop()  
        gys = [output.grad for output in f.outputs]  
        gxs = f.backward(*gys)  
        if not isinstance(gxs, tuple):  
            gxs = (gxs,)  
  
        for x, gx in zip(f.inputs, gxs):  
            if x.grad is None:  
                x.grad = gx  
            else:  
                x.grad = x.grad + gx  
  
            if x.creator is not None:  
                add_func(x.creator)
```

在正向传播中得到的辈分属性图如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220100813.png)

（1）设置 y.grad = 1 并调用 y 的 backward 函数进行反向传播计算
（2）先加入函数 D 到 funcs 中
（3）分别计算 b，c 的导数，并将函数 B，C 加入到 funcs 中
（4）取出函数 C，y->c->a 路径上 y 对 a 的偏导，并将函数 A 加入到 funcs 中
（5）加入的时候调用的是 add_func 函数，加入函数 A 到 funcs 之后会根据辈分属性排序，下一次取出的是函数 B 而不是函数 A 了
（6）取出函数 B，计算 y->b->a 路径上 y 对 a 的偏导，与 y->c->a 路径上 y 对 a 的偏导相加，得到 y 对 a 的导数，此时仍然会调用 add_func 函数，但是函数 A 已经加入到 funcs 中了，因此利用 seen_set 会进行去重，funcs 中仍然只有函数 A
（7）取出函数 A，调用 A.backward 函数，得到 y 对 x 的导数，反向传播结束
### Python 中的内存回收机制与弱引用（？）

（1）Python 中的一切皆对象
（2）对象会有一个引用计数属性，当对象参与赋值，函数传参，加入列表等操作的时候引用计数 + 1，反之，如果被赋值的对象被清空等等，引用计数 -1
（3）当引用计数 = 0 的时候 Python 会从内存中回收对象
（4）如果出现循环引用，则被赋值对象清空之后引用计数也不会变为 0，这会导致对象一直在内存中，除非调用 GC 机制进行回收

```python
a = obj();
b = obj();
c = obj();
a.b = b;
b.c = c;
# 上面是正常引用，此时a对应的对象的引用计数是1，b和c对应的obj()	对象的引用计数是2

a = b = c = None;# 被赋值对象清空之后
# 1. 三个obj()对象的引用计数先都-1，变成0 1 1
# 2. a对应的obj()对象引用计数变为0，被回收，这导致引用b的对象的引用计数-1，然后导致c对应的obj()对象引用计数-1，导致3者都变为0，被回收


```

下面是循环引用机制：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260205195147.png)

这种情况哪怕清空了被赋值对象，最后三个对象的引用计数结果还是不为 0（不用纠结为什么），这导致用户无法使用这三个对象了，但是他们还是存在内存，造成内存浪费

在 Dezero 框架中，也出现了循环引用，这里生成函数被自己的输出变量引用，输出变量同时被自己的生成函数引用：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260205195327.png)

这个时候必须采取方法来处理，这里采用弱引用机制，让 Function 记住自己中间输出变量的时候使用弱引用，不增加中间输出变量的引用计数，这样当输出变量对象的被赋值对象清空的时候引用计数就会减为 0，让 Python 回收用户无法操作的对象，节省内存


如果是弱引用，则需要加上括号来访问对象：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260205200211.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260205200224.png)

在 Function 类中使用下面语句添加弱引用：

```python
class Function:  
    def __call__(self, *inputs):  
        xs = [x.data for x in inputs]  
        ys = self.forward(*xs)  
        if not isinstance(ys, tuple):  
            ys = (ys,)  
        outputs = [Variable(as_array(y)) for y in ys]  
  
        self.generation = max([x.generation for x in inputs])  
        for output in outputs:  
            output.set_creator(self)  
        self.inputs = inputs  
        # 这里让生成函数记住中间输出变量的时候采用弱引用机制来避免生成函数和中间输出变量之间的循环引用  
        # 这里的outputs全是对中间输出变量的弱引用  
        self.outputs = [weakref.ref(output) for output in outputs]  
        return outputs if len(outputs) > 1 else outputs[0]
```

对于弱引用的对象，访问的时候必须加上括号，所以在 Variable 类中的 backward 方法中进行修改：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220102927.png)

这样就解决了循环引用的现象，在下面的循环中，如果仍然是循环引用，那么哪怕下次循环的 x 和 y 变量被覆盖，上次循环中的中间对象也不会被回收。

改成弱引用之后，被覆盖之后会回收（距离原理先不考虑）

### 对函数的封装

```python

# 下面两种写法是等价的：
# 没有封装的情况（繁琐）
def add_verbose(x0, x1):
    adder = Add()        # 1. 创建实例
    result = adder(x0, x1)  # 2. 调用实例
    return result

# 封装后的简洁版本
def add(x0, x1):
    return Add()(x0, x1)  # 一行搞定
```
### 反向传播的状态切换与 with 切换机制

有时候框架本身并不需要进行反向传播，仅仅需要进行正向传播即可，那么在正向传播过程中让函数记住自己的输入变量和输出变量，以及让输出变量记住自己的生成函数就是在浪费内存

因此需要一个机制，让仅仅需要进行正向传播的时候不进行上述连接，从而节省大量内存

这里定义了一个 Config 类，这个类中只有一个属性 enable_backprop，它为 true 的时候表示启用反向传播，为 false 的时候表示禁用反向传播（不进行连接）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220114454.png)

```python
class Function:  
    def __call__(self, *inputs):  
        xs = [x.data for x in inputs]  
        ys = self.forward(*xs)  
        if not isinstance(ys, tuple):  
            ys = (ys,)  
        outputs = [Variable(as_array(y)) for y in ys]  
  
  # 只有设置为true的时候才会在正向传播的过程中进行连接，从而节省内存
        if Config.enable_backprop:  
            self.generation = max([x.generation for x in inputs])  
            for output in outputs:  
                output.set_creator(self)  
            self.inputs = inputs  
            self.outputs = [weakref.ref(output) for output in outputs]  
  
        return outputs if len(outputs) > 1 else outputs[0]  
  
    def forward(self, xs):  
        raise NotImplementedError()  
  
    def backward(self, gys):  
        raise NotImplementedError()
```

为了更方便的启用和禁用反向传播机制，这里采用了 with 语句：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220114736.png)

with 语句包括两个部分，一个部分是 with 作用域：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220114941.png)

一个是 contextlib 模块中设置 config_test () 的实现：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220115029.png)

总的来说就是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220115043.png)

当执行语句 `with config_test():` 进入 with 作用域之后，它首先是执行预处理，即 print(start)，然后执行 try，实际上就是将 with 作用域中的 print (process) 放到了 yield 中，最后执行后处理，上图代码执行结果就是：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220115215.png)

按照这个原理，我们让 with 作用域中执行的是禁用反向传播的模式，退出 with 之后又回到默认的启用反向传播的模式：

```python
@contextlib.contextmanager  
def using_config(name, value):  
    old_value = getattr(Config, name)  
    setattr(Config, name, value)  
    try:  
        yield  
    finally:  
        setattr(Config, name, old_value)

with using_config('enable_backprop', False):  
    x = Variable(np.array(2.0))  
    y = square(x)
```

上述执行 with 语句的过程就是将 ` x = Variable(np.array(2.0))  y = square (x) ` 放到了 try 中，然后从 `old_value = getattr(Config, name) ` 开始往下执行
（1）先获得旧值，就是 true
（2）然后设置新值，false
（3）然后进行正向传播，这个是没有连接的正向传播
（4）最后再将属性设置为原来的旧值，就是 true

可能有多次需要使用不进行连接的正向传播过程，如果每次都写 `using_config('enable_backprop', False)` 有点长了，因此将这个函数封装到 `no_grad` 函数中：

```python

def no_grad():  
    return using_config('enable_backprop', False)

with no_grad():  
    x = Variable(np.array(2.0))  
    y = square(x)
```

这样只要是不进行求导的过程，只用使用下面代码就可以了：

```
with no_grad():  
    x = Variable(np.array(2.0))  
    y = square(x)
```


### 运算符重载 - 使用数学符号操作 Variable 类

主要利用的是魔法方法 `__mul__` 和 `__add__`

首先得先有实现 `Variable` 类的乘法和加法方法，比如乘法方法：

```python
class Mul(Function):  
    def forward(self, x0, x1):  
        y = x0 * x1  
        return y  
  
    def backward(self, gy):  
        x0, x1 = self.inputs[0].data, self.inputs[1].data  
        return gy * x1, gy * x0
        
# 对这个方法进行封装
def mul(x0, x1):  
    return Mul()(x0, x1)

```

然后在 `Variable` 类中实现 `__mul__` 方法：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220154041.png)

这样达到的效果是，可以直接在代码中使用乘法符号，而不用再显示调用 `mul(a,b)` 了

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220154106.png)

另外一个简洁的写法是直接令 `Variable.__mul__ = mul`，这里 mul 是上文用于封装的函数，它也是对象，直接将其赋予 `Variable` 类的 `__mul__` 属性即可

实际上这里执行 `*` 操作的原理是：
根据 `y = a * b`
（1）首先调用 a 的 `__mul__` 方法
（2）a 中没有实现 `__mul__`，然后调用 b 的 ` __rmul__ ` 方法
（3）如果这两个方法都不行没有实现，则出错
### 运算符重载 

#### 实现 `Variable * ndarry`，右乘一个 ndarry 类

这里主要是想要达到 `y = a * ndarry` 的效果，这里的 a 是 `Variable` 类型，让其可以直接与一个 ndarry 类型进行运算；基本思想是将 ndarry 转换成 `Variable` 类型，然后让两者运算即可

首先增加一个类型转换的函数：

```python
def as_variable(obj):  
    if isinstance(obj, Variable):  
        return obj  
    return Variable(obj)
```

然后在 Function 类中增加：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220155249.png)

上文的乘法，或者加法函数都是继承自 Function，因此这些函数调用的时候都会执行这个类中的这行代码，将所有不是 Variable 类的数据转换成 Variable 类

#### 实现 `Variable * int(float)`，右乘一个数值

首先，需要在封装 `add(x0, x1)` 函数的地方增加一个将数值转换成 ndarry 的代码：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220161419.png)

这样当执行下面的指令的时候：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220161503.png)

（1）首先执行 x 重载的 `__add__` 方法，这个方法会调用 `add(x0, x1)`
（2）在 `add(x0, x1)` 中将数值 `x1` 转换成了 ndarry 对象
（3）执行 `Add()` 这个类继承自 `Function类`，它的下述代码将所有的 inputs 转换成了 `Variable` 类变量
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220155249.png)
（4）这样后续执行的就都是 Variable 类的相加了

乘法也是类似的
#### 实现 `int * Variable`，左乘一个数值

上文实现的是右乘一个数值，这里是左乘一个数值，实际上只用加上 `Variable.rmul = add` 就行了

这里当执行 `y = 2.0 * x`，这里 x 是一个 `Variable` 类的时候

（1）首先调用的是左侧对象的 `__mul__` 方法，显然没有实现
（2）然后调用的是右侧对象的 `__rmul__` 方法，它由 `Variable.rmul = mul` 来实现

`__rmul__` 的默认参数传递如下：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220162429.png)

这里是乘法，但是以加法为例：
它调用封装的 `add(x0, x1)` 函数的时候实际上是调用 `add(x, 2.0)`，这样刚好让数值对象变成了 `add` 的第二个参数，而在 `add()` 函数实现中，有将第二个参数转换成 ndarry 的代码：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220161419.png)

这样传递给 Add 的时候就不是数值类型了，而是 ndarry 类型

（3）然后就是 `Function` 类的操作，先将所有 inputs 转换成 Variable 类，然后再进行后续运算

#### 实现 `ndarry * Variable`，左乘一个 ndarry 类型

这里最开始调用的是 ndarry 的 `__mul__` 运算，因为两个对象进行乘法的时候调用左边的 `__mul__` 的优先级更高，`ndarry` 也实现了自己的 `__mul__` 的操作，显然这样的话会出问题，因此我们想要一开始就调用 `Variable` 类型的 `__rmul__` 方法，而不是一开始就调用 `ndarry` 的 `__mul__` 方法

这里的措施是设置 `Variable` 的实例运算符优先级更高：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220163432.png)

这样就会调用右边 `Variable` 类的 `__rmul__` 方法，只用再加上一个 `Variable.__rmul__ = mul` 就行了

这样在执行 `mul` 方法的时候会自动将左边的 `ndarry` 类型转换成 `Variable` 类型，然后进行后续操作

### 实现运算符重载的一般步骤（添加函数的步骤）

以除法为例要实现 `y = x0/x1`

**（1）首先这个函数要继承 `Function` 类，实现自己的正向传播和反向传播**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220170434.png)

（2）进行运算符重载，让其满足左除或者右除数值或者 ndarry 类

（3）对于除法来说，首先是右除，如果右边是 ndarry 类，这里直接根据 `Function` 类中将 ndarry 类转换成 Variable 类即可；如果右边是数值类，则在打包函数中先将其转换成 ndarry 类，然后再根据 `Function` 类中将 ndarry 类转换成 Variable 类的逻辑进行处理：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220171114.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220171123.png)

（4）然后是左除，如果左除的是数值类，则进行除法运算的时候是调用右边 `Variable` 类的 `__rtruediv__` 方法，这里跟加法和乘法不同，不能仅仅添加一行 `Variable.__rtruediv__ = div` 完事，因为要考虑左右顺序；当调用右边的 `Variable` 类的 `__rtruediv__` 方法时，需要重新写一个打包函数 `rdiv(x0, x1)`

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220171606.png)

他的调用逻辑是：
（1）调用右边 `Variable` 类的 `__rtruediv__` 方法，self 变量，即 `Variable` 类变量转递给 x 0, 数值变量传递给 x 1
（2）在打包函数中，将数值 x 1 转换成 ndarry 类型的变量
（3）然后调用 Div () 的时候必须转换顺序，因为是数值变量除以 `Variable` 类变量，所以是 `Div()(x1, x0)`
（4）这里与加法和乘法是不同的，加法和乘法直接用原来的打包函数也不会有影响，但是这里的乘法和减法必须调换 x 0 和 x 1 的顺序

完整代码：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220171931.png)

### 模块化以及文件结构

我们创建一个 dezero 的包，并在其中创建一个 `core_simple` 的文件，将步骤 23 之前实现的类以及函数放到 `core_simple.py` 这个文件中

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220175149.png)

这样在别的文件就可以使用 `from dezero.core_simple import Variable` 来使用 `core_simple` 文件中的类了

这里的 `__init__.py` 文件是导入 `dezero` 的时候执行的第一个文件，我们将执行运算符重载的函数放在这个文件中，这样当导入包的时候就直接执行了运算符重载，这里运算符重载的定义实际上还是在原来的 `core_simple.py` 文件中：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220181602.png)

只不过执行这个函数是在 `__init__` 文件中

最后为了保证可以正确导入用户自定义的包，需要在调用 dezero 模块中添加下面的代码：

```python

if '__file__' in globals():  
    import os, sys  
    sys.path.append(os.path.join(os.path.dirname(__file__), '..'))
```

假如项目结构如下所示：

![](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260220181145.png)

这串代码在 example. py 文件中的作用就是将 my_project 加入到 python 的模块搜索路径中，从而保证可以正确导入 dezero 包
```python
# example.py 中的代码
if '__file__' in globals():
    import os, sys
    sys.path.append(os.path.join(os.path.dirname(__file__), '..'))

# 路径分析：
# __file__ = "/home/user/my_project/examples/example.py"
# os.path.dirname(__file__) = "/home/user/my_project/examples"
# os.path.join(..., '..') = "/home/user/my_project"
# 最终将 "/home/user/my_project" 添加到 sys.path

```

## 高阶导数
### 实现函数的泰勒展开

以 `sin x` 为例，它在 0 处的泰勒展开是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260221152209.png)

这是一个由普通的加减乘除，以及幂次方组成的函数，我们可以根据它的拉格朗日余项来控制精度：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260221152337.png)

这里的 `math.factorial()` 是用来计算阶乘的，t 就是每次循环的余项，当这个余项的精度小于 0.0001 的时候，返回 sin x 的泰勒展开的多项式结果 y

这个 y 仍然是一个 Variable 变量，它是由上文已经实现的加减乘除操作组成，计算图是已知的，因此可以直接使用 y.backward () 来计算 y 对 x 的导数了，并且这个导数实际上就是 y = sin x 对输入变量 x的导数

### 牛顿法进行迭代计算

牛顿法的本质实际上是用二次函数对函数进行近似，比如对于 f (x) 让其近似为 x = a 处的二阶泰勒展开：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260222150831.png)

对于这个二次函数来说，它的最小值是：
$$
x_{1} = a - \frac{f'(a)}{f''(a)}
$$

下一步，将 a 换成这个最小值 $a_{new} = a - \frac{f' (a)}{f'' (a)}$，让 f (x) 在 $a_{new}$ 处展开，得到一个新的二次函数，这个二次函数的最小值点是 $x_{2} = a_{new} - \frac{f'(a_{new})}{f''(a_{new})}$

这样就完成了 x 1 -> x 2 的转变

事实上，根据 $a_{new}$ 的关系，我们可以得到：

$$
x_{2} = x_{1} - \frac{f'(x_{1})}{f''(x_{1})}
$$
这样就得到了牛顿法更新最小值点的递推关系

所以牛顿法的思想是不停的更新二阶泰勒展开点 a，用每次二次函数的最小值点当作下一次的展开点 a，用展开点的二次函数的最小值点来不断迭代更新整个函数的最小值点 x

从最终结果来看，只要一开始指定一个 x 1，按照：

$$
x_{n+1} = x_{n} - \frac{f'(x_{n})}{f''(x_{n})}
$$
进行迭代更新即可

### 高阶导数的实现，以 $y = x^{2}$ 为例

#### 实现过程

首先 pow 函数的 forward 和 backward 方法如下：

```python
class Pow(Function):  
    def __init__(self, c):  
        self.c = c  
  
    def forward(self, x):  
        y = x ** self.c  
        return y  
  
    def backward(self, gy):  
        x, = self.inputs  
        c = self.c  
        gx = c * x ** (c - 1) * gy  
        return gx
```

它继承的Function 类如下：

```python
class Function:  
    def __call__(self, *inputs):  
        inputs = [as_variable(x) for x in inputs]  
  
        xs = [x.data for x in inputs]  
        ys = self.forward(*xs)  
        if not isinstance(ys, tuple):  
            ys = (ys,)  
        outputs = [Variable(as_array(y)) for y in ys]  
  
        if Config.enable_backprop:  
            self.generation = max([x.generation for x in inputs])  
            for output in outputs:  
                output.set_creator(self)  
            self.inputs = inputs  
            self.outputs = [weakref.ref(output) for output in outputs]  
  
        return outputs if len(outputs) > 1 else outputs[0]
```

一阶导的执行过程：

```python
x = Variable(np.array(2.0))
y = x ** 2 
y.backward(create_graph=True)
```

在这个过程中 y 和 x 都是 Variable 类型的变量，并且 y 的生成函数是 `pow`，形成的计算图是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260222191914.png)

接着调用 `y.backward()`，这里的 `create_graph = True` 的含义是在反向传播的过程中仍然让变量与函数之间建立链接，具体的过程是：

```python
def backward(self, retain_grad=False, create_graph=False):  
    if self.grad is None:  
        xp = dezero.cuda.get_array_module(self.data)  
        # 创建一个与self.data类型相同的全1的数据，并封装成Variable类型  
        self.grad = Variable(xp.ones_like(self.data))  
  
    funcs = []  
    seen_set = set()  
  
    def add_func(f):  
        if f not in seen_set:  
            funcs.append(f)  
            seen_set.add(f)  
            funcs.sort(key=lambda x: x.generation)  
  
    add_func(self.creator)  
    while funcs:  
        f = funcs.pop()  
        gys = [output().grad for output in f.outputs]  # output is weakref  
  
        with using_config('enable_backprop', create_graph):  
            gxs = f.backward(*gys)  
            if not isinstance(gxs, tuple):  
                gxs = (gxs,)  
  
            for x, gx in zip(f.inputs, gxs):  
                if x.grad is None:  
                    x.grad = gx  
                else:  
                    x.grad = x.grad + gx  
  
                if x.creator is not None:  
                    add_func(x.creator)  
  
        if not retain_grad:  
            for y in f.outputs:  
                y().grad = None  # y is weakref
```

（1）调用 Variable 类型变量 y 的 backward 函数，首先，创建一个 y.grad，并令其初值是 1，它同样是一个 Variable 类型的变量

（2）获得变量 y 的生成函数 pow，利用 pow 的输出变量的导数（这里是 y.grad）来计算 pow 的输入变量 x 的导数，具体来说就是将 y.grad 传入 pow 函数的 backward () 方法，来计算 y 对 pow 函数的输入变量的导数，pow 函数的 backward 方法如下：

```python
    def backward(self, gy):  
        x, = self.inputs  
        c = self.c  
        gx = c * x ** (c - 1) * gy  
        return gx
```

在这个 backward过程中 gy，以及 self. inputs 实际上都是已经存在的 Variable 类型的变量，而对这些变量执行 `gx = c * x ** (c - 1) * gy  ` 操作实际上是跟正向传播是类似的，根据上文对乘法运算的重载，他会在原来正向传播的基础上建立反向传播的计算图：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260224125503.png)


之前仅仅是数的运算，数的运算并不会使用重载运算符的函数，因此不会在 backward 的过程中建立连接，但是这里将每个函数的 backward 方法都改成了 Variable 变量的计算，这样使用的就是上文重载过的运算了，所以同样会建立一个从 y.grad 到 x.grad 的计算图

这样在第一次的 y.backward () 之后，就得到了 x.grad 以及上面的计算图（连接过程）

（3）x.grad 是一个 Variable 类型的变量，实际上 `x.grad` 就是 $\frac{dy}{dx}$，想要求 $\frac{d^{2}y}{dx^{2}}$，也就是求变量 $\frac{dy}{dx}$ 对 x 的导数，它是 y 对 x 的一阶导，我们同样调用它的 x.grad. backward () 方法，这样得到的 x.grad 就是 y 对 x 的二阶导结果了，**另外需要注意的是, 这里求二阶导的时候与一阶导 x.grad 本身是无关的, 它所利用的只是在求一阶导的过程中建立的二阶导函数的连接关系**

这里再以 $y = x^{3}$ 举例来说明它的本质，这个函数的一阶导是 $y = 3 x^{2}$，当 x = 1 的时候

(1) 进行一次正向传播, 建立了计算图：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260222195117.png)

（2）利用这个计算图，调用 y.backward ()，他会设置 y.grad = 1，当然这也是一个 Variable 类型变量；在反向传播的过程中，它使用了 $y = x^{3}$ 它的一阶导的表达式 $y = 3x^{2}$，即，利用 $3 * x * *2 * y.grad = x.grad$ （这个在 pow() 的 backward 方法中有实现）计算出来了 x.grad，这个过程很关键，由于 y.grad= 1，所以它等价于 `3 * x ** 2 = x.grad`，它的形式刚好就是 $y = x^{3}$ 的一阶导函数的形式，并且输入是 x，输出是 x.grad，这行代码的执行过程在正向传播的基础上，建立了这个导函数的计算图：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260224123503.png)


x.grad 本身的数值是多少没有意义，有意义的是它建立了一个 $y= x^{3}$ 的导函数 $y = 3x^{2}$ 的一个正向传播计算图（这里的 y.grad = 1 恰好没有影响），并且 x.grad 是输出变量，输入变量还是原本的 x  ！

**求一阶导的反向传播过程恰好就是二阶导函数的正向传播过程！并且输入变量，输出变量恰好就是 x 和 x.grad  !**，那么令 x.grad. grad = 1，然后进行一次反向传播，刚好就求出了 x = x 0 情况下的二阶导！

这个就是求高阶导的本质

#### 在反向传播的过程中禁止反向传播

实际上按照上文描述，反向传播的过程同时也是导函数的正向传播的过程，在这个过程中使用 python 的 with 机制：

```python
def backward(self, retain_grad=False, create_graph=False):  
    if self.grad is None:  
        xp = dezero.cuda.get_array_module(self.data)  
        # 创建一个与self.data类型相同的全1的数据，并封装成Variable类型  
        self.grad = Variable(xp.ones_like(self.data))  
  
    funcs = []  
    seen_set = set()  
  
    def add_func(f):  
        if f not in seen_set:  
            funcs.append(f)  
            seen_set.add(f)  
            funcs.sort(key=lambda x: x.generation)  
  
    add_func(self.creator)  
    while funcs:  
        f = funcs.pop()  
        gys = [output().grad for output in f.outputs]  # output is weakref  
  
        with using_config('enable_backprop', create_graph):  
            gxs = f.backward(*gys)  
            if not isinstance(gxs, tuple):  
                gxs = (gxs,)  
  
            for x, gx in zip(f.inputs, gxs):  
                if x.grad is None:  
                    x.grad = gx  
                else:  
                    x.grad = x.grad + gx  
  
                if x.creator is not None:  
                    add_func(x.creator)  
  
        if not retain_grad:  
            for y in f.outputs:  
                y().grad = None  # y is weakref
```

 `create_graph` 是 `Config` 类中的一个属性名字，这个属性用来决定在正向传播的过程中是否建立连接，具体看上文的`“反向传播的状态切换与 with 切换机制”`

这里的 `create_graph=False` 一开始设置成 false，含义就是在反向传播的过程中会经历导函数的运算，并建立导函数的正向传播计算图（建立连接），这里设置成 false 就是反向传播的时候并不建立导函数的连接，仅仅将 x.grad 计算出来就完事，比如上文的 $y = x^{3}$，它的 backward 实现是 $3x^{2}*y.grad = x.grad$，这行代码执行的时候会建立连接，但是如果 `create_graph=False` 一开始设置成 false 那么就不会建立连接，x.grad 这个 Variable 类型变量本身并不记忆它的生成函数，也就无法调用 x.grad. backward () 来计算二阶导了

`x.grad.backward(create_graph=True)` 它求出了 x 的 k 阶导函数，并建立了 y = f (x) 的 k 阶导函数的计算图

#### 一个复杂的复合函数高阶导的过程

下面再以 x -> cos -> c -> sin -> y 这个复合过程说明求高阶导的过程

（1）首先正向传播建立计算图：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260224122345.png)

（2）然后调用 y.backward 计算 y 对 x 的一阶导，首先是 y 对中间变量 c 的一阶导，按照代码中的逻辑，调用的是 sinx 的 backward 方法：

```python
class Sin(Function):  
    def forward(self, x):  
        xp = cuda.get_array_module(x)  
        y = xp.sin(x)  
        return y  
  
    def backward(self, gy):  
        x, = self.inputs  
        gx = gy * cos(x)  
        return gx
```

这里的 self. inputs 就是变量 c，gy 就是 y.grad，这些都是 Variable 变量，当计算完 c.grad 的时候建立了计算图，注意它不是凭空建立的，而是在前面正向传播的基础上建立的计算图，它相当于新创建了一个函数结点 cos，这个 cos 的输入就是变量 c，然后与 y.grad 乘积得到 c.grad：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260224122907.png)

（3）计算完了 c.grad 之后继续向前进行反向传播，计算 x.grad，同样调用的是 cos 的 backward 方法：

```python
class Cos(Function):  
    def forward(self, x):  
        xp = cuda.get_array_module(x)  
        y = xp.cos(x)  
        return y  
  
    def backward(self, gy):  
        x, = self.inputs  
        gx = gy * -sin(x)  
        return gx
```

形成的计算图如下所示：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260224123703.png)

加上函数和变量的代际关系如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260224124415.png)


稍微整理一下，可以得到二阶导的时候的等价计算图：


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260224123730.png)

此时调用 x.grad. backward ()，那么就是在这个计算图上进行反向传播，这里省去了代际关系

（1）x.grad = 1
（2）计算对-s 的导数，即 cc * 1，也就是 cos (cos x) *  1
（3）计算对 cc 的导数，即-s * 1，也就是 - sin x * 1
（4）计算对 s 的导数，也就是- cos (cos x) *  1
（5）计算对 c 的导数，也就是 `-sin c *( - sin x * 1) = sin(cos x) * sin x `
（6）计算两个路径对 x 的导数，两者加起来：

$$
\cos x * (-\cos(\cos x)) + (-\sin x * \sin(\cos x) * \sin x )
$$

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260224124847.png)

这个结果是正确的，当然上面都是在 x 有具体数值的情况下的过程，最后的结果就是代入 x 0 到 $\cos x * (-\cos(\cos x)) + (-\sin x * \sin(\cos x) * \sin x )$ 这个式子的结果


## 创建神经网络

### 向量的反向传播（逐元素进行复合的函数）

（1）这里是针对输入输出是向量的情况，比如 y = F (x)，这里的 y 和 x 都是向量
（2）这里也是针对逐元素的函数，比如 y = sin (x) 这样的函数，逐元素的含义是 y 这个向量的每个维度仅仅取决于 x 的每个维度，即：如果 x = `x1, x2, x3` 那么 y 就是 `sin(x1), sin(x2), sin(x3)`，一般来说，如果 y = F (x)，这里 x 是一个向量的话，y 对 x 的导数是一个雅可比矩阵：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260225194259.png)
而在逐元素的函数中，这个雅可比矩阵就退化成了一个对角矩阵，因为对于 yi 来说，它只是 xi 的表达式，它对其他的变量求偏导的结果都是 0：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260225194439.png)

对于这种逐个元素形成的计算图，可以将输入和输出当作一个 Variable 标量（输入和输出仅仅是一个 Variable，而 Variable 类内部是一个向量）进行，它经过 numpy 的计算可以得到正确的结果，比如 y = cos(sin (x))

它形成的计算图是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260225200832.png)

从理论上讲，这个计算图反向传播是两个雅可比矩阵的乘积：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260225201035.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260225201042.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260225201100.png)

但是由于这是逐元素函数进行的复合，它在计算的时候不用构造雅可比矩阵，而是之间利用 numpy 自己的计算规则逐个元素计算即可

下面来分析这个过程

对于下面这个计算图来说，在正向传播的过程中，x，t 1, y 这三个其实都只是单个 Variable 变量，只是这个 Variable 变量内部是一个向量
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260225200832.png)

进行正向传播建立连接的过程如下所示：

```python

class Function:  
    def __call__(self, *inputs):  
    	# 这里传入并列表化之后，inputs是一个列表，这个列表只有一个元素
    	# 这个元素是200维的数组，x是输入向量
        inputs = [as_variable(x) for x in inputs]  
  		
  		# 将列表元素取出，xs是输入变量x的数据本身，是一个200维的数组
        xs = [x.data for x in inputs]  
        # 这里xs不是一个列表，而是一个ndarry的数组，它这里*xs解包之后实际上传入的参数仍然只有一个
        # 经过下方的sin.forward实现之后，ys同样是一个200维度的ndarry数组
        ys = self.forward(*xs)  
        if not isinstance(ys, tuple):  
            ys = (ys,)  
        # 这里进行列表化，实际上outputs列表只有一个元素，一个200维度的ndarry数组
        outputs = [Variable(as_array(y)) for y in ys]  
  
        if Config.enable_backprop:  
            self.generation = max([x.generation for x in inputs])  
            for output in outputs:  
                output.set_creator(self)  
            self.inputs = inputs  
            self.outputs = [weakref.ref(output) for output in outputs]  
  
        return outputs if len(outputs) > 1 else outputs[0]

class Sin(Function):  
    def forward(self, x):  
        xp = cuda.get_array_module(x)  
        y = xp.sin(x)  
        return y  
  
    def backward(self, gy):  
        x, = self.inputs  
        gx = gy * cos(x)  
        return gx
```


这个计算图的结构本身与一维数据的结构没有区别，只是 Variable 内部的变量是多维的，利用 numpy 本身的特性进行多维正向传播

同理在针对多维数组进行反向传播计算导数的时候，也是以 Variable 整体进行反向传播计算的，在这个过程中自动利用 numpy 的计算特性，得到了向量 y 的每个维度对 x 的每个维度的导数

这里仅仅介绍 y 对中间变量 t 1 的求导过程：

```python

def backward(self, retain_grad=False, create_graph=False):  
    if self.grad is None:  
        xp = dezero.cuda.get_array_module(self.data)  
        # 创建一个与self.data类型相同的全1的数据，并封装成Variable类型  
        # self.grad就是一个200维度的ndarry数组
        self.grad = Variable(xp.ones_like(self.data))  
  
    funcs = []  
    seen_set = set()  
  
    def add_func(f):  
        if f not in seen_set:  
            funcs.append(f)  
            seen_set.add(f)  
            funcs.sort(key=lambda x: x.generation)  
  # 将y的生成函数假如到funcs中，这里就只有一个cos
    add_func(self.creator)  
    while funcs:  
        f = funcs.pop()  
        # 这里取出cos 的输出变量的导数，这里实际上就是y.grad，并将其封装成一个列表
        # gys是一个列表，这个列表只有一个元素，即一个200维度的数组
        # `gys[0]` 是一个 `Variable` 对象
        #  该 Variable 的数据：`gys[0].data` 是形状为 `(200,)` 的 NumPy 数组（全 1）
        gys = [output().grad for output in f.outputs]  # output is weakref  
  
        with using_config('enable_backprop', create_graph):  
            # 这里的*是将列表解包的操作  
            # 假如gys = [a, b, c]  
            # 那么这里实际上等价于gxs = f.backward(a, b, c)  
            
            # 这里将gys解包，实际上解的包仅仅是gys这个列表本身，而不是将gys[0]中的200维度的数组解包
            # 后续进行backward计算的时候也是利用numpy特性进行计算
            
            # 后面的x.grad也是单个的一个Variable对象，只是这个对象中是一个ndarry的数组
            gxs = f.backward(*gys)  
            if not isinstance(gxs, tuple):  
                gxs = (gxs,)  
  
            for x, gx in zip(f.inputs, gxs):  
                if x.grad is None:  
                    x.grad = gx  
                else:  
                    x.grad = x.grad + gx  
  
                if x.creator is not None:  
                    add_func(x.creator)  
  
        if not retain_grad:  
            for y in f.outputs:  
                y().grad = None  # y is weakref
```

这里 `gxs = f.backward(*gys) `，gys 解包之后传递的参数是一个 200 维度的全 1 向量，它调用了 f.backward 方法，即：

```python
class Cos(Function):  
    def forward(self, x):  
        xp = cuda.get_array_module(x)  
        y = xp.cos(x)  
        return y  
  
    def backward(self, gy):  
    	# x也是一个200维度的向量，就是Variable变量t1的数据
        x, = self.inputs  
        # 它与gy这个200维度的全1向量进行cos的导数的乘法
        # 这里利用了numpy的特性，默认代表的是逐元素相乘，得到的同样是一个200维度的向量
        # 这样gx的每个维度都是y的每个维度对t1的每个维度的导数了，它同样是一个向量
        gx = gy * -sin(x)  
        return gx

```

接着 gx 继续向前传播，这样就求出了 y 对 x 的每个维度的导数，**在这个过程中不用构造雅可比矩阵进行矩阵乘法，而是直接利用 numpy 的特性进行逐元素相乘反向传播**

这里可行的本质就是函数的复合是逐元素的，那么反向传播计算的时候 numpy也是逐元素的，因此最后得到的就是输出变量 y 逐个元素维度对输入变量 x 的逐个元素维度的导数，而不用构造雅可比矩阵进行计算


### 求和的反向传播与正向传播

这里的求和是指对向量或者矩阵矩阵中的元素进行求和

它的反向传播是将输出变量的导数复原成输入变量：

![image.png|417](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301203054.png)

### 广播的正向与反向传播

这里是为了让框架进行下面的操作：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301195906.png)

这里在执行加法运算的时候 x 0 和 x 1 的两个变量利用 numpy 的机制自动进行了广播（对 x 1 进行了广播），但是反向传播的时候，很显然，计算 y 对 x 1 的反向传播并没有处理广播的反向传播，传给 x 1 的导数与 x 0 是一样的，这显然不对

广播函数的反向传播原理如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301200322.png)

因此这里 x 1 先广播到 `[10, 10, 10]`，然后再与 x 0 相加得到 y
y 对 x 1 的导数就是
（1）先得到 y 对 x 1 广播之后的 `[10,10,10]` 的中间导数 `[1, 1, 1]`
（2）然后再执行 `sum_to` 函数沿着广播方向相加，得到 `[3]`

这里之所以要求和，实际上本质是广播对原来输入变量的复制，比如下面的例子：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302152059.png)

如果要求z 对x 的导数，首先是z 对y 的导数，是 `[1, 1, 1]`，即z 对 `[y0, y1, y2]` 的导数就是 `[1, 1, 1]`，而 `y0, y1, y2` 都是等于x 0 的即，`[y0, y1, y2] = [x0, x0, x0]`，y 0 = x 0, y 1 = x 0, y 2 = x 0，这样广播就相当于创建了新的变量并且与原来的变量保证了一个相等的映射关系，因此这里z 对中间变量 y 0, y 1, y 2 的导数实际上是在三个路径上对x 0 的导数，因此z 最终对单个维度的x 0 的导数当然要加起来

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302152610.png)



### 矩阵乘法的正向与反向传播（最终结果是标量）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301205036.png)

这里的矩阵乘法不再是一个逐元素的复合函数了，因此中间输出变量 y 对 x 的导数就是一个完整的雅可比矩阵

这里求 L 对 x 的导数的时候，显然本质上还是对 x 的逐个元素进行求导的，按照链式法则，对于任意一个变量 xi，实际上都通过了 j 个 y 路径到达 L，所以 L 对 xi 的导数就是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301205453.png)

这里的 y 是向量 x 与矩阵 W 进行矩阵乘法的结果，因此 yj 是：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301211444.png)

所以 y j 对 xi 的导数就是 Wij：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301211509.png)

这样就将矩阵乘法也转化成了类似普通乘法那样与另一个乘数的关系

如果将上式最后的结果写成向量的形式，那么：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301211813.png)


所以 L 对 x 形成的导数向量就是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301211848.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301211857.png)

这说明它转化成了一个类似普通乘法的形式，$\frac{\partial L}{\partial y}$ 是输出变量对中间变量的导数，W^T 是另一个乘数

这里虽然是向量与矩阵的乘积的反向传播求导推导过程，但是推导到一般的矩阵反向传播也是成立的：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301212256.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301212301.png)

按照这个原理，可以非常轻松的实现矩阵的反向传播了

### 切片函数的正向传播与反向传播

切片函数就是获得输入变量 x 的部分元素，它的反向传播就是将被提取的部分设置成 1，其他的部分设置成 0：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304175042.png)

这是因为输出变量 y 与 x 中被提取部分元素的关系是相等关系，所以 y 对这些元素求导结果就是 1，而 y 与 x 中的其他元素无关，所以 y 对其他元素的求导就是 0


### 线性回归实现

（1）准备数据集

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302154850.png)

这几行代码生成了一个简易的数据集，这个数据集在 y = 5 + 2 x 附近波动：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302155531.png)

（2）目标转化

我们需要做到的就是找到一个模型f，使得输入xi 的值之后得到的预测值 f (xi) 与xi 实际对应的yi 差距尽可能的小，具体来说，我们需要找到一个模型f 使得下面的残差尽可能的小：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302155833.png)

这个叫做均方误差

（3）线性回归

由于是线性回归，这里的模型f 实际上可以写成 f (xi) = xi * W + b，因此实际上就是找W 和b 让上面的损失函数L 尽可能的小

假设这里的每一个输入变量xi 是一个 4 维的数据，那么xi 构成的整体输入x就是一个 n 行 4 列的矩阵x，当然这里的W 也是一个一列 4 行的列向量

那么模型f 就可以用一个矩阵乘法来实现：

![image.png|645](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302161416.png)

而对于上面生成的简单数据集来说，单个元素xi 只有一个维度，它就退化成一个 100 行的列向量：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302161514.png)

f = x * W + b，使得下面的式子最小：

$$
L = \frac{1}{N} \sum_{i = 1}^{N} (f(x) - y)^{2} = \frac{1}{N} \sum_{i=1}^{N} (x * W + b - y)^{2}
$$
这里的x 是一个 100 * 4 的矩阵，W 是一个 4 * 1 的向量，b 可以是一个 4 * 1 的向量，当然也可以是一个 1 * 1 的单个元素数（会通过广播变成和 x * W 同样的维度），y 是一个 100 * 1 的向量，N 也是 100

现在我们的目标就是，在知道了x 和y 的情况下（上文生成的），求W 和b 让L 最小

注意虽然上文生成 x 和y 这些数据的时候我们用了线性函数生成，**但是实际中我们是不知道获得的数据x 和y 是如何生成的，我们只能假设用一个模型用来拟合这些数据**，比如这里，我们假装用一个线性模型来拟合它，设出线性模型中的参数W 和b，然后构造一个损失函数，并求解一个优化问题

### 梯度下降思想

假如下面的一个函数：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302171417.png)

想要求y 最小的时候的 x 1 和 x 0 的值，实际上就是它的极值点

梯度下降求极值点的思想就是先随机指定一个 (x 0, x 1)，然后求y 对他们的导数，比如指定:
$$
(x_{0}, x_{1}) = (0, 2.0)
$$

这里y 对他们的导数分别是：

$$
(-2.0, 400.0)
$$

这个就是梯度，假如 x 0 和 x 1 沿着这个梯度方向更新，则y 值是上升最快的方向，那么沿着梯度的反方向更新，则是y 值下降最快的方向

那么我们沿着这个梯度的反方向更新 x 0 和 x 1，这样不停的重复迭代，就能不断接近y 的最小值，这个方法就是梯度下降法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302171950.png)


### 梯度下降法来处理线性回归

我们有一堆数据x，以及x 的对应y，需要用一个线性模型来拟合它，实际上就是求参数W 和b，使得下面的函数值最小：

$$
L = \frac{1}{N} \sum_{i = 1}^{N} (f(x) - y)^{2} = \frac{1}{N} \sum_{i=1}^{N} (x * W + b - y)^{2}
$$

按照上面的梯度下降的思想，随机指定一个W 和b，然后反复用L 对 W 和b 求导然后沿着梯度反方向更新即可

下面来详细说明这个反向传播过程


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302174747.png)

假设这里x 是一个 100 * 4 的矩阵

W 和b，在初始化的时候：

```python
# W是一个4 * 1的全0的二维矩阵，即[[0, 0, 0, 0]^T]
# b是一个1维数组，只有一个元素0的一维数组，即[0]
W = Variable(np.zeros((4, 1)))  
b = Variable(np.zeros(1))
```

首先是获得y 的预测值 `y_pred`

```python
def predict(x):  
    y = F.matmul(x, W) + b  
    return y
```

这里矩阵乘法的正向和反向传播如下：

```python

class MatMul(Function):  
    def forward(self, x, W):  
        y = x.dot(W)  
        return y  
  
    def backward(self, gy):  
        x, W = self.inputs  
        gx = matmul(gy, W.T)  
        gW = matmul(x.T, gy)  
        return gx, gW
```


这行代码 `y = F.matmul(x, W) + b  `，首先是 x 和W 进行了矩阵乘法，然后再与b 进行了加法，矩阵乘法得到的是一个 100 * 1 的向量，与b 相加的时候使用的是 Add 类函数，它在自己的 forward 实现中利用 numpy 机制自动对b 进行了广播

这里形成的计算图如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302210335.png)

接着 pred 再调用均方误差函数得到标量L，均方误差函数的实现如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302210745.png)

它直接继承了 Function，这样做的原因是避免形成过多的中间 Variable 变量，因此这个函数所贡献的计算图就只有输入两个 Variable 变量以及输出一个 Variable 变量了

所以对于下面的均方误差L 函数来说：

$$
L = \frac{1}{N} \sum_{i = 1}^{N} (f(x) - y)^{2} = \frac{1}{N} \sum_{i=1}^{N} (x * W + b - y)^{2}
$$

x * W + b 是 predit 函数，它生成了一个 Variable 变量 y_pred，然后 y_pred 与 y 共同作为均方误差的输入变量，输出就是L，正向传播的计算图如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302212658.png)

至于反向传播，只要把上面各个生成函数的 backward 方法写好即可

最后反复迭代的代码如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302212816.png)

总结：
（1）给定了x 和对应的y，这里用一个线性模型来拟合，构造一个均方误差函数F
（2）求线性模型中的参数 W 和b，这里用梯度下降法，首先随机指定W 和b，然后进行一次F 正向传播得到L ，再进行一次反向传播求L 对W 和b 的导数
（3）沿着导数的反方向更新W 和b，继续步骤（2），迭代多次

### 一个简易的神经网络的实现

实际上就是对上文的线性回归进行套娃，比如上面是用一个线性回归来得到预测 y_pred，这里则是，套了两层用来根据输入变量x 得到 y_pred，我们要求的模型就是 W 1, b 1, W 2, b 2，构造一个损失函数让其最小即可

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302215116.png)

注意神经网络模型以线性变换->激活函数->线性变换->激活函数的模式进行
激活函数这里是 sigmoid，它的实现如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303170748.png)

这里对输入默认都是逐元素进行的
### 汇总参数的 `Layer` 类


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303171830.png)

生成数据集，利用一个二层神经网络模型来拟合它，假设将 x 输入到模型中的预测值是 y_pred，那么其实就是求让 y_pred 与 y 距离最小的情况下求模型中的参数（距离用上文的均方误差函数来衡量）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303172415.png)

这里的 Linear 是继承自 Layer 类，Layer 类的实现如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303172809.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303172651.png)

Layer 类给 Linear 类提供了基本的功能，包括：
（1）保存输入变量和输出变量，比如 x * W + b = y_pred，这里保存 x 和 y_pred 到 self. inputs 和 self. outputs
（2）保存这个过程中的参数 W 和 b 到 self. params 中
（3）让 Linear 类可以通过调用函数的方式进行调用比如` l = Linear (10)`，这里后续可以通过 `l(x)`，来进行 x * W + b = y_pred 的计算

Linear 类的实现如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303173330.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303173336.png)

这里 Linear 类在 Layer 类的基础上进行了功能的添加

首先是初始化的过程
（1）它接收这个线性回归层（其实也可以理解成线性回归函数），的输入和输出的维度，变量类型并进行保存
（2）它需要进行初始化 x * W  + b 中的参数 W 和 b，这里是根据输入维度和输出维度进行初始化的，比如输入维度为 3，保存为 I，输出维度为 1，保存为 1，那么它就会创建一个 3 * 1 的随机参数 W；当然这里 b 是偏置是可有可无的，参数 W 也可以放在进行 forward 计算的时候根据输入变量 x 的第二个维度进行初始化设置

然后是它实现了 `forward` 函数
（1）这个函数调用了原来 Function 类的线性运算代码，因此它创建的实际上还是一个基于 Function 类的计算图

所以总的来说 Linear 类的功能是
（1）接收调整这个线性运算的参数的维度，数据类型等信息，初始化一个 Linear 类的实例，比如 `l1 = Linear(10)`，这行代码就是创建了一个默认输出维度是 10 的线性运算实例 `l1`，并在初始化的过程中随机指定了线性回归的参数，并保存在了自己的 params 成员中
（2）按照函数调用的方法接收输入变量，然后得到预测值 y_pred，比如通过调用 `y_pred = l1(x)` 在这个过程中它调用了它继承的 Layer 类的 `__call__` 方法，将 x 和 y_pred 保存在了自己的成员变量中，然后调用 forward 方法，这个方法中调用了继承自 Function 类的线性运算代码创建了 x * W + b 的计算图

所以 `Linear` 类实际上是保存了输入，输出，以及随机指定了参数 W 和 b，然后调用 Function 类的功能创建了计算图：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303175825.png)

它主要就是起到了管理输入输出变量以及参数的功能

所以下面的两行代码：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303180016.png)

它实际上是创建了两个 Linear 实例对象，l 1 管理了一个输出维度是 10 的对象，l 2 管理了一个输出维度是 1（标量的对象），这里 l 1 的随机生成的参数 W 1 的第二维度就是 10，l 2 的随机生成的参数 W 2 的第二维度就是 1，W 1 和 W 2 的第一维度是在 forward 中根据输入参数 x 的第二维度来进行调整

接着是定义两层神经网络模型：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303180326.png)

l 1 的 Linear 类实例创建并管理了 W 1 和 b 1 （这里没有指定偏置，所以是空），l 2 的 Linear 实例类创建并管理了 W 2 和 b 2（这里没有指定配置，b 2 是空），两个 Linear 类都利用自己的输入，输出，自己创建的参数，通过 forward 方法调用 Function 类创建了计算图：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303183228.png)

上面的两个 l 1 和 l 2 就是用来生成并管理自己的参数 W 和 b 的，当然他们同样也保存了输入和输出变量；下方的计算图就是他通过 forward 函数生成的

接着是梯度下降过程：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303183421.png)

这个过程就按照 predict 以及均方误差函数建立的计算图进行反向传播计算 W 1 和 W 2 的导数即可

然后按照梯度的反方向更新这些参数，然后循环接着更新这些参数

### 将 `Layer` 类继续汇总

更改 `Layer` 基类的实现：

![image.png|652](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303214135.png)

这里修改的第一行是为了在最开始的时候形成一个更大的 Layer 类，这个最大的 Layer 类中可以放入同样的 Layer，比如下面的代码：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303214257.png)

model 是最大的 Layer 类，在这个类中的 params 参数中仍然放入了两个继承 Layer 类的 Linear 实例，类似于下图：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303214630.png)

在想办法获得上图中的最大 Layer 类中的参数以及作为最大 Layer 类参数的 Layer 类中的参数的时候，用下面的代码：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303214752.png)

它通过下面的代码进行调用：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303214855.png)


它在访问最大的 Layer 类的时候，遍历这个类的所有 params 参数，如果这个参数 obj同样是一个 Layer，则利用语句 `yield from obj.params()`，递归的将 obj 中的参数逐个返回：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303215421.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303215428.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303215439.png)

### Model 类

model 类继承自 Layer 类，主要用来更加清晰的定义我们的模型，它的实现如下，只有一个画出计算图的方法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304151151.png)

使用 model 类处理模型如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304151407.png)
如果我们要使用某个模型就在 Model 类的基础上再进行定义

### 实现全连接神经网络（MLP）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304151814.png)

这里的 MLP 类继承自 Model，Model 继承自 Layer 类，用来更加清晰的管理整个模型中的参数

`fc_output_sizes` 指定了整个神经网络中的各个层的输出参数：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304152957.png)

`activation` 指定了激活函数，这里用的是 F 中的 sigmoid 函数

下面的代码根据传入的神经网络的参数创建了一堆用于管理线性模型参数的 Linear 类
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304153108.png)

`layer = L.Linear(out_size)` 创建了一个管理线性模型的 Linear 类，这个类仍然继承自 Layer 类，它的主要功能就是初始化 x * W + b 中的参数 W 和参数 b，同时 Linear 类实现了自己的 forward 方法，通过 l (x) 来调用 forward 方法，在 forward 方法中调用了 Function 类中的函数，利用 x, W, b 来创建计算图

如果这里的 `fc_output_sizes = (10, 20 ,1)`，那么 `model = MLP((10, 20, 1))` 执行完了之后形成的管理的类结构就是：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304154035.png)


当然这里的 W 参数可能在 MLP 类初始化的时候还没有设定（根据 Linear 类的实现，在进行 forward 计算的时候才会根据输入变量 x 指定 W 参数的维度，进行计算构造计算图）

如果执行了 `model(x)`，那么就会调用 MLP 类的 forward 方法：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304154121.png)

他会挨个取出 MLP 所管理的所有 Linear 类，这些 Linear 类根据输入 x，创建 W 参数，并进行计算构建计算图

### 全连接神经元与全连接神经网络

这里全连接神经网络的含义是所有的输入神经元都连接到了输出神经元，以 x * W = y 为例，假如这里 x 是 3 * 4 矩阵，W 是 4 * 4 矩阵，y 是 3 * 4 矩阵

这里的输入数据是三个行向量，分别是 x 1，x 2, x 3，输出数据也是 3 个行向量，分别是 y 1, y 2, y 3
y 1 是 x 1 的输出，y 2 是 x 2 的输出，y 3 是 x 3 的输出

这里的全连接是针对一对输出和输出而言的，比如行向量 x 1 和行向量 y 1，x 1 有 4 个元素，y 1 也有四个元素，y 1 分别是 x 1 的四个元素乘各自的偏置然后相加得到的：

![image.png|435](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305174837.png)

输入 x 1 的四个元素构成了 4 个输入神经元，输出 y 1 的四个元素构成了 4 个输出神经元，4 个输入神经元通过乘权重得到了 y 11 第一个输出神经元，这个就是建立了 4 个输入神经元与输出神经元 y 11 的连接：

![image.png|335](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305175010.png)

剩下的 3 个输出神经元同样如此，他们都建立了和 4 个输入神经元的连接，即每个输出神经元都建立了和所有输入神经元的连接，这个就是全连接了

同样下面的另一对输入和输出 x 2 和 y 2 也是全连接

因此这样套娃下去，所建立的神经网络就是全连接神经网络了

需要注意的是输入是一个 3 * 4 的矩阵，实际上 x 1 , x 2, x 3 这三个输入互相是不相干的，x 2 并不影响 x 1 的输出，直接按照矩阵来说的话，x * W = b 相当于并行建立了 3 个神经网络

所以实际上神经网络就只针对一对输入和输出，这里之所以写成矩阵的形式是为了利用 GPU 中的并行计算

### 全连接神经网络的矩阵求导

对于单个输入来说，比如 x 是 1 * 4 的数据，经过 W = 4 * 2 得到 y 它的维度是 1 * 2，x * W = y

这里 y 对 W 的导数是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309094334.png)

即 y 对 W 的求导是一个 4 * 2 的矩阵

但是如果 x 是两个输入数据构成的矩阵，比如 x = 2 * 4 维度，W 是 4 * 2 的矩阵，输出 y 则是 2 * 2 的维度

此时 y 再对 W 求导仍然是上面的公式

并且仍然是一个 4 * 2 的矩阵，这个矩阵与只有单个输入 x 的相比，它实际上是每个 y 的导数 对相应单个输入 x 情况下梯度矩阵的按元素叠加

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309095020.png)

上图左侧是单个输入 x * W = y 对 W 求导的过程，第一个元素就是两个元素相乘的结果

右侧是两个输入 x * W = y 对 W 求导，得到的梯度矩阵，可以看作是 x 1 * W = y 1，x 2 * W = y 2，两个式子 y 1 和 y 2 分别对 W 求导后的矩阵按元素相加，这里的 x i 均是 1 * 4 的元素，yi 是 1 * 2 的元素
### 使用 SGD 类来进行参数更新

SGD 类继承自 Optimizer 类，Optimizer 类实现如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304172524.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304172530.png)

这里的 target 在后续会传入一个具体的 model 类，比如上文的 MLD 实现的全连接神经网络模型类

实例方法 update 用于收集传入的 target 中的所有的参数，然后按照具体的更新方法进行更新（调用 update_one 进行更新）

update_one 的实现在后续继承 Optimizer 类中

总的来说 Optimizer 类提供了功能有：
（1）获得模型，以及获得模型的所有参数到 params 实例属性中
（2）进行调用 update 函数进行更新，这个函数中会调用 update_one 函数挨个更新 params 中的参数，update_one 在后续继承 Optimizer 类的具体更新方法中实现

比如下面的 SGD 类，即它实现了按照随机梯度下降的方式来更新参数：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304173000.png)

利用 SGD 类处理上文实现的全连接神经网络如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304173026.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304173107.png)

### softmax 的正向传播

假设有 n 个 y，y 1, y 2, y 3.... yn
这个函数是用于计算每个 yk（k = 1 ~ n）占总的 y 1 + y 2 + ... yn 的概率：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304194202.png)

这里的 pk 是 yk 占总共的比例（进行指数运算之后）

这里 softmax 的普通实现如下（没有继承 Function 类变成 Dezero 中的函数）：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304195933.png)

假设这里 x 是一个 4 * 3 的矩阵：

![image.png|326](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304195947.png)

首先，将 x 变成 Variable 变量，然后进行 exp 运算，这里运算是逐元素的，所以这里 y 仍然是一个 4 * 3 的矩阵，是上面的 x 每个进行 exp 之后的矩阵

然后对 y 沿着轴 1 的方向进行求和，并保证维度不变，即对这个 4 * 3 的矩阵，对每个行向量求和，并得到了一个 4 * 1 的矩阵（列向量），也就是 sum_y

然后用 y / sum_y，这里会将 sum_y 广播成 4 * 3 的矩阵，然后进行逐元素相除，相当于 y 的每一行的每个元素都除以了这一行的和，于是就得到了 y 这个 4 * 3 的矩阵，每行元素的 softmax 的结果：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304205941.png)

### softmax 的反向传播


首先关于涉及矩阵的求导（最后结果是标量的反向传播），别思考中间的求导过程，直接思考最后的标量 L 是怎么由输入变量路径得到的，它必须得观察矩阵之间的对应关系，它的本质其实还是对矩阵的每个元素进行多元微分求导，将每条不同的路径相加的结果

比如一个 2 * 2 的矩阵 X，x 11 - x 12 = y 11，x 11 + x 12 = y 12，x 21 - x 22 = y 21， x 21 + x 22 = y 22：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314102341.png)

那么在进行反向传播求 L 对 x 11 的导数的时候，实际上 x 11 是通过两条路径到达 L 的：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314102543.png)

因此 L 对矩阵 X 的求导矩阵仍然是一个二维矩阵，并没有进行升维

得到上面的单个输入变量的公式之后，再思考怎么将其与 L 对 Y 的求导矩阵结合起来：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314104217.png)


softmax 也是这样的思路

我们先看单个样本 n 维度的情况：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314104305.png)

这里按照普通的函数求导法则可以推导 yi 对 xj 求导的公式如下：

![image.png|385](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314104344.png)

我们从最后的 L 对这里的单个 xj 的多元微分求导路径的角度来思考，根据 softmax 函数的公式，实际上从 xj 到 L 其实有 n 条路径：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314104718.png)

因此我们可以直接得到 L 对输入样本的某个维度 xj 的导数了，L 是标量，所以这里 L 对 x 的导数也是一个向量：

![image.png|423](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314104940.png)

由于这里的输入 X 和输出 Y 都是单个向量，所以向量的雅可比矩阵比较容易写出来：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314105022.png)

上面的矩阵也就是：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314105158.png)

所以写成矩阵乘法的形式可以得到：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314105544.png)

将偏 yi 比上偏 xj 拆开就可以得到（这里没有遵循特别严格的转置）：

![image.png|611](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314105650.png)

这里单样本比较容易写出 Y 对 X 的雅可比矩阵，但是如果对于多样本就不怎么好表示出 Y 对 X 的雅可比矩阵了，所以最好还是从多元微分链式法则的角度来思考 L 对 X 的导数矩阵是什么


假设下面的 n 个样本组成的 n * n 的矩阵，经过 softmax 函数之后得到 Y
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314105851.png)

想要知道 L 对 X 矩阵的导数，首先看看 L 对 x 11 的导数，这里 x 11 仍然只是通过 y 11, y 12, .. y 1 n 这 n 条路径到达 L，与 Y 矩阵其他行的变量无关，所以可以很轻松的直接写出 L 对 X 矩阵的导数：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314110152.png)

这里的 d 表示最终的损失 L 对变量求导的意思，dy 11 表示 L 对变量 y 11 的求导

而这个矩阵完全可以拆成 Y 以及 L 对 Y 的导数矩阵 dY 的相乘结果：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314110624.png)

这里圆圈中的点表示矩阵乘法的意思

因此就可以很容易的写出 softmax 的反向传播了：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314110702.png)


### 交叉熵误差损失函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304202816.png)

这里的 t 是实际的真实数据，比如 `[1, 0]` 表示猫的概率是 1，狗的概率是 0
p 是输入模型经过 softmax 之后的数据，比如模型预测 `[0.8, 0.2]` 预测猫的概率是 0.8，狗的概率是 0.2，将 p 取对数之后相加，然后再取反的结果就是损失：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304203037.png)

这里和上面的模型一样，要求的是让 L 尽可能小的情况下神经网络的参数值

事实上这里不用将全部的训练集数据 t 与预测 p 挨个相乘然后求得结果

由于 t 是 one-hot 编码，正确的是 1，错误的是 0，我们只需要将 t 中为 1 的位置所对应的 p 中的数据加起来即可，比如 `t = [1, 1, 0 , 0]，p = [0.1, 0.2, 0.5, 0.2]`，这里只需要将 p 的前两个位置的数据提取出来取对数求和即可：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304203346.png)

在某些情况下 t 不是 one-hot 编码，t 可能只是标签，比如 `p = [[1, 2, 3], [4, 5, 6]]` 这是一个 2 行的输出，每个输出 3 个维度，这里的 `t = [0, 2]`，这里的 t 的含义是指第一个输出属于第 0 个类别，第 2 个输出属于第 2 个类别，即第一个车输出 `[1, 2, 3]` 中的 1 是正确类别，第二个输出中 `[4, 5, 6]` 中的 6 是正确类别，这里的 t 的等价 one-ht 编码就是 `t = [[1, 0, 0], [0, 0, 1]]`；此时第一个输出的交叉熵损失直接就是 1 参与运算，第二个输出的交叉熵损失就是 6 参与运算

实现交叉熵误差损失函数如下所示：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305172400.png)

这里创建了一个两层的神经网络，输出维度分别是 10 和 3

这里的输入 x 是 4 * 2 的数据，经过神经网络运算之后的结果 y 是 4 * 3，这里的 x 是 4 个样本数据，每个样本数据都是 2 个维度，它经过神经网络之后变成了 4 个输出数据，每个数据都是 3 个维度

这里主要说明下交叉熵误差损失函数的实现：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305175554.png)

这里的函数内部的 x 变量是上文仅仅经过了模型运算之后的 y，`y = model(x)`，传入了 `softmax_cross_entropy_simple(y, t)`，还没有经过 softmax 函数运算

t 是每个输出变量（1 * 3 的行向量）的正确类别，这里 `t= [2, 0, 1, 0]`，表示的就是第一个输出行向量选下标 2 的类别，第二个输出行向量中选输出 0 的类别，第 3 个输出行向量选下标 1 的类别，第 4 个输出行向量选下标 0 的类别

经过一次 softmax，将单纯模型的 4 * 3 的输出转化成了 `[0, 1]` 中的值：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305180041.png)

具体原理见上文

`log_p = log(p)` 是对上面进行 softmax 得到的矩阵进行取对数的结果

然后是代码 `tlog_p = log_p[np.arange(N), t.data]`，这个代码中的 np. arange (N) 的作用是生成 `[0, 1, 3, ..n - 1]` 这样一个列表，t.data = `[2, 0, 1, 0]`，这行代码是一个高级索引，他的含义是在 log_p 这个矩阵中，第 0 行取出第二列元素，第 1 行取出第 0 列元素，第 3 行取出第 1 列元素，第 4 行取出第 0 列元素

这里的 N 是样本的数量，输入矩阵 x 是 4 * 2 的矩阵，所以就是 4 行， N = 4

这里的输出是 4 * 3 的矩阵，每个样本变成了 3 个数据，这里的t.data = `[2, 0, 1, 0]` 的含义就是第 1 个样本的输出中第 2 个数据是正确类别，第 2 个样本的输出中第 0 个数据是正确类别，第 3 个样本的输出中第 1 个数据是正确类别，第 4 个样本的输出中式第 0 个数据是正确类别，这个就是训练数据，这里与 one-hot 编码有点不一样，one-hot 编码是正确类别是 1，其他都是 0，然后用编码乘对应的预测数据，加起来取平均就是交叉熵误差损失，这里用编码乘对应的预测数据的时候，由于 on e-hot 编码是 01 交替的，所以就等价于将 one-hot 编码为 1 所对应的预测数据选出来加起来即可

这里的 t.data 就是这样，`t[0]` = 2，起始就等价于编码 `[0, 0, 1]`，与预测矩阵的第一个输出数据进行点乘，得到了交叉熵损失函数第一个样本的贡献了

### softmax-交叉熵误差的正向传播

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314121941.png)

这里的 log_p 是对输入 x 进行 `log(softmax(x))` 的结果

假设 x 只是一个行向量则 softmax (x) 就是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314122103.png)

对他取对数可以得到公式：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314122119.png)

这里的 logsumexp 函数的功能就是求 $\log\left( \sum_{j} e^{xj} \right)$，将其放到 utils 这个文件中，是因为还需要进行一些工程上防止溢出的处理

因此这里 `log_p = x - log_z`，就是 x 的每个维度元素都减去所有元素的指数之和求对数的结果，也就等价于每个元素先进行一次 softmax，然后再取对数的结果了

当 x 是 n 个样本的矩阵的时候，logsumexp 实际上求得是每个行向量的所有维度的元素的指数和求对数的结果，然后相减的时候会将 log_z 自动广播，这样就达到对输入矩阵 x 进行 softmax 并取对数的结果了：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314123008.png)

逐元素相减，就是每个行向量样本进行 softmax 并取对数的结果了

因此这里的 log_p 就是输入特征 x 然后对每个行向量样本进行 softmax 并取对数的结果了

t 是每个样本的标签，t.ravel 的作用是将 t 展开成一个一维的数组

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314151713.png)

最后两行代码就是将每个样本的对应标签处的概率加起来，然后除以总样本数，就得到样本的平均交叉熵损失了

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314151902.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314151921.png)

### softmax-交叉熵误差的反向传播

首先仍然是先考虑单个样本

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314153110.png)

这里的锁链关系如下所示：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314153630.png)

所以仍然按照多元微分的思想来看的话，求 L 对每个样本中的 zi 的偏导公式就是下图，这里的求和就是所有的锁链关系加起来的结果：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314153710.png)

首先看 $\frac{\partial L}{\partial p_{j}}$，根据 L 的表达式：

$$
L = -y_{1}*\log p_{1} - y_{2}*\log p_{2}\dots-y_{c}*\log p_{c}
$$

所以 L 对 p j 的偏导就是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314153918.png)

然后再看 $\frac{\partial p_{j}}{\partial z_{i}}$

p 1, p 2, p 3... pC 是 z 1, z 2, z 3.. zC 进行 softmax 的结果，上文其实已经得到 pj 对某个 zi 求导的结果了，实际上就是

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314154230.png)

整理这个式子，可以统一写成：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314154519.png)

将这个式子代入：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314154548.png)

继续展开可以得到：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314154645.png)

也就是说，对于一个 C 维度的单样本输入 z 1, z 2, .... zC，它经过 softmax 之后得到的是 p 1, p 2, ... pC，同时这个样本还对应一个 one-hot 向量 t，t = y 1, y 2, ... yC，这个 one-hot 向量中只有一个是 1，其他的都是 0

**那么最后的交叉熵损失 L 对单样本的某个输入 zi 的偏导非常简单，就是 zi 所对应的 pi - yi**

然后，假如 x 是多个样本矩阵的情况：

这多个样本在计算总的 L 的时候是先计算单行样本的交叉熵损失，然后再除以样本个数，得到交叉熵损失的均值：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314160313.png)

这里 L 1 就仅仅与第一行的样本有关，仅仅是 x 11, x 12, x 13 的函数

因此，比如L 对 x 11，或者 L 对 x 12 的偏导如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314160432.png)

所以最终结果 L 对输入 x 的导数矩阵非常简单，就是先乘个 1/N，然后矩阵 P 与矩阵 t 相减即可：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314162236.png)


```python
def backward(self, gy):  
    x, t = self.inputs  
    # N是样本的个数，CLS_NUM是单个样本的维度
    N, CLS_NUM = x.shape  
  	
    gy *= 1/N
    # 对输入矩阵x，进行一次softmax得到P  
    p = softmax(x)  
    # convert to one-hot  
  	
  	# t这里是标签，这里需要将其转换为one-hot编码矩阵
    t_onehot = np.eye(CLS_NUM, dtype=t.dtype)[t.data] 
    
    # 最后直接将p矩阵与t_onehot相减就行了 
    y = (p - t_onehot) * gy  
    return y
```

另外这里的 gy 实际上是最终输出变量对 L 这个标量的导数，所以 gy 也是一个标量，一般来说 softmax 交叉熵误差就已经是最后一层了，所以 gy 一般就是一个标量 1


### 螺旋数据分类

螺旋数据集如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305195543.png)

这里的 x 是 300 个输入变量，每个输入变量 2 个维度，同样，t 是一个 300 维度的列向量，取值是 0，1，2，它指定了每个 x 的输入（行向量）属于的类别

比如每个 x 行向量，点在二维坐标上如下所示：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305195709.png)

这里的 t 就指定了每个行向量是属于哪个类别

比如第 10 个 x 行向量是 ：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305195746.png)
这里的 `0.05984409, 0.081167` 就表示了上图中的一个点，这里的 `t[10]` 就表示了这个点所属的类别了


然后是小批量数据处理
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305200454.png)

这里的思想是，因为 x 有 300 个输入，如果一次性的全部放入神经网络迭代的话可能会计算非常慢，这里的思想是将 300 个数据分为 30 批，`batch_size = 30`，每批 10 个数据
进行 30 轮的迭代，每一轮完成工作：

（1）对这 10个训练数据放入 model 中，算出输出 y，然后计算一次损失函数，然后计算梯度更新一次参数
（2）模型获得新参数之后，再输入下一组 10 个训练数据，算出输出 y，然后计算一次损失函数，然后计算一次梯度，更新一次参数
（3）这样重复进行 30 次

这样的处理方式可以充分利用这些数据，并且又不至于让模型的规模太大

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305201310.png)

这里的 `index = np.random.permutation(data_size)` 是生成一个从 `[0, N-1]` 的随机数列表，它的作用是等价于将 x 的 300 行数据每轮进行重新排列，最外层的循环，进行了 300 轮的随机排列，每轮生成一个随机排列之后，生成了一个 index，它表示 x 的行号的随机排列

进入内存循环，内存循环了三十次，下面的三行代码：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305201742.png)

第一行代码是内存循环每轮按顺序取出 index 中的十个数据，用这 10 个数去取 x 的 10 行数据和对应的标签 t，构成了一个小批量的训练数据集，总的思想效果如下：

```
(1)最外层循环300次
(2)每次最外层循环都随机打乱x的行
	(1)内层迭代30次
	(2)每次都按顺序取出x的十行数据以及这些数据对应的分类
	(3)形成了一个小批量训练数据集, 用这个小批量数据集输入模型运算一次，更新一次参数
	(4)回到步骤(2)继续按顺序取出x的十行数据，用更新过的参数，运算一次，再更新一次参数
(3)回到步骤(2)，再打乱一次x的行，继续在原来的模型基础上迭代30次，更新参数
```

这样就可以在保证模型规模不大的情况下，充分利用这 300 个训练数据集来训练模型了

最后是计算损失

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306114556.png)

在这个代码中，y 是 30 * 3 的输出数据，有 30 个数据，每个数据都是 3 个维度, batch_t 也是一个 30 维度的数据，每个维度 `t[i]` 表示的是第 i 个输出 `y[i]` 所处的类别，比如
`t[i] = 2`，表示 `y[i]` 这行输出数据中，第 2 个维度（从 0 开始）是正确类别

softmax_cross_entropy 函数实现如下：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260305175554.png)

这个函数的作用是：
（1）对 y 这个 30 * 3 的矩阵的每一行数据进行 softmax，然后取对数得到 `log_p`
（2）利用 `tlog_p = log_p[np.arange(N), t.data]` 对 y 的每一行计算交叉熵误差损失函数
（3）最后将每一行的交叉熵误差取一个平均，于是就得到了训练一次模型的 30 个数据的平均交叉熵损失函数值

最后
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306114217.png)
这里是将整个模型所有的交叉熵损失总和加起来得到训练一次 30 个数据的交叉熵损失总和

这里之所以这样做的原因是为了计算打乱一次 300 个输入数据的行之后，然后迭代十次，每次迭代训练了 30 个数据，这十次迭代所训练的 300 个数据的交叉熵误差损失的平均：

这里的 data_size = 300
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306114440.png)

### Dataset 类

这个类的实现如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306104836.png)

他有两个最基本的功能

（1）通过 `__getitem__` 让这个类实例可以通过类似普通的 py 列表一样利用方括号来访问数据
（2）`__len__` 这个魔法方法，让类实例可以通过类似普通的 py 列表一样，利用 `len(x)` 来访问数据的维度
（3）这个类在初始化的时候调用 perpare 方法，将数据和标签存放到自己的 data 和 label 中

一个继承自这个类的 Spiral 如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306105831.png)

在 Spiral 类中自己实现了 perpare 函数，在初始化的时候将数据存放到了自己的 data 和 label 属性中

使用这个类实例如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306105927.png)

这里设置 train = true 表示取出的是训练数据，初始化之后就取出了 300 * 2 的训练数据以及标签放到了 data 和 label 中了

这里可以直接通过方括号来访问这个类，这里取出的就是第一个数据，以及这个数据所对应的标签

使用 Dataset 类进行神经网络训练代码如下：

（1）初始化数据集，模型，优化器：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306111101.png)

（2）指定每次迭代的次数：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306111129.png)

这里 train_set 与上文一样，是 300 个，batch_size = 30，即每次用三十个数据输入神经网络进行模型训练，这里的 max_iter = 10，每次用三十个数据输入神经网络进行模型训练，迭代 10 次

（3）进行循环迭代训练

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306111313.png)

i 最外层的迭代是 300 次，每次打乱所有的行；内层迭代十次，每次用 30 个数据训练模型
这里打乱的目的跟上文是一样的，内存迭代十次每次取得 30 个数据是按顺序取得，通过最外层的随机打乱，这样可以保证内存的十次迭代，每次迭代都是 30 个不同的数据组合去训练模型
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306111327.png)


ii 内层循环，迭代 10 次，每次用按顺序取得 30 个数据训练模型

这里的 `batch_size = 30`，每次从随机打乱的 index 300 个行号中从前往后取出 30 个行号数据，得到 batch_index

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306112411.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306112425.png)

这里的 batch 的结构就是 `[[x2, t2], [x5, t5]....]`
最后后面两个就是将所有的 x 数据取出来作为神经网络的输入 batch_x 以及将这些输入变量对应的标签提取出来作为 batch_t，用来后续计算损失函数
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306111658.png)


（4）计算损失

这里的损失计算思路和上文的一样，先是计算内层一次迭代 30 个训练数据的损失之和，然后将十次迭代的 300 个数据的损失加起来取平均，得到一次随机打乱行得到的平均交叉熵误差损失

最后的 `pirnt()` 打印的就是每次打乱 300 个数据行之后的平均交叉熵损失了

### Dataset 类改进，数据预处理

在 Dataset 类中加入下方代码
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306145647.png)

这里的 transform 和 target_transform 是创建数据集的时候指定的对输入数据和标签进行处理的函数

如果他们是 none，则被指定为一个返回自身的匿名函数，相当于没有进行处理

下方，在使用方括号访问数据的时候调用 transform 和 target_transform

比如下面的例子

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306145940.png)

这里指定了对输入数据进行除以 2，那么在后续利用 `train_set[2]` 访问输入数据的时候，此时返回的就是第 2 行（从 0 开始）被除以 2 的一个行向量

### Dataloader 类

这个类的实现如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308171100.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308171301.png)

这是一个实现了 `__iter__` 和 `__next__` 的类，也就是一个迭代器

它的几个实例属性如下：

（1）dataset 属性，保存的是一个 Dataset 类
（2）batch_size 属性，训练一次的小批量数据大小，比如上文中，每次取出 30 数据放到模型中进行训练
（3）shuffle，用于决定每次训练之后是否对数据进行重排
（4）max_iter，用于决定每轮迭代的次数，比如 300 个数据，每次用 30 个数据对模型进行训练更新参数，那么迭代的次数就是 300 / 30 = 10 次
（5）reset () 方法，这个用于对 Dataset 中的数据进行重排，当然实际上的操作是生成随机的行号列表，然后用这些随机的行号去访问数据，从而达到重排的效果，当然这里如果 shuffle 被设置成 false 则不会进行重排，reset 方法在 Dataloader 类在初始化的时候调用一次，在迭代 mat_iter 次数后会调用一次
（6）`__next__` 方法，它的实现如下所示：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308173033.png)

它每次获得模型的迭代轮次，然后获得更新模型的数据量大小，从随机生成的行号中按迭代轮次取出 batch_size 个行号，然后根据行号从 dataset 中取出输入数据以及对应的标签，这里利用 `set.dataset[i]` 实际上是利用上文的 dataset 类中的 `__getitem__` 方法利用方括号来访问数据：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308193244.png)

它返回的是输入数据以及它对应的标签形成的列表，数据格式类似下面：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306105927.png)

因此这里的 batch 实际上是 batch_size 个上图的列表

通过后面的两行语句分别将 30 个输入数据和 30 个对应的标签提取出来

这里使用 Dataloader 类如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308193705.png)

首先创建了两个 Dateset 类，放进了两个 Dataloader 中，代码中的 `for x, t in train_loader` 就是用来调用 Dataloader 中的迭代功能的，这里的 for 每循环一次 `__next__` 方法中的 `iteration` 就+1，然后取出 batch_size 个数据以及标签并返回，接着 `for x, t in train_loader` 进行下一次循环，然后按顺序继续从一个随机打乱的行号中取出 batch_size 个数据和对应标签并返回

### accuracy 函数

这个函数用于评估模型预测的训练集数据与实际正确数据之间的差距

```python
def accuracy(y, t): 
	y, t = as_variable(y), as_variable(t) 
	# 将输出数据y中（每个输出数据是一个1*3的行向量）每行的最大值的下标提取出来表示的是预测的类别，同时转化成t的形状
	pred = y.data.argmax(axis=1).reshape(t.shape) 
	# 将预测类别与t的实际类别进行比较
	result = (pred == t.data) 
	# 计算预测正确的标签所占的比例
	acc = result.mean() 
	return Variable(as_array(acc))
```

这里的 `y.data` 是一个 30 * 3 的矩阵，包括 30 个输出数据，每个输出数据是 3 个维度，3 个维度中值最大的那个下标就作为输出的预测类别，然后与实际类别 t 挨个进行比较得到 result

### 螺旋数据集的训练过程

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308200413.png)

`max_epoch` 参数，它 = 300，说明要随机打乱 300 行，每打乱一次行就按顺序取出 10 次 30 个数据用来训练模型

`batch_size` 参数，它 = 30，说明每次从打乱的行中按顺序取出 30 个数据用来训练更新模型参数，重复十次

`hidden_size` 参数，这个是全连接神经网络的中间隐藏层的输出维度，输入数据是 300 * 2，经过一层 x * W 得到一次中间输出结果，这里的 W 的输出维度就是 `hidden_size`

`lr` 是创建迭代器需要传入的步长参数


然后是准备数据的过程：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308201310.png)

首先是创建了两个 Dateset 类，这两个类一个存放的是训练数据集一个是测试数据集，Dataset 类中存放着取出的数据，并提供了按照方括号来访问数据的方法

然后是两个 Dataloader 类，Dataloader 类中存放了 Dataset 类，Dataloader 类实现了迭代器功能，支持在外面用 `for xxx in train_loader` 来迭代访问 Dataset 中的数据，具体来说就是利用迭代次数作为行号来访问 Dataset 中的测试数据和所对应的标签，并在迭代一轮之后随机打乱数据（实现方式是生成随机打乱的行号，利用行号来访问原数据从而达到随机访问的效果）


然后是创建模型和优化器：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308203303.png)

这里创建了两个模型，用 MLP 来管理这个模型中的所有参数，具体的 MLP 中实际上有两个 Linear 类（继承自 Layer 层），MLP 通过调用这两个 Linear 类来进行运算，并管理这两个 Linear 类的所有参数

优化器中保存了这个模型，以及更新的步长，当模型的 loss 调用自己的 backwrad 方法之后，得到输出变量对参数的导数，就调用优化器的 update 方法，按照梯度方向对模型的参数挨个进行更新

下面是模型进行训练的代码：

```python
for epoch in range(max_epoch):  
    sum_loss, sum_acc = 0, 0  
  
    for x, t in train_loader:
    	# 取出30个数据进行一次模型计算  
        y = model(x)  
        # 计算30个数据的平均交叉熵损失
        loss = F.softmax_cross_entropy(y, t)
        # 计算30个数据的平均预测正确率  
        acc = F.accuracy(y, t)  
        model.cleargrads()
        # 进行一次反向传播计算输出loss对参数的导数  
        loss.backward()  
        # 更新这些参数
        optimizer.update()  
  		# 将30个训练数据的交叉熵损失和平均准确率加起来
        sum_loss += float(loss.data) * len(t)  
        # 这里的sum_acc这里乘len(t)之后实际上就是这30个数据中预测正确的输出数据个数了
        sum_acc += float(acc.data) * len(t)  
  
    print('epoch: {}'.format(epoch+1))  
    # 计算这300个数据的平均交叉熵损失函数
    # 计算这300个数据的预测准确率
    print('train loss: {:.4f}, accuracy: {:.4f}'.format(  
        sum_loss / len(train_set), sum_acc / len(train_set)))  
  
    sum_loss, sum_acc = 0, 0
    # 300个数据训练完了之后用训练好的参数在测试集上计算交叉熵误差和预测准确率  
    with dezero.no_grad():  
        for x, t in test_loader:  
            y = model(x)  
            loss = F.softmax_cross_entropy(y, t)  
            acc = F.accuracy(y, t)  
            sum_loss += float(loss.data) * len(t)  
            sum_acc += float(acc.data) * len(t)  
  
    print('test loss: {:.4f}, accuracy: {:.4f}'.format(  
        sum_loss / len(test_set), sum_acc / len(test_set)))
	# 这300个数据每次取30个数据训练一次模型更新一次参数，进行10次结束之后
	# 随机打乱这300个数据再重复一次上述过程
	# 打乱数据是在迭代完了之后for x, t in train_loader:自动在train_loader中进行的
```


## MINST 训练过程

### 数据集介绍

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309113810.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309114607.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309113816.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309114615.png)


这些代码的操作是取出训练数据集和测试数据集的大小

这里的训练数据集的单个输入元素是一个单通道的特征图，1 * 28 * 28 的数据
输出 t 表示对应的标签（即属于哪个类别）

这里的训练集中有 60000 个数据，即 60000 个单通道特征图，以及他们所对应的类别

测试数据集中有 10000 个数据，以及他们所对应的类别
### 数据预处理

这里的每个输入数据都是 `1 * 28 * 28` 的单通道特征图，由于上文的全连接神经网络的每个输入数据都是行向量，单个维度，所以这里需要将其展平为 1 * 784 的数据，同时需要将这里面的数据全部除以 255 转化成 0 ~ 1 之间的数据：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309120011.png)

这里在创建 Dataset 类的时候通过指定 transform 预处理函数为 f 来进行转化，Dataset 类中进行预处理的逻辑如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309120342.png)

在通过方括号获得数据的时候它会调用 tranfsorm 来对数据进行处理，然后再返回
### 模型训练

训练 MNIST 数据的过程如下：

```python
# 将所有数据随机打乱5次，每次随机迭代600次，每次迭代用100个数据去更新模型
max_epoch = 5  
# 小批量训练模型，每次用100个输入作为模型输入去更新模型的参数
batch_size = 100  
# 两层全连接神经网络，中间一层的输出是1000维度
hidden_size = 1000  
  
# 将60000个训练数据读取到train_set数据集中，每个数据的类型如上文所示
train_set = dezero.datasets.MNIST(train=True)  
# 将10000个测试数据读取到test_set数据集中
test_set = dezero.datasets.MNIST(train=False)

# 将测试数据集放入到Dataloater中让其可支持迭代  
train_loader = DataLoader(train_set, batch_size)  
test_loader = DataLoader(test_set, batch_size, shuffle=False)  
  
model = MLP((hidden_size, 10))  
# 创建优化器，这里的步长缺省，在类实例中默认是0.01
optimizer = optimizers.SGD().setup(model)  
#model = MLP((hidden_size, hidden_size, 10), activation=F.relu)  
#optimizer = optimizers.Adam().setup(model)  
  
for epoch in range(max_epoch):  
    sum_loss, sum_acc = 0, 0  
  	
  	# 每次从迭代器中取出100个数据输入到模型中进行训练
    for x, t in train_loader:  
        y = model(x)  
        loss = F.softmax_cross_entropy(y, t)  
        acc = F.accuracy(y, t)  
        model.cleargrads()  
        loss.backward()  
        optimizer.update()  
  
        sum_loss += float(loss.data) * len(t)  
        sum_acc += float(acc.data) * len(t)  
  	# for循环结束之后相当于60000个数据，每次取出100个输入进模型进行训练，进行了600轮的训练和参数更新
  	# sum_loss是这60000个数据的总的交叉熵损失
  	# sum_acc是这60000个数据的总的精确个数
    print('epoch: {}'.format(epoch+1))  
    print('train loss: {:.4f}, accuracy: {:.4f}'.format(  
        sum_loss / len(train_set), sum_acc / len(train_set)))  
    
    sum_loss, sum_acc = 0, 0  
    with dezero.no_grad():  
        for x, t in test_loader:  
            y = model(x)  
            loss = F.softmax_cross_entropy(y, t)  
            acc = F.accuracy(y, t)  
            sum_loss += float(loss.data) * len(t)  
            sum_acc += float(acc.data) * len(t)  
  
    print('test loss: {:.4f}, accuracy: {:.4f}'.format(  
        sum_loss / len(test_set), sum_acc / len(test_set)))
```

### ReLU 激活函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309152759.png)

它的反向与正向传播如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309152826.png)

反向传播的逻辑是，当输入 x > 0 的时候直接将中间输出变量的导数返回，当输入 x < 0 的时候导数就是 0，所以这里用了一个变量 mask 进行区分

# 面试 Q&A

（1）仿照代码随想录中的造轮子的说法来写简历

## 核心模块

### `Variable` 类

**属性**

（1）data 属性，是一个 ndarry 实例
（2）creator 属性，记住它的生成函数，用于构建计算图
（3）generation 属性，用于反向传播中用于处理函数计算的优先级，是它的生成函数的 generation 属性 + 1
（4）grad 属性，记录输出变量对该变量的导数，仍然是一个 Variable 实例


**主要方法**

backward 方法

这个方法是输出变量进行调用，它的核心逻辑是类似层序遍历一样，从最后面的输出变量开始，设置一个与输出变量维度相同的全 1 数组，然后逐层按照优先级取出变量的生成函数，从后往前计算输出变量对中间各个变量的导数

### `Function` 类

包括属性
（1）`inputs`，是一个输入的 Variable 列表，在正向传播计算的过程中每个函数用于记住它的所有输入变量，哪怕只有一个输入，也会
（2）`outputs`，是一个输出的 Variable 列表，在正向传播计算的过程中记住它的所有输出变量
（3）`generation`，是在正向传播中函数的优先级，它设置成它的所有输入变量中最大优先级的那一个


#### 加法的正向反向传播

（1）对于最基础的 x 1 + x 2 = y，加法的正向传播是直接将 y 的导数赋值给 x 1. grad 和 x 2. grad
（2）假如 x 1 或者 x 2 的维度在这个过程中进行了广播，比如 x 2 变成了 x 2'，那么对 x 2 的导数需要沿着广播方向加回去


#### `sum` 函数的正向反向传播

这里的求和是指对向量或者矩阵矩阵中的元素进行求和

它的反向传播是将输出变量的导数复原成输入变量：

![image.png|417](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301203054.png)

#### 广播函数的正向反向传播

实际上是 `sum` 函数的逆过程
广播函数的反向传播原理如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260301200322.png)

因此这里 x 1 先广播到 `[10, 10, 10]`，然后再与 x 0 相加得到 y
y 对 x 1 的导数就是
（1）先得到 y 对 x 1 广播之后的 `[10,10,10]` 的中间导数 `[1, 1, 1]`
（2）然后再执行 `sum_to` 函数沿着广播方向相加，得到 `[3]`

这里之所以要求和，实际上本质是广播对原来输入变量的复制，比如下面的例子：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302152059.png)

如果要求z 对x 的导数，首先是z 对y 的导数，是 `[1, 1, 1]`，即z 对 `[y0, y1, y2]` 的导数就是 `[1, 1, 1]`，而 `y0, y1, y2` 都是等于x 0 的即，`[y0, y1, y2] = [x0, x0, x0]`，y 0 = x 0, y 1 = x 0, y 2 = x 0，这样广播就相当于创建了新的变量并且与原来的变量保证了一个相等的映射关系，因此这里z 对中间变量 y 0, y 1, y 2 的导数实际上是在三个路径上对x 0 的导数，因此z 最终对单个维度的x 0 的导数当然要加起来

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260302152610.png)

#### 切片函数的正向和反向传播

提取位置部分的导数原样返回，其他部分设置成 0

这是因为输出变量只是提取部分的导数，输出变量与其他部分是无关的，所以原形状的其他部分的导数就设置成 0
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304175042.png)

#### 矩阵乘法的正向和反向传播

在目前的框架功能中，不论最后是均方误差损失还是交叉熵损失，最后的结果都会是一个标量

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312164004.png)

这个标量对 y 的导数也是一个 N * H 的矩阵，L 对 x 和 W 的导数分别是下面两个公式：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312164254.png)

它的推导如下，比如下面的 x 是 3 * 3 矩阵，W 也是 3 * 3 矩阵，两者的矩阵乘积也是 3 * 3 矩阵，y 经过一系列的运算得到标量 L，L 对 y 的导数也是 3 * 3 的矩阵

那么要求 L 对 x 的导数，实际上就是 L 分别对 x 这个 3 * 3 矩阵的每个变量的导数，以 L 对 x 11 求导为例，x 11 只通过 y 11, y 12, y 13 三条路径作用于 L，所以 L 就从 y 11，y 12, y 12 这三条路径反向传播到 x 11

y 11，y 12, y 13 分别关于 x 11 的表达式如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312170024.png)

因此 L 对 x 11 的导数其实就是这三条路径混合求导全加起来：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312170152.png)

锁链图如下：
![image.png|463](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312170159.png)


再将 L 对 y 的导数矩阵与 W 并列可以观察：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312170453.png)

所以下面的公式就是正确的了：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312170516.png)

同理 L 对 W 的各个变量导数也是这样推导的：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312170537.png)

这就是矩阵的反向传播实现了，非常简单，转置一下再相乘就得到了
#### 均方误差损失函数的正向与反向传播

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313093213.png)


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313092912.png)

这里设计的均方误差损失的计算输入 x 0 和 x 1 是一维的数组，如果是二维的矩阵上述的 `len(diff)` 需要更改成 `diff.size`

如果是一维数组，它的计算原理如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313095020.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313095030.png)

这里仍然是逐元素的，此外就是上方代码中的 gy 其实只是一个标量 1，但是进行乘法运算的时候自动广播成了 n 个 1 的数组，然后进行了后续的运算：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313095500.png)

#### 线性运算的正向和反向传播

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313100233.png)

它实际上就是矩阵乘法的反向传播，y = x * W + b

#### sigmoid 激活函数的正向和反向传播

（1）正向传播用了一个 sigmoid 等价的函数形式，用 tanh 来进行计算，来避免数值溢出
（2）反向传播就是普通的求导建立计算图

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313213833.png)


#### softmax 的反向传播

（1）对于一个样本向量来说，yi 对 xj 的求导，如果 i == j，则求导之后是 yi (1 - yi)，如果 i != j，则求导之后是-yi * yj
（2）对于单个样本向量，最后的损失 L 对输入 X 的求导，本质上是用多元微分的链式法则来推导的，比如 x 1 实际上与 y 1 ~ yn 都有关系，那就将 L 对 y 1~yn 这 n 条路径上的偏导全部加起来得到对 x 1 的导数
（3）对于 n 个样本的向量矩阵，他的本质也还是多元微分的链式法则，因为单个样本行向量中的每个元素，比如 x 11 实际上也只与输出 Y 的对应行向量中的所有元素有关，x 11 也是经过了 y 11~y 1 n 这 n 条路径到达 L，所以也还是可以直接写出 L 对 x 11 的导数

（4）将所有的 x 的导数写出来之后可以得到 L 对 X 的公式就是：$Y * \left( \frac{\partial L}{\partial Y} -\left( Y * \frac{\partial L}{\partial Y} \right).sum(1) \right)$，这里的 sum (1) 是沿着横向求和的意思

#### softmax - 交叉熵误差损失函数的正向传播与反向传播

这里将 softmax 和交叉熵误差损失放在一起实现

（0）标签转换成 one_hot 矩阵来分析
（1）首先是单样本的时候
（2）再从单样本扩展到多样本矩阵的情况，虽然变成了矩阵，但是第一行样本的每个变量对最后的损失的贡献所走的路径经过的都是 softmax (x) 矩阵的第一行向量
（3）记住最后的公式，softmax 矩阵与 one_hot 矩阵逐元素相减再除以样本的个数




### Parameter 类

这个类继承自 Variable，它的作用主要是区分作为变量的 Variable 实例还是作为参数的 Variable 实例
### Layer 类

这个类的主要功能就是
（1）保存管理所有的参数
（2）对参数进行初始化
（3）调用 Function 实例中的方法建立计算图

#### 基类实现

**属性**

（1）`_params` 集合属性，这个属性保存了所有的 Parameter参数
（2）inputs 属性，这些都是 Variable 实例，当然，如果是 Parameter 实例的化会单独保存在_params 属性中
（3）outputs 属性，这些也都是 Variable 实例

**方法**

（1）`__call__` 方法，他让 Layer 实例也可以像函数调用那样进行使用，在 call 方法中，通过调用 `forward` 方法来进行运算
（2）`forward` 方法，这个方法实际上是获得了输入之后通过调用 Function 类实例来运算并建立计算图
（3）`params` 方法，这个方法用于获得当前 Layer 层的所有参数

#### Linear 类

**属性**

（1）in_size，也就是样本的维度，比如输入是 100 * 4 的矩阵 x，那么就是 100 个样本，每个样本是 4 个维度，这个可以在创建的时候缺省，后续在 forward 方法实现中自动调用 x.shape[1]获得它的维度
（2）out_size，这个是输出样本的维度
（3）dtype，参数的数据类型

**方法**

（1）`init_W` 方法，这个方法初始化一个 (in_size, out_size) 的参数矩阵 W
（2）`forward` 方法，这个方法获得了输入 x, W（可能会先初始化），b，之后调用了 Function 实例 linear 方法，建立了计算图

**主要功能**

（1）重载了 `__call__` 方法，让 Linear 类可以按照函数调用的方式进行
（2）在__call__方法中，Linear 类实例记住了自己所有的 inputs 变量，并将 Parameter 类实例放到了 `_param` 集合中
（3）最核心的功能就是管理 `_param` 集合中的参数，包括将他们初始化，清空他们的梯度等
（4）在 forward 方法中调用了 Function 实例，通过调用他们以及自己的参数和输入建立正向传播计算图

#### Model 类

这个类主要是为了进一步的打包并且抽象模型

比如上文的两层神经网络，它实际上由两个 Linear 层构成，这两个 Linear 层实际上可以直接放到一个 Layer 类中，即 Model 类，让这个类进行统一管理

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303214630.png)

#### MLP 类（全连接神经网络层）

这个类继承自 Model 类，它主要的功能是创建一个全连接神经网络，MLP 类中可以有多个 Linear 层（Linear 类），MLP 类通过初始化的输入来决定具体有多少个（层）的 Linear，以及每层 Linear 的输出维度是多少

**初始化参数**

（1）`fc_output_sizes`，它是一个列表，指定了 MLP 类每层 Linear 的输出维度，比如 `fc_output_sizes = [10,1]`，那就说明这个 MLP 类由两层的 Linear 构成，第一层的输出维度是 10，最后一层的输出维度是 1

（2）`activation`，它指定了每层使用的激活函数，默认是 sigmoid

**基本属性**

（1）`layers`，这是一个列表，存放了 MLP 的所有的 Linear 层，也就是这个列表存放了 MLP 包含的所有的 Linear 实例

（2）`activation`，指定了全连接神经网络所使用的激活函数


#### 为什么创建 Layer 类和 Model 类

（1）如果不创建 Layer 类来统一管理参数，那么每次更新参数的时候得手动挨个更新，会非常麻烦，还容易出错，让 Layer 类记住它被创建调用的时候的所有参数，那么在清除参数导数，以及更新参数的时候直接通过 Layer 中的方法就可以了

（2）如果不创建 Model 类，那么对于多个层的神经网络，还得手动挨个创建，比如如果是 5层的全连接神经网络，那就得创建五个 Linear 类实例，然后挨个进行调用，以及将输出激活，更新参数的时候也得挨个进行更新；创建了 Model 类，让其记住它的所有层，通过 Model 类的方法递归的获得它的 Layer 类中的所有参数并更新显然更方便；后续的大一点的神经网络，比如 CNN 也是如此


### Optimizer 类

这个类记住了某个模型，比如 MLP 模型，然后他会暴露给外面一个 update 方法，这个方法中实现了对 MLP 模型中的参数的更新，后续就直接调用它的 update 方法就可以更新参数了，就不用用户自己在代码中写循环结构进行参数更新

**属性**

（1）target，这个属性存放了具体的模型
（2）hooks，这个属性中存放了具体的钩子函数

**方法**

（1）setup 方法，这个方法让 Optimizer 中的 target 记住了需要用来更新的模型
（2）update 方法，这个方法从 target 目标模型中取出所有的参数，然后挨个调用 update_one 方法来挨个更新这些参数
（3）update_one 方法，这个方法由具体的 Optimizer 实例对象来实现

#### SGD 类

这个类继承自 Optimizer，它主要的功能是实现了 update_one 方法，即梯度下降来更新某个具体的参数

**属性**

（1）继承了 Optimizer 的所有属性
（2）lr 属性，更新的步长

#### momentum 类


#### Adam 类

**基础属性**

（1）t，步长每进行一轮的参数更新，t ++
（2）alpha，学习率用于最后更新参数的时候发挥作用
（3）beta 1，用于调整一阶矩 mt 的超参数
（4）beta 2，用于调整二阶矩 vt 的超参数
（5）ms，这是一个字典，用于存放每个不同的待更新的参数的 mt 历史积累
（6）vs，这也是一个字典，用于存放每个不同的待更新参数的 vt 历史积累

为什么需要 ms 和 vs 两个字典，这是因为 Adam 算法对每个参数的更新策略是不一样的，对于 SGD 来说，不管是哪一个参数，只用减去这个参数的梯度 * 学习率就 OK 了，可以直接从每个参数本身直接获取. grad 数据；但是对于 Adam 算法，它更新每个参数的时候，必须要知道这个参数在历史过程中的一阶矩积累和二阶矩积累，所以就需要用一个字典来维护每个参数在不同的步数更新之后的 mt 和 vt 积累，这个字典中键就是每个参数，值是每个参数在步骤 t 更新完了之后的 mt 和 vt 积累

**基本方法**

（1）lr 方法，当获得历史的一阶矩积累 mt 和二阶矩积累 vt 之后，可以将他们两的修正和参数更新两步统一起来得到下面的公式，这里的 lr 方法就是下图中的一大坨利用超参数和学习率结合起来的东西；这样当获得 mt 和 vt 之后，直接调用 self. lr 然后乘起来就可以更新参数 $\theta$ 了

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314212439.png)

（2）update_one 方法，这个方法是用来挨个更新参数的，它将参数作为键来查找 ms 和 ts 两个字典中，这个参数的历史 mt 积累和 vt 积累，然后进行积累的更新，然后获得 self. lr，直接按照公式进行参数更新即可

另外，第一次更新参数的时候 mt 和 vt 初始化为 0，然后积累一次更新参数
### DataSet 类

它的功能是
（1）读取数据，从网站下载或者直接从文件中读取
（2）数据预处理
（3）重载了 `__getitem__` 方法，让用户可以像访问数组一样访问它读取存放在 data 中的数据

**读入参数**

（1）train 参数，表明是否是训练数据
（2）transform 参数，这个是用于对输入样本进行预处理的函数，默认是 `lambda x : x`，即样本本身
（3）target_transform 参数，这个是对标签数据进行预处理的函数，默认是 `lambda x : x`，即本身

**属性**

（1）train 属性，用来表示 Dataset 中的数据是否是训练数据
（2）data 属性，Dataset 类中保存所需要的所有样本数据
（3）label 属性，每个样本所对应的标签

**方法**

（1）`__getitem__` 方法，这个方法让 Dataset 类支持数组方式取出数据，比如 spir_dataset 是一个 Dateset 类实例，这个方法让其可以通过 `spir_dataset[0]` 取出第 0 行的样本数据以及这个样本所对应的标签，下图是取出一个 300 * 2 维度数据的第 0 行样本数据以及它所对应的标签
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306105927.png)

（2）`prepare` 方法，用于读取数据到 data 属性中，以及样本所对应的标签到 label 中

### DataLoader 类

数据加载器，它的基本功能是：

（1）保存所有的样本到它的 Dataset 属性中
（2）决定每次小批量训练模型的数据大小，以及进行一轮 epoch 的最大迭代次数
（3）随机打乱数据
（4）成为一个迭代器，支持从外部利用 for XXX in dataloader 语句从 dataset 中取出小批量数据进行模型的训练更新

**基本属性**

（1）dataset 属性，保存的是一个 Dataset 类，Dataloader 就从这个 Dataset 中读取数据
（2）batch_size 属性，训练一次的小批量数据大小，比如上文中，每次取出 30 数据放到模型中进行训练
（3）shuffle，用于决定每次训练之后是否对数据进行重排
（4）max_iter，用于决定每轮迭代的次数，比如 300 个数据，每次用 30 个数据对模型进行训练更新参数，那么迭代的次数就是 300 / 30 = 10 次
（5）index 属性，这个属性在 reset 方法中被更新，它是随机打乱的所有样本数据的行号，通过随机打乱行号形成行号列表，然后利用这个行号从样本 Dataset 中取数据从而达到随机打乱样本的效果

**基本方法**

（1）reset () 方法，这个用于对 Dataset 中的数据进行重排，当然实际上的操作是生成随机的行号列表，然后用这些随机的行号去访问数据，从而达到重排的效果，当然这里如果 shuffle 被设置成 false 则不会进行重排，reset 方法在 Dataloader 类在初始化的时候调用一次，在迭代 mat_iter 次数后会调用一次
（2）`__next__` 和 `__iter__` 方法，这两个方法让 Dataloader 类成为一个迭代器，在模型训练代码中通过 `for XXX in Datalodaer` 来反复从 Dataloader 类保存的 Dataset 中取出 batch_size 个样本来进行一次模型迭代参数更新


## 功能实现逻辑

### Function 类中的forward 函数的输入和输出

`forward` 函数在 Function 的 `__call__` 方法中被调用，在 `__call__` 方法中，首先回将输入转换成 Variable 实例，然后取出 Variable 实例中的实际数据传入到 forward 方法中

因此对于 forward 方法，它的实际输入是 ndarry 实例，返回也是 ndarry 实例，返回之后接着在 `__call__` 方法中将返回的 ndarry 实例转换成 Variable 实例，并让函数记住这个输出（建立连接）

### Function 类中的 backward 方法的输入和输出

Function 类中的 backward 方法，或者说某个函数的 backward 方法，实际上会在 Variable 实例中的 backward 方法中进行调用（也就是进行迭代反向传播的时候调用）

此时在 Variable 实例中的 backward 方法中会首先生成一个与输出变量形状相同的全 1 数组，并将其转换成 Variable 实例，然后传入到特定函数的 backward 方法中

因此某个 Variable 实例中的 grad 属性同样是 Variable 实例

并且 Function 类中的 backward 方法它的输入是 Variable 实例，它的输出同样是 Variable 实例

### define and run 和 define by run 的区别

（1）define 是事先定义好了计算图，然后图构建完成之后在这个静态的计算图之上进行正向或者反向传播，比如早期的 tensorflow，还有 MXNet，早期主推静态
（2）define by run 是边计算，边构建计算图，运算完成之后，按照这些运算的操作反向逐步完成求导，也就是反向传播，比如 pytorch，还有 Chainer


### Define by Run 功能的实现

（1）介绍 Variable 类，上文
（2）介绍 Function 类，上文
（3）说说正向传播的过程
（4）举个例子说说反向传播的过程

### forward 功能的实现套路（框架中的功能函数实现套路）

以加法为例

**（1）用一个普通的函数封装实际的功能函数类**
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310192650.png)
进行一些预处理，比如将非 Variable 实例转换成 Variable 实例

**（2）在普通函数中调用一个继承自 Function 类的实际功能函数**

**（3）这个功能函数继承了 Funtion 中的 `__call__` 方法，这个方法让一个类对象可以按照函数调用的方式使用**

**（4）`__call__` 方法中实现了一些通用的功能，比如，让函数实例记住它的输出，并调用函数的实际 forward 方法，得到输出，然后记住它的输出，并让输出记住这个函数，也就是它的生成函数，这样逐步建立计算图**

### backward 功能的实现

（1）首先，每个继承自 Function 的函数实例会自己实现的导函数，也就是 backward 方法，比如 sin x，它的导函数是 cos x，那么它的 backward 方法就是 cos (x)
（2）实际的反向传播的过程，是 Variable 实例中的一个方法，通过调用最终的输出变量 y.backward 进行
（3）这个方法首先会初始化一个与输出变量形状一致的全 1 数组，然后创建一个 funcs 列表，首先将输出变量的生成函数放入其中，然后从列表中取出一个方法，调用它的 backward 实现，计算出输出变量对输入变量的导数，由于这个函数记住了输入变量，所以可以直接设置输入变量的导数了，接着再将输入变量的生成函数放入到 funcs 列表中，然后再取出这些函数重复同样的过程，这样有点像类似 bfs 的过程，逐层往前计算出输出变量对中间所有变量的导数
### 高阶导数的功能实现

它的核心思路是在反向传播的过程中建立了导函数的正向传播计算图

（1）每个函数的 backward 方法实际上都是这个函数的导函数的运算
（2）每个具体函数的 backward 方法，会在反向传播的迭代过程中调用（Variable 实例中的 backward 方法调用），这些具体函数的 backward 方法的输入输出仍然是 Variable 实例对象
（3）进行导函数运算的时候调用的运算符号，比如加减乘除或者幂运算，这些都是实现好了的 Function 实例，他们在运算的过程中会在原来的正向传播的计算图基础上建立导函数的正向传播计算图
（4）因此最后反向传播得到的 x.grad 实际上是导函数计算图上的输出，而 x.grad 仍然是 Variable 实例，此时直接调用 x.grad. backward () 方法就行了
### 多维数组，逐元素反向传播的实现

假如输入是一个矩阵，经过逐元素的函数，比如 y = sin (x)，那么计算 y 对 x 的导数的时候

假如 x 是一个 n * n 的矩阵，经过一次 forward 得到 y，y.data 也是一个 n * n 的矩阵 (当然 x 和 y 本身都是 Variable 实例)

但是这里 y 这个矩阵的每个元素 yij 只是由 x 对应位置 xij 所得到的，yij 不是别的 xkl 的函数，因此想要求 y 这个矩阵对 x 的导数，实际上与雅可比矩阵没有什么关系，就只是 yij 对 xij 的导数

在反向传播的时候 y.grad. data 是一个 n * n 的全 1 矩阵，它作为 sin x 的 backward 方法的参数，进行 cos (x) * y.grad 得到 x.grad，此时cos (x) * y.grad 仍然是逐元素的，这个结果很显然是正确的

### 线性回归的实现

（1）初始化一个 100 * 4 的输入，并指定一个真实系数矩阵 4 * 1 的
（2）通过 x * W + b + noise，得到 x 以及对应的 y 训练数据
（3）初始化 W
（4）步长选择为 0.001，迭代一万次

### MINST 训练

#### 数据预处理

（1）这里的原始数据是 (1, 28, 28) 的单通道特征图数据，这里首先将其展平为为一维向量，全连接神经网络的单个样本必须是向量才行（模型中的输入可以是一个矩阵，但是这个矩阵是多个行向量样本组成的）

（2）将归一化，将每个像素值除以 255 变成 `[0, 1]` 中的数值，这样做的目的是：
i 避免数值过大导致权重更新过于剧烈，进行震荡训练不稳定
ii 如果数值过大可能达到中间的激活函数 sigmoid 的边缘了，这样会导致梯度变成 0 造成梯度消失
iii 保证所有的样本值都在相同的范围内，可以让一些优化器性能更好

#### 迭代次数

（1）小批量数据取 100，每次取 100 个数据进行模型更新，一共迭代 600 次
（2）epoch 取 5，进行 5 轮大更新
（3）两层神经网络，hidden_size = 1000

## 优化

### 加减乘除中的额外处理

#### 实现 Variable + 标量 或者标量 + Variable

通过运算符重载完成，调用左边的 Variable 的 `__add__` 方法，或者右边 Variable 的 `__radd__` 方法，将标量转换为一个列表，内部进行 ndarry 相加的时候会自动进行广播，因此在反向传播的时候需要加回去

#### 实现 Variable + ndarry 或者 ndarry + Variable

由于每个函数继承了 Function 类，在调用函数的时候，这里执行加法调用的是加法函数，会将非 Variable 类的 ndarry 实例转换成 ndarry 实例

#### ndarry 的自动广播机制处理

在进行 Variable + 标量的时候，此时会自动将标量转换成一个 ndarry 数组并在调用加法操作的时候将其转换成 Variable 实例

在进行加法操作的时候 ndarry 实际上会将这个标量形成的数组自动广播成另一个数组的形状，然后得到结果，比如 1 * 3 的数组与的标量形成的数组相加

调用 add 方法的时候如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310192650.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310192714.png)

如果是一个 Variable + 标量

此时会先在 add 函数中将 x 1 转换成一个 ndarry 实例，然后再实际调用 Add（）方法，相当于变成了 Variable + ndarry 实例了

然后在 Add 方法类初始化的过程中，将输入全部转换成 Variable 实例，再调用 forward 方法

#### sigmoid 函数优化实现

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313212520.png)

根据恒等式可以得到 sigmoid 和 tanh 函数的关系：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313212544.png)

所以最好是在 sigmoid 函数的正向传播中换成下面那个 tanh 的实现，这样两者是恒等的，而之所以 tanh 中的指数不会溢出，是因为 python 中的 tanh 实现并不是简单的指数运算之后的叠加，他会进行一系列的变形进行数值运算，所以不会溢出

## 深度学习的基础知识

### 优化器
#### 牛顿法

（1）首先指定这个函数上的点 a 0，然后在 a 0 处进行二阶泰勒展开
（2）这个二阶泰勒展开有一个最小值 x 1，下一步在 x 1 处继续泰勒展开，得到一个新的二次函数，这个新的二次函数的最小值在 x 2 处，就这样不断更新 x
（3）这个过程的公式可以总结为 $x_{n+1} = x_{n} - \frac{f'(x_{n})}{f''(x_{n})}$



#### 梯度下降法 SGD

假设 y = F (x 0, x 1)，我们的目标是找到 F 最小的时候的 x 0 和 x 1

自变量的梯度是 y 分别对 x 0 和 x 1 求偏导

函数沿着梯度方向增长最快

那么沿着梯度的反方向减少最快

那么随机指定两个点 x 0， x 1，求梯度，沿着梯度反方向更新 x 0 和 x 1 即可

#### 梯度下降法的缺点

（1）可能陷入到局部最优点，而非全局最优
（2）当函数非均匀的时候，比如 z = 1/20 x  + y^2，对于一个点 (x 0, y 0) 来说，它的梯度方向在 x 方向上很小，但是在 y 方向上很大，这样更新下一个 (x 1, y 1) 它可能在反复走之字形路线，导致迭代次数很大，效率不高

#### Momentum

这个算法是对 SGD 的改进，他的公式是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312174501.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312174443.png)

这里的 v 其实就是历史梯度的积累
（1）初始的时候 v 设置成空，就用普通的梯度进行一次参数更新，并且得到 v 1
（2）下一次将 v 1 代入，得到 v 2，然后用 v 2 更新得到下一次参数

他的优点在于：
（1）如果当前梯度与历史的梯度方向一致，则会加速更新
（2）如果当前梯度与历史的梯度方向相反，则会减缓震荡
（3）梯度的积累也可以帮助跳出局部最优解

#### AdaGrad

（1）这个算法积累了更新过程中所有梯度的平方和 h
（2）在利用梯度更新参数的时候需要先将梯度除以平方和 h，然后再更新参数
（3）这样参数中被大幅更新的参数的学习率会变小，同时迭代次数越多最后更新的幅度会慢慢变小

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312175244.png)

#### Adam 算法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312205550.png)

（1）这里的 gt 就是输出变量对参数的梯度
（2）mt 可以看成是梯度的一次方按照超参数的比例的不断积累，vt 可以看成是梯度的二次方按照超参数的不断积累
（3）t 是更新的步数，每进行一次更新 t 就 ++

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312205607.png)

每次用 mt 和 vt 去更新参数之前需要先进行一次纠正

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312205613.png)
也可以将这个公式展开得到：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314212439.png)

（1）从直观上来讲，mt 越大表示历史的梯度方向越一致，从而最后更新参数的时候在这个方向上更新越多，沿着这个方向更新加速
（2）vt 越大，说明某个参数的梯度跨度越大，此时更新参数的时候 vt 在分母，这样这个参数更新的时候反而幅度会更小，这样可以减少梯度的震荡
### 激活函数

#### 激活函数的作用

激活函数的引入主要是为了引入非线性，如果只是单纯的线性运算叠加，则多层是没有意义的，完全可以等价为单层只是系数发生变化

#### sigmoid 激活函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312221614.png)

优先：可以压缩数值，输出映射到(0, 1)，有利于标准化输出
缺点：有梯度消失问题

#### tanh 激活函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312222002.png)

优点：将数值压缩到 (-1, 1) 之间，在原点附近的梯度比 sigmoid 大，收敛更快
缺点：仍然有梯度消失问题

#### ReLU 激活函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312221715.png)

优点：高效缓解了梯度消失问题
缺点：有一些神经元会永远输出 0

#### Leaky ReLU 函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312222155.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312222204.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312222218.png)

#### softmax 激活函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312222514.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312222524.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312222606.png)

softmax 一般只用于输出层，隐藏层不会使用它

#### 使用场景

（1）sigmoid 和 tanh 一般用于二分类问题，不常用于深层神经网络
（2）ReLU 和它的变体一般用于深层卷积神经网络
（3）softmax 一般用于多分类问题的最后一层，用来输出概率否是
### 深度学习与机器学习的区别

（1）深度学习使用的是多层神经网络，而传统的机器学习模型一般较浅，通常由少量的层组成
（2）深度学习模型可以在训练过程中自动提取特征，而传统的机器学习依赖手工设计的特征
（3）深度学习所需的计算资源很多，超过机器学习，但是深度学习的性能一般会超过传统机器学习

### 如何评估一个深度学习模型

通过准确率，精确率，召回率，均方误差，交叉熵误差损失等等

### 训练集，验证集，测试集分别是什么？

当给定的数据足够多的时候我们可以将数据分别划分为这三个集合

其中训练集就是单纯用来模型训练的，其目标就是要让训练误差最小化，一般任何一个模型当训练次数足够多的时候都可以保证训练误差是越来越小的，比如线性回归和多项式回归，当迭代次数足够多这两者都可以保证训练误差足够小

验证集是用来选择模型和调整参数的，比如线性回归和多项式回归在训练集上的表现都非常好，那就应该将两者在验证集上比一下，看选择哪个，或者已经选定了线性回归，其在训练集上表现很好，那么就应该在验证集上测试一下，调整调整参数，用来防止过拟合的出现

测试集是用来最终评估模型的，是用来反应模型的真实能力的

但是一般数据不够充分的话就划分两个数据集，训练集和测试集就完毕了


### iteration 和 epoch

iteration 是指一次迭代，是利用一次小批量数据进行一次参数更新的过程

epoch 是用所有的训练数据对模型进行的一次完整的训练

一个 epoch 通过包括多个 iteration

比如 10000 个训练样本，batch_size 可以设置成 100，训练 10 个 epoch

那么一次 epoch 需要进行 100 次迭代，每次迭代用 100 个样本来更新模型参数

下一次 epoch 可以随机打乱这 10000 个样本，继续进行 100 次迭代更新模型参数


### 梯度消失和梯度爆炸的原因

#### 梯度消失

梯度消失：
（1）参数的初始值设置过小
（2）网络的层数过深，并且整体的梯度层<1 的时候反向传播的连乘会让梯度指数衰减
（3）如果输入过大或者过小，激活函数 sigmoid 和 tanh 的导数会非常小，这样反向传播的时候梯度也会消失，可以换成 ReLu 激活函数

梯度消失解决方法：
（1）权重进行合理的初始化
（2）batch normalization 批量归一化，让输入稳定分布，将激活值拉到稳定区间
（3）更换成 ReLu 激活函数

#### 梯度爆炸

爆炸原因：

（1）初始参数设置过大
（2）网络过深，并且每层的梯度 > 1，这样连乘起来导致梯度过大

解决方法：
（1）进行合理的参数初始化
（2）利用 L 1 和 L 2 正则化

### 常见的初始化方法

#### 初始化的作用

（1）防止梯度消失和梯度爆炸
（2）加速模型收敛
（3）提高模型性能



#### Xavier 初始化方法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312212735.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312212752.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312212759.png)

适用于 tanh 和 sigmoid 激活函数

#### He 初始化方法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312213107.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312213123.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312213137.png)

### 正则化方法

#### batch normalization

在选择样本的一个小批量进行模型训练的时候，如果这个小批量的样本分布是一个比较奇怪未知的分布，那么经过一层层非线性变换以及不同的参数更新，每层的分布都会发生变化以及无法预测，这会导致网络很难收敛

为了解决这样的问题，需要让每一层的样本服从类似的分布

具体做法是：

（1）当每一层的的输入经过运算得到中间结果之后，需要计算这些中间结果的均值和标准差
（2）中间结果每个元素通过减去均值并除以标准差，让他们归一化到均值为 0，标准差为 1 的正态分布中
（3）为了避免分母为零，需要在分母中加上一个 `epsilon` 微小量，同时，为了避免每层的分布都完全一样，还需要加上两个参数 $\beta, \alpha$

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260312215548.png)

这样保证了每层的分布都是类似的，我们就可以用较大的学习率，从而加快网络的训练

#### L 1 和 L 2 正则化

