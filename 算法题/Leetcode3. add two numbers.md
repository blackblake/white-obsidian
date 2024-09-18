### （1）Better Soulutions

```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummyHead = new ListNode(0);
        ListNode* tail = dummyHead;

	    //进位数
        int carry = 0;

        while (l1 != nullptr || l2 != nullptr || carry != 0) {
            int digit1 = (l1 != nullptr) ? l1->val : 0;
            int digit2 = (l2 != nullptr) ? l2->val : 0;

			//sum：读取到的两个本位的数字之和
            int sum = digit1 + digit2 + carry;
            //digit：留在本位的数字
            int digit = sum % 10;
            //carry：向下一位的进位数
            carry = sum / 10;

			//注意创建新结点的方法
            ListNode* newNode = new ListNode(digit);
            tail->next = newNode;
            tail = tail->next;

            l1 = (l1 != nullptr) ? l1->next : nullptr;
            l2 = (l2 != nullptr) ? l2->next : nullptr;
        }

        ListNode* result = dummyHead->next;
        delete dummyHead;
        return result;
    }
```

> [!NOTE] Notice
Notice how this function reads digit by digit from two lists that are not necessarily equal in length,and calculates the standard of each digit and the carry to the next digit


### （2）Summery
1. **how to convert char to int**
```cpp
char a = '4';
int ia = a - '0';
```
exa:
```cpp
current->next = new ListNode(str[i] - '0');
```

2. **the difference between constructing a node and a list**
Creating a new linked list usually means creating the first node of the linked list, also known as the "head node".So if I create a new list with only one node, then I'm actually creating a new node.
That is, `creating a linked list and creating a node are the same statements`.
```cpp
/*
struct ListNode {
    int data;
    ListNode* next;

    // constructor
    ListNode(int val) : data(val), next(nullptr) {}
};
*/

ListNode* node = new ListNode(5);

```
