
# 普通语法


### 函数签名

下图中的 greet 函数定义是 py 3.5 之后的新语法，其实和普通定义没啥区别，主要用于区分 py 这个函数具体功能的

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316215524.png)


### `__file__` 变量，`os.path.abspath()` 函数，`os.path.dirname()` 函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314173007.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314173015.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314173025.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314173127.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314173139.png)


### 负索引

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314081612.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314081622.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314081632.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314081652.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314081705.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314081718.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314081730.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314081739.png)


### enumerate() 内置函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313220931.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313220945.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313220952.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313221000.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313221014.png)


### 三元条件表达式

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313100359.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313100409.png)



### 运算符重载

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310162533.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310162555.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310162603.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310162614.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310162725.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310162735.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310162745.png)

这里 v + 3 的时候 v 是 self，3 + v 的时候调用的是__radd__此时 v 是 self 而不是 3
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310162954.png)

### 嵌套函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152905.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152924.png)


### 匿名函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306145014.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306145207.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306145215.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306145233.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260306145321.png)

### 高级索引

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304205316.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304205325.png)

相当于 socres 这个矩阵 `scores[list1, list2]`，list 1 选中行，list 2 选中 list 1 中的行所对应的列，然后取出来作为一个数组



### `return` 与 `yield` 的区别

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303160406.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303160412.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303160502.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303160537.png)



## 数据容器

### `set`


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152416.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152512.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152520.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152530.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152553.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152601.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152620.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152628.png)

## 报错 

### `raise`

它通过用户在代码中进行检查，并主动抛出错误异常，从而可以更方便发现错误原因

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310150835.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310150845.png)

## 模块，包，导入

### 模块

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163644.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163651.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163657.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163706.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163726.png)

### 包

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163946.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163953.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310164125.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310164132.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310164141.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310164154.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310164224.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310164230.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310164237.png)

### 导入函数模块

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163521.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163527.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163540.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163549.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163556.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310163607.png)

# 内存回收机制

### Py 中的变量和对象

（1）简单来说，py 中的变量只是一个标签，存放的是对象的引用（地址），而对象则是内存中的一块实体，存放着实际的数据
（2）变量有自己的作用域，超过作用域会被回收，而对象则创建在堆上，通过引用计数机制或者垃圾回收机制进行回收

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316114213.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316114232.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316114321.png)

### Python 中的全局变量和类中的属性变量

比如下面这段循环引用的代码

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316114639.png)

简单来说，他们的本质没有区别，都是存放的某个对象的引用（地址），只不过 a, b 是全局变量，他们在全局命名空间中，而 self. ref 是类中的属性，他们在每个类的内存空间中

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316114809.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316114825.png)

所以这里造成循环引用的原因也就很清晰了，虽然删掉了全局空间中的变量，但是每个类对象自己的内存空间中仍然有变量对对方进行引用，而此时类对象又不可访问，这就造成了这两个类对象不可能通过引用计数机制来回收内存了
### Python 中的对象和引用以及函数传参中的引用和别名

（1）Python 中一切皆是对象，对象创建之后会在内存中创建一个区域，并返回这个空间的地址，成为引用
（2）容器对象，比如 list 或者 tuple 存储的是其他对象的引用，而不是对象本身

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316113413.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316113420.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316113439.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316113533.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316113541.png)

### Python 中的引用计数机制

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316113005.png)
```python
import sys

class Person:
    pass

# 对象被创建
p1 = Person()

# getrefcount函数返回的引用计数比实际的引用计数大1，因为getrefcount函数本身也创建了一个临时的引用。
print(sys.getrefcount(p1)) # 2

# 对象被引用
p2 = p1

# sys.getrefcount()函数创建的是临时引用，这个引用在函数返回后就会被立即销毁。所以，即使你多次调用sys.getrefcount()，也不会导致引用计数器持续增加。
print(sys.getrefcount(p1)) # 3
print(sys.getrefcount(p1)) # 3

def log(obj):
    print(sys.getrefcount(obj)) # 5

# 对象被作为参数，传入到一个函数中
log(p1) # 这里注意会 +2, 因为内部有两个属性引用着这个参数
print(sys.getrefcount(p1)) # 3

# 对象作为一个元素，存储在容器中
l=[p1]
print(sys.getrefcount(p1)) # 4

```

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316113302.png)

这里的对象离开它的作用域其实就是对这个对象的引用变量超出了它的作用域之后被自动回收而导致的对象引用计数--

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316115233.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316115243.png)

### py 中的循环引用

比如下面的例子：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260205195147.png)

这里 a, b, c 都是全局变量，他们各自引用了三个对象

同时着三个对象内部也进行了互相引用

这就导致哪怕全局变量 a, b, c 被回收之后，着三个对象的引用计数仍然不为 0，但是同时这三个对象也已经无法访问了，留在内存空耗，这就是循环引用


# 面向对象

## 面向对象基本知识
### 类方法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316220804.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316220811.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316220833.png)


### 静态方法


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316220415.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316220441.png)

### `__new__` 方法

这个 new 构造器实际上就是一个普通的静态方法，只不过它有一个参数 cls，它在 init 方法之前被调用，必须有返回值，这个 cls 参数是 py 自动将当前类传递过去的

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316222835.png)

这里再调用 A () 的时候是 Py 硬编码将类 A 传递给 `__new__` 的第一个参数，虽然效果上与类方法或者实例方法中的自动绑定类或者实例到第一个参数是一样的，但是底层机制不一样（不纠结）






### `__init__` 方法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316223323.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316223328.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260316223339.png)


### `setattr()` 方法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314082353.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314083138.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314083149.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260314083216.png)

### `super()`

这个在子类中调用 `super()` 实际上就是等价于调用子类的父类的意思

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303150944.png)


### py 类的实例属性，类属性和实例方法

#### 实例属性

（1）通常在 `__init__` 方法中通过 self. XXX = xxx 来进行创建
（2）它存储在对象的 `__dict__` 中
（3）在别的方法中使用 `self.XXX = xxx` 也会创建实例属性
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304161239.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304161248.png)

#### 类属性

实例属性与类属性的区别在于，实例属性是每个类的实例独有的，每个类实例都有属于自己的独有的副本；而类属性则是每个实力类共有的，他们共享类属性的副本，某个类实例对类属性进行更改，会导致其他的类属性拿到这个属性的值发生变化

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304161143.png)

#### 实例方法
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304161327.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304161344.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260304161355.png)


## 修饰器

### `@property`

这个修饰器可以将方法变成属性，也就是调用类中的方法的时候可以按照访问属性那样进行

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152116.png)


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310152100.png)


## python 中的魔法方法（前后双下划线特殊方法）

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



### `__init__` 方法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303153113.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303153142.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303153214.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303153227.png)


### `__setattr__` 魔法方法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303150556.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303151019.png)

比如可以通过下面的处理，让在给类变量实例设置数值的时候进行限制：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303151032.png)

这里的 age 和 25，会分别作为参数传递给 `__setattr__` 中 name 和 value

注意这个方法被调用始终是将属性添加到实例的 `__dict__` 属性中，比如下面：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313201339.png)

执行 `l = Layer()` 的时候，进入到 Layer 的 `__init__` 方法中，然后执行 `self._params` 这个就会转到执行下面的 `__setattr_` 方法进行属性设置拦截

这时还没有 `_params` 这个属性，所以会跳过两个 if 语句调用 `super().__setattr__` 这条语句，这里之所以是调用父类的 `__setattr__` 方法，原因是如果调用的是子类的 `__setattr__` 此时会反复进入这个子类实例的 `__setattr__` 进行无穷递归报错了，而调用父类的 `__setattr__` 方法则不会，并且虽然调用的是父类的 `__setattr__` 方法，但是还是会将属性设置到子类实例的 `__dict__` 中

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313201919.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260313201928.png)

### `__call__` 魔法方法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303151525.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303151538.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303151636.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303151653.png)

### `__len__` 方法

它让这个类就像 pthon 内置的类一样可以使用 `len(x)` 这种 python 的基本方法


### 类的 `__dict__` 与实例的 `__dict__` 属性

这里的区别在于 `person = Person()`，这里的 person 是 Person 这类的类实例，而 Person 是类本身

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303152534.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303152543.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303152650.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260303152804.png)

### 迭代器（实现了 `__iter__` 与 `__next__` 魔法方法的对象）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308164953.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308165027.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308165038.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260308165046.png)

# py 八股文

## Numpy

## Python

### 四种基本数据结构

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315203552.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315203521.png)

### return 和 yield 的区别

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315203645.png)

### 深拷贝和浅拷贝

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315203822.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315203830.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315203845.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315203853.png)

### range 函数的用法

（1）返回一系列连续增加的整数
（2）生成一个列表对象

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315204221.png)

### is 和 == 的区别

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315204238.png)

### py 中的表达式和语句的区别是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315204453.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315204501.png)

### 什么是 Lambda 函数

拥有 labmda 关键字，并且只有一行的简洁版函数，这一行只能是一个表达式，不能是语句

比如 `f = lambda x : x`

### 字符串拆分的方法

#### `split` 方法，只能指定一个分隔符

```python
line = "I am super man!"
#String的split方法
print(line.split(" ")) #以空格拆分
输出['I','am','super','man!']

```

#### 函数 re. split () 这个函数允许为分隔符指定正则表达式

```python
#re.split方法
import re 
print(re.split("[m]",line))
输出['I','a','','super','','an!']
```

### 单引号，双引号，三引号

单引号和双引号没有什么区别，都是用来表示字符串的

三行号：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315205053.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315205100.png)

### py 中的传参

#### 必选参数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315205614.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315205621.png)

#### 默认参数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315205636.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315205910.png)

#### 注意事项

（1）函数定义时默认参数必须在必选参数之后
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315211343.png)



（2）函数定义时默认参数的数值就已经绑定了函数，而不是函数调用的时候绑定，因此为了避免错误，默认参数必须绑定不可变对象
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315211355.png)


（3）函数调用的时候可以通过关键字传参跳过默认参数，但是仍然必须保证传参顺序

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315212535.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315212542.png)

### 可变对象与不可变对象

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315212658.png)

### Python 中的装饰器

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315212739.png)

比如 `@property` 装饰器，可以让类的函数像类的属性一样进行调用

### Py 类中变量的访问权限问题
三类
（1）普通命名的变量，外部随意调用
（2）双下划线开头和结尾变量，可以调用，但是一般有特殊的用途
（3）单下划线开头变量，半私有变量，类或者子类中使用
（4）双下划线开头变量，私有变量，只能类的内部使用

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315213147.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315213155.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315213211.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315213540.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315213333.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315213341.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315213626.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315213642.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315213703.png)


### 解释型语言和编译型语言

编译型：Cpp，C 等
解释型：Python

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315213807.png)

### Python 中的 list 和 Numpy 中的 ndarry 的区别

（1）list 元素类型可以不一样，但是 darry 必须一样
（2）list 中每个元素大小可以不一样，因此不支持取出列，但是 ndarry 元素大小一样，支持取出列
（3）list 中存放的是元素的地址，而非数据，ndarry 中存放的只是 4 个数据
（4）一个 ndarry 是内存中一个连续的块，而 list 中存放的是地址，元素本身可能不连续

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260315213951.png)

### init 和 new 的区别是什么？

