
### 1）思路
![[Pasted image 20241130183348.png]]

### 2）实现
![[Pasted image 20241130183400.png]]![[Pasted image 20241130183425.png]]

### 3）注释
	
1. **关于初始化--为什么不管始点v到i有没有边，都置lowcost\[i]=edges\[]\[i] ?**
	![[Pasted image 20241130183439.png|450]]
	
2. **为什么对lowcost数组的遍历，下标都是从0到g.n ?**
	答：因为lowcost数组的元素代表的是n个顶点到U集的最短距离，所以元素个数一定是有n个，遍历索引当然是从0到n-1；
	

### 4）示例

![[Pasted image 20241130183945.png|500]]
##### 邻接矩阵 接矩阵(部分)：
![[Pasted image 20241130184214.png|300]]

##### 过程解释：
![[ad29422a2dd8d1358dbfbfdd9edb83f.jpg]]