
# 普通语法




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


# 面向对象

## 面向对象基本知识
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

