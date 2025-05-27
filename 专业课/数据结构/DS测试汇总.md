
## 复杂度

### 1

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250526202941.png)

这道题，当 i = m 的时候

j 层的循环的取值分别是 1， 2， 4 ....... m，一共 $\log_{2}m$ 次

因此 k 层循环中语句的执行次数就是 1 + 2 + 4 +,...... m，次，公比是 2，项数是 $\log_{2}m$ 级别

所以当 i = m 的时候语句执行的次数就是：

$$
\frac{1(1 - 2^{\log_{2}m})}{1 - 2} = m级别
$$
即 i = m 的时候关键语句执行次数的级别是 m 次

所以总的执行次数就是 n^2 级别

B 正确

再看一道：
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250526202959.png)

跟上一道题类似

i 的取值是 1 2 4 8... n，项数是 $\log_{2}n$ 级别

所以内层执行的总次数是 1 + 2 +  4 + 8 + ..... n ，项数是 $\log_{2}n$ 级别

等比数列求和，加起来总次数就是 n 了

B 正确
### 2
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250526204326.png)

画出递归树，每一层的执行次数都是 n 次，这道题形成的递归树高度不超过 log 2 n，所以总的复杂度就是 nlog 2 n
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250526204558.png)

## 线性表

### 1

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250526205315.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250526205356.png)

### 2 循环队列 rear 指向最后一个元素以及 rear 指向最后一个元素下一个元素判空判满总结

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250526210839.png)

判空或者判满主要是假设原始队列为空，然后加入一个元素，分析 rear 和 front 指向得到的

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

## 栈，队列，数组

## 树

### 1 n 个结点的树的形态问题

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250526215542.png)

对于任意一个树，根据左孩子右兄弟画法，其都有一颗唯一的二叉树与之对应，并且根据转化规则，这个二叉树没有右子树

这个二叉树的左子树是一个完整二叉树，一共有 n 个结点，所以 n 个结点的树的形态个数就与 n - 1 个结点的二叉树的形态个数一样

这是因为这颗二叉树的左子树的 n - 1 个结点构成的二叉左子树，其任意一个形态，加上根结点形成的整个二叉树，根据左孩子右兄弟转化，都必然有一颗唯一的树形态与其对应；同理反过来，任意一个 n 个结点的树，通过左孩子右兄弟转化，其必然对应一个没有右子树的二叉树，这个二叉树左子树有 n - 1 个结点，对应其一种形态

所以n 个结点的树的形态就和 n - 1 个结点的二叉树形态是一一对应的

所以这道题目就是 3 个结点的二叉树的形态个数，选 B

### 2 

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250526220337.png)

## 图

## 查找

## 排序

