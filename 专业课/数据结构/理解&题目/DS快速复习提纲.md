# 复杂度


# 线性表

# 栈，队列，数组

## 栈

### 中缀表达式转后缀表达式和前缀表达式的快速方法是什么？
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250613145618.png)

### 中缀表达式转后缀表达式的栈方法是什么？


==中缀表达式栈模拟转后缀==

从左到右遍历中缀表达式，输出成后缀表达式也是从左到右的构建过程

在遍历过程中：

（1）如果是操作数，直接输出
（2）如果是左括号，则入栈
（3）如果是右括号，则挨个弹出栈顶元素，输出，直到遇到左括号，弹出
（4）如果是操作符，则挨个弹出栈顶操作符，输出，直到栈顶操作符优先级严格小于当前操作符或者遇到左括号（这里是严格小于，转前缀是栈顶大于等于），将操作符入栈
（5）如果遍历完全完全，则弹出栈中所有元素，输出（除了括号）

括号只弹出不输出
### 中缀表达式转前缀表达式的栈方法是什么？


==中缀表达式栈模拟转前缀==

从右到左遍历中缀表达式，输出成前缀表达式也是从右到左的构建过程

在遍历过程中：

（1）如果是操作数，直接输出
（2）如果是右括号，则入栈
（3）如果是左括号，则挨个弹出栈顶元素，输出，直到遇到右括号，弹出
（4）如果是操作符，则挨个弹出栈顶操作符，输出，直到栈顶操作符优先级严格大于或等于当前操作符或者遇到右括号，将操作符入栈
（5）如果遍历完全完全，则弹出栈中所有元素，输出（除了括号）

### 后缀表达式的计算方式？

## 队列

### 循环队列各种类型判空的方法是什么？

判空或者判满主要是假设原始队列为空，然后加入一个元素，分析 rear 和 front 指向从而得到初始为空的情况

==rear 指向最后一个元素==

加入一个元素的代码就是:

```cpp
rear = (rear + 1) % M
q[rear] = value
```

假设一开始为空，那加入一个元素之后要保证 front 和 rear 都指向这个元素，所以 rear 必然在 front 前面一个

所以判空语句就是 `(rear + 1) % M == front`，结果为 true 的时候为空

队列判满，队列满的时候还是保证 rear 指向最后一个元素，front 指向第一个元素，这个时候整个数组不能够全部放入元素，因为这个时候仍然满足 `(rear + 1) % M == front` 与队列空冲突，所以长度为 N 的数组只能放入 N - 1 个元素

所以判满语句就是 `(rear + 2) % M == front`，结果为 true 的时候为满

M 是数组的长度即 N

==rear 指向最后一个元素的下一个元素==

加入一个元素的代码是：

```cpp
q[rear] = value
rear = (rear + 1) % M
```

假设一开始为空，加入一个元素之后要保证 fornt 指向第一个元素，rear 指向第一个元素的下一个元素，所以空的时候 front 和 rear 必然指向同一个位置

所以判空语句就是 `rear == front`，结果为 true 的时候为空

当队列满的时候，假设数组的长度为 N，由于 rear 要指向队尾元素的下一个，所以数组中应该只能放入 N- 1 个元素，如果放入 N 个元素，那此时 rear 和 fornt 指向同一个位置，与判空矛盾了

所以判满语句就是 `(rear + 1) % M == front`，结果为 true 的时候为满

上述 M 就是数组长度 N


==总结：==

判断队空：就是在假设队空的情况下加入一个元素保证指针指向来分析队空的时候的指向，从而判断队空的条件

判断队满：就是在假设队满的情况下，利用此时的指针指向来分析判断队满的条件

取模一定是数组的长度而不是队列的元素长度


## 数组

### 三元组表是什么？

（1）用于存放系数矩阵
（2）表中每个表项有三个元，分别是矩阵中元素的行，列，元素的值
（3）表中按行递增，行相同则按列递增

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251111163928.png)

### 十字链表是什么？

（1）用于存放稀疏矩阵
（2）矩阵中的每个元素都对应一个结点，这个结点有 3 个数据域，和 2 个指针域
（3）数据域分别存放元素的行号，列号，元素的值
（4）指针域的 right 指针指向与这个元素结点同一行的下一个元素结点，指针域的 down 指针指向与这个元素同一列的下一个元素
（5）矩阵中的每行元素，每列元素都会形成一个链表，为了快速定位，每行，每列的链表都会有一个头结点

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251111164958.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251111165006.png)


更复杂的用于存放图的十字链表见后文章节
# 串 (KMP 算法)


### 模式串和主串的概念（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015152918.png)


### 朴素的模式匹配算法代码实现（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015153142.png)

假如模式串的长度是 m，主串的长度是 n，n > m

朴素算法的思想就是按顺序选择主串中长度为 m 的子串，然后与模式串进行比较

如果用双指针的算法来处理就是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015153604.png)

这里 i - j + 2 的含义是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015153651.png)

当不匹配的时候如上图所示，实际上 j 的值就是两个指针往右移动的次数，此时 j = 2，对于 i 来说则是 i 指向了起点（本轮是 3）往后的第 j（2）个单位 4 处

所以 i - j 的意思是让 i 回到起点的前面一个位置，上图的 2 处，然后再 + 2 的含义是 i 再往后移动两个位置到了下一个字串的位置，上图的 4

然后 j = 1，模式串也从头开始
### 朴素模式匹配算法的时间复杂度（见 PPT）

其时间复杂度就是 mn 的



### KMP 算法的代码实现（见 PPT）

```cpp

int index_KMP(SString S, SString T, int next[]){
	int i = 1, j = 1;
	while(i <= S.length && j <= T.length){
		if(j == 0 || S.ch[i] == T.ch[j]){
			i ++;
			j ++;
			
		}else{
			j = next[j];
		}
	}
	
	if(j > T.length) return i - T.length;
	
	return 0;
}
```
j == 0 不是初始条件，而是当模式串的第一个字符与目标串不匹配的时候会让 j 指向 0；进入循环之后 i ++, j ++，此时 j 指向模式串的第一个字符，由于 i 上一次循环指向的字符与模式串的第一个不匹配，所以这一次直接让 i 往后指向了一个字符；两者重新开始匹配

退出循环有两种情况

（1）j <= T.length 时退出循环，说明此时 i > S.length；意味着目标串都遍历完了也没有匹配到，此时匹配失败

（2）j > T.length 时退出循环，只有两者匹配了 j 才会++，当 j > T.length 说明模式串一定全部匹配完了，此时 i 指向主串匹配最后一个字符的下一个位置，减去模式串的长度就是匹配的起始位置

当匹配失败的时候主串中的 i 指针不往后回溯（直接记住），j 指针得往前回溯，或者说 i，j 指针不动模式串得往后滑动，让 j 往前指，j 指向的新位置由 next 数组来确定（直接记住）
### KMP 算法的思想是什么？


KMP 算法规定了一个 `next[j]` 数组，当 `s.ch[i]`，与 `T.ch[j]` 不匹配的时候，不再执行 `i - j + 2`，以及 `j = 1`，而是 i 不动，j 按照 next 数组进行移动（这是规定，直接记住，不用知道为什么），比如下面的例子，规定，这些规定就是 next 数组在代码中的作用，这里不用知道为什么这样规定,下文来说明如果计算 next 数组

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015155129.png)

这里，按照下图，在某一次字串匹配中，当第 5 个元素匹配失败之后，按照规则 i 不动，j 移动到 2，
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015155213.png)

当然也可以看作模式串往右滑动，让 j 指向模式串第 2 个元素，然后继续向右匹配：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015155336.png)

### KMP 算法的复杂度（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015154101.png)


### next 数组的含义（见 PPT）

next 数组就是上文当某次匹配失败的时候 j 移动的规则，即

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015155442.png)

比如上文的 j = 5 的时候是第 5 个元素匹配失败，此时就是 `j = next[j]`，我们事先通过计算 next[5] = 2，所以匹配失败之后 j = next[5] = 2 了
### next 数组的求法（见 PPT）


==next[1]和 next 2]==

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015155655.png)

==其他 next==

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015155744.png)

以下图为例：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015155952.png)

当求 `next[3]` 的时候，以模式串为标准，next[3]就是模式串的第 3 个位置没有匹配上，但是第 1，2 个位置匹配上了，所以就画出上图的情况，上图就是模式串前两个匹配上，但是第 3 个没有匹配上的情形，也可以画成下图：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015160327.png)

然后往右移动模式串，上文情况是模式串移动过了分界线，j 指向了 1，所以 next[3] = 1：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015160450.png)

**再看一种移动模式串没有越过分界线，对的上的情况**

这是第 5 个元素不匹配的时候，需要计算 next[5]，此时前 4 个元素都匹配上的情况：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015160546.png)

往后移动模式串的时候分界线之前的模式串可以对的上了（g 对应了 g）：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015160622.png)

此时 j 指向了第二个元素，所以 next[5] = 2

==再看一个完整的例子==

**（1）next[1]和 next[2]无脑写 0 和 1：**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015160909.png)

**（2）算 next[3]**

此时模式串的前两个字符匹配上了，所以往后移动模式串
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015160938.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015161055.png)
模式串移动过了分界线，j 指向第一个元素，所以 next[3]=1

**（4）算 next [4]**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015161139.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015161148.png)

能对上的时候 j 指向 2，所以 next [4] = 2

**（5）算 next[5]**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015162220.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015162231.png)

这里 j 指向 3 的时候分界线前面的就可以对上了，所以 next[5] = 3

**(6) 算 next[6]**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015162321.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251015162330.png)


### KMP 算法的进一步优化，nextval 数组的求法（见理解）

这里的优化主要是优化 next 数组，将 next 数组替成 nextval 数组，然后上述 KMP 算法的代码不变，用 nextval 数组来更改 j 指针即可

优化的思路是 nextval[1]无脑写零

对于 next[j] = k，它的含义是当模式串的第 j 个字符与主串的第 i 个字符不匹配的时候，就将 j 跳转到第 k 个字符进行匹配，那此时如果第 k 个字符与第 j 个字符一样的话，事实上第 k 个字符也不会匹配上，于是得继续往前跳到 next[k] = l，即跳到第 l 个字符继续与主串的第 i 个字符继续进行匹配

因此对于模式串的第 j 个字符与主串的第 i 个字符没有匹配上之后，应该在 next 数组的基础上，直接跳转到与第 j 个字符不一样的位置上，然后继续向后比较，这个就是 nextval 数组的含义

因此当第 j 个字符与主串的第 i 个字符不相等的时候，查看 next[j]（值为 k），即查看在 next 数组的基础上将要跳转的下一个位置 k，如果第 j 个字符与第 k 个字符相等，那么就应该跳转到 nextval[k]，即 `nextval[j] = nextval[k](nextval[next[j]])`

如果模式串第 j 个字符与第 k 个字符不相等，那么保持不变，即 `nextval[j] = next[j]`

==优化代码如下：==

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250502163637.png)


==举例如下：==

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250502163618.png)


# 树

## 数和二叉树的基本概念

### 分支结点和叶子结点的定义（见课本）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251007190738.png)

### 有序树和无序树的定义（见课本）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251007190816.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251007190823.png)


### 路径和路径长度定义（见课本）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251007190844.png)

这里的路径和路径长度的定义与图中的定义是一样的，毕竟树就是特殊的图

### 结点的深度和高度，树的高度定义（见课本）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251007190914.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251007190823.png)
### 结点的度和树的度的定义（见课本）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251007190930.png)

结点的度，也就是结点延申出来的分支的个数
## 树的基本性质

### 结点数和度数的关系，3个基本方程（见题目）

假设总结点个数是 n，总度数是 m，n 1 表示度为 1 的结点个数，n 2 表示度为 2 的结点个数，n 0 表示叶结点

（1）总结点个数 - 1 = 总度数，即 n - 1 = m
（2）总结点个数  = n 0 + n 1 + n 2 +.....
（3）总度数 = n 1 + 2 n 2 + 3 n 3...

特别的，对于二叉树来说

n - 1 = m；
n = n 0 + n 1 + n 2；
m = n 1 + 2 n 2

根据上面三个方程联立得到：

n 2 + 1 = n 0，也就是二叉树的度为 2 的结点个数 + 1 = 叶结点个数
### 度为 m 的树和 m 叉树的区别（见题目）

说了 m 叉树，但是不一定有结点具有 m 个分支，只能说每个结点最多有 m 个分支

说了树的度为 m，说明这个树中必然有一个结点有 m 个分支

m 叉树不代表一定有结点有 m 个分支！

### 度为 m 的树第 i 层的最多结点数（见课本）

假设根结点在第 1 层

这种情况是每一层的结点都延申出 m 个分支，全部挂满

第 i 层最多 $m^{i - 1}$ 个结点

### 高为 h 的 m 叉树最多的结点数（见课本）

假设根结点在第 1 层

这种情况是每一层的结点都延申出 m 个分支，全部挂满，按照等比数列求和进行计算即可

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251007190541.png)


### 高为 h 的 m 叉树至少有多少个结点; 高为 h 度为 m 的树至少有多少个结点（见 PPT）

m 叉树，那只要树中所有结点的度不超过 m 即可，它的结点个数最少的情况是只每层只有一个结点：

高为 h 的度为 m 的树至少的结点个数，既然这个树的度为 m，那说明必然有一个结点有 m 个分支，那么就在最少结点的 m 叉树基础上找一个结点让其有 m 个分支即可：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251007191128.png)



### n 个结点的 m 叉树的最小高度以及推导（见题目）

高度最小的情况就是所有结点都有 m 个孩子，如果这个树的高度是 h，那么如果想要高度 h 的树的结点个数最大的话，那显然每个结点都必须挂满 m 个子结点，第一层 1 个结点，第二层 m 个结点，第三层 m^2 个结点。。。。。

在这种情况下我们可以列出不等式：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250503145020.png)

得到：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250503145112.png)

即：
$$
\log_{m}(n(m - 1) + 1) \le h < \log_{m}(n(m - 1) + 1) + 1
$$

也就是：$h = \lceil \log_{m}(n(m - 1) + 1) \rceil$，上取整，表示大于等于这个量的最小整数


### 上取整和下取整的符号以及含义（见题目）


 $\lfloor \rfloor$ 是下取整符号，表示不大于自己的最大整数，即 $\lfloor 4.9\rfloor = 4$，$\lceil \rceil$ 是上取整符号，不小于自己的最小整数， $\lceil 4.9 \rceil = 5$，如果是一个整数，不管是向上还是向下取整结果都是数字本身

## 二叉树的定义和基本特性

二叉树的定义和有序性（见课本）

满二叉树和完全二叉树的定义（见 PPT）

满二叉树的三个特点（见 PPT）
	叶子结点特点
	度为 1 的结点特点
	结点 i 的左右儿子编号，父节点编号

完全二叉树的四个特点（见 PPT）
	叶子结点特点
	度为 1 的结点特点
	结点 i 的左右儿子编号，父节点编号
	分支结点和叶子结点编号特点

二叉排序树的定义（见 PPT）

二叉平衡树的定义（见 PPT）

## 二叉树的性质

==二叉树==

与度数和树的结点个数有关的两个基本方程，二叉树的度为零的结点和度为 2 的结点的关系（见 PPT）

二叉树第 i 层最多的结点个数（见 PPT）


高为 h 的二叉树最多有多少个结点（见 PPT）

==完全二叉树==

n 个结点的完全二叉树的高度 h 的两种推导？一种以满二叉树一层最后一个结点做参考，一种以满二叉树一层第一个结点做参考，两种推导方式 h 和 n 满足的不等式关系是什么？（见 PPT）

完全二叉树的度为 1 的结点个数的特点（见 PPT）

完全二叉树 n 0 + n 2 的特点？（见 PPT）
 
完全二叉树根据结点个数 n 计算 n 1, n 0, n 2 怎么计算？（见 PPT）

## 二叉树的存储

==顺序存储==

完全二叉树的顺序存储如何根据结点 i 判断其是否有左孩子，右孩子？如何判断 i 是否是叶结点？（见 PPT）

非完全二叉树的顺序存储特点是什么？（见 PPT）

非完全二叉树的顺序存储最坏情况是什么？（见 PPT）

下标非 1 的数组中二叉树的顺序存储（见题目）

### 下标非1的数组中的二叉树的顺序存储如何与数组下标规律对应起来？

仍然对这些数组中的元素从1开始编号，按照 i 是当前结点，2 i 是左儿子，2i + 1是右儿子这样的规律对应，只不过访问实际数组元素的时候用从1开始的编号 - 1就是实际的下标了

例如下面的数组：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251020182826.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251020182928.png)

这些元素是按照从0 开始的下标顺序存在在数组中的，空缺的位置用-1代替

我们访问这个数组的元素的时候是假想从1开始对这些元素进行编号，比如，对于第k个元素（从1开始），他的左儿子编号就是2k，右儿子编号就是2k + 1，那么我们实际访问第k个元素的时候就是a[k - 1]，访问第k个元素的左儿子就是a[2k - 1]访问第k个元素的右儿子就是a[2k]，直接在从1开始的编号系统下 - 1即可





==链式存储==

### n 个结点的二叉链表中，空指针域的个数（见课本）

## 二叉树的遍历

### 根据前序遍历和中序遍历还原二叉树（见 PPT）

### 根据后序遍历和中序遍历还原二叉树（见 PPT）

### 根据层序遍历和中序遍历还原二叉树（见 PPT）

## 线索二叉树

### 线索二叉树是干什么的？

线索二叉树主要就是为了可以方便的找到树中中序遍历的直接前驱或者后继：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026192545.png)

### 线索二叉树是逻辑结构还是物理结构？

物理结构
### 线索二叉树的结点域结构是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026192631.png)

如果一个结点有左儿子，那么左指针就指向左儿子，如果左指针指向空，那么就将左指针指向直接前驱

### 二叉树线索化的过程是什么？

其实很简单，以中序线索化为例，就是将一个结点的左儿子空指针指向中序遍历的前驱结点，将一个结点的右儿子空指针指向中序遍历的直接后续结点

基本思想就是用一个 p 指针和 pre 指针，在中序遍历的过程中对 p 进行左儿子线索化和 pre 进行右儿子线索化

### 二叉树线索化的代码是什么？中序遍历为例

```cpp

TreeNode* pre = NULL;


void dfs(TreeNode* root){
	if(!root) return;
	
	dfs(root->left);
	
	if(!root->left){
		root->left = pre;
		root->LTag = 1;
	}else LTag = 0;
	
	if(pre && !pre->right){
		pre->right = root;
		pre->RTag = 1;
	}else pre->LTag = 0;
	
	pre = root;
	
	dfs(root->right);
}
```

按照中序遍历进行线索化的二叉树如下：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026194334.png)

但是在选择题中，一般只用知道怎么指即可，可以直接写出遍历序列，然后看看谁是谁的前驱，然后画上箭头即可

### 线索二叉树如何针对一个结点找到它的先序前驱和先序后继？

==找先序前驱==

对于这个结点

（1）如果这个结点的 LTag = 1，那么先序前驱就是左指针指向的结点
（2）如果这个结点的 LTag = 0

这个很复杂，二叉链表无法直接找到当前结点的先序直接前驱，只能重新先序遍历二叉树一遍

==找先序后继==

对于这个结点

（1）如果这个结点的 RTag = 1，那么先序后继就是右指针指向的结点
（2）如果这个结点的 RTag = 0，说明此时一定有右儿子
	i 如果左儿子为空，那么先序后继就是右儿子
	ii 如果左儿子不为空，那么先序后继就是左儿子

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026195553.png)

### 线索二叉树如何针对一个结点找到它的后序前驱和后序后继？

==找后续前驱==

（1）如果 LTag = 1，那么此时左儿子指向的就是后续前驱
（2）如果 LTag = 0，那么此时一定有左儿子
	i 如果右儿子为空，那么此时后续前驱就是左儿子结点
	ii 如果右儿子不为空，那么此时后去前驱就是右儿子结点

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026200028.png)


==找后续后继==

（1）如果 RTag = 1，那么此时右儿子指向的就是后续后继
（2）如果 RTag = 0，那么此时一定有右儿子

此时不好找到后续后继，只能重新后续遍历一遍二叉树

### 线索二叉树如何针对一个结点找到它的中序前驱和中序后继？

如果一个结点的 LTag 或者 RTag = 1，那么此时左儿子或者右儿子指向的就是中序前驱或者中序后继

如果一个结点的 LTag = 0，那么这个结点一定有左子树，此时它的中序前驱就是左子树中最右侧的结点（不一定是最下层，但是一定是最右的结点，这里的右是指一直往右子树延申，而不是空间位置更右的意思）

如果一个结点的 RTag =0，那么这个结点一定右右子树，此时它的中序后继就是右子树中最左侧的结点（右子树中尽可能往左子树延申的结点）


## 树的存储结构

### 树的顺序存储是怎样的？

### 树的双亲表示法存储是怎样的？代码是什么？

### 森林是怎样通过双亲表示法存储的？

### 双亲表示法的优缺点是什么？应用场景是什么？

### 树的孩子表示法存储是怎样的？代码是什么？

### 森林的孩子表示法存储是什么？

### 孩子表示法的优缺点是什么？

### 树的孩子兄弟表示法的是什么？代码是什么？

### 森林的孩子兄弟表示法是什么？如何存储的？

## 树，森林，二叉树的转换

### 树如何转换成二叉树？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910120429.png)

对于树中的每一个结点，左指针指向自己的**第一个儿子**，右指针指向自己的兄弟，即左儿子右兄弟

### 森林如何转换成二叉树？
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910120702.png)

### 二叉树如何恢复成树？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251022160322.png)

### 二叉树如何恢复成森林？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251022160357.png)


## 树和森林的遍历

### 树的先根遍历的代码和过程是什么？

### 树的先根遍历与转换成二叉树的遍历有什么联系？

### 树的后根遍历的代码和过程是什么？

### 树的后根遍历与转换成二叉树的遍历有什么联系？

### 森林的先序遍历过程是什么？

### 森林的先序遍历与树的遍历联系是什么？

### 森林的先序遍历与转换成二叉树的遍历有什么联系？

### 森林的中序遍历过程是什么？

### 森林的中序遍历与树的遍历联系是什么？

### 森林的的中序遍历与转换成二叉树的遍历有什么联系？

## 哈夫曼树

### 结点的权定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028081825.png)


### 结点的带权路径长度定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028081836.png)


### 树的带权路径长度 WPL 的定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028081844.png)


### 哈夫曼树（最优二叉树）的定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028081856.png)


### 哈夫曼树的构造过程

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028081923.png)


### 哈夫曼树的叶结点特性

每个初始结点都将成为叶结点，并且结点的权值越小，离根结点的距离越大

### 哈夫曼树的结点总数特点

最终哈夫曼树的结点总数一定是 2 n - 1

### 哈夫曼树的度为 1 的结点特点

哈夫曼树中没有度为 1 的结点

### 哈夫曼树的唯一性

哈夫曼树不唯一，但是树的最小带权路径长度一定唯一


### 固定长度编码和可变长度编码的定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028082313.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028082333.png)


### 前缀编码的定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028082420.png)


### 对字符集进行哈夫曼编码的过程

对于一个字符集 a，b，c，d，e...。

假设有下面一串字符 aabbccdddeeeeee

将字符出现的频度作为字符结点的权值，然后按照构造哈夫曼树的过程构造一个哈夫曼树，从这棵树的根结点开始，往左走就是 0，往右走就是 1，然后从根结点到叶结点的路径就是这个叶结点字符的编码了

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028082457.png)
### k叉哈夫曼树的性质

（1）k叉哈夫曼树中只会有叶结点和度为k的结点，即n0和nk

（2）根据度和结点总数的关系有n0 + nk = k * nk + 1

### k 叉哈夫曼树如何进行合并？

根据上文，nk = (n0 - 1) /(k - 1)

由于nk的个数是整数个，所以n0- 1必须可以整除k - 1，这里的n0就是最开始的结点，如果给定的这些结点个数-1不能整除k-1，那就得加上权值为0的虚结点

然后按照 k 个结点 k 个结点这样合并即可，与哈夫曼树类似，哈夫曼树是 2 个结点 2 个结点合并

## 并查集

### 并查集的并操作原理是什么？

### 并查集的查的两个基本操作原理是什么？

### 双亲表示法的代码是什么？

### 并查集的存储结构是什么？初始化代码是什么？

### 并查集的查操作（找到元素所属的集合）的原理和代码是什么？

### 并查集的并操作（合并两个集合）的原理和代码是什么？

### 对并查集并操作的优化是什么？代码是什么？

### 并查集查找操作的优化是什么？（路径压缩）代码是什么？


# 图

## 图的基本概念

### 图的定义，图的顶点集和边集的特点（空或者非空）
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910122031.png)
顶点集不能为空，但是边集可以为空
### 有向图和无向图
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910122001.png)

### 弧尾和弧头的概念
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910122126.png)

弧头就是箭头方向
### 简单图和多重图的概念

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910122139.png)


### 无向图顶点的度，顶点的入度和出度的概念，有向图顶点的度

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910122155.png)

在无向图中，按照边来分析，一个边贡献了两个度，所以总度数就是边的两倍

在有向图中，按照边来分析，一个边贡献了一个入度和一个出度，所以总入度 = 总出度 = 边的个数
### 无向图和有向图中顶点的度之和与边的条数的关系

在无向图中，按照边来分析，一个边贡献了两个度，所以总度数就是边的两倍

在有向图中，按照边来分析，一个边贡献了一个入度和一个出度，所以总入度 = 总出度 = 边的个数

### 路径和回路、简单路径和简单回路，路径长度，点到点之间的路径的概念，连通与强连通


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910122828.png)

连通是无向图中的概念，强连通是有向图中的概念
### 连通图和强连通图的概念

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910122858.png)


### 连通图最少边的个数

连通图 N 个顶点，最少的边个数是 N - 1 条



### 非连通图最多边的个数

隔离一个顶点出去，剩下的顶点两两之间都有边

所以边最多就是 :

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910123112.png)


### n 个顶点的有向图，保证强连通最少边数是多少？

要形成一个环，所以是 n 条边

### 子图的概念

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024192346.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024192442.png)


### 生成子图的概念

生成子图就是保证顶点和原来的图顶点一样，边是原来的图的子集

### 连通分量的概念（包含两点）


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024192506.png)


	是一个连通的子图
	这个子图尽可能的大

### 强连通分量的概念（包含两点）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024192530.png)


	是一个强连通的子图
	这个子图尽可能的大

### 连通图的生成树的概念

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024192614.png)


	包含全部顶点
	是连通的
	边尽可能的少（固定个数）

连通图的生产树的边的个数固定是 n - 1
### 连通图生成树的边的个数

固定是 n - 1

### 生成森林的概念


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024192712.png)

	由非连通图的各个连通分量生成的生成树构成

### 边的权和带权图

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024192737.png)


### 图的带权路径长度概念

一条路径上的所有边的权值之和

### 无向完全图的概念，边的个数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024192821.png)


### 有向完全图的概念，边的个数

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024192838.png)


### 稀疏图和稠密图的概念


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250930170450.png)

稀疏和稠密是在顶点个数一定，考察的边的情况下的概念，稀疏就是边很少
### 树和图的关系，n 个结点的树的边的个数

树是没有回路而且连通的无向图

n 个顶点的树，它的边的个数必为 n - 1

### 图的边数大于什么了有回路？


因此对于一个无向图来说，如果边的个数 > n- 1，则一定会出现回路

## 图的存储 - 邻接矩阵和邻接表

### 无向图的邻接矩阵是怎么存储的

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250930165731.png)


### 有向图的邻接矩阵是怎么存储的

![](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250930165731.png)
### 邻接矩阵的代码实现

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250930165752.png)


### 邻接矩阵法的空间复杂度是多少，使用场景是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250930170053.png)

因为邻接矩阵的空间复杂度为固定的 n 平方，如果图中的边太少的话，很多空间都浪费了，所以用来存放稠密图这 n 方个空间才能得到充分利用

### 无向图怎么根据邻接矩阵求顶点的度


对于顶点 v 来说

遍历邻接矩阵 A【v】【i】，i 从 0 到 n，将全部等于 1 的值加起来

### 有向图怎么根据邻接矩阵求顶点的度

对于顶点 v 来说

遍历第 v 行，算出所有入度
遍历第 v 列，算出所有出度

### 邻接矩阵的 $A^n[i][j]$ 表示的含义是什么？


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250930170529.png)

### 图的邻接表存储是什么？代码是什么？
见代码汇总


### 邻接表的空间复杂度是多少，使用场景是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250930172847.png)

对于一条边来说，比如边 A - B，它在顶点 A 引出的边链表中，会存在一个边结点结构，这个结点指向的顶点是 B；同理它在 B 顶点引出的边链表中也会对应一个边结点，这个结点指向的顶点是 A

所以对于一个图中，如果有 K 条边，那么在邻接表中对应的边结点结构有 2 K 个，同时如果有 N 个顶点，所对应的顶点个数是 N 个

所以整个邻接表的空间大小就是 2 K + N
### 无向图的邻接表存储中，边结点个数与实际边的个数的关系

实际边的个数是|E|个，每个边会对应两个边结点，比如 A-B，对应两个边结点，分别在 A 和 B 头结点引出的两个链表中，所以边结点的个数实际上是实际的边的个数的 2 倍

### 无向图怎么根据邻接表存储找到顶点的度

遍历这个顶点结点引出的边链表即可

### 有向图怎么根据邻接表存储找到顶点的出度和入度

遍历这个顶点 A结点引出的链表来找到出度

遍历其他所有结点引出的链表，看看这些边结点是否包含顶点 A 来找到入度（得遍历所有的边结点）

### 邻接表和邻接矩阵的表示方式唯一吗？

邻接表的表示方式不唯一，因为每个顶点作为头结点引出的边链表的顺序是任意的

邻接矩阵的表示方式是唯一的

## 图的存储 - 十字链表
### 十字链表存储的是什么图？

有向图

### 十字链表的顶点结点结构和含义是什么？

#### 顶点编号

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030112452.png)

这里顶点编号其实完全可以看成图的顶点数组中的下标

#### 顶点数据域

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030112554.png)

数据域用于存放顶点中的数据

#### firstin 域

这个域指向的是一个弧结点，这个弧结点的弧以当前顶点结点作为弧头，这里严格来说是指向第一个以这个顶点为弧头的弧结点，但是一般第一或者第二没有什么明显区分，所以这里直接随机指向一个以这个顶点为弧头的弧结点，比如编号 0 的顶点它作为了两个弧的弧头即 3 -> 0 和 2 -> 0，这里的 firstin 指向弧 3->0 或者弧 2->0 其实都行：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030112945.png)

#### firstout 域

这个指针域与 firstin 指针域是完全相反的，它指向的是一个以当前顶点为弧尾的弧结点，比如上图中对于编号为 0 的顶点，有 0 -> 1 和 0->2 两条以 0 号顶点为弧尾的弧结点，所以这里的 firstout 域就会指向其中一个弧结点

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030113230.png)


### 十字链表的弧结点的结构和含义是什么？

#### tailvex 域

这个域中存放的是这个弧的弧尾结点编号

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030113345.png)

#### headvex 域

这个域存放的是这个弧的弧头结点编号，tailvex 域和 headvex 域其实就可以唯一确定一条弧了

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030113432.png)

#### hlink 域

这个域指向的是一个弧结点，是与当前弧结点相同的下一条弧结点，比如上图中的 0->1 弧，与他弧头相同的是 3->1 弧，所以这个域就会指向 3->1 弧：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030113615.png)

#### tlink 域

这个域指向的是弧尾相同的下一条弧，比如上图的弧 0->1 与它弧尾相同的是弧 0->2，所以这个域就指向弧 0->2

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030113729.png)


### 用十字链表存放一个图的过程

#### 先画图中的所有顶点结点

标出顶点编号以及顶点中的数据，这一步将所有的顶点全部画出来：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030113911.png)

#### 按照辅助矩阵的方式将弧结点放好

这里只用将弧结点中的两个顶点编号域写好即可，按照这个顶点编号用矩阵的方式进行存放：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030114455.png)

#### 开始连接弧结点中的 hlink 和 tlink

每个弧结点的 hlink 指向的是弧头相同的下一条弧结点，tlink 指向的是弧尾相同的下一个弧结点

如果按照上文矩阵方式存放话，**同一行的弧结点的弧尾都是相同的，同一列的弧结点的弧头都是相同的**，这样连线起来就非常方便了

因此下图绿色箭头就是连接同一行的 tlink，下图蓝色箭头就是连接同一列的 hlink：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030114949.png)


这些弧结点每一行或者每一列的最后一个结点就将它的 tlink 或者 hlink 置空

#### 连接顶点结点的 firstin 域

顶点的 firstin 域指向的是以这个顶点为弧头的第一个弧结点，对于顶点编号为 0 的顶点来说，它 firstin 指向的就是第 0 列的某个弧结点：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030115208.png)

同理，对于编号为 1 的顶点，它的 firstin 域指向的就是一个以编号为 1 为弧头的弧结点，因此编号为 1 的顶点的 firstin 域指向的就是第 1 列的某个弧结点：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030115428.png)

于是，按照这个规律将所有的顶点结点的 firstin 域连接起来即可：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030115504.png)

#### 连接顶点结点的 firstout 域

每个顶点的 firstou 域指向的是一个以这个顶点为弧尾的某条弧，也就是这个顶点对应的行的某条弧，按照这个规律连接即可：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030115620.png)

### 十字链表存储一个图的优点是什么？

对于普通的邻接表来说，知道一个顶点的出度是很容易的，只用通过顶点直接遍历这个顶点引出的所有边结点即可，但是知道这个顶点的入度则需要遍历整个邻接表的所有边结点才行

但是对于十字链表来说，知道一个顶点的入度，直接从顶点的 firstin 指针域往后遍历，找到一个指向这个顶点的弧，然后再从这个弧出发通过 hlink 找到与这个弧弧头相同的下一条弧，也就是指向这个顶点的下一条弧，这样只用遍历这个顶点的 firstin 和后续的 hlink 引出的一个链表就行了

同理对于十字链表，知道一个顶点的出度，直接从顶点的 firstou 指针域往后遍历，找到一个以当前顶点为弧尾的弧结点，然后再从弧结点的 tlink 出发往后遍历，这样同样只用遍历这个顶点的 firstout 和后续的 tlink 引出的一个链表就可以了

这样就不用遍历所有的边了


## 图的存储-邻接多重表

### 邻接多重表存的是什么图？

无向图，它与十字链表相对应

### 邻接多重表的顶点结构是什么？

#### 数据域

这个存放顶点的数据部分

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030144151.png)


#### firstedge 域

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030144249.png)

这个指向第一条依附于这个顶点的边

### 邻接多重表的边结构是什么？
#### ivex 域

这个存放这个边结点依附的第一个顶点

这里存放的是无向图，所以没有弧的概念，这里的 ivex 随便存放这个边依附的一个顶点即可

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030144518.png)


#### ilink 域

这个域与 ivex 是配对的，它指向下一条依附于 ivex 的边
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030144906.png)

#### jvex 域与 jlink 域

jvex 域与 ivex 域是一样的，存放这个边依附的另一个顶点

jlink 域指向下一条依附于 jvex 这个顶点的边

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030145107.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030145118.png)



### 邻接多重表存放无向图的过程

#### 首先画出所有的顶点

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030145229.png)

#### 然后画出所有的弧结点，按照 ivex 相同的弧结点放在相同的行即可，不用按照十字链表那样按照矩阵排布

这里的无向图的每一个边都应该对应一个弧结点，弧结点的 ivex 和 jvex 存放顺序无所谓，比如 0 - 1 这个边，ivex 和 jvex 可以分别存放 0 1，也可以分别存放 1 0：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030145525.png)

#### 连接顶点的 firstedge 域以及顺便连接弧结点中的 ilink 或者 jlink 域

顶点的 firstedge 域只用连接一个依附于它的边即可，只要这个边的 ivex 或者 jvex 是这个顶点，那顶点的 firstedge 就可以连上这个边

然后顺着这个边，连上这个边的 ilink 或者 jlink，将所有依附于这个顶点的边串起来即可，比如从顶点 0 开始串起来：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030150315.png)

按照这样的规律将每个顶点以及依附于这个顶点的边全部串起来即可：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251030150415.png)

### 邻接多重表的好处是什么？

在普通邻接表存储一个无向图的时候，如果我们想要删除 0 - 1 这样一个边

那么我们就得现在顶点 0 引出的边链表中删除这个边

同时还得在顶点 1 引出的边链表中删除这个边

我们得从两个顶点出发遍历两次才能删掉这个边

但是在邻接多重表中，我们可以任意从 0 号顶点或者 1 号顶点出发都可以找到这个边，直接删掉即可

**邻接多重表的根本优势就在于无向图中的每一个边，邻接多重表始终都是用一个边结点来表示的，并且从任意一个顶点出发都可以将依附于这个顶点的边串起来，相当于不同的顶点串起来的边链表复用了；而在普通的邻接表中，无向图的一条边是用两个边结点来表示的，分别处在两个不同的顶点引出的边链表中**
## 图的基本操作


## 图的遍历

图的广度优先思想（三点）

图的广度优先搜索代码实现

在邻接矩阵和邻接表上的 BFS 有什么区别

邻接矩阵和邻接表 BFS 的空间复杂度

BFS 在邻接矩阵上的时间复杂度

BFS 在邻接表上的时间复杂度

用邻接表存储的图通过 BFS 生成广度优先生成树的过程

广度优先生成森林的概念



图的 DFS 代码实现

DFS 的空间复杂度

DFS 在邻接矩阵和邻接表上的时间复杂度

根据具体的邻接表或邻接矩阵的存储而得到的 DFS 序列过程（注意是一个确定的过程，也是一个确定的序列，因为存储是确定的，而代码也是确定的）

基于邻接表和邻接矩阵存储得到的 DFS 的序列有什么区别？（图是固定的，但是邻接表存储不是唯一的，所以邻接表存储发生变化之后 DFS序列不唯一，但是邻接矩阵唯一）

用邻接表存储的图通过 DFS 生成广度优先生成树的过程（访问完顶点 A 之后，如果能从顶点 A 访问顶点 B，那么 A，B 之间的边就保留作为生成树的一条边，BFS 也是这样的过程）

根据不同的邻接表存储的生成树的唯一性（不唯一）

图的连通性与 BFS/DFS 调用次数的关系

图的强连通性与 BFS/DFS 调用次数的关系 

深度优先生成森林的概念

## 最小生成树

最小生成树的定义

最小生成树的研究对象

Prim 算法的具体过程，以及详细实现过程（伪代码）

Kruskal 算法的具体过程

Prim 算法和 Kruskal 算法的时间复杂度

Prim 和 Kruskal 算法适用的图的对象

Kruskal 算法适用并查集的思想过程


## 最短路径 - BFS 算法

单源最短路径的定义

BFS 算法使用的场景

BFS 算法的具体代码

## 最短路径 Dijkstra 算法

### 图的带权路径长度定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250930193953.png)

### Dijkstra 算法的过程

```c

//一个 dist数组保存源点到其余各个节点的距离，dist[i] 表示源点到节点i的距离。初始时，dist数组的各个元素为无穷大
 int dist[n];
 int state[n];
 dist[1] = 0
 //state[i] = 1表示点i进入了集合S中，表示这个点的最短距离已经找到
 //state[i] = 0表示点i仍在集合T中，表示没有确定最短路径的节点中距离源点最近的点
 for(i:1 ~ n)
 {
     t <- 没有确定最短路径的节点中距离源点最近的点;
     state[t] = 1;
     //更新此时t可以到达的所有顶点j的dist[j]，如果t->j + dist[t] < dist[j]，说明更新完了t之后可以源点可以从t再到j比原来的路径更短，所以更新，否则不更新
     
     for(对于t可以到达的所有顶点j){
     	if(dist[t] + t->j < dist[j]) 更新t->j的dist[j] = dist[t] + t->j;
     }
     
 }

```

### Dijkstra 算法的复杂度

妥妥的 n^2

### Dijkstra 适用的场景（可以处理带环图，不能处理有负权的图，也不能处理带负权环路的图）

总的来说 Dijkstra 不能处理带负权的图
## Floyd 算法

Floyd 算法的 DP 思想

Floyd 算法的代码

Floyd 算法中矩阵 `A[i][j]` 和矩阵 `path[i][j]` 的更新原理和过程

Floyd 算法适用的场景（可以处理带环图，可以处理负权图，不能处理带负权回路的图）

Floyd 算法的时间复杂度

## 有向无环图 DAG 来描述表达式

有向无环图 DAG 的定义

将表达式树合并成 DAG 的详细过程

## 拓扑排序

### AOV 网的定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024080619.png)


### 在 DAG 图中输出拓扑排序的过程

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024080648.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024080704.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024080717.png)

### 拓扑排序判断环的原理

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024080758.png)

也就是拓扑排序输出到最后面，图不为空，说明图里面有顶点，并且没有入度为0的顶点

那就说明整个图出现了环路

### 拓扑排序的代码实现（两种 BFS 和 DFS）


### 拓扑排序代码的时间复杂度

### 逆拓扑排序的定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024081007.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024081025.png)

### 逆拓扑排序的代码实现

### 逆拓扑排序的 DFS 代码实现

### 根据 DFS 拓扑排序判断环的代码

## 关键路径

### AOE 网的概念（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922193226.png)


### AOE 网的两个性质（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922193236.png)


### 关键路径和关键活动的概念（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922193315.png)

### 活动的最早开始时间和最晚开始时间是什么？

活动是指图上的弧，这个活动的开始时间与弧的起点时间和终点时间有关

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922194131.png)

### 事件的最早开始时间与最晚开始时间是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922194259.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922194307.png)

### 活动的时间余量（见 PPT）活动的时间余量与关键路径的关系是什么？


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922194335.png)

也就是活动的最晚开始时间 - 最早开始时间
### 求事件最早发生时间的过程（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922194539.png)

也就是对于一个顶点 k，它的最早开始时间与所有指向它的顶点 j 的最早开始时间有关

具体来说，就是所有指向它的顶点 j 的最早开始时间加上边的权值；这些数字中最大的那个是 k 的最早开始时间

很显然，只有最大的那个边的活动完成之后，这个事件才能发生

### 求事件最迟发生时间的过程（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922194954.png)

这个过程记住即可

### 求活动的最早开始时间的过程（见 PPT）


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922195045.png)


### 求活动的最晚开始时间的过程（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922195138.png)


### 求活动的时间余量的过程（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922195158.png)


### 求关键活动的过程（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922195228.png)

### 求关键路径上的事件的另一种做法

其实只要对于一个事件，有ve[i] = vl[i]，那么这个事件就是关键路径上的事件
### 关键活动和关键路径的特性（4 条）（见 PPT）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251024172807.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250922195413.png)

延长某个关键路径上的关键活动，则整个工期一定延长，因为关键路径就是从源点到汇点的最长路径，你这里延长了，整个关键路径一定延长

但是缩短了某条关键路径上的关键活动时间，整个工期不一定缩短，因为另一个关键路径可能时间还是比它长

### 多源点多汇点的AOE网络关键路径求解（求解AOE 网络关键路径的加强版算法）

多源点多汇点可以通过加一个超级源点（连接多个源点的边权为0）、一个超级汇点（连接多个汇点的边权为0）的方式转换为单源点和单汇点

在这种情况下，增加了超级汇点和超级源点的AOE网表达的含义和原来的AOE网所表达的含义就是一样的，求增加了超级汇点和超级源点的AOE网的关键路径过程，就是求原来的多源点多汇点的图的关键路径的过程，这两者等价

对于带超级源点的图来说，它的超级源点的ve[vb] = 0，所以对于原图中的多个源点，这些源点只会有超级源点指向它，所以这些多个源点的ve[i] = 0（超级源点的 ve[i]  + 边权 0）

**所以对于带多个源点的图，我们应该手动模拟带超级源点的最开始几步的过程，即，将这些汇点的ve[i] 全部设置成0，然后按照拓扑序列的顺序求ve[i]数组**

对于带超级汇点的图来说，最后一步，求超级汇点的ve[i]，实际上是ve[i]数组中的最大值 L，然后加上边权0，因为是多个汇点指向超级汇点，按照算法一定是多个汇点的 ve[i] + 边权 0 取大的结果，所以超级汇点的 ve[i]一定 = 0；然后求解数组vl【】，在求解vl【】的最开始几步，是先将超级汇点的vl[i]设置成它本身的ve[i]，然后求解所有指向超级汇点的点的vl[i]，这些点实际上是原图中的多个汇点，这些汇点的vl[i]，在带超级汇点的图中，按照求解算法，会被全部设置成 L

**所以对于带多个汇点的图，我们应该手动模拟带超级汇点的最开始几步求解vl【】的过程，即将这些多个汇点的vl[i]全部设置成L，然后按照逆拓扑序列的顺序求解 vl 数组**

==代码实现==

这个代码的目的就是求解关键路径上的顶点的个数（事件的个数），我们默认在这个图上加了一个超级源点和超级汇点，得到了一个等价的 AOE 网，并模拟了这个等价的 AOE 网求解 ve 和 vl 的前几步过程

此外，如果一个事件的最早开始时间和最晚开始时间相等的话，则这个点就是关键路径上的点

[晴问算法](https://sunnywhy.com/camp/3415/model/4144?itemId=3417)

```cpp
/**
 * @param G: 邻接矩阵，表示有向无环图，按二维数组的方式用下标即可访问内部元素
 * @param n: 图中顶点的数量
 * @param p: 图的拓扑序列，长度为n，包含0到n-1的所有整数
 * @return: 返回一个整数，表示图的关键节点数量
 */

int countCriticalNodes(int** G, int n, int p[]) {
    int ans = 0;
    //首先求拓扑排序中每个事件的最早发生时间
    int ve[n];
    
    //模拟带超级源点的图的前几步过程，这里将所有点的ve全部设置成0
    //这样不仅多个源点的ve被设置成了0，其他内部点的ve也全部设置成了0，但是内部点设置成0不要紧
    //按照算法内部点一定有一个点指向它，所有指向它的点 + 边权取大之后是结果，这里0就是最小一定会被更新
    //所以一定可以得到内部点的ve的正确结果
    for(int i = 0; i < n; i ++) ve[i] = 0;
    int L = 0;

    for(int i = 0; i < n; i ++){
        int v = p[i];
        for(int j = 0; j < n; j ++){
            if(G[j][v] != 0){
                ve[v] = max(ve[v], ve[j] + G[j][v]);
            }
        }
        L = max(L, ve[v]);
    }

    //计算每个事件的最晚发生时间
    int vl[n];
	
	//模拟带超级汇点的求解vl数组的前几步过程，这里将所有点的vl全部设置成1
	//这里不仅将多个汇点的vl设置成了L，其他内部点的vl也设置成了L，但是内部点设置成L不要紧
	//内部点一定会指向一个点，按照算法，所有它指向的点的vl-边权取小之后就是结果，这里L就是最大的一定会被更新
	//所以一定可以得到内部点的vl的正确结果
    for(int i = 0; i < n; i ++) vl[i] = L;
    for(int i = n - 1; i >= 0; i --){
        int v = p[i];
        for(int j = 0; j < n; j ++){
            if(G[v][j] != 0){
                vl[v] = min(vl[v], vl[j] - G[v][j]);
            }
        }
        if(ve[v] == vl[v]) ans ++;
    }
    return ans;

  
  

}
```


# 查找

## 查找的基本概念

静态查找表和动态查找表的概念

查找长度与平均查找长度的概念

## 顺序查找

查找判定树的画法

查找判定树分析 ASL 的两个结论

顺序查找成功的平均查找长度

顺序查找失败的平均查找长度

有序顺序表顺序查找成功的平均查找长度

有序顺序表顺序查找失败的平均查找长度

关键字查找概率不相等的情况下顺序查找的优化方法


## 折半查找（二分查找）

### 二分的适用条件

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250928172051.png)

### 二分的模板 (Acwing 内容)



### 二分查找的代码（408 DS 中的内容）



### 二分查找判定树的构造，构造树的过程中的两个结论（mid 向下取整）

下面的 mid 以及 low 以及 high 都是下标的含义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250928172250.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250928172227.png)



### 二分查找判定树的构造，构造树的过程中的两个结论（mid 向上取整）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250928172831.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250928172838.png)

### 二分查找树的构造过程（按照 mid 下取整的方式）

这里可以不用老实计算 mid 的具体数值

mid 下取整，如果整个结点的个数是奇数个，则等分
如果整个结点的个数是偶数个，则左边结点个数比右边结点个数少 1
按照这个规律直接划分结点即可

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028083144.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028083441.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028083449.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251028083500.png)


### 二分查找成功和失败的 ASL 的计算（一颗具体的树的计算方式）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250928173012.png)

在这颗树中，给出一个结点查找成功是查找到非叶结点上面，这里一共 11 个非叶结点

如果查找 29 成功，那就是 1 次，他对 ASL 的贡献就是 1 * 1/11

如果查找 13，37 成功，这两个元素的查找次数分别都是 2 次，它们对 ASL 的贡献就是分别 2 * 1/11

如果查找第三层的元素成功，这些元素对 ASL 的贡献就是 3 * 1/11

如果查找第四层的元素成功，这些元素对 ASL 的贡献就是 4 * 1/11

所以查找成功 ASL 的计算就是 1/11 (1 + 2 * 2 + 4 * 3 + 4 * 4) = 3

如果是查找失败，则是查找到方形结点，不过查找到方形结点本身不算一次次数：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250928173800.png)





### 二分查找判定树与平衡二叉树的关系


二叉查找树判定树只可能最后一层没有铺满，所以其一定是一个平衡二叉树

### 二分查找判定树与二叉排序树的关系

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250928174433.png)

二叉查找判定树一定是二叉排序树

### 二叉判定树的失败结点的个数（空链域的个数）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250928172632.png)


### 折半查找的时间复杂度，折半查找查找成功和失败的最多查找次数

折半查找不会超过树的高度，因此时间复杂度就是 O (logn) 级别


查找失败就是查找到空结点，最多查找次数就是树的高度

查找成功最多就是查找到空结点的上一层

### n 个元素的折半查找，等概率情况下的平均查找长度（大约是多少）（见教材）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250928180228.png)

### n 个元素的二分查找判定树的高度 h 的表达式

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250928174525.png)

### n 个元素二分查找判定树，查找失败的时候的最多比较次数

查找失败就是查找到空结点，注意空结点不用再比较一次，所以比较最多次数就是树的高度次

最多比较次数就是查找道最后一层了

所以是树的高度 h 次

## 分块查找

### 分块查找的结构特点

### 分块查找的基本思想

### 关键字索引块的关键字特征

### 二分查找关键字索引块的基本思想，low， high 指针的最后指向规律（必须停在 low > high 的时候）

### 二分查找关键字索引块为什么最后要在 low 指向的索引块中查找

### 分块查找顺序查找索引的查找成功次数计算

### 分块查找二分查找索引的查找成功次数计算

### 分块查找的查找成功 ASL 的计算（两种查找索引方式）

### 均匀分为 b 块，每块 s 个元素顺序查找索引的查找成功 ASL 计算（b 个索引块的平均查找长度 + 块内 s 个元素的平均查找长度）

### n 个元素均匀分为 b 块，每块 s 个元素顺序查找的 ASL 最小的情况

### 顺序表的分块查找在动态查找中的缺点，以及解决缺点的方法


## 二叉排序树（二叉查找树 BST ）

### 二叉排序树的定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251020165022.png)


### 二叉排序树的中序遍历的特点

![](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251020165022.png)



### 二叉排序树的构造操作

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103094526.png)


### 二叉排序树的删除操作（三种情况）


**删除结点是叶结点：直接删除**

 **删除结点只有左子树或者右子树：子树上移直接替换这个结点：**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103094809.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103094858.png)


**删除结点有左子树和右子树**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103094936.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103094954.png)

或者直接前驱：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103095045.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103095055.png)

 ### 一颗二叉排序树最好情况的最多查找次数是多少

一颗二叉排序树的最多查找次数就是树的高度

最好情况，就是 n 个结点的二叉树的最少高度，也就是 $h = \lceil \log_{2}(n + 1) \rceil$

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103100026.png)

### 一个二叉排序树最坏情况的最多查找次数是多少

最坏情况，排成了一个链条：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103100052.png)


### 二叉排序树最好情况下的查找复杂度量级是多少

O (logn)

### 二叉排序树最坏情况下的查找复杂度量级是多少

O (n)

 ### 二叉排序树最好情况下的平均查找长度量级是多少

O (logn)

### 二叉排序树最坏情况下的平均查找长度量级是多少

O (n)

### 二叉排序树平均查找长度的计算（给定一个确定的二叉排序树的平均查找长度的计算方法）


其实就是每棵树的结点的高度乘这个结点的概率：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103102523.png)


## 平衡二叉树（AVL）

### 平衡二叉树的定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026205735.png)


### 结点的平衡因子

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026205750.png)


### 平衡二叉树插入结点后不平衡的调整对象

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026205920.png)


### LL 插入是什么，以及 LL 插入后的恢复操作

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026210020.png)
这里的思考题不用管


### RR 插入是什么，以及 RR 插入后的恢复操作

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026210222.png)


### LR 插入是什么，以及 LR 插入后的恢复操作

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026210336.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026210409.png)


### RL 插入是什么，以及 RL 插入后的恢复操作

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026210436.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026210444.png)


### 旋转操作的两个规律（左孩子只能右旋，右孩子只能左旋）

### 平衡二叉树的最多查找次数与树高的关系

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026210534.png)

这也是为什么树要保持平衡的原因

### 平衡二叉排序树的查找复杂度与树高的关系

![](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026210534.png)

### 深度为 h 的平衡二叉树的最少结点个数（递推公式）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026210629.png)

这里假设整个平衡二叉树的深度是 h，那么对于根结点来说，它一定有一个深度为 h-1 的子树（因为这里假设深度就是 h，必然有一个深度为 h-1 的子树），这个子树的结点最少按照定义就是 n_{h-1}

要想整个树的结点个数最少，所以根结点的另一个子树的深度一定是 h-2，不能再是 h-1 了，因为一个 h-2 深度的子树+h-1 深度的子树的结点个数一定小于两个深度为 h-1 的子树的个数，此外为了保证是平衡二叉树，另一个子树的深度不能低于 h-1，所以另一个子树要保证最少，所以结点个数一定是 n_{h-2}

所以对于任意一个结点来说，假设这个结点往下结点个数最小的情况下的深度为 h，那么 n_h 就可以得到上图的递推公式了
### n 个结点的平衡二叉树的最大深度量级

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026211041.png)


### 平衡二叉树的平均查找长度量级

![](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026211041.png)

最大深度量级以及平均查找长度都是树的高度的量级，所以都是 log 2 n 级别


### 平衡二叉树的删除操作



## 红黑树（RBT）的定义和基本性质

红黑树相对于平衡二叉树的优点

平衡二叉树和红黑树的适用场景

红黑树的结点定义代码

红黑树与二叉排序树的关系

红黑树的五个基本要求
	结点颜色
	根结点颜色
	叶结点颜色
	相邻结点颜色
	某个节点到任意叶结点路径上黑结点个数关系

红黑树的叶子结点与二叉排序树叶结点的区别

结点黑高的概念（不包括结点本身的颜色）

红黑树的内部结点概念

根结点的黑高 bh 与红黑树最少内部结点个数 n 的关系

红黑树的路径长度的概念（从某个结点出发到达另一个结点的边数）

红黑树的性质（见 PPT）
	某个结点到叶结点的最长路径与最短路径的关系
	n 个内部结点的红黑树的高度 h 与 n 个内部结点的关系
	
红黑树的查找操作

红黑树的查找操作的复杂度

红黑树的查找与 AVL 查找的关系


## 红黑树的插入操作

红黑树的插入操作基本过程

红黑树插入后的调整过程

## 红黑树的删除

目前不考

## B 树定义和性质

### m 叉查找树的概念和结构

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910131111.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910131133.png)


### B 树的定义（三个方面）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910131228.png)

这里之所以不包括根结点是因为 B 树可以只有一个关键字，此时根结点不可能有上取整（m/2) 个子树，所以不包含根结点，只有一个关键字的情况高度就是 1，不影响什么，所以将根结点排除在外；但是注意根结点也可以至多 m - 1 个关键字

这里除了根结点之外所有非叶结点至少有 $\left\lceil  \frac{m}{2}  \right\rceil$ 颗子树，或者说至少有 $\left\lceil  \frac{m}{2}  \right\rceil - 1$ 个关键字是继承了m叉查找树的性质


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910131235.png)

主要归纳就是：
（1）根结点和非根非叶结点的关键字个数限制和子树限制
（2）叶结点的结构和位置
（3）非叶结点的结构，包括关键字之间关系和子树指向的关键字之间的关系

（1）根结点的关键字个数是 `[1, m-1]`，非根非叶结点关键字个数是 $\left[ \left\lceil  \frac{m}{2}  \right\rceil-1, m - 1 \right]$
（2）根结点的子树个数是 `[2, m]`，非根非叶节点子树个数是 $\left[ \left\lceil  \frac{m}{2}  \right\rceil, m \right]$
（3）叶结点在最后一层，并且在同一层，叶结点相当于外部的结点，不带信息
（4）非叶结点中的关键字有序排列，且指向的子树的关键字与结点内部关键字也是有序排列的，这里可以降序也可以升序

### B 树种叶子结点和终端结点的定义（外部结点也会当作 B 树中的结点）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251022162624.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251022162647.png)

终端结点是最后一层的结点，这一层结点没有子树，他们关键字之间的指针指向叶子结点（空结点）
### B 树的阶是什么

就是m阶B树的m

### B 树中非叶结点的结构


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251022162857.png)


### B 树的核心特性 (4点) B 树的定义

 #### **根结点的子树个数范围，关键字的个数范围**

根结点的关键字可以只有一个，关键字最多m - 1个，所以关键字的范围是 `[1, m - 1]`，所以子树个数的范围是 `[2, m]`


#### **其他非叶结点的子树个数范围，关键字数的范围**

其他非叶节点的关键最多限制和根结点一样，最多m - 1个，但是要求关键字最少是 $\left\lceil  \frac{m}{2}  \right\rceil -1$ 个

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251022163124.png)


#### **任意结点的子树高度**


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251022163136.png)


#### **每个结点的关键字的值与子树的值的关系**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251022163147.png)


### n 个关键字，m 阶 b 树的最小高度和最大高度（按照表格进行推导）

最小高度就是让关键字尽可能填满B树的每个结点，从根结点开始，每个结点都有m - 1个关键字，在这种情况下 h 不能再小了，再小就填不下 n 个固定的关键字了，所以 h >= 这个高度

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251022163221.png)

最大高度就是让关键字尽可能少填结点，即每个结点的关键字尽可能少，此时根结点只有一个关键字，引出2个分叉，其他结点有 $\left\lceil  \frac{m}{2}  \right\rceil-1$ 个关键字，引出 $\left\lceil  \frac{m}{2}  \right\rceil$ 个分叉，这种情况下 h 不能再高了，再高完全没有意义了，所以 h <= 这个高度

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251022163609.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251022165721.png)

这里的思想就是假如高度为h，每个结点的关键字尽可能填的少，这样分叉就尽可能的少，这样算出高度h在这种情况下的关键字个数
然后n个关键字 >= 这个个数从而解得h <= 上图的式子

或者换一种思考方式按照每个结点的关键字尽可能填的少，这样分叉就尽可能的少的这种方式来填n个关键字，这样可以保证h最大，按照这种情况取等算出的h就是最大的h，然后h小于这个式子即可


### n 个关键字的 b 树的失败结点个数

B 树的叶子结点是失败的结点

B 树的关键字简单理解来说是用来分割区间的，n 个关键字那就是将区间划分成了 n + 1 个区间，落在这 n  + 1 个区间种的数就是查找失败，就是落在叶子结点上，**所以叶子结点的个数就是 n + 1 个**：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250517092922.png)

### h 层 b 树的最少结点个数

h层b树的最少结点就是每个结点的分叉尽可能的少（根结点1个关键字，其他结点 $\left\lceil  \frac{m}{2}  \right\rceil -1$ 个关键字），按照这样的规律增长到h层

## B 树的操作

### B 树的查找操作

假如我们要查找的是关键字 80

（1）B 树一般是存放在磁盘中的，但是根结点一般会提前存放在内存中：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101151707.png)

（2）根结点之间的关键字是有序的，这样，可以直接在根结点之间的关键字之间进行二分查找，找到小于 80 的最大的关键字 64，然后通过 64 旁边的指针知道 64 右边的指针指向的关键字地址

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101152048.png)

（3）找到了 64 右边指向的结点的地址之后，需要将指向的结点调入内存，然后再将 80 与这个结点的关键字进行对比：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101152152.png)

（4）发现这个结点关键字有 80，查找成功：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101152226.png)

### B 树查找所进行的磁盘 IO 次数是什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101152333.png)

### B 树的插入操作

#### 插入后的结点仍然符合定义

这里的符合定义主要就是指仍然满足 m 阶 B 树对于根结点以及非叶节点非根结点的关键字个数定义，比如下面的 3 阶 B 树，插入 25 之后仍然满足 3 阶 B 树定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101152758.png)

#### 插入后的结点不符合定义

比如这里插入了关键字 60 到一个 3 阶 B 树的结点中，3 阶 B 树的结点个数范围是【1，2】：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101153111.png)

此时的调整策略是这个结点中从左到右，编号 m/2 上取整的关键字上移到父节点中（从左到右从 1 开始编号），其余的结点进行分裂，分别作为上移关键字的左右子树：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101153229.png)

当然，有可能关键字进入父节点之后父节点也不满足了 B 树的定义，此时就是套娃，对父节点进行同样的调整
### B 树的删除操作

这里的终端结点是指不含信息的叶子结点的上一层结点

#### 删除终端结点关键字

##### 删除完仍然符合定义

比如删除关键字 70，删除完了仍然符合结点的关键字个数定义，此时删了就行，不用调整

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101153927.png)

##### 删完不符合 B 树定义，但是兄弟结点删一个结点符合定义

相当于字节结点删除了，但是兄弟结点可以借我一个：

比如删除 65：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101154116.png)

这个结点关键字删了不符合 3 阶 B 树定义，但是它的右边兄弟结点关键字够借，所以会借一个关键字，此时的调整策略是父节点关键字下来填充 65 这个位置，兄弟结点关键字上去填充父节点下去的关键字：


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101154315.png)


##### 删完不符合 B 树定义，兄弟也不够借的情况

比如删除结点 50，它的兄弟结点都不够借：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101154410.png)


此时的调整是它的父节点关键字会下来一个与兄弟合成一个新的结点：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101154537.png)

#### 删除非终端结点关键字

比如删除关键字 80：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101154608.png)

此时不论删除 80 之后是否仍然符合 B 树定义，也不能直接删除 80，因为此时无法处理 80 左右的指针

此时的调整策略是将 80 与它的前驱 78 或者后继 82 进行交换，交换完毕之后转换成删除终端结点的策略：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101154822.png)

此时变成了删除终端结点 80 的操作，这里是删完仍然符合 B 树定义，所以直接删了即可：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251101154849.png)




## B+树

### B+树和 B 树的区别是什么？（B+树的定义）

**（1）B+树每个结点（非叶结点）的关键字个数与引出的子树个数相同**

B+树的一个结点含有 n 个关键字同时这个结点会引出 n 个子树，即 B+树结点的关键字个数与子树的个数相同；而 B 树的一个结点有 n 个关键字，则子树的个数是 n + 1 个

**（2）B+树的叶子结点含有存储的所有关键字信息，并且这些叶子结点本身就是从小到大顺序链接的**

B+树的非叶结点关键字只是一个索引叶结点的作用；而 B 树的叶子结点完全没有信息，B 树的所有关键字都在自己的结点上

B+树很像分块查找，但是 B 树更像是一个更复杂的二分查找树

**（3）B+树的非终端结点关键字是子树根结点关键字中的最值**

B+树的非终端结点关键字就是起到一个索引的作用，一个结点的一个关键字引出对应一个子树，这个关键字的值就是这个子树根结点中关键字的最大值

**（4）B+树的根结点的关键字个数是 `[2, m]`，引出的子树个数是 `[2, m]` 个**

B 树的根结点的关键字个数是 `[1, m-1]` 个，引出的子树个数是 `[2, m]` 个

**（5）B+树的非根非叶结点的关键字个数是 $\left[ \left\lceil  \frac{m}{2}  \right\rceil,m \right]$ 个，引出的子树个数同样是 $\left[ \left\lceil  \frac{m}{2}  \right\rceil,m \right]$ 个**

B 树的非根分支结点的关键字个数是 $\left[ \left\lceil  \frac{m}{2}  \right\rceil-1,m -1\right]$ 个，引出的子树个数是 $\left[ \left\lceil  \frac{m}{2}  \right\rceil,m \right]$

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251109161207.png)





### B+树的查找过程是什么？

以查找关键字 44 为例，在根结点的时候 44 小于 59，所以走的是 59 的那条路径
当然，如果要查找的是 72 的话，由于 72 大于 59，而这里关键字又是自己引出的子树根结点中关键字的最大值，所以 72 大于 59 引出的子树的所有关键字，因此 72 应该走 97 引出的子树那条路

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251109162709.png)


（1）B+树从根结点开始的查找，无论查找的关键字是否出现在分支结点，它都要查找到最后一层叶子结点
（2）B+树同样支持从叶子结点从左到右的直接顺序查找




## 散列表的基本概念

### 散列表的概念

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920101134.png)

### 散列函数（哈希函数）的概念

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920101158.png)

（1）哈希函数的输入是关键字，关键字是我们需要存放的一个个数据
（2）哈希函数的输出是关键字存放的地址
### 冲突的概念

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920101219.png)


### 同义词的概念

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920101236.png)


### 减少冲突的两种方法


设计合理的散列函数



两种方法避免冲突
	拉链发
	开放定址法
### 散列表装填因子的概念，装填因子与冲突的关系是什么？

装填因子的定义：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250517131548.png)


散列表的平均查找长度是与装填因子直接相关的，直观来看，**装填因子越大，散列表就越满，于是就越容易发生冲突**；并且当表长和表中元素达到一定数量之后平均查找长度和失败查找长度都可以与装填因子建立一个函数关系
## 散列函数的构造

### 散列函数构造的四个注意事项
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920101547.png)

### 除留余数法的散列函数构造原理和适用场景

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920101624.png)


### 除留余数法的质数选择规则

选择不大于散列表的长度 m 但是最接近或等于 m 的质数 p

### 直接定址法的原理和适用场景

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920101720.png)


### 直接定址法的优缺点

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920101748.png)


### 数字分析法的原理和适用场景
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920101807.png)

### 平方取中法的原理和适用场景

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920101837.png)

## 处理冲突的方法 - 拉链法

### 拉链法的基本原理
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920102105.png)

### 拉链法的插入操作

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920102125.png)

这里采用头插法插入到同一个空闲位置处
### 拉链法的查找操作

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920104454.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920104540.png)

### 拉链法的查找长度计算方法

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920104611.png)


### 一个小细节：查找失败与空指针对比的时候不算进查找长度



## 处理冲突的方法 - 开放定址法

### 开放定址法的基本原理
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920105029.png)

### 发生第 i 次冲突后新的散列地址的计算公式，i 的取值范围，i = 0 和 i = m - 1 的含义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920105403.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920105416.png)

i = 0 表示第 0 次冲突之后下一次探测的位置（其实就是最开始 H(key) 探测的位置）

i = 1 表示第 1 次冲突之后下一次探测的位置是 H 1，式子中加上第一次冲突的偏移量 d 1

### 线性探测法的公式是什么？

$$
H_{i} = (H(key) + d_{i}) \% m
$$

Hi 表示发生了第 i 次冲突之后得到的新的映射地址

di 的序列是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251026173801.png)

i = 0 的时候是第一次映射的地址，也就是第 0 次冲突之后得到的新的映射地址
### 线性探测法的基本原理

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920105734.png)

上图的第一次是指发生了第一次冲突，而不是表示第一次冲突之后 H 0 表示第一次冲突之后探测的位置

H 1 是第一次冲突之后探测的位置，在这个位置处发生了第二次冲突
### 平方探测法的基本原理

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920110012.png)


### 双散列法的基本原理

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920110103.png)

### 伪随机法的基本原理

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920110114.png)

### 开放定址法如何查找一个元素

如上图所示跟插入操作类似
### 开放定址法如何删除一个元素
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920110343.png)

### 开放定址法删除元素的易错点

在删除元素的时候不能直接抹除这个元素，而是应该进行逻辑删除，如果直接抹除它变成一个空位置的话，在下次查找或者删除的过程中就会出错，比如下图，第三个位置处就是之前删除操作抹除的位置

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250920110438.png)

## 成功查找次数和失败查找次数的计算

**散列表的成功查找次数比较好算：**

概率一般都是等概率的，是关键字的个数分之一

然后散列表构造好了之后就看每个关键字的查找次数了，按照解决冲突的方法直接观察计算即可

**散列表的失败查找次数：**

概率一般也是等概率的，首先看失败的可能

其实这里的失败可能次数与**散列函数的取值可能一样**，而不是散列表的大小一样

比如散列函数是%11 的，那么失败次数就可能有 11 种结果，概率就是 1/11

比如散列函数是 `(key * 3) % 7`，那么取值就只有 7 种情况

然后看每种失败结果查找的次数，这里还是得先根据题目构建散列表，比如下面的散列表，散列函数就是模 11，解决冲突的方法是线性探测法：

![image-20241125194509105](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20241125194509105.png)

当查找到数组地址中的元素是空的时候就失败了，以 H (key) = 1 这种为例

第一次哈希地址是 1，比较一次

然后由于是线性探测，因此下一次哈希地址时 2，再比较一次

下一次哈希地址 3，下一次是 4，下一次是 5，下一次是 6，7，然后到 8，发现为空，失败

因此比较次数就是 8 次

H (key) = 8 这种情况，此时哈希地址中的元素就是空，直接就失败，因此比较次数就是 1 次

所以失败的查找次数平均就是：

![image-20241125194752791](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20241125194752791.png)



### 一个小细节：开放定址法，查找失败的时候查找空的地方也要算比较一次

这里与拉链法刚好相反
# 排序

## 排序的基本概念

算法的稳定性概念

内部排序和外部排序的概念

## 插入排序

### 插入排序的思想和代码实现

简单来说就是将未排序的数组的元素一个个往前比较插入到有序序列中第一个比它小的元素的后面，有序序列中的其他元素需要后移

```
//待排元素都存放在数组w中
void insert_sort(){
    //从下标1开始，即从第二个元素开始
    for(int i = 1; i < n; i ++){
        int t = q[i];//这个是待插入的元素
        int j = i;//j用来往前遍历找到位置
        //如果前面一位元素比当前待插入的元素大
        while(j && q[j - 1] > t){
            //此时将前面元素向后移动一位
            q[j] = q[j - 1];
            j --;//继续向前一位元素比较
        }
        //退出循环之后此时j要么指向了0，表示前面的所有元素都比t大，应该将t放在第一位
        //要么此时j是上一次循环比t大的元素向后移动一位空出的位置，然后j - 1即此时j指向的位置前一位元素小于或等于t，那么此时j就应该存放元素t
        //要么就没有进入循环，有序元素末尾元素比t小或等于，直接将t插入原位置
        
        //所以此时推出循环之后j一定指向t应该插入的位置
        q[j] = t;
    }
}
```

### 优化的插入排序的思想和代码实现

上文的插入排序是找到合适的位置之后将后面的所有元素统一后移，并且找位置的方式是一个个往前枚举，这里找位置和移动元素的复杂度都是 O (n) 的	

因为前面的数是有序的，因此我们在找这个合适的位置的时候可以用二分法找到第一个大于 `t` 的元素，这样找位置就是 O (logn) 的复杂度；然后再将后面的元素统一后移，这样移动的速度还是 O (n)，但是优化了找位置的速度

```cpp
void binary_insert_sort(){
    for(int i = 1; i < n; i ++){
        if(q[i - 1] <= q[i]) continue;//这里进行特判，如果前面有序数组的所有元素都比当前元素小就不用进行二分浪费时间了
        //这样前面就必然存在一个比当前元素大的元素
        
        int t = q[i];//当前待排元素是t
        //我们的目的就是二分出第一个比当前元素大的元素
        //进行二分
        int l = 0, r = i - 1;
        while(l < r){
            int mid = l + r >> 1;
            if(q[mid] > t) r = mid;
            else l = mid + 1;
        }
        //此时l = r 指向第一个比t大的元素
        //将r后面的元素全部向后移动
        for(int j = i - 1; j >= r; j --){
            q[j + 1] = q[j];
         
        }
        //将t放到正确的位置上
        q[l] = t;
    }
}
```


注意这里我们的二分操作是找到比当前元素大的最小元素，因此是区间右侧性质的左端点，所以用的二分模板是下面：

```cpp
bool check(int mid){
    ......
}

void bsearch(int l, int r, int t){
    
    while(l < r){
    	int mid = l + r >> 1;
        //找到区间中右侧性质的左端点，check函数必须满足右侧性质，本题中就是>t
        if(check(mid)) r = mid;
    	else l = mid + 1;//mid不满足右侧性质，那么显然mid指向的是左边性质区域，我们应该向右缩小区域
    }
    return l;
}
```

如果我们要找的是左侧性质的右端点，则使用的模板是下面，并且 check () 函数需要满足左边区间的性质：
```cpp
bool check(){
}

void bsearch(int l, int r, int t){
	while(l < r){
		int mid = l + r + 1 >> 1;
		//找到左边区间的右端点，这里的check满足的是左边区间的性质并且改变的是左端点，显然此时的mid满足左边区间的形式，那么左端点就应该设置为mid，表示我们寻找的左侧性质的右端点在右边[mid, r]部分
		if(check(mid)) l = mid;
		else r = mid - 1//不满足左侧性质，那么此时mid肯定指向右侧性质，所以我们应该将区域往左缩小
	}
}
```

这样记忆：
（1）找左边区间右端点则 check 函数必须满足左侧性质；找右侧区间左端点，check 函数必须满足左侧性质，这里的满足性质指的是 check 函数中 mid 满足某侧性质后返回 ture，满足左侧性质就是 mid 满足左侧性质后返回 ture
（2）check 函数满足的是哪边的性质，那么满足 check 函数更改的就是哪边的端点；这时根据代码意思写出 if 和 else 两个语句即可
（3）mid = l + r + 1 >> 1 和某个区间 - 1 是绑定的，mid = l + r >> 1 与+1 是绑定的，先写出 check 函数，然后判断区域应该往哪里缩小，写出 else 后面的语句，然后再写 mid 应该如何改变

### 插入排序的时空复杂度和稳定性

==普通插入排序==

**最好情况：**

最好情况就是原本数组就是有序的，外层循环执行 n 次，内部循环直接不进入，因此就是 `O(n)`

**平均情况：**

近似理解一下，内层循环平均遍历 i/2 次，因此总的时间复杂度是：
$$O(n^2)$$

**最坏情况：**

最坏情况，就是 $O(n^2)$

==稳定性==

所谓排序稳定性就是数组中相等的两个元素在排序之后会不会改变在原来数组中的相对位置

显然这里后面的元素当遇到前面比自己小或者等于的元素的时候不会将前面的元素向后移动，**因此是稳定的**

## 希尔排序

### Shell 排序的思想和代码实现

见代码汇总

### Shell 排序的时空复杂度和稳定性

这里只做一个结论记住即可，希尔排序的时间复杂度就是：
$$
O(n^{\frac{3}{2}}) = O(n\sqrt{n})
$$

辅助空间复杂度是 O (1) 的

因为有分组的处理，因此**希尔排序是不稳定的**
## 冒泡排序

冒泡排序的思想和代码实现

冒泡排序的时空复杂度和稳定性

冒泡排序是否可以用于链表

冒泡排序最好和最坏的情况

冒泡排序的交换次数与比较次数的区别

## 快速排序

### 快速排序的基本思想

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251027084210.png)

代码实现看代码汇总
### 快速排序一次划分的概念和特点

### 快速排序的最好情况和最坏情况的空间复杂度

最好情况是每次的枢轴元素将数组均匀划分为两个部分，这样递归深度是 log n 级别的，总的时间复杂度就是 nlogn 级别

最坏情况是每次枢轴元素都选择最小的元素，或者最大的元素，将数组划分极不均衡，这样递归深度是 n 级别的，总的复杂度就是 n^2 级别



### 快速排序的最好情况，最坏情况，平均情况的时间复杂度

### 快速排序的稳定性

快速排序是不稳定的算法



### 一趟排序与一次划分的定义区别（408 为准）

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251027085944.png)
快速排序的第一趟会有一个正确的元素处于正确的位置上（左边元素比其小，右边元素比其大），并且将数组划分为两个部分
第二趟结束后第一趟划分的两个部分分别会有元素处于正确的位置上（左边元素比其小，右边元素比其大）

快速排序每一趟都至少会有一个元素被放在正确的位置上

而快速排序的一次划分则是针对一个部分数组，这个数组中有一个元素会被放在正确的位置上，但是一趟排序可能包含多个划分
## 简单选择排序

### 选择排序的思想

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910141544.png)

### 简单选择排序的思想和代码实现

冒泡排序可以看作是简单选择排序的优化

简单选择排序就每次选择一个后面没有排序的元素中最小的元素，然后放在正确的位置上

比如 `1 3 5 2 4`

第一次，找到 1，放到第一个位置上

第二次，遍历除了 1 后面的所有元素，找到 2，然后与 3 交换放到第二个位置上结果是 `1 2 5 3 4`

第三次，遍历 2 后面的所有元素，找到 3，3 与 5 交换，然后将 3 放到正确的位置上

......

```cpp
void select_sort(){
    //外层循环n - 1次，每次将第i个位置处放入正确的元素
    for(int i = 0; i < n - 1; i ++){
        int k = i;//从第i个位置处往后遍历找到正确的元素
        //找到最小的元素放到正确的位置上
        for(int j = i + 1; j < n; j ++){
            //每次都记录值最小的元素的下标
            if(q[j] < q[k])
                k = j;
        }
        //此时k记录的是值最小的元素的下标
        swap(q[k], q[i]);//将这个最小的元素放到第i个位置处
    }
}
```

### 简单选择排序的时空复杂度和稳定性

**最好情况：**


$$
O(n^2)
$$
**平均情况：**
$$
O(n^2)
$$


**最坏情况：**
$$
O(n^2)
$$
**稳定性：**



**简单选择排序是不稳定**的，比如数据 `2 2 1`

第一次外层循环将0位置处放入1时第一个2会与1交换，这样相同元素的相对位置就发生了变化

这样就不稳定了

其实所有不稳定的排序都可以变成稳定的排序，只要再加一个关键字即可，变成双关键字排序：`(key, key2)`

一开始的key2都是从小到大按照原来数组中的顺序递增且不相等

先按照key比较，当key相同的时候key2小的在前面就行了
## 堆排序

### 完全二叉树存储在数组中的下标关系

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910132215.png)

从下标 1 开始，对于一个下标 i 的结点，左儿子的下标是 2 i，右儿子的下标是 2 i + 1

最后一个内部结点的下标是 n/2
### 大根堆和小根堆的定义

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910132308.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910132317.png)


### 大根堆中代码中 down (i) 操作的含义

`down(i)` 操作其实就是将以编号 i 为根结点的堆调整为一个大根堆

down (i) 其实就是将结点编号 i 的结点下移操作，比如下面的一个大根堆，根结点是 3，显然就不满足大根堆的定义，因此我们就需要进行 down (1) 操作：

<img src="https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20241129102409218.png" alt="image-20241129102409218" style="zoom:50%;" />

down (1) 操作就是以 3 结点为父节点的 3, 5, 4 这个三角结构，3 不满足大根堆定义，那就找到 3，5，4 中的最大值，然后 3 与 5 交换，这样这个三角结构就满足了大根堆定义，而右子树本来就满足定义，所以不用调整，然后再调整以 3 为根结点的 3，4，1 这个三角结构，这三者中的最大值是 4，因此 4 与 3 互换，这样从上到下，就满足了堆结构：

<img src="https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20241129102555155.png" alt="image-20241129102555155" style="zoom:50%;" />

由于堆的层数是 logn 层，而 down 操作每层是常数操作，因此 down 操作的最后的时间复杂度是 O (logn)

 这是一个大根堆，每次都将最大的元素放到父结点，当然小根堆也是一样的，每次将三角中最小的元素放到父结点就好了



### 大根堆建立的原理和具体代码（筛法建立大根堆）

这里的建堆采用的是从下到上的方式进行，其中叶结点不用堆化，因为叶结点已经就满足堆的定义了，所以我们从最后一个非叶结点的结点开始向下堆化，在完全二叉树中，这个结点的下标就是 `n/2`

这样从下到上，第i层结点进行堆化的时候，第i层的每个结点的子树都已经满足堆的定义了，所以我们只需要对这个结点本身进行向下堆化让这个结点作为父节点的三角满足堆的定义就可以了：

<img src="https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/image-20241129104543958.png" alt="image-20241129104543958" style="zoom:50%;" />

从下到上，一层层进行堆化操作，这样就可以依次将每层的子树调整成符合大根堆的形式，建堆的总的时间复杂度通过证明可以得到是O(n)的

```cpp

//从第一个非叶结点开始从下到上进行建堆操作
for(int i = n/2; i ; i --) down(i);

//向下堆化down操作，这是一个递归的过程，对于大根堆来说，每次将三个元素中的最大的放在根结点，然后对交换后的元素继续向下down操作
//比如一个三角结构中左儿子是最大的，那就让根结点与左儿子交换，然后再对左儿子结点下标进行down操作
//down(i)就是将以i为根结点的子树调整成稳定的结构，但是每调用一次down函数其实只是将以i为根结点的三角结构调整成稳定的
void down(int u){
    int t = u;//t用来记录最大的结点下标
    //这两个if语句用来寻找三角中最大的下标
    if(u * 2 <= st && q[u * 2] > q[t]) t = u * 2;
    if(u * 2 + 1 <= st && q[u * 2 + 1] > q[t]) t = u * 2 + 1;
    //如果这个三角本身就是稳定的，因为我们是从下到上调整的，所以这个三角，加上这个三角形成的子树都是稳定的
    //只有三角进行了调整，我们才继续往下
    if(u != t){
        //将三角结构调整成稳定的，此时t记录的是结点最大的下标
        swap(q[u], q[t]);
        //调整之后此时编号t的结点元素是原来的根结点元素，此时继续向下调整
        
        //由于是从下到上进行调整，原来根结点的三角结构中根结点u的两个子树都是稳定的，然后t指向两个儿子中的最大的儿子编号，将根结点u（根结点）与t结点进行交换之后，t结点元素变成原来的根结点元素，此时t结点的子树元素发生改变（变小），可能是不稳定的，所以需要调整t结点这个子树
        down(t);
    }
}
```

### 大根堆排序的原理和具体代码

每次都将一个建好的大根堆的根结点与最后一个叶结点进行互换，然后将最后一个叶结点从堆中剔除，然后从根结点开始从上到下调整大根堆

```cpp

int q[i]//存放所有的元素
int st = n//元素的个数是n
//堆排序，下标从1开始的
    
//向下堆化down操作，这是一个递归的过程，对于大根堆来说，每次将三个元素中的最大的放在根结点，然后对交换后的元素继续向下down操作
//比如一个三角结构中左儿子是最大的，那就让根结点与左儿子交换，然后再对左儿子结点下标进行down操作
//down(i)就是将以i为根结点的子树调整成稳定的结构，但是每调用一次down函数其实只是将以i为根结点的三角结构调整成稳定的
void down(int u){
    int t = u;//t用来记录最大的结点下标
    //这两个if语句用来寻找三角中最大的下标
    if(u * 2 <= st && q[u * 2] > q[t]) t = u * 2;
    if(u * 2 + 1 <= st && q[u * 2 + 1] > q[t]) t = u * 2 + 1;
    //如果这个三角本身就是稳定的，因为我们是从下到上调整的，所以这个三角，加上这个三角形成的子树都是稳定的
    //只有三角进行了调整，我们才继续往下
    if(u != t){
        //将三角结构调整成稳定的，此时t记录的是结点最大的下标
        swap(q[u], q[t]);
        //调整之后此时编号t的结点元素是原来的根结点元素，此时继续向下调整
        
        //由于是从下到上进行调整，原来根结点的三角结构中根结点u的两个子树都是稳定的，然后t指向两个儿子中的最大的儿子编号，将根结点u（根结点）与t结点进行交换之后，t结点元素变成原来的根结点元素，此时t结点的子树元素发生改变，可能是不稳定的，所以需要调整t结点这个子树
        down(t);
    }
}

void heap_sort(){
    int sz = n;//整个堆的大小
    //从第一个非叶结点开始从下到上进行建堆操作
    for(int i = n/2; i ; i --) down(i);
    //这里每次都将堆顶元素放入到q[st]上，即堆顶元素与q[st]元素交换
    ////然后st--表示堆的元素减一，表示删除掉原来的堆顶元素，其实这时原来堆顶元素已经放入了q[st]中了，即q[]最后一个元素是最大的
    //然后对新的堆顶进行down操作，此时的堆是1~st-1个元素的新堆了，q[]最后一个元素是原来的堆最大的，已经放入到了数组的末尾
    
    for(int i = 1; i <= n; i ++){
        swap(q[st], q[1]);
        st --;
        //将根结点元素与数组的最后一个元素交换之后根结点的三角结构发生了破坏，需要从根结点开始调整
        down(1);
    }
    //这样结束后q[]数组中就是有序的从小到大的元素了
}
```

### 大根堆建堆过程的时间复杂度

建堆的过程的复杂度是 O (n)，从最后一个非叶结点开始调整，这是通过求和计算出来的



### 大根堆排序的时间复杂度

排序的时候需要每次从根结点向下调整，每次调整的过程最多对比树的高度，所以总的复杂度就是 O (nlogn)

### 大根堆的稳定性

堆排序是不稳定的

### （大）小根堆插入元素的过程


![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910140224.png)


比如插入一个元素 13 到数组的尾部，在逻辑上作为最后一个叶结点

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910140146.png)

调整之后的结果是：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910140244.png)

大根堆的插入操作也是这样，先将元素插入到堆的末尾，如果这个元素比父节点元素大，则一路上升，一直到无法继续上升
### （大）小根堆删除元素的过程

直接用最后一个元素替代这个元素的位置，然后从这个位置开始向下 down 操作：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910140400.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910140409.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910140429.png)


### （大）小根堆插入和删除元素的时间复杂度

插入和删除与down 操作差不多，最多比较树的高度级别的次数

所以复杂度就是 O (log n)

## 归并排序

### 归并排序的合并过程

其实就是经典的合并有序链表的过程

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910142716.png)


### 2-路归并的含义

2 路归并就是在合并的时候一次合并的是两个链：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910142857.png)

与之对应的是 4 路归并，每次合并的时候是合并 4 个段，选出一个最小的元素需要对比四次：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910142940.png)


### m -路归并选出一个合适的元素的比较次数


比较 m - 1 次如上图所示，并且是每选出一个元素都需要比较 m- 1 次

### 2 路归并排序过程中一趟是指什么？

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250910143203.png)

### 归并排序的代码实现

```cpp

int q[n];//这里是待排的数组元素
int w[n];//这里是归并时候需要用到的辅助数组

void merge_sort(int l, int r){
    //如果l >= r说明此时递归到了只有一个元素，不用合并，直接返回即可
    if(l >= r) return;
    //首先确定划分中点
    int mid = l + r >> 1;
    //然后分别递归左右两边
    merge_sort(l, mid);
    merge_sort(mid + 1, r);//跟二分模板一样，如果mid = l + r >> 1则划分的区间是[l, mid], [mid + 1, r];
    //左右两边排好序之后进行二路归并
    int i = l;//左指针，从左边区间的第一个元素开始
    int j = mid + 1;//右指针，从右边区间的第一个元素开始
    int k = 0;//这里的k是临时数组的下标
    
    //如果左边没有走完并且右边没有走完的时候进行合并
    while(i <= mid && j <= r)
        if(q[i] <= q[j]) w[k ++] = q[i ++];//这里左边小于等于的时候将这个元素放入到辅助数组中，保持稳定性
    	else w[k ++] = q[j ++];
    //上述循环结束的时候必然是一部分走完了，然后另一部分剩下了
    //此时剩下部分直接接到辅助数组的尾部即可
    while(i <= mid) w[k ++] = q[i ++];
    while(j <= r) w[k ++] = q[j ++];
    
    //接着将临时数组的元素复制到原来的数组中，此时合并完成
    //注意我们临时数组合并的其实是q[l ~ r]部分，因此q的下标应该从l开始，然后将临时数组中的k个数据赋值回q的[l, r]部分
    for(int i = l, j = 0; j < k; j ++, i ++) q[i] = w[j];
    
}
```


### 归并排序的的时空复杂度与稳定性

这里选择中间的数进行划分，一共需要递归 logn 层，每层合并的时候都是 n 的复杂度，因此不管是最好，平均还是最坏的复杂度都是：
$$
O(nlogn)
$$

==稳定性==

归并排序的时候合并的是相邻两个区间部分

合并的时候采用的是二路归并，合并两个有序区间的通用算法，左指针元素<=右指针元素，此时将左指针元素放到辅助数组中，从而保持了相对顺序

因此**归并排序是稳定的**

## 基数排序

### 基数排序的分配和收集过程是什么

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103145845.png)

假如有上面的一串 3 位数字，如果想要将他们进行递减排序

这里 d = 3，就是每个关键字的位数，这里是 3 位
r = 9，这里是关键字的每一位的取值，从 0 ~ 9

首先将 r 分为 9 ~ 0，从高到低排布，如上图所示（因为是要递减排序）形成一个个的队列

**（1）对个位进行一趟分配和回收**

这里的一趟分配是指，按照上图的原来数字顺序，将数字按照个位串到各个队列上，形成一次分配，每次都是从队尾进入到各个队列

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103150332.png)

由于是倒序，所以一趟收集就是从 r 大的队列开始，从队头依次将元素取出：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103150446.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103150458.png)

**（3）然后，再按照这个得到的个位的顺序，对十位进行一次分配和收集**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103150604.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103150622.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103150643.png)

**（4）第四趟，再按照百位进行分配和收集**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103150737.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103150756.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20251103150809.png)

这样就得到了递减的序列了
### 基数排序的 r 是指什么

r 是关键字每个位的范围，上文中就是 3 位关键字，每一位的范围都是 0 ~ 9，所以 r  = 0 ~ 9，一个 10 个队列，这 3 位就在这 10 个队列上进行分配和回收

### 基数排序如何得到递增的序列

收集的时候按照从小的队列到大的队列收集即可

### 基数排序如何得到递减的序列

收集的时候按照从大的队列到小的队列收集即可

### 基数排序的时间复杂度

假设关键字是 d 位，每一位的范围都是 r，一共 n 个关键字

一共需要 d 趟，每一趟需要进行 n 次分配，以及 r 次收集，所以复杂度就是 O (d (n + r))

### 基数排序的稳定性

稳定的

### 基数排序的使用场景

它适用于 d 和 r 比较小，但是 n 比较大的场景，因为此时 d (n + r) 的复杂度会小于 n^2

但是如果 d 比较大，n 比较小，那么 d（n + r）复杂度就会大于 n^2

### 基数排序中的 d 和 r 分别是指什么

d 就是关键字的位数，r 是关键字每一位的范围

假设基数排序进行了 k 趟，则趟数越靠后的关键字的位优先级越高，比如上面的 3 位关键字排序，最后一趟的百位优先级最高，基数排序达到的效果就是关键字位优先级高的先决定排序，如果关键字位相同，则看下一位
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250520174450.png)

这道题，显然 k 1 的优先级更高，所以 k 1 一定是靠后进行的，所以 AC 错

这里要求 k 1 相同的情况下 k 2 小的应该在前面

当刚开始进行 k 1 排序的时候是在一个按照 k 2 递增的序列上进行的，按照题意进行排序，意思是对于 k 1 相等的前后两个元素此时不应该改变他们按照 k 2 的递增位序，也就是不应该改变 k 1 相等的两个元素之间在刚开始进行 k 1 排序的时候的位序，那 k 1 用一个稳定的排序算法即可

所以 D 正确
## 外部排序

### 外部排序过程以及读写磁盘的规律分析

外部排序的内存与磁盘间进行数据交换是以磁盘块进行的（无论是几路归并都是如此）

对于 2-路归并来说，假设磁盘中有 32 块数据需要排序，这时内存中只需要有三个缓冲区就可以了：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522212403.png)

#### 形成初始归并段


**首先读入缓冲区**
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522212739.png)

**两个缓冲区使用一次内部排序**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522212805.png)

**利用输出缓冲区写入磁盘**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522212825.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522212832.png)

**形成一个初始归并段**
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522212855.png)

对 32 块磁盘数据都进行这样的操作形成 8 个初始归并段：

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522212943.png)

对于初始操作的读写磁盘次数，由于读写都是以块作为单位进行的，并且每块都是装满元素的，不存在每次读少于三个元素的情况，而初始趟中每个元素都被读入了内存一次，内存一次读 3 个元素，一共是 3\*16 个元素，所以读的次数就是 16 次，同理每个元素都被写入了磁盘一次，内存写磁盘一次写 3 个元素，一共是 3\*16 个元素，所以写的次数就是 16 次

所以第一趟读写的次数就是 16 + 16 = 32 次

这个过程简单来说就是：
**（1）读入几个磁盘的数据到内存中（通常是读入内存大小的数据），经过一次内部排序，形成一个有序的初始归并段**

**（2）在初始形成归并段的时候，数据占据了 16 个磁盘，那就读了磁盘 16 次，同时写了磁盘 16 次**

这是因为每个数据都会被读入内存一次，同时写入外存一次



#### 第一趟归并，将相邻两个归并段的元素读入内存归并排好序之后再写入磁盘

**先将两个归并段中较小的块中的元素读入到缓冲区 12**
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522213727.png)

**然后在褐色缓冲区中进行归并：**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522213804.png)

**褐色缓冲区满了之后写入磁盘**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522213832.png)

**继续在褐色缓冲区归并直到蓝色缓冲区空了，读入一个磁盘块到内存**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522213929.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522214142.png)


这里缓冲区 1 只能读入归并段 1 的磁盘块，因为 27 是属于归并段 2 的元素，按照归并的合并规则 27 此时必须要与归并段 1 中的元素比较，而不是直接将 27 放入到褐色缓冲区，放入褐色缓冲区就意味着是排好序的元素，但是 27 还没有跟归并段 1 中的其他元素进行比较呢，当然不能算有序

**将相邻的两个归并段归并成一个更长的序列**

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522214241.png)

此时形成了一个新的更长的归并段

**对剩余的小的归并段执行相同的操作，形成了 4 个更长的归并段**
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250522214347.png)

这是第一趟归并，这一趟归并其实和初始情况是一样的，每个元素都被读入到了内存一次，内存每次读写一块磁盘，一共是 3（每块大小）\* 16 块个元素，所以读磁盘的次数就是 16 次，同理，一共是 3 \* 16 个元素，每个元素被写入磁盘一次，每次写入 3 个元素，所以就是写入 16 次

第一趟归并就是 16 次读和 16 次写，一共 32 次，跟初始情况一样

**由于外部排序每次都是按照块进行读写的，并且块中的每个元素都被读和写一次，所以从整体宏观来看，每趟读的次数其实就是元素在磁盘中所占据的块数，写的次数跟读的次数一样，也是元素在磁盘中所占的块数**

#### 第二趟归并，将上述四个归并段执行类似的操作，归并成两个更大的归并段，每个归并段内部有序

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250523102748.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250523102805.png)

在这一趟归并中，与前面的阶段类似，每一个元素都被读入到内存一次，一共 3\*16 个元素，每次读 3 个元素，所以一共读取了 16 次；同理这些元素都写入磁盘一次，每次写 3 个元素，一共写了 16 次

#### 最后一趟，将所有元素归并成一个整体有序的序列

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250523103022.png)


一共一次初始内部排序，三趟归并，读写磁盘就是 16 \* 4 次读 + 16 \* 4 次写

#### 读写磁盘次数总结

（1）外部排序过程首先读入磁盘数据到内存，经过内部排序形成初始归并段
（2）对于每个归并段，利用归并排序的思想读入内存进行合并成一个有序的更大的归并段
（3）第一次形成归并段，或者后续每轮的归并过程，每个过程中，整个外存数据占据了多少个磁盘，那就读入了多少个磁盘，同时写出了多少个磁盘
### 外部排序的效率分析 + 归并的趟数分析

外部排序的时间开销有三个部分：

读写磁盘的时间 + 初始内部排序的时间+ 每趟归并的时间

其中读写磁盘的时间占大头

对于一个初始情况下 r 个归并段，k 路归并，形成的树高为 h 的归并树：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250523104516.png)

跟归并树的分析一样，这 r 个初始归并段都在树的最底层（归并排序中的初始归并段是一个个的元素），所以我们可以得到一个不等式：

$$
r \leq k^{h - 1}
$$
这个式子得到的原因是归并树是一颗倒扣的树，从根结点的有序文件开始，每次分支 k 个（k 路归并），要保证初始归并段所在的高度 h 层的结点应该可以覆盖掉所有的归并段 r

得到：
$$
h - 1 \geq \log_{k}r
$$
h 在这里应该是能够覆盖 r 的最小正数，因为这个归并树的形态就是底层可以覆盖 r 个归并段结点就行了，不是覆盖了 r 个结点之后更高的形态（如上图）

所以：

$$
h - 1 = \lceil \log_{k}r \rceil 
$$

h - 1 就是趟数，每趟读写磁盘的次数是一定的（外存元素占据了多少个磁盘，每趟就读少个次，写多少次），所以如果我们能够增大 k 即变成多路归并，或者减小 r，即初始情况生成的归并段尽可能少（每次进行内部排序的元素更多），那么我们就可以减少读写磁盘的趟数，从而降低时间

但是归并的路数 k 不能增大太多，因为 k 太大的话，每一趟归并的时间开销会增大

r 也不能太少，r 太少的话初始情况需要内部排序的元素就会变多，时间开销也会变大
并且如果关键字的个数 N 不变的话，内存中的工作区大小是 L，那么初始归并段的数量就是 N/L = r，L 太大，内部排序一次的时间开销就会更大


总的来说记住：

（1）归并趟数 s = $\lceil \log_{k}r \rceil$，这里 r 是初始归并段个数，k 是归并路数（每次归并多少个段）
（2）增大归并路数和减少初始归并段个数都能减少趟数，从而减少磁盘 IO 次数
（3）增大归并路数会增大每趟归并的时间开销（k 路归并每次选出一个合适的元素需要对比 k-1 次，而 2 路归并只需对比 1 次）
（4）减少初始归并段 r 会导致初始时读入内存进行内部排序一次的时间开销变大
## 败者树

### 败者树解决的问题是什么？（针对外部排序多路归并的优化是什么）

（1）k 路归并的时候，即 k 个归并段同时进行归并，选出一个合适的元素每次需要比较 k-1 次
（2）即随着归并的路数上升，每次归并的时候代价在上升
（3）败者树就是为了解决这个问题，

### 败者树的概念和构造过程

### 败者树每次选出一个最小元素的过程

### 败者树的高度

### 败者树每次选出一个最小元素比较的最多次数

### 败者树的树高与比较趟数的关系

## 置换-选择排序

置换选择排序解决的是什么问题？

置换选择排序的基本原理和过程


## 最佳归并树

归并树合并的读写磁盘次数与带权路径长度关系

最佳归并段的归并过程

给定的结点无法构成一个严格的 k 叉归并树的处理方法

k 叉树的归并需要添加的虚段结点的个数计算


## 问题记录




