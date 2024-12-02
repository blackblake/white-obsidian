
![[Pasted image 20241202201446.png|500]]

```cpp
class Solution {
public:

	void Floyd(vector<vector<int>>& edges, int n, vector<vector<int>>& A) {
        
        //初始化A数组，注意这道题的edges数组和常规的不一样
        for (int i = 0; i < edges.size(); i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int w = edges[i][2];
            A[u][v] = w;
            A[v][u] = w;
        }


        for (int i = 0; i < n; i++) {
            A[i][i] = 0; // 自环距离为0(为什么自环距离是0而不是INF?)
        }
        
		//实际的Floyd算法就是这个三层for循环,前两个都是初始化(准备工作)
        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (A[i][k] != INT_MAX && A[k][j] != INT_MAX) {//必须写,否则溢出
                        A[i][j] = min(A[i][j], A[i][k] + A[k][j]);//是A不是edges
                    }
                }
            }
        }
    }

    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        vector<vector<int>> A(n, vector<int>(n, INT_MAX));//建立A要用二维数组
        Floyd(edges, n, A);

        int result = 0;
        int minCnt = INT_MAX;

        for (int i = 0; i < n; i++) {
            int cnt = 0;
            for (int j = 0; j < n; j++) {
                if (i != j && A[i][j] <= distanceThreshold) {//i!=j
                    cnt++;
                }
            }
            if (cnt < minCnt) {
                minCnt = cnt;
                result = i;
            } else if (cnt == minCnt) {
                result = max(result, i);
            }
        }

        return result;
    }

};
```

1. **最短路径数组A要用vector二维数组存储，否则引用传参会很麻烦：**
	`vector<vector<int>> A(n, vector<int>(n, INT_MAX));`
	 - 注意必须初始化第二维==vector\<n>==，而`INT_MAX`是用来初始化A的所有元素的最大整数值；
	 - 第二个参数`vector<int>(n, INT_MAX)`意思是A数组的元素"是什么"(指明是具有n个int元素的vector数组)，和`INT_MAX`的作用差不多

2. 这道题定义的edges数组和常规不一样，在Floyd算法初始化A数组的时候要注意

3. Floyd中要把所有`A[i][i]`设置为0(为什么不设置为INT_MAX)
	- 因为初始化A数组的时候把所有的A\[i]\[j]都设置为INT_MAX了，但是其中A\[i]\[i]即自环的最短路径应该是0而不是INT_MAX！所以要单独改过来；

4. Floyd更新`A[i][j]`的时候一定要排除INT_MAX！否则会溢出
```cpp
if (A[i][k] != INT_MAX && A[k][j] != INT_MAX) 
{
	A[i][j] = min(A[i][j], A[i][k] + A[k][j]);
}
```
注意==Floyd算法更新语句中的是 **A**\[i]\[k]+**A**\[k]\[j] 而不是 **edges**\[i]\[k]+**edges**\[k]\[j]**==；

5. 读入边的时候，一定要处理重复输入，比如同一条边输入了两次，但权不同，要取最小值！（虽然这题没体现）
```cpp
	// 读入图的边
    for (int i = 1; i <= m; i++) {
        int x, y, z;
        cin >> x >> y >> z;
        A[x][y] = min(A[x][y], z);//注意处理所输入的重复的边！
    }
```