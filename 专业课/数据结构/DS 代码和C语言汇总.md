## C 语言

### 结构体的定义和声明

```c
struct Student {
    char name[50];
    int age;
    float gpa;
};

//声明一个结构体变量student1
struct Student student1;

//声明一个指向结构体变量的指针
struct Student* pstu;


```
### typedef 作用

`typedef` 一般用于简化类型声明，可以用自己定义的名字来代替一个类型

比如在结构体中：

```c

//这里使用了typedef之后struct后面跟不跟结构体的名字都无所谓
//反正是用TreeNode代替了整个结构体，PTreeNode代替了整个结构体指针
typedef struct Node{
	int val;
	Node* left;
	Node* right;
}TreeNode, *PTreeNode;

```

于是后面就可以用 `TreeNode` 来代替 `struct Node` 声明变量了，以及可以用 `PTreeNode` 来声明指向 `struct Node` 的指针

```
TreeNode root; // 等价于 struct Node root
PTreeNode root1; //等价于 struct Node * root1
```
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
## 图

### 图的邻接矩阵存储代码

```c
#define MAX 100 //顶点的最多个数
typedef struct{
	char Vex[MAX];//存放顶点
	int Edge[MAX][MAX];//数组存放边
	int vnum, arcnum;//图的顶点数和边数
}Graph;
```

这个存储结构是整个图的存储结构
### 图的邻接表存储

```c
//顶点的数据结构
typedef struct VNode{
	VertexType data;//这个是顶点信息
	ArcNode* first;//指向这个顶点的第一条弧
}Vnode, AdjList[MAX];//顶点数组

//边的数据结构
typedef struct ArcNode{
	int adjvex;//这个边指向哪个顶点
	struct ArcNode* next;//指向下一条弧的指针
	
}ArcNode;

//用邻接表表示的图
typedef struct{
	AdjList vertices;//存储的顶点数组
	int vexnum, arcnum;//图的顶点和边的个数
}Graph;
```

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250507171329.png)


### 图的 BFS

```c
bool visited[MAX];//访问标记数组

//从顶点v开始广搜G
void BFS(Graph G, int v){
	visited(v);//访问顶点v
	visited[v] = true;//对v进行标记
	
	Enqueue(Q, v);//将顶点v加入到队列中
	
	//如果队列不为空，一直循环
	while(!isEmpty(Q)){
		Dequeue(Q, v);//队列头的顶点弹出，赋值给v
		
		//取出v的所有没有访问过的邻接点，访问，加入到队列中
		while(w = FirstNeighbor(G, v); w >= 0; w = NextNeighbor(G, v, w)){
			//如果v的这个邻接点之前没有访问过，则访问，同时将这些点加入到队列中
			if(!visited[w]){
				visit(w);
				visited[w] = true;
				EnQueue(Q, w);
			}
		}
	}
}

	
		
//还要防止图不是连通的情况，扫描数组，从每个没有遍历的顶点开始BFS
void BFSreverse(Graph G){
	//先对图的访问数组进行初始化
	for(int i = 0; i < G.vexnum; i ++){
		visited[i] = false;
	}
	initQueue(Q);//初始化辅助队列
	//从visited数组中的false开始遍历
	for(int i = 0; i < G.vexnum; i ++){
		if(!visited[i])
			BFS(G, i);
	}
}	
```

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20250508172017.png)


### 图的 DFS

```c
bool visited[MAX];

//从顶点v开始DFS图G
void DFS(Graph G, int v){
	visit(v);//访问顶点v
	visited[v] = ture;//将顶点v标记访问过
	
	//开始深搜
	for(w = FirstNeighbor(G, v); w >= 0; w = NextNeighbor(G, v, w)){
	// 如果v连接的结点w没有被访问过,继续DFS
		if(!visited[w]){
			DFS(G, w);
		}
	}
}


//避免非连通的图

void DFSTravese(Graph G){
	for(int v = 0; v < G.vexnum; v ++){
		visited[v] = false;
	}
	//与BFS类似，从每个没有遍历过的顶点开始DFS
	for(int v = 0; v < G.vexnum; v ++){
		if(!visited[v])
			DFS(v);
	}
}

```

### Prim 算法

```c
假设初始时有T, S两个集合，图的所有顶点初始在S集合中，结果顶点集是T
dist[N]表示每个点到连通部分T的距离，初始时为无穷
    
随机加入S中的一个点到T中，更新其余点到T中的距离
 for(i = 0; i < n; i++)
 {
     t <- 找到集合S距离集合T最近的点;
     t加入到集合T中，用t更新其他点到集合T的距离;
     从S中剔除点t
 }
```

### 最短路径 - BFS 算法

```c
//求顶点u到其他顶点的最短路径
//d[i]表示从顶点u到顶点i的最短路径
//path[i]表示顶点i在最短路径上的直接前驱
void BFS_MIN_Distance(Graph G){
	//初始化所有顶点距离源点的距离
	for(int i = 0; i < G.vexnum; i ++){
		d[i] = 0x3f3f3f3f;
		path[i] = -1; 
	}
	d[u] = 0;
	visited[u] = ture;
	EnQueue(Q, u);
	while(!isEmpty(Q)){
		DeQueue(Q, u);
		for(w = FirstNeighbor(G, u); w >= 0; w = NextNeighbor(G, u, w)){
			if(!visited[w]){
				//从u访问u的所有邻接点，距离当然是u到源点的距离加一
				d[w] = d[u] + 1;
				//从u访问u的所有邻接点，将这些点加入到队列中
				//这些点都是从u过来的，所以路径的上一个顶点就是u了
				path[w] = u;
				visited[w] = true;
				EnQueue(Q, w);//将顶点w加入队列中
				
			}
		}
	}
}
```

### Floyd 算法

```c

for(int k = 1; k <= n; k ++)
	for(int i = 1; i <= n; i ++)
		for(int j = 1; j <= n; j ++)
			d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
			
```


三层循环，时间复杂度妥妥的 $O(V^3)$
### 拓扑排序的代码实现（BFS）



邻接表存储：队列中的所有顶点都会被遍历一次，将顶点的邻接点加入到队列中的过程会将这个顶点的所有边遍历一次，所以所有顶点和所有边都会被遍历一次，因此时间复杂度就是 $O(E +V)$

邻接矩阵存储：找某个顶点的邻接点的时候会遍历矩阵中的一行所有值，即 V 个值，而 V 个顶点每个顶点都需要这样遍历，所以时间复杂度就是 $O(V^2)$

只要是跟 BFS 相关的，其复杂度都大致是上面两种

### 逆拓扑排序的代码实现



## 查找

### 二分的模板

二分模板是下面，找区间右侧性质的右端点

```cpp
bool check(int mid){
    ......
}

void bsearch(int l, int r, int t){
    
    while(l < r){
    	int mid = l + r >> 1;
        //找到区间中右侧性质的右端点，check函数必须满足右侧性质，
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

### 二分查找的代码




### 二叉排序树的结构定义

### 二叉排序树的查找代码

### 二叉排序树的插入代码

### 二叉排序树的构造代码

### 红黑树的结点定义代码


## 排序

### 插入排序

