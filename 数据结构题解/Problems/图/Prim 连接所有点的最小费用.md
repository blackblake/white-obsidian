
```cpp
class Solution {
public:
    void prim(int n,vector<vector<int>>A,vector<int>&lowcost,vector<int>&closet,vector<bool>&done,int& sum){
    for(int cnt=0;cnt<n-1;cnt++){//别忘了循环n-1次！（别写错循环起点终点了）
        //找到距离当前已遍历集的距离最近的未遍历点k
        int min=INT_MAX;
        int k=0;
        for(int i=1;i<n;i++){
            if(lowcost[i]<min){
                if(done[i]==false){
                    min=lowcost[i];
                    k=i;
                }
            }
        }
    
        //添加k到已遍历集
        done[k]=true;
        //sum+=lowcost[k];//sum的操作也可以放在这里！因为一旦done[k]为true的lowcost[k]被确定了，以后就不会更改了！
        //（当然，done[k]为false即未遍历过的点的lowcost肯定需要更新）

        //更新lowcost和closet数组
        for(int i=0;i<n;i++){
            if(!done[i]&&lowcost[i]>A[k][i]){//注意“!done[i]”，只能更新没被遍历过的！
                lowcost[i]=A[k][i];//这里不像最短路径算法要更新成lowcost[i]+A[k][i],更新A[k][i]就行！
                closet[i]=k;
            }
        }
    }

    for(int i=0;i<n;i++){
        sum+=lowcost[i];//好好体会一下为什么能够通过给lowcost数组求和来得到sum
        //（lowcost数组的lowcost[i]不是始点到i点的距离，而是已遍历点集合中，到i的距离最近的那个点 到i的距离）
    }

}

    int minCostConnectPoints(vector<vector<int>>& points) {
        int n=points.size();
        vector<int>lowcost(n);
        vector<int>closet(n,0);//closet数组其实没必要
        vector<bool>done(n,false);
        done[0]=true;

        vector<vector<int>>A(n,vector<int>(n));
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                A[i][j]=abs(points[j][0]-points[i][0])+abs(points[j][1]-points[i][1]);
            }
        }

        lowcost[0]=0;
        for(int i=0;i<n;i++){
            lowcost[i]=A[0][i];
        }

        int sum=0;
        prim(n,A,lowcost,closet,done,sum);

        return sum;
    }
};

```

1. `for(int cnt=0;cnt<n-1;cnt++)`
	别忘了是循环n-1次（我一开始写成`cnt<n`了）,更别忘了需要循环！

2. prim算法就3步：找到lowcost数组的最小值及其对应点序号k、更新`done[k]=true`、更新`lowcost`数组（**==注意只更新未遍历过的点的lowcost值！！(`!done[i]`)==**）

3. 好好体会一下为什么能够通过给lowcost数组求和来得到sum
    `lowcost`数组的`lowcost[i]`不是始点到i点的距离，而是已遍历点集合中，到i的距离最近的那个点 到i的距离）
