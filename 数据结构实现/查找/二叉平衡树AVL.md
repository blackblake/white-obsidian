
>对于左/右旋的代码，画出这两种图，照着就能写出来

![[Pasted image 20241215101143.png|250]]       ![[Pasted image 20241215101253.png|180]] 
![[Pasted image 20241215101023.png|250]]        ![[Pasted image 20241215101317.png|180]]

![[Pasted image 20241215151306.png]]
### 1）树节点
---
```cpp
//  AVL 树节点结构体
struct AVLNode {
public:
    T key; 
    AVLNode* left;
    AVLNode* right; 
    int height; // 节点的高度(比BST多出来的！！)

    // 构造函数，初始化节点的键值
    AVLNode(T k): key(k), left(nullptr), right(nullptr), height(1){}
};
```

### 2）树及基本旋转操作
---
```cpp
// AVL 树结构体
stuct AVLTree {
private:
    // 指向树根节点的指针
    AVLNode<T>* root;

    // 获取节点高度的函数
    int height(AVLNode<T>* node)
    {
        if (node == nullptr)//注意处理空结点
            return 0;
        return node->height;
    }

    // 获取节点的平衡因子的函数
    int balanceFactor(AVLNode<T>* node)
    {
        if (node == nullptr)
            return 0;
        return height(node->left) - height(node->right);
    }

    // 右旋函数
    AVLNode<T>* rightRotate(AVLNode<T>* y)
    {
        AVLNode<T>* x = y->left;
        AVLNode<T>* T2 = x->right;

        // 执行旋转
        x->right = y;
        y->left = T2;

        // 更新高度
        y->height
            = max(height(y->left), height(y->right)) + 1;
        x->height
            = max(height(x->left), height(x->right)) + 1;

        // 返回新的根节点
        return x;
    }

    // 左旋函数
    AVLNode<T>* leftRotate(AVLNode<T>* x)
    {
        AVLNode<T>* y = x->right;
        AVLNode<T>* T2 = y->left;

        y->left = x;
        x->right = T2;

        // 更新高度
        x->height
            = max(height(x->left), height(x->right)) + 1;
        y->height
            = max(height(y->left), height(y->right)) + 1;

        // 返回新的根节点
        return y;
    }
```


### 3）插入/删除函数
---
```cpp
    // 插入结点的函数
    AVLNode<T>* insert(AVLNode<T>* node, T key)
    {
        // 二叉搜索树插入操作(和BST的插入节点代码一样)
        if (node == nullptr)
            return new AVLNode<T>(key);

        if (key < node->key)
            node->left = insert(node->left, key);
        else if (key > node->key)
            node->right = insert(node->right, key);
        else
            return node;

        // 更新该祖先节点的高度(从此开始是AVL的独有代码)
        node->height = 1+ max(height(node->left),height(node->right));

        // 获取该祖先节点的平衡因子
        int balance = balanceFactor(node);

        // 如果节点变得不平衡，则有 4 种情况

        // 左左情况
        if (balance > 1 && key < node->left->key)
            return rightRotate(node);

        // 右右情况
        if (balance < -1 && key > node->right->key)
            return leftRotate(node);

        // 左右情况
        if (balance > 1 && key > node->left->key) {
            node->left = leftRotate(node->left);
            return rightRotate(node);
        }

        // 右左情况
        if (balance < -1 && key < node->right->key) {
            node->right = rightRotate(node->right);
            return leftRotate(node);
        }

        return node;
    }

    // 删除值为key的结点的函数
    AVLNode<T>* deleteNode(AVLNode<T>* root, T key)
    {
        // 执行标准的二叉搜索树删除操作
        if (root == nullptr)
            return root;

        if (key < root->key)
            root->left = deleteNode(root->left, key);
        else if (key > root->key)
            root->right = deleteNode(root->right, key);
        else {
            // 只有一个子节点或没有子节点的情况
            if ((root->left == nullptr)|| (root->right == nullptr)) {
                AVLNode<T>* temp= root->left ? root->left : root->right;
                
                if (temp == nullptr) {
                    temp = root;
                    root = nullptr;
                }
                else
                    *root = *temp;
                delete temp;
            }
            else {
                AVLNode<T>* temp = minValueNode(root->right);
                root->key = temp->key;
                root->right = deleteNode(root->right, temp->key);
            }
        }

        if (root == nullptr)
            return root;

        // 更新当前节点的高度
        root->height = 1 + max(height(root->left),height(root->right));

        // 获取该节点的平衡因子
        int balance = balanceFactor(root);

        // 如果该节点变得不平衡，则有 4 种情况(大致处理和插入函数相同)

        // 左左情况
        if (balance > 1 && balanceFactor(root->left) >= 0)
            return rightRotate(root);

        // 左右情况
        if (balance > 1 && balanceFactor(root->left) < 0) {
            root->left = leftRotate(root->left);
            return rightRotate(root);
        }

        // 右右情况
        if (balance < -1 && balanceFactor(root->right) <= 0)
            return leftRotate(root);

        // 右左情况
        if (balance < -1
            && balanceFactor(root->right) > 0) {
            root->right = rightRotate(root->right);
            return leftRotate(root);
        }

        return root;
    }
```

1. 别忘了删除/插入后要 **==更新高度==**
2. 删除/插入函数都是在BST的删除/插入函数原封不动的基础上加入了这两个操作而已：
	- 更新高度
	- 左右旋

### 其他
```cpp
	// 查找值为key的结点的函数
    AVLNode<T>* minValueNode(AVLNode<T>* node)
    {
        AVLNode<T>* current = node;
        while (current->left != nullptr)
            current = current->left;
        return current;
    }
    
    // 中序遍历函数
    void inorder(AVLNode<T>* root)
    {
        if (root != nullptr) {
            inorder(root->left);
            cout << root->key << " ";
            inorder(root->right);
        }
    }

    // 在以根节点为根的子树中查找键值的函数
    bool search(AVLNode<T>* root, T key)
    {
        if (root == nullptr)
            return false;
        if (root->key == key)
            return true;
        if (key < root->key)
            return search(root->left, key);
        return search(root->right, key);
    }
};
```

```cpp
int main()
{
    AVLTree<int> avl;

    // 向 AVL 树中插入节点
    avl.insert(10);
    avl.insert(20);
    avl.insert(30);
    avl.insert(40);
    avl.insert(50);
    avl.insert(25);

    // 打印 AVL 树的中序遍历结果
    cout << "AVL 树的中序遍历结果: ";
    avl.printInorder();

    // 从 AVL 树中删除节点
    avl.remove(30);
    cout << "删除 30 后的中序遍历结果: ";
    avl.printInorder();

    // 在 AVL 树中查找节点
    cout << "25 是否在树中? "
         << (avl.search(25) ? "是" : "否") << endl;
    cout << "30 是否在树中? "
         << (avl.search(30) ? "是" : "否") << endl;

    return 0;
}
```

