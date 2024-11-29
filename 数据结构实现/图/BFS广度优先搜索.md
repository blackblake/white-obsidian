
适用于解决两个点之间的**最短路径**问题：因为广搜是从起点出发，以起始点为中心一圈一圈进行搜索，一旦遇到终点，记录之前走过的节点就是一条最短路。
### 邻接矩阵MatGraph

![[Pasted image 20241129093209.png]]

```cpp
void BFSA(MatGraph&G)//非连通图的BFS
{
	for(int i=0;i<G.n;i++){
		if(visited[i]==0){
			BFS(G,i);
		}
	}
}

void BFS(MatGraph& G,int v)// BFS（邻接矩阵）
{
	int visited[MAXV]//全局数组
	memset(visited,0,sizeof(visited));//初始化visited数组,所有元素都初始化为0
	
	queue<int>qu;//定义队列
	qu.push(v);//顶点v进队
	
	cout<<v<<" "//访问顶点v(这个操作以实际情况而异)
	visited[v]=1;//标为已访问
	
	
	while(!qu.empty())
	{
		int u=qu.front();
		qu.pop();
		
		for(int i=0;i<g.n;i++)//g.n是邻接矩阵的顶点数
		{
			if(g.edges[u][i]!=0&&G.edges[u][i]!=INF)//若存在边<u,i>,且
			{
				if(visited[i]==0)//顶点i未被访问
				{
					cout<<i<<" ";//访问邻接点i
					visited[i]=1;//标为已访问
					
					qu.push(i);//临界点i进队
				}
			}
		}
	}
}
```

### 邻接表AdjGraph

```cpp
void BFS(AdjGraph &G, int v) // 广度优先遍历（邻接表）
{
    int visited[MAXV];                          // 定义访问标记数组
    memset(visited, 0, sizeof(visited));        // 初始化访问标记数组为0
    
    queue<int> qu;                              // 定义一个队列
	qu.push(v);                                 // 起始顶点入队

    cout << v << " ";                           // 访问起始顶点 v
    visited[v] = 1;                             // 设置起始顶点为已访问
    

    while (!qu.empty())                         // 当队列不为空时循环
    {
        int u = qu.front();                     // 队列头部元素出队
        qu.pop();

        ArcNode *p = G.adjlist[u].firstarc;     // 获取顶点 u 的第一条邻接边
        while (p != NULL)                       // 遍历所有邻接边
        {
            if (visited[p->adjvex] == 0)        // 如果邻接点未被访问
            {
                cout << p->adjvex << " ";       // 访问邻接点
                visited[p->adjvex] = 1;         // 标记邻接点为已访问
                
                qu.push(p->adjvex);             // 邻接点入队
            }
            p = p->nextarc;                     // 获取下一条邻接边
        }
    }
}

```


