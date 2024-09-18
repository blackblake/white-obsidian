```cpp
#include <iostream>
#include <vector>
using namespace std;

  struct ListNode {
      int val;
      int addr;
      int nextAdd;
      ListNode *next;

      ListNode() : addr(0),nextAdd(0),val(0), next(nullptr) {}
      ListNode(int x,int y) : addr(y),nextAdd(0),val(x), next(nullptr) {}
      ListNode(int x,int y,int z) : addr(y),nextAdd(z),val(x), next(nullptr) {}
      ListNode(int x,int y,int z, ListNode *next) : addr(y),nextAdd(z),val(x), next(next) {}

  };

  
struct List
    {
        ListNode* head;
        int n;//总结点数

        List(){
            head=new ListNode();
        }

        void printList(){
            ListNode* pointer=head;
            while (pointer!=nullptr)
            {
                cout<<pointer->val<<" ";
                pointer=pointer->next;
            }
            cout<<endl;
        }

    //重排算法
 void reorderList() {
    
    if (!head || !head->next) return; // 边界条件

    int move = n/2;  // 需要被插入的结点数
    int unmove = n - move; // 前面的结点数


    //beforeRear:倒数第二个结点的指针
    ListNode* beforeRear = head;
    while (beforeRear->next->next!=nullptr) {
        beforeRear = beforeRear->next;
    }

    //rear:尾结点指针
    ListNode* rear=beforeRear->next;

    //front:始终指向尾结点要插入的位置的前一个结点的指针
    ListNode* front=head;

    int cnt=0;
    //一共有move个要插入的结点，故插入move次
    while(cnt!=move){
        beforeRear->next=nullptr;
        rear->next=front->next;
        front->next=rear;

        beforeRear = head;
        while (beforeRear->next->next!=nullptr) {
            beforeRear = beforeRear->next;
        }

        rear=beforeRear->next;
        front=front->next->next;
        cnt++;
    }
  
  }
};


int main(){

    List l;
    int add,num;//第一行输入首届点地址、总结点数
    cin>>add>>num;

    l.n=num;

	//用vector存储结点，待全部存好后再连接
    vector<ListNode*> listVec(num);//注意一定要写这个参数"(num)"！！

    for(int i=0;i<num;i++){
        int x,y,z;
        cin>>y>>x>>z;
		
		//为当前输入的数据创建一个结点，x、y、z分别对应结点的val、addr、nextAdd
        ListNode* p=new ListNode(x,y,z);
        //然后把指向所创建结点的指针存到vector里作为第i个元素
        listVec[i]=p;
	  
	  //如果遇到了首结点，就把其连接到head后面
        if(y==add){
            l.head->next=p;
        }
    }  
    

    for(int i=0;i<num;i++){
       //如果nextAdd是-1，说明是尾结点
        if(listVec[i]->nextAdd==-1){
            listVec[i]->next=nullptr;
        }
        
        //连接各结点
        for(int j=0;j<num;j++){
            if(listVec[j]->addr==listVec[i]->nextAdd){
                listVec[i]->next=listVec[j];
            }
        }
    }
  
    l.reorderList();
    
    l.printList();

    return 0;

}
```


> [!NOTE] 经验
> 1. 一定要在开始写代码之前构思好，画好示意图。别一想到可行的方法就写。要想想，有没有更好写、更容易实现的（即使时间复杂度高也没关系）
>    
> 2. 一个很好的想法：由于此题输入结点的顺序是不按顺序来的，所以在循环中，当前结点的下一个结点可能还没有出现过，这就需要先把这些结点及其数据存放起来，待全部输入完后再连接。我这里用了一个 **vector<ListNode*> listVec(num);** 来存储，这个方法蛮好
>    
> 3. 但是要注意，**vector创建的时候一定要写好结点数“listVec(num)”，否则无法加入元素！！**

