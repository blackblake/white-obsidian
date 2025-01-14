
```cpp
class Solution {
public:
    bool DFS(int i,int n,vector<vector<int>>&A,vector<int>&done,int destination){
        bool is=false;
        done[i]=1;
        if(i==destination){
            return true;
        }
        else{
            for(int j=0;j<n;j++){
                if(done[j]==0&&A[i][j]==1){
                    is=DFS(j,n,A,done,destination);
                    if(is==true){return is;}
                } 
           }
        }

        return is;
    }

    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        //顶点序号从0开始，数组大小设为n即可
        vector<int>done(n,0);
        vector<vector<int>>A(n,vector<int>(n,INT_MAX));
        for(int i=0;i<edges.size();i++){
            A[edges[i][0]][edges[i][1]]=1;
            A[edges[i][1]][edges[i][0]]=1;//无向图一定要注意初始化两个元素！！！
        }

        return DFS(source,n,A,done,destination);
    }
};
```

1. **==无向图一定要注意初始化`edges[i][j]`和`edges[j][i]`两个对称的元素！！==**
2. DFS函数里返回值的设置有些不对：
```cpp
if(done[j]==0&&A[i][j]==1){
    is=DFS(j,n,A,done,destination);
    if(is==true){return is;}//一开始我没有这一句,导致找到的目标路径却被后面的false刷新了
} 
```


3. 上面这个题解用的是邻接矩阵，但是只通过了22个用例，因为内存超出限制了，必须要用邻接表！

```cpp
class Solution {
public:

    bool dfs(int source, int destination, vector<vector<int>>& adj, vector<bool>& visited) {
        // 如果当前节点是目标节点，直接返回 true
        if (source == destination) {
            return true;
        }

        // 标记当前节点为已访问
        visited[source] = true;

        // 遍历当前节点的所有邻居
        for (int i = 0; i < adj[source].size(); ++i) {
            int w = adj[source][i]; 
            if (!visited[next]) {//邻接表不需要考虑INT_MAX
                if(dfs(w, destination, adj, visited))
	                return true; // 如果找到一条有效路径，则直接返回 true
            }
        }

        // 如果所有邻居都无法到达目标节点，返回 false
        return false;
    }

    // 主函数
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        // 构建邻接表
        vector<vector<int>> adj(n);

        // 遍历边的列表，手动添加每条边到邻接表
        for (int i = 0; i < edges.size(); ++i) {
            int x = edges[i][0];
            int y = edges[i][1]; 

            // 将边添加到邻接表中
            adj[x].push_back(y);
            adj[y].push_back(x); // 无向图，双向添加边
        }

        // 初始时所有节点均未访问
        vector<bool> visited(n, false);

        return dfs(source, destination, adj, visited);
    }
};

```

1. **==邻接表不是一个链表，而是一个二维数组==**
2. 邻接表对于无向图也要“对称添加”
```cpp
adj[x].push_back(y);
adj[y].push_back(x); // 无向图，双向添加边
```
