### 从文件读取输入

```cpp
#include <fstream> //一定要包含这个头文件

int main() {
    ifstream infile("in.txt");  // 打开文件
    int number;
    
    while (infile >> number) {  // 把原来的cin换成infile
        cout << number << endl; 
    }

    infile.close();  // 记得关闭文件
    return 0;
}

```

###  输出到文件

```cpp
#include <fstream>

int main() {
    ofstream outfile("out.txt");  // 打开或创建输出文件

    // 输出若干整数到文件
    for (int i = 1; i <= 5; ++i) {
        outfile << i << " ";  // 把cout换成outfile
    }

    outfile.close();  // 关闭文件
    return 0;
}

```