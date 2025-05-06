
## 树

### 树的双亲表示法

```c
// 定义树中的最多节点个数
#defint MAX_SIZE  100

//树结点的定义
typedef struct{
	// 结点的数据元素
	EleType data;
	// 结点的双亲位置
	int parent;
}TreeNode;

//整棵树的定义
typedef struct{
	//nodes用来存放所有的结点，这个结点数组每个都是一个TreeNode的结构
	TreeNode TreeNodes[MAX_SIZE];
	int n;//树中的结点个数
}Tree;
```

### 树的先根遍历

```c
// 先序遍历树
void preorderTree(TreeNode* root) {
    if (root == NULL) return;

    // 访问当前节点
    printf("%c ", root->data);
    // 先序遍历当前节点的子树森林
    preorderTree(root->firstChild);
    // 先序遍历当前节点的兄弟节点
    preorderTree(root->nextSibling);
}
```

效果等同于转换成二叉树之后的先序遍历
### 树的后根遍历

```cpp
void PostOrderTree(TreeNode* root){
	if(root == NULL) return;
	
	PostOrderTree(root->firstChild);
	
	// 访问当前节点
	printf("%c ", root->data);
	
	PostOrderTree(root->nextSibling);
}
```

效果等同于转换成二叉树之后
的中序遍历
### 森林的先序遍历

```cpp

// 先序遍历树
void preorderTree(TreeNode* root) {
    if (root == NULL) return;

    // 访问当前节点
    printf("%c ", root->data);
    // 先序遍历当前节点的子树森林
    preorderTree(root->firstChild);
    // 先序遍历当前节点的兄弟节点
    preorderTree(root->nextSibling);
}

// 先序遍历森林
void preorderForest(Forest* forest) {
    if (forest == NULL || forest->trees == NULL) return;
    // 先序遍历森林中的第一棵树
    preorderTree(forest->trees);
}
```

效果等同于转换成二叉树之后的先序遍历
### 森林的中序遍历

```cpp

// 中序遍历树
void inorderTree(TreeNode* root) {
    if (root == NULL) return;

    // 中序遍历当前节点的子树森林
    inorderTree(root->firstChild);
    // 访问当前节点
    printf("%c ", root->data);
    // 中序遍历当前节点的兄弟节点
    inorderTree(root->nextSibling);
}

// 中序遍历森林
void inorderForest(Forest* forest) {
    if (forest == NULL || forest->trees == NULL) return;
    // 中序遍历森林中的第一棵树
    inorderTree(forest->trees);
}

	
```

效果等同于转换成二叉树之后的中序遍历

### 并查集的初始化

```c
#define SIZE 13

int UFSets[SIZE]; //集合元素数组

//初始化并查集
void Initial(int S[]){
	for(int i = 0; i < SIZE; i ++){
		S[i] = -1;
	}
}
```

### 并查集的查操作

```c
int Find(int S[], int x){

	// 反复回溯x的根结点
	while(S[x] >= 0)
		x = S[x];//将x指向当前x的根结点
		
	//最后返回的时候S[x] = -1，此时x就是根结点
	return x;
}
```

查操作与树的高度相关，最坏情况下的复杂度是 $O(n)$
### 并查集的并操作

```c
//合并并查集中的两个集合，只用合并这两个集合的根结点即可，即让其中一个根结点的双亲指向另一个根结点
//这里传入的参数是两个集合的根结点
void Union(int S[], int root1, int root2){
	if(root1 == root2) return;
	
	//让第二个结点根结点的双亲指向第一个结合的根结点
	S[root2] = root1;
}
```

### 并查集并操作的优化（小数合并到大树）

```c
void Union(int S[], int root1, int root2){
	if(root1 == root2) return;
	
	//如果root1对应的集合的树的结点个数小于root2的结点个数
	if(S[root2] > S[root1]){
		//将小树合并到大树，即将root1合并到root2上，这样可以保证树的高度尽可能不会增加
		//但是树的高度其实还是会随着合并增加的
		S[root1] = root2;
		//修改root2的结点个数
		S[root2] += S[root1];
	}else{
		S[root2] += S[root1];
		S[root1] = root2;
	}
}
```

合并到最后，并查集的树的高度不会找过 $\lfloor \log_{2}n \rfloor + 1$，这样可以将查找操作的时间复杂度优化到 $O(\log_{2}n)$

### 并查集查操作的优化（路径压缩）

```c
//返回x的所在集合编号（根结点编号），加上路径压缩优化
int find(int S[], int x){
    //这个递归非常巧妙，先一路回溯到根结点
    //然后再把根结点的值不断返回赋予回溯路上的S[x],一直到最开始查询的S[x]
    
    //S[x] = x说明此时是根结点
    //函数返回的时候是一路返回这个集合中的根结点编号，将这个编号赋给路径上的所有结点x的S[x]
    if(S[x] != x) S[x] = find(S, x);
    return S[x];
}
```

在一次搜索的过程中，将搜索路径上的每个点的父结点都更改为根结点，这样在下次寻找的过程中就不用再往前回溯了，于是就将后面的搜索复杂度降低到几乎常数级别
