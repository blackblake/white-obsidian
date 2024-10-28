【问题描述】用队列实现以下算法：从txt文件读入一个整数数组，数组长度在0到200之间，设计一个算法，将所有偶数位的元素移动到所有奇数位的元素的前面，要求它们的相对次序不变，使用txt文件输出移动后的结果。

用队列以外的数据结构实现不得分。

【类库使用要求】不允许使用STL中的容器类，比如queue、list、vector、stack等，如用到队列、链表的操作需要自行编写代码。

【输入形式】

文件输入，文件名为in.txt

【输出形式】

文件输出，文件名为out.txt

【样例输入】

1 2 3 4 5 6 7 8

【样例输出】

2 4 6 8 1 3 5 7

---
### 我的错误代码

```cpp
#include<iostream>
#include<fstream>
using namespace std;

struct queueNode
{
    int data;
    queueNode* next;
    queueNode(){
        next=nullptr;
    }
    queueNode(int val){
        data=val;
        next=nullptr;
    }
};


int main(){
    
    int n;int N=0;
    queueNode* head=new queueNode();
    queueNode* rear=head;
    while(cin>>n){
        queueNode* s=new queueNode(n);
        rear->next=s;
        rear=rear->next;
        N++;
    }
	
    queueNode*p=head;
    int x=1;
    
    while ((N%2==0&&x<=N/2)||(N%2==1&&x<=N/2+1))
    {
        if(p->next->data%2==1){
            queueNode*ptr=p->next;
            p->next=ptr->next;
            rear->next=ptr;
            ptr->next=nullptr;
            rear=rear->next;
        }
        p=p->next;
        x++;
    }
    
    queueNode*p2=head->next;
    while (p2){
        cout<<p2->data<<" ";
        p2=p2->next;
    }
    
    return 0;
}
```


### 误区
---
题目要求==偶数位置==（即0, 2, 4, ...）的元素在==奇数位置==（1, 3, 5, ...）之前输出，而代码中是根据==**元素值是否为奇数或偶数**==来判断，这并不符合题目要求。

**==“偶数位置”，不是“偶数元素”！==**

### 如果百思不得其解，一定要重新读题，确定自己没有理解错误题意！！


### 收获

> 1）ifstream文件输入是可以接受while(cin)并以\^Z结束输入的，输出错误时不必怀疑这一点
> 
> 2）getline怎么用？
> 
> 3）STL的queue模板的==back( )==方法返回的是**队[尾]元素**，==front( )==返回**队[头]元素**；而==pop( )==方法只弹出**队[头]元素**，**不返回**元素值
> 
> **注意区分front的队[头]元素和back的队[尾]元素，一定不要弄混！！**

