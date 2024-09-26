
### 基本思想
---
链表的二路归并排序（Merge Sort for Linked Lists）是一种基于分治思想的排序算法，其主要思想是将链表分成两半，分别对两部分递归地进行排序，然后将排序后的两部分合并成一个有序的链表。具体步骤如下：

1. **分割链表**：通过快慢指针（一个每次移动一步，一个每次移动两步）找到链表的中间节点，将链表分为两部分。
   
2. **递归排序**：对这两部分分别递归进行归并排序，直到子链表长度为1或0，达到递归终止条件。

3. **合并有序链表**：将两个已排序的子链表进行合并，合并的过程类似于合并两个有序数组的过程，从两个链表的头部依次取出较小的元素，形成一个新的有序链表。

**归并排序的时间复杂度是 \(O(n \log n)\)**，因为每次将链表分为两半需要 \(O(\log n)\) 次分割，而每次合并两个链表的时间复杂度为 \(O(n)\)。这种排序方法对链表非常适用，因为链表的随机访问较慢，而归并排序只需要顺序访问元素，没有额外的空间复杂度需求。


### 举例
---
我们来通过一个具体的例子演示链表的二路归并排序过程。假设我们有一个链表，节点的值依次为：

`4 -> 2 -> 1 -> 3`

下面是链表二路归并排序的详细步骤：

#### 步骤 1：分割链表

使用快慢指针将链表分成两半：

1. 快指针一次走两步，慢指针一次走一步，快指针到达链表末尾时，慢指针正好在链表中间位置。
   
2. 链表被分为两部分：
   - 左半部分：`4 -> 2`
   - 右半部分：`1 -> 3`

#### 步骤 2：递归排序

对每个部分继续递归地进行二路归并排序：

**对左半部分 `4 -> 2` 进行排序：**

1. 再次使用快慢指针分成两部分：
   - 左：`4`
   - 右：`2`
   
2. 由于每部分只有一个节点，直接返回排序后的链表：
   - 排序后的左半部分：`2 -> 4`

**对右半部分 `1 -> 3` 进行排序：**

1. 使用快慢指针分成两部分：
   - 左：`1`
   - 右：`3`
   
2. 每部分只有一个节点，直接返回排序后的链表：
   - 排序后的右半部分：`1 -> 3`

#### 步骤 3：合并有序链表

将两部分递归排序后的链表合并：

1. 合并 `2 -> 4` 和 `1 -> 3`：
   - 比较 `2` 和 `1`，选择较小的 `1`，将 `1`作为新链表的头节点。
   - 比较 `2` 和 `3`，选择较小的 `2`，接在 `1` 后面。
   - 然后比较 `4` 和 `3`，选择较小的 `3`，接在 `2` 后面。
   - 最后剩下 `4`，接到链表末尾。
   
合并结果为：`1 -> 2 -> 3 -> 4`


---

```cpp
struct LinkNode {

    int data;

    LinkNode* next;

    LinkNode() {

        next = nullptr;

    }

    LinkNode(int val) {

        data = val;

        next = nullptr;

    }
};

  
// 合并两个有序链表(包括了左右各只有一个结点的排序！)
LinkNode* merge(LinkNode* left, LinkNode* right) {

    LinkNode* rear = new LinkNode();
    LinkNode* head = rear;

    while (left && right) {
        
        if (left->data < right->data) {
            rear->next = left;
            left = left->next;//别忘了把left(right)往前进！
        } 
        
        else {

            rear->next = right;

            right = right->next;//别忘了把left(right)往前进！

        }

        rear = rear->next;

    }

    // 将剩余的节点接到合并后的链表后面
    if (left) rear->next = left;
    if (right) rear->next = right;

    return head->next;
}
  

LinkNode* listSort(LinkNode* head) {

    // 如果链表为空或只有一个节点，直接返回
    // 这一句必须加上！！因为当递推到只有一个结点的时候，这句话就是终止条件！
   if (!head || !head->next) return head;

  
    // 快慢指针法找中间节点
    //必须反复背下来！
    LinkNode* slow = head;
    LinkNode* fast = head->next;

    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }

    // 断开链表
    //一定不能忘记断开，不然会导致运行时错误!
    LinkNode* right = slow->next;
    slow->next = nullptr;

    // 递归排序左半部分和右半部分
    LinkNode* left = listSort(head);
    right = listSort(right);

    // 合并已排序的左右部分
    return merge(left, right);
}

  

int main() {

    ifstream infile("in.txt");

    int val;
    LinkNode* head = new LinkNode();
    LinkNode* rear = head;

    while (infile >> val) {
        LinkNode* p = new LinkNode(val);
        rear->next = p;
        rear = rear->next;
    }

    infile.close(); //输入文件in.txt关闭后才能打开输出文件out.txt！

    head->next = listSort(head->next);

    ofstream outfile("out.txt"); //输入文件in.txt关闭后才能打开输出文件out.txt！

    LinkNode* printlist = head->next;

    while (printlist != nullptr) {
        outfile << printlist->data << " ";
        printlist = printlist->next;
    }

    outfile.close();
    return 0;
    
}
```

---
### 经验

- **一般链表题不会用到`LinkList`，只需要`LinkNode`就够了；尤其是函数的形参，一定尽量设置成`LinkNode*`而非`LinkList`。**
	
- **二路归并时，别忘了把left(right)往前进！** 
	
-  **输入文件in.txt关闭后才能打开输出文件out.txt！**
	
-  **如果链表为空或只有一个节点，直接返回（ 所有对链表操作的函数，开头必须加上这一句！！）**
```cpp
if (!head || !head->next) return head;
```
	
- **快慢指针法找中间节点 必须反复背下来！** 
```cpp
    LinkNode* slow = head; //slow初始位置是头结点
    LinkNode* fast = head->next; //而fast初始位置是首结点！

    while (fast && fast->next) { //注意循环条件！
        slow = slow->next;
        fast = fast->next->next;
    }
```
	
-  **一定不能忘记断开链表，不然会导致运行时错误!** 
	
- **反复学习listSort( )函数的思想，很重要！**  
	
- **关于递推算法** [How to write recursion](obsidian://open?vault=Obsidian%20Vault&file=%E7%AE%97%E6%B3%95%E9%A2%98%2F%E6%80%8E%E6%A0%B7%E5%86%99%E9%80%92%E5%BD%92%E7%AE%97%E6%B3%95)
 