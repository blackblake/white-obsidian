1. **LinkNode和LinkList都用struct而不是class**

2. **注意构造函数没有类型，不要写作**

```cpp
void LinkNode(){}
```
而应该写成：
```cpp
LinkNode(){...};
```

3. **链表的构造函数就一行代码**：
```cpp
 head = new LinkNode();
```
 `记住这个new语句`

4. **删除一个指针指向的结点：delete（用在析构、删除函数里）**
```cpp
delete pre; // 删除 pre 指向的节点
```

5. **head应该单拿出来作为一个成员，否则没有通用性**

6. LinkNode的有参和无参构造函数 都没有给next的参数，但不是说next不需要初始化，而是**不管有参无参，都要把next初始化为NULL**：

```cpp
LinkNode(){
	next=NULL;
};

LinkNode(int d){
	data=d;
    next=NULL;
}
```

7. **LinkNode( )的有形参版本只有一个参数data，而next没有参数**


8. **快慢指针法找中间结点**（一大问题就是**对奇偶分类讨论** ）
```cpp
int middle(LinkList L){

    LinkNode* fast=L.head->next; //链表L的头结点是“L.head”，别忘了要加“L.”
    LinkNode* slow=L.head->next; //一定注意这里f、s初始化为首结点head->next而不是头结点head!!

    while(fast->next!=nullptr){

        if(fast->next->next==nullptr){//如果满足这个条件，说明有偶数个结点
            return slow->data;
        }

        else{
            fast=fast->next->next;
            slow=slow->next;
        }
    }

    return slow->data;
}
```

> [!NOTE] 注意
>1# fast、slow初始化为首结点head->next而不是头结点head!!
>2# 链表L的头结点是“L.head”，别忘了要加“L.”      
>3# 思考：我是如何处理链表可能有奇数或偶数个结点这一问题的？
>4# 我这里创建链表用的是头插法，所以顺序是8-7-6-5-4-3-2-1，是倒着的！要用**尾插法才是正序！！**


6. 最大值结点个数
```cpp
int maxcount(LinkList L){

    LinkNode* p=L.head->next;//一般都是把指针初始化为L.head->next而不是L.head

    int max=0;

    int cnt=1;

    //while(p->next!=nullptr)

    while(p!=nullptr){//这里不应该是“p->next”，否则p无法指向最后一个结点，导致遗漏

        if(p->data>max){
            max=p->data;
            cnt=1;
        }

        else if(p->data==max){cnt=cnt+1;}
        p=p->next;
    }

    return cnt;
}
```

> [!NOTE] 注意
> **1#** 遍历链表用的while语句不应该是“while(p->next!=nullptr)”，否则p无法指向最后一个结点，导致遗漏，应该是“while(p!=nullptr)”，即去掉“->next”
> **2#** 遍历链表时，一般都是把指针初始化为L.head->next而不是L.head


7. **删除最大值结点**
```cpp
  int delmax(LinkList& L) {//一定注意：要删除L的结点，是对L做了修改，一定要写成引用传参！！
  
    if (L.head == nullptr || L.head->next == nullptr) {
        return -1;
    }

    // 初始化 max 为第一个节点的数据而不是0
    int max = L.head->next->data;
    LinkNode* p = L.head->next;

    // 第一次遍历找到最大值
    while (p != nullptr) {
        if (p->data > max) {
            max = p->data;
        }
        p = p->next;
    }
  
    // 删除所有与最大值相等的节点
    LinkNode* slow = L.head;
    LinkNode* fast = slow->next;

    while (fast != nullptr) {
        if (fast->data == max) {
            slow->next = fast->next;
            delete fast;
            fast = slow->next;
        } else {
            slow = fast;
            fast = fast->next;
        }
    }

    return max;
}
```

> [!NOTE] 注意
> 删除结点的函数需要对L进行修改，所以一定要改为 **【引用传参】！！**否则会输出一堆莫名其妙的地址

