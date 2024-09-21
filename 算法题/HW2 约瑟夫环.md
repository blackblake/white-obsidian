
---
### 我的实现：

```cpp
struct LinkNode
{
    int data;//每个人最初时的编号
    LinkNode* next;
    LinkNode(){
        data=0;
        next=nullptr;
    }
    LinkNode(int d){
        data=d;
        next=nullptr;
    }
};

  //删除报N的结点、并将其添加到新队列里的算法
void kickout(LinkNode*& head,LinkNode*& rear,int N){

    int i=1;
    LinkNode* p=head;//指向首结点
    
    while (i!=N-1)
    {
        i++;
        p=p->next;
    }

    //此时p已指向报N-1的人,接下来删除第N个人
    LinkNode* temp=p->next;
    p->next=temp->next;
    temp->next=nullptr;

    //把第N个人入新链表newlist
    if(rear->data==0){/*如果rear->data=0，说明现在newlist链表只有一个头结点
    但是我的设计里面不能有头结点*/
        rear->data=temp->data;//直接把"头结点"的值由初始化的0变成要尾插的结点的值，而不是真的添加新的结点，这样就可以达到“去掉头结点”的目的
    }
    else{
        rear->next=temp;
        rear=rear->next;
    }


    //更新head结点，下次报数从被删除结点的下一个开始
    head=p->next;
}
  

LinkNode* Joseph(LinkNode* &head,int N){

    LinkNode* newlist=new LinkNode();//新队列
    LinkNode* rear=newlist;
    
    while (head->next!=head)//如果不是只剩下一个结点
    {
        kickout(head,rear,N);
    }
    
    //把剩下的最后一个结点也入新队
    head->next=nullptr;
    rear->next=head;
    rear=head;
    rear->next=newlist;

    return newlist;
}
```

这个实现的缺点就是当输入好几个N的时候，运行时间太长了

---
### 更好的实现

```cpp
struct LinkNode {
    int data;
    LinkNode* next; 
    LinkNode() : next(NULL) {}
    LinkNode(int d) : data(d), next(NULL) {} 
};

int main() {
    LinkNode* head = new LinkNode(); // Create the linked list
    LinkNode* tail = head; // Pointer to the end of the list
    int m, n; // Input the linked list length

    cin >> m;
    for (int i = 1; i <= m; i++) {
        tail->next = new LinkNode(i);
        tail = tail->next; // 尾插法
    }

    while (cin >> n) { //头插法
        LinkNode* p = head;

		// 创建新的环，仍使用尾插法
        LinkNode* new_head = new LinkNode();
        LinkNode* new_tail = new_head; 
        
        while (head->next != NULL) { // 若旧环内仍有数字
            // Jump n-1 times
            for (int i = 1; i < n; i++) {
                if (p->next != NULL) // Jump to p->next
                    p = p->next;
                else
                    p = head->next;
            }
            
            // 跳n次 之 后p−>n e x t 就 是 要 被 删 掉 的 结 点
            if (p->next != NULL) {
            // 尾 插 法 将p−>n e x t 插 入 新 链 表
                new_tail->next = new LinkNode(p->next->data);
                new_tail = new_tail->next;
                p->next = p->next->next;
            } 
            
            else { //然后删掉p->next 
                new_tail->next = new LinkNode(head->next->data);
                new_tail = new_tail->next;
                head->next = head->next->next;
            }
        }
        head = new_head; //更新为删除了第N个结点后的链表
    }

    head = head->next;
    while (head != NULL) {
        cout << setw(4) << head->data; // Ensure width is 4
        head = head->next;
    }

    return 0;
}

```

---
### 总结

- **一般链表题不会用到`LinkList`，只需要`LinkNode`就够了；尤其是函数的形参，一定尽量设置成`LinkNode*`而非`LinkList`。**
	
- **这题里我的所有head指的就是“首”结点而非“头”结点 **
