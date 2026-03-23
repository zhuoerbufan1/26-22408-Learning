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

### 虚析构函数及其作用

（1）虚析构函数是基类中用 virtual 关键字修饰的析构函数
（2）它的作用主要是为了解决类似 `Animal* a = new Dog()` 写法，即基类指针指向派生类对象的时候，通过 `deleta` 释放 a 的时候只调用了基类的析构函数，而不调用派生类的析构函数从而导致的内存泄漏问题

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321222645.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321222655.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321222702.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321222714.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321222723.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321222801.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321222810.png)


### 继承中的构造函数与析构函数的调用顺序规则

（1）先初始化基类的构造函数
（2）再初始化派生类的

析构则是完全反过来

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321222235.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321222248.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321222317.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321222342.png)

### this 指针

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321212505.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321212514.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321212527.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321212558.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321212613.png)


### 类与对象

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


### 友元函数是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321210916.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321210933.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321210949.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321211002.png)

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

（1）基类中加上关键字 `virtual` 的函数
（2）子类中通过关键字 `override` 重写的函数
（3）主要作用就是实现运行时多态

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309203102.png)

#### 虚函数表（属于类中的东西）

（1）包含虚函数的类，在编译的时候生成的一个函数指针数组，按顺序存放所有的虚函数地址
（2）子类继承父类的虚函数表，并用自己重写的虚函数地址覆盖掉父类中被重新的虚函数地址：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260309203318.png)

#### 虚函数指针（属于对象中的东西）

（1）每个包含虚函数的类的实例化对象它自己都会有一个虚函数指针指向自己所处类的虚函数表
（2）哪怕是基类指针指向了派生类对象，比如 `Animal* a = new Dog()`，此时这个 Dog 对象（new 出来的）自己的虚函数指针仍然指向自己的派生类 Dog 类的虚函数表

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

# cpp 八股

### 面向过程和面向对象是什么？

面向过程：分析解决问题的步骤，用函数一个个实现，使用的时候一个个调用，比如 c 语言

面向对象：将构成问题的事务分解成一个个对象，建立对象的目的不是为了完成一个步骤，而是为了描述解决整个问题的步骤中的行为，比如 cpp

面向过程适合小型系统的开发，面向对象适合大型软件的开发

### 面向对象的三个基本特征是什么？

封装：将客观事务封装成抽象的类，并向暴露一些接口，比如 private, protect, public 等
继承：继承原来类的所有功能，不用重写原来的类的代码
多态：多态是不同的事务在接收同一事务产生的不同行为

### 面向对象的对象是什么？

其实就是类的实例，是把属性和操作数据的方法封装在一起的基本单元

### 多态的原理以及几个基本概念见上文

### 内联函数是什么？

通过 inline 关键字修饰的函数，核心作用是编译器在调用处直接展开函数体，代替函数调用的过程，主要用来消除函数调用的时候的开销

### 构造函数和析构函数

构造函数：可重载

类的特殊成员函数，名称与类名相同，核心作用是创建对象的时候自动调用完成对象的初始化

析构函数：不可重载

类的特殊函数，名称是~ + 类名，核心作用是对象销毁的时候自动调用，完成资源释放




### 友元函数是什么？

突破类的封装设计而设计的特殊函数，它不是类的成员函数，在类的外部定义，在类中通过 friend 关键字进行修饰，可以访问类中被封装的所有成员

### cpp 中的全局变量和局部变量是否可以重名

可以，在函数内部引用这个变量访问的是局部变量

### cpp 中的栈和堆区别是什么？

栈：系统自动管理的区域，按照先进先出的原则，用来存放函数的局部变量，函数参数，以及函数调用的返回地址，分配和释放由编译器自动进行

堆：动态分配的区域，需要我们通过 new 和 malloc 手动管理，释放的时候也需要 delete 手动进行释放

### new 和 malloc 的区别是什么？

new：
（1）自动计算大小，并返回对应的指针
（2）调用对象的构造函数进行初始化

malloc:
（1）需要手动指定分配的字节数，返回 void* 需要强制转换
（2）仅仅分配原始内存不进行初始化


delete：
（1）会调用析构函数释放内存

free:
（1）仅仅释放内存

new 和 delete 都支持重载，malloc 和 free 是通用的内存分配与释放函数

### cpp 中的 this 指针是什么？

（1）是一个隐含的指针，在类的每一个成员函数中，静态成员函数除外
（2）它指向调用该成员函数的对象本身

### static 关键字

（1）如果是函数内部：则这个变量只会初始化一次（第一次调用函数），生命周期贯穿整个程序，但是作用域仍然仅限于函数内部
（2）如果修饰是类的成员：
	i 如果是变量，属于类本身，而非对象，所有对象共享，通过 Myclass :: count 访问，必须在类的外面定义一次
	ii 如果是函数，属于类本身，而非对象，所有对象共享，通过 Myclass:: function 访问，没有 this 指针

### const 关键字

（1）定义只读的常量，不可被更改
（2）如果修饰变量，则变量必须初始化，不可更改
（3）如果修饰指针，则指针指向的值不可修改或者指针本身的地址不可修改
（4）如果修饰函数，表示函数不会修改类的非静态成员变量

### 指针和引用的区别是什么？

指针是存放地址的变量，可以为空，可重定位
引用是变量的别名，必须初始化，不可能更改

### 指针越界是什么？内存泄漏是什么？

指针越界是指针访问超出合法范围内存
内存泄漏是指内存没有释放但是指针丢失，导致消耗内存

### 深拷贝和浅拷贝是什么？

浅拷贝：只复制对象的引用，新旧对象共享内存
深拷贝：复制对象以及所有嵌套内容，新旧对象之间独立

### 继承和重载的区别是什么？

继承是指子类复用父类的属性和方法，同时可以扩展和重写父类功能

重载是同名函数或者运算符因参数不同实现不同的逻辑，提升调用灵活

### cpp 的封装有哪几类关键字？访问权限分别是什么？

public：大家都可以访问
private: 只能类内部进行访问，子类无法访问
protected: 只能类内部和子类进行访问，外部无法访问

### 运算符重载是什么？

就是赋予已有的运算符的新的功能，本质上重载之后的运算符是一个特殊的函数

### 函数重载是什么？

同一个作用域之下，多个同名但是参数列表不同的函数，编译器会根据传入的参数自动匹配对应的函数，是多态的一种体现

### cpp 中的对象可以按照生存期的不同分为哪些？（4 类）

局部对象：函数内部的对象
全局对象：函数外部定义的对象
静态对象：static 修饰的对象，只初始化一次，生命周期贯穿整个程序
动态对象：new 创建和 delete 释放的对象


### 继承和派生是什么？

继承是子类从父类（或者基类）获得成员的行为
派生是父类（或者基类）创建子类的过程

这两个是同一过程的两种不同的视角

### 什么是多重继承

一个类可以从基类（父类）继承属性的行为，在 cpp 中一个派生类可以同时拥有多个基类

### 虚继承是什么？

（1）它是为了解决比如菱形继承这类可能导致二义性的问题

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321221053.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321221104.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321221135.png)


（2）通过在继承生命中加上 virtual 关键字可以避免派生类中生成多个基类的实例，从而解决菱形继承中的二义性问题

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321221229.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321221235.png)

### cpp 的构造函数有哪几种？

（1）默认
（2）带参
（3）拷贝
（4）委托

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321221408.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321221516.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321221442.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260321221502.png)

### 什么是抽象类和纯虚函数

**抽象类**

（1）抽象类是不能实例化的类，只是提供一个接口
（2）它可以有普通的成员函数，变量，但是必要要有一个纯虚函数

**纯虚函数**

抽象类中声明的虚函数，没有函数体，只有一个 = 0

### 什么是虚析构函数？有什么用？

（1）虚析构函数是基类中用 virtual 关键字修饰的析构函数
（2）它的作用主要是为了解决类似 `Animal* a = new Dog()` 写法，即基类指针指向派生类对象的时候，通过 `deleta` 释放 a 的时候只调用了基类的析构函数，而不调用派生类的析构函数从而导致的内存泄漏问题

### 为什么进行虚析构，但是没有虚构造？

（1）虚函数机制需要虚指针已经存在并且指向一个虚函数表
（2）但是构造函数的任务就是初始化对象，设置虚指针等
（3）假如构造函数设置成虚函数，那么就会出现顺序矛盾，构造函数是虚函数，需要初始化好的虚指针以及虚函数表，但是构造函数本身就是完整这个的，这就会出错

### 哪些函数不能被声明为虚函数？

（1）构造函数，原因如上
（2）不是成员函数的普通函数，一般类似 C 语言中的那种函数
（3）静态成员函数，所有对象共享同一份代码，没必要多态
（4）内联函数，友元函数