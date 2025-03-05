
```cpp
// 二叉搜索树的节点结构
struct Node {
    int data; // 节点数据
    Node* left; // 左子节点指针
    Node* right; // 右子节点指针
};

// 创建新节点的函数
Node* createNode(int data)
{
    Node* newNode = new Node();
    newNode->data = data; // 设置节点数据
    newNode->left = newNode->right = nullptr; // 初始化左右子节点为空
    return newNode; // 返回新创建的节点
}

// 在二叉搜索树中插入节点的函数
Node* insertNode(Node* root, int data)
{
    if (root == nullptr) { // 如果树为空，返回新节点
        return createNode(data);
    }

    // 否则，递归向下查找插入位置
    if (data < root->data) {
        root->left = insertNode(root->left, data); // 插入到左子树
    }
    else if (data > root->data) {
        root->right = insertNode(root->right, data); // 插入到右子树
    }

    // 返回（未改变的）节点指针
    return root;
}

// 二叉搜索树的中序遍历函数
void inorderTraversal(Node* root)
{
    if (root != nullptr) {
        inorderTraversal(root->left); // 遍历左子树
        cout << root->data << " "; // 输出节点数据
        inorderTraversal(root->right); // 遍历右子树
    }
}

// 在给定的二叉搜索树中搜索指定键的函数
Node* searchNode(Node* root, int key)
{
    // 基本情况：根节点为空或键在根节点处
    if (root == nullptr || root->data == key) {
        return root;
    }

    // 键大于根节点的键
    if (root->data < key) {
        return searchNode(root->right, key); // 在右子树中搜索
    }

    // 键小于根节点的键
    return searchNode(root->left, key); // 在左子树中搜索
}

// 查找中序后继的函数
Node* minValueNode(Node* node)
{
    Node* current = node;
    // 循环向下查找最左侧的叶子节点
    while (current && current->left != nullptr) {
        current = current->left;
    }
    return current; // 返回最小值节点
}

// 删除节点的函数
Node* deleteNode(Node* root, int data)
{
    if (root == nullptr)
        return root; // 如果树为空，返回空

    // 如果要删除的数据小于根节点的数据，则在左子树中查找
    if (data < root->data) {
        root->left = deleteNode(root->left, data);
    }
    // 如果要删除的数据大于根节点的数据，则在右子树中查找
    else if (data > root->data) {
        root->right = deleteNode(root->right, data);
    }
    // 如果数据与根节点的数据相同，则这是要删除的节点
    else {
        // 只有一个子节点或没有子节点的节点
        if (root->left == nullptr) {
            Node* temp = root->right; // 保存右子节点
            delete root; // 删除当前节点
            return temp; // 返回右子节点
        }
        else if (root->right == nullptr) {
            Node* temp = root->left; // 保存左子节点
            delete root; // 删除当前节点
            return temp; // 返回左子节点
        }

        // 有两个子节点的节点：获取中序后继（右子树中的最小值）
        Node* temp = minValueNode(root->right);

        // 将中序后继的内容复制到当前节点
        root->data = temp->data;

        // 删除中序后继
        root->right = deleteNode(root->right, temp->data);
    }
    return root; // 返回当前节点
}

```


```cpp
// Main function to demonstrate the operations of BST
int main()
{

    Node* root = nullptr;
    // create a BST
    root = insertNode(root, 50);
    root = insertNode(root, 30);
    root = insertNode(root, 20);
    root = insertNode(root, 40);
    root = insertNode(root, 70);
    root = insertNode(root, 60);
    root = insertNode(root, 80);

    // Print the inorder traversal of a BST
    cout << "Inorder traversal of the given Binary Search "
            "Tree is: ";
    inorderTraversal(root);
    cout << endl;

    // delete a node in BST
    root = deleteNode(root, 20);
    cout << "After deletion of 20: ";
    inorderTraversal(root);
    cout << endl;

    // Insert a node in BST
    root = insertNode(root, 25);
    cout << "After insertion of 25: ";
    inorderTraversal(root);
    cout << endl;

    // Search a key in BST
    Node* found = searchNode(root, 25);

    // check if the key is found or not
    if (found != nullptr) {
        cout << "Node 25 found in the BST." << endl;
    }
    else {
        cout << "Node 25 not found in the BST." << endl;
    }

    return 0;
}
```