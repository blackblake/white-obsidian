### Solution

```cpp
	LinkNode* head = new LinkNode(); 
	LinkNode* tail = head;
    int m, n; 

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
	
- **这题里我的所有head指的就是“首”结点而非“头”结点**
