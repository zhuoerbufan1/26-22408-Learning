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

### python 中的魔法方法（前后双下划线特殊方法）

这类方法允许让用户自定义的类就像 python 内置的类一样进行使用，比如：

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

v1 = Vector(1, 2)
v2 = Vector(3, 4)

# 这些操作会失败：
# v1 + v2  # 不能相加
# print(v1)  # 输出：<__main__.Vector object at 0x...>
# len(v1)  # 错误：Vector 对象没有长度

# 使用魔法方法增强之后
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):  # 实现加法
        return Vector(self.x + other.x, self.y + other.y)
    
    def __str__(self):  # 实现字符串表示
        return f"Vector({self.x}, {self.y})"
    
    def __len__(self):  # 实现长度
        return 2  # 总是返回2，因为是二维向量

v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1 + v2)  # Vector(4, 6)
print(v1)      # Vector(1, 2)
print(len(v1)) # 2
```

魔法方法主要是为了用户可以方便使用自定义类的

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

### 多维参数的实现

实际上这个框架的多维变量的操作使用的是 numpy 本身的特性，以下面的例子进行分析

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260224154727.png)

首先这里 `x = Variable(np.linspace(-7, 7, 200))` 生成的是一个 Variable 变量，这个变量的 x.data 的数值是一个包含 200 个元素的列表（注意这里 x 仍然是一个 Variablde，x 本身不是列表，只是一个单独的 Variable 变量，而 x 的元素，即 x.data是一个列表）

调用 F.sin (x)，这个函数的实现以及在这个过程中的例子如下：

```python

class Function:  
    def __call__(self, *inputs):  
    	# 这里传入并列表化之后，inputs是一个列表，这个列表只有一个元素
    	# 这个元素是200维的数组
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


在这里多维数组的运算并没有用到这个框架本身的特性，而是将多维数组当作一个 Variable 类内部的数据，实际上经过 y = sin (x) 之后创建的计算图是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260224160138.png)

这个计算图的结构本身与一维数据的结构没有区别，只是 Variable 内部的变量是多维的，利用 numpy 本身的特性进行多维正向传播，或者反向传播

同理在针对多维数组进行反向传播计算导数的时候，也是以 Variable 整体进行反向传播计算的，而不是对 Variable 类中的多维数组中的数据进行一次反向传播

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
  # 将y的生成函数假如到funcs中，这里就只有一个sin 
    add_func(self.creator)  
    while funcs:  
        f = funcs.pop()  
        # 这里取出sin 的输出变量的导数，这里实际上就是y.grad，并将其封装成一个列表
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

## 创建神经网络

