# 普通语法

### static 关键字

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311194448.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311194457.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311194512.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311194523.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311194530.png)


## 文件操作

### fopen 函数



![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212104.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212200.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212222.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212231.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212212.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212247.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212322.png)


### fseek 函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212514.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212623.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212635.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212651.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310212658.png)

### fwrite 函数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310213805.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260310213818.png)

这里的 ptr 指针指向实际内存中想要写入到文件中的数据位置

### fread 函数

![](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311201025.png)
![](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311201041.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260311201058.png)

# 面向对象

### 类的对象

这是一个类：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309202916.png)

这是对象：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309202934.png)

### `Animal* a = new Dog()` 写法

这里的 Dog 是继承自 Animal 的类：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309201151.png)

这种写法在编译的时候认为指针 a 指向的是 Animal 类，但是在实际运行的时候指针 a 实际上指向了堆中的 Dog 实例，这里的 Dog 是 Animal 的派生类，Cpp 允许这样的派生转型

这种写法也让运行时多态成为可能

如果是静态多态（编译时多态），如果父类的方法在子类中被重载（比如上图的 speak 不再是虚函数，只是普通的方法，并且在子类中被重载），那么此时 `Animal* a = new Dog()` 虽然在运行的时候指向了 Dog 实例，但是在编译的时候编译器认为 a 就是 Animal 实例，因此通过运行 `a->speak()`，这种编译时多态绑定方法，调用的就只会是父类 Animal 中实现的 `speak()`，而不是子类中重载的 `speak()` 方法

想要达到运行的时候调用的是子类的 `speak()` 方法，只能向上图那样父类将 `speak()` 写成虚函数，然后子类重新虚函数



### 三大特性是什么？

封装，继承，多态

### 封装是什么？

数据和操作打包在一起，通过 public，private, protected 来控制对外的暴露

#### public, private, protected 关键字的作用范围

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309204353.png)

### 继承是什么？

让子类可以拥有另一个父类的属性和行为，可以在原有的基础上进行改进

## 多态

### 虚函数，虚函数表，虚函数指针

#### 虚函数

（1）积累中加上关键字 `virtual` 的函数
（2）子类中通过关键字 `override` 重写的函数
（3）主要作用就是实现运行时多态

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309203102.png)

#### 虚函数表

（1）包含虚函数的类，在编译的时候生成的一个函数指针数组，按顺序存放所有的虚函数地址
（2）子类继承父类的虚函数表，并用自己重写的虚函数地址覆盖掉父类中被重新的虚函数地址：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309203318.png)

#### 虚函数指针

（1）每个包含虚函数的类的实例化对象它自己都会有一个虚函数指针指向自己所处类的虚函数表
（2）哪怕是基类指针指向了派生类对象，比如 `Animal* a = new Dog()`，此时这个 Dog 对象自己的虚函数指针仍然指向自己的派生类 Dog 类的虚函数表

#### 三者完成运行时多态原理

当执行 `a->speak()` 的时候，由于 speak () 是虚函数，此时会读取 a 所指向的 Dog 实例对象中的虚函数指针，这个虚函数指针指向了 Dog 类中的虚函数表，找到这个表之后再从中找到 Dog 类中重写的 speak 函数地址，然后执行这个重新的 speak 函数，实现运行时多态的功能

即运行时绑定，所谓绑定就是根据函数名字决定具体该执行哪个函数


### 纯虚函数是什么？

（1）基类中没有函数体，只有一个 = 0 的虚函数
（2）所有继承这个基类的类都必须强制重写这个虚函数
（3）含有纯虚函数的基类无法被实例化，只能作为一个接口使用

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309205256.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309205307.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309205319.png)

### 多态是什么？

多态就是同一个接口（或者函数），但是有不同的行为

### 编译时多态是什么？

**编译时多态**：编译的时候决定该调用哪个函数，比如函数重载等，它跟每个具体的类是绑定死的，只能通过具体的类来调用：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309200738.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309200744.png)

这里在编译的时候认为 p 就是指向父类 Base 的，因此调用 p->speak () 的时候调用的是父类的方法，而不是子类中重载的方法

### 运行时多态是什么？

通过虚函数，虚函数表和虚函数指针共同实现，如下所示：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309203102.png)

当执行 `Animal* a = new Dog()` 的时候，它通过上文所述的虚函数，虚函数表，虚函数指针来实现运行时绑定

它主要是处理下面的场景：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309205448.png)

如果没有运行时多态，只有静态多态，那么想要画不同的图，则需要区分清楚不同的类实例对象，分别调用他们重载的方法：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309205739.png)

如果实现了运行时多态，则可以创建一个存放基类指针的 `vector<Shape*>` 的数组，然后取出数组元素统一调用他们的 draw 方法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309205935.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309205942.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309205949.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309210000.png)

