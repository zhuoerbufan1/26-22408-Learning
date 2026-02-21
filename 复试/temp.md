```c
#include <cstdio>

#include <algorithm>

using namespace std;

  

const int MAXN = 1000;

int a[MAXN], cnt[10];                           // a 为数字数组，cnt 统计每个数字出现次数

  

int main() {

    int n, k;

    scanf("%d %d", &n, &k);                    // 读入 n 和 k

  

    bool hasDuplicate = false;                 // 标记是否存在重复数字（用于“白耗”多余交换）

    for (int i = 0; i < n; i++) {

        scanf("%d", &a[i]);                    // 读入每位数字

        cnt[a[i]]++;                           // 计数该数字出现次数

        if (cnt[a[i]] >= 2) hasDuplicate = true;        // 若出现次数 ≥2，则存在重复

    }

  

    // 不可行情形：

    // 1) n==1 无法做任何交换；

    // 2) n==2 且第二位为 0 时，唯一的一次交换会使最高位为 0（前导零），不合法

    if (n == 1 || (n == 2 && a[1] == 0)) {

        printf("-1\n");                 // 直接输出 -1

        return 0;

    }

  

    // 从左到右，寻找“最右的最大值”且严格大于 a[i] 的位置 pos，与 a[i] 交换

    for (int i = 0; i < n - 1 && k > 0; i++) {

        int mx = a[i], pos = -1;                   // mx 当前扫描到的最大值；pos 为其最右位置

        for (int j = n - 1; j > i; j--) {          // 从右往左扫，确保取到“最右”的最大

            if (a[j] > mx) {                       // 只在严格更大时更新

                mx = a[j];                         // 更新最大值

                pos = j;                           // 记录位置

            }

        }

        if (pos != -1) {                           // 找到可提升的更大数

            swap(a[i], a[pos]);                    // 交换提升当前位

            k--;                                   // 消耗一次交换

        }

    }

  

    // 处理剩余次数

    if (k > 0) {

        if (!hasDuplicate) {                       // 无重复数字时，不能交换相同数“白耗”

            if (k % 2 == 1) {                      // 偶数次可两两抵消；若为奇数需真实动一次

                swap(a[n - 1], a[n - 2]);          // 交换末两位

            }

        }

        // 有重复数字：可交换两相同数字白耗任意次数，结果不变，无需实际执行

    }

  

    for (int i = 0; i < n; i++) printf("%d", a[i]);

    return 0;

}
```

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260212152747.png)

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260214102906.png)
![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260214112331.png)


```cpp

#include<iostream>

#include<stdio.h>

#include<algorithm>

#include<vector>

#include<queue>

#include<cstring>

using namespace std;

  

struct Knight{

    int x, y;

    int f, h, g;

};

  

struct CompareKnight{

    bool operator()(const Knight &a, const Knight &b){

        return a.g > b.g;

    }

};

  

int cntDist(Knight a, Knight b){

    return abs(a.x - b.x) * abs(a.x - b.x) + abs(a.y - b.y) * abs(a.y - b.y);

}

  
  
  

priority_queue<Knight, vector<Knight>, CompareKnight> q;

int dx[8] = {1, 2, -1, -2, 1, 2, -1, -2};

int dy[8] = {2, 1, 2, 1, -2, -1, -2, -1};

int mov[1010][1010];//move[i][j]表示从源点移动到{i, j}这个点需要的移动次数

//当move[i][j] = 0的时候它不在队列中

// 这个数组同样可以用于路径回溯过程

  
  
  

int aStar(Knight start, Knight end){

    start.g = 0;

    q.push(start);

    mov[start.x][start.y] = 1;

    while(q.size()){

        Knight curKnight = q.top();

        q.pop();

        // cout << curKnight.x << " " << curKnight.y << endl;

        if(curKnight.x == end.x && curKnight.y == end.y) return mov[end.x][end.y]-1;

        for(int i = 0; i < 8; i ++){

            Knight nextKnight;

            nextKnight.x = curKnight.x + dx[i];

            nextKnight.y = curKnight.y + dy[i];

            // cout << nextKnight.x << " " << nextKnight.y << endl;

            if(nextKnight.x >= 1 && nextKnight.x <= 1000 && nextKnight.y >= 1 && nextKnight.y <= 1000){

                if(mov[nextKnight.x][nextKnight.y] == 0){

                    nextKnight.g = cntDist(start, nextKnight) + cntDist(end, nextKnight);

                    mov[nextKnight.x][nextKnight.y] = mov[curKnight.x][curKnight.y] + 1;

                    q.push(nextKnight);

                }

            }

        }

    }

}

  
  
  

int main(){

    int n;

    cin >> n;

    while(n --){

        Knight start;

        Knight end;

        memset(mov, 0, sizeof(mov));

        cin >> start.x >> start.y >> end.x >> end.y;

        cout << aStar(start, end) << endl;

        while(q.size()) q.pop();

    }

    return 0;

}
```

![image.png](https://typora-1310242472.cos.ap-nanjing.myqcloud.com/typora_img/20260221170028.png)
