### 邻接矩阵

```cpp
int visited[MAXV];

void DFSA(AdjGraph& G)//对于非连通图！！
{
	for(int i=0;i<G.n;i++){
		if(visited[i]==0){DSF(G,i);}//一定要保证最后所有顶点都处于被访问过的状态
	}
}

void DFS(MatGraph& G,int v)
{
	cout<<v<<" ";//对当前顶点访问
	visited[v]=1;
	
	for(int w=0;w<G.n;w++)//遍历邻接矩阵中,当前顶点的这一整列
	{
		if(G.edges[v][w]!=0&&G.edges[v][w]!=INF)//若存在边<v,w>
			if(visited[w]==0)//并且w没有被访问过
			{
				DFS(G,w);//递归访问
			}	
		}
	}
}
```

### 邻接表

```cpp
int visited[MAXV];

void DFS(AdjGraph& G,int v)
{
	cout<<v<<" ";
	visited[v]=1;
	
	ArcNode* p=G.adjlist[v].firstarc;
	
	while(p!=NULL){
		int w=p->adjvex;
		if(visited[w]==0){DFS(G,w);}
		p=p->nextarc;
	}
}
```
