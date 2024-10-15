
- **用vector声明二维数组，不能只初始化外层的大vector，一定要把小vector也给初始化，否则会导致运行时错误！**
```cpp
// 初始化为N行，每行有2个string
vector<vector<string>> vec(N, vector<string>(2));
```

- 其实就是书上的**中缀表达式转化为后缀表达式** 和 **后缀表达式计算** 的原代码，一定要背下来！

```cpp
//表达式类
class Express {
    string exp;//中缀表达式
    string postexp;//后缀表达式

public:
    Express(string str) {   
        exp = str;
        postexp = "";
    }

    void Trans() {//中缀表达式转化为后缀表达式
        stack<char> opor;//操作符栈
        int i = 0;
        char ch, e;
        while (i < exp.length()) {
            ch = exp[i];
            if (ch == '(') {//左括号直接进栈
                opor.push(ch);
            } 
            else if (ch == ')') {
                while (!opor.empty() && opor.top() != '(') {
                    //直接把栈顶字符弹出并加入到postexp后面
                    e = opor.top();
                    opor.pop();
                    postexp += e;
                }
                opor.pop();//弹出左括号
            } 
            else if (ch == '+' || ch == '-') {
                while (!opor.empty() && opor.top() != '(') {
                    e = opor.top();
                    opor.pop();
                    postexp += e;
                }
                opor.push(ch);
            } 
            else if (ch == '*' || ch == '/') {
                while (!opor.empty() && opor.top() != '(' && (opor.top() == '*' || opor.top() == '/')) {
                    e = opor.top();
                    opor.pop();
                    postexp += e;
                }
                opor.push(ch);
            } 
            else if (isalpha(ch)) { // 处理字母，使用其ASCII值
                int ascii_val = static_cast<int>(ch); // 获取字母的ASCII码值
                postexp += to_string(ascii_val); // 转成字符串加入表达式
                postexp += "#"; // 加入标识符区分多位数字
            } 
            else { // 处理数字
                string d = "";
                while (ch >= '0' && ch <= '9') { // 遇到数字
                    d += ch;
                    i++;
                    if (i < exp.length()) {
                        ch = exp[i];
                    } 
                    else {
                        break;
                    }
                }
                i--;
                postexp += d;
                postexp += "#"; // 添加标记以区分操作数
            }
            i++;
        }
        while (!opor.empty()) {
            e = opor.top();
            opor.pop();
            postexp += e;
        }
    }

    double GetValue() {//计算后缀表达式的值
        stack<double> opand;
        double a, b, c, d;
        char ch;
        int i = 0;
        while (i < postexp.length()) {
            ch = postexp[i];
            switch (ch) {
                case '+':
                    a = opand.top(); opand.pop();
                    b = opand.top(); opand.pop();
                    c = b + a;
                    opand.push(c);
                    break;

                case '-':
                    a = opand.top(); opand.pop();
                    b = opand.top(); opand.pop();
                    c = b - a;  //注意顺序是b-a
                    opand.push(c);
                    break;

                case '*':
                    a = opand.top(); opand.pop();
                    b = opand.top(); opand.pop();
                    c = b * a;
                    opand.push(c);
                    break;

                case '/':
                    a = opand.top(); opand.pop();
                    b = opand.top(); opand.pop();
                    c = b / a; //是b/a!!
                    opand.push(c);
                    break;

                default://遇到数字
                    d = 0;
                    while (ch >= '0' && ch <= '9') {//注意数字可能有多位！!
                        d = 10 * d + (ch - '0');//这里对多位数字的处理！！
                        i++;
                        ch = postexp[i];
                    }
                    opand.push(d);
                    break;
            }
            i++;
        }
        return opand.top();
    }
};

int main() {
    int N;
    cin >> N;
    
    vector<vector<string>> vec(N, vector<string>(2)); // 初始化为N行，每行有2个string
    for (int i = 0; i < N; i++) {
        cin >> vec[i][0] >> vec[i][1];
    }

    for (int i = 0; i < N; i++) {
        Express obj1(vec[i][0]);
        Express obj2(vec[i][1]);
        obj1.Trans();
        obj2.Trans();
        double a = obj1.GetValue();
        double b = obj2.GetValue();
        if (a == b) {
            cout << "TRUE" << endl;
        } else {
            cout << "FALSE" << endl;
        }
    }

    return 0;
}
```

1. GetValue函数
- 所有运算都是b+a,b-a,b×a,b/a（**b在前**）
- 对数字的处理是难点：①数字有可能是多位，所以要循环读取`while (ch >= '0' && ch <= '9')`；②如何由逐个读取的单个数字组成一个多位数字？方法是每次将d乘以10再加上新的数字`d = 10 * d + (ch - '0');`


---

或者有更简洁的代码：
```cpp
#include <cctype> //用于提供isdigit()、isalpha()、isspace()，判断一个char字符是字母、空格还是数字

// 运算符优先级
int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    return 0;
}

// 计算值
int applyOp(int a, int b, char op) {
    switch(op) {
        case '+': return a + b;
        case '-': return a - b;
        case '*': return a * b;
        case '/': return a / b;
    }
    return 0;
}

// 计算表达式的值
int evaluate(string expr) {
    stack<int> values;  
    stack<char> ops;    
    int n = expr.length();

    for (int i = 0; i < n; i++) {
        // 当前字符是一个数字
        if (isdigit(expr[i])) {
            int val = 0;
            
            // 处理多位数字(很重要!)
            while (i < n && isdigit(expr[i])) {
                val = val * 10 + (expr[i] - '0');
                i++;
            }

            values.push(val);
            i--; // 让循环退回到正确位置
        }

        // 当前字符是一个字母
        else if (isalpha(expr[i])) {
            // 使用字母的 ASCII 值
            values.push((int)expr[i]);
            //将字母转化为ASCII值的方法————强制类型转换
        }

        // 当前字符是左括号，直接压栈
        else if (expr[i] == '(') {
            ops.push(expr[i]);
        }

        // 当前字符是右括号，处理括号内的表达式
        else if (expr[i] == ')') {
            while (!ops.empty() && ops.top() != '(') {
                int val2 = values.top(); values.pop();
                int val1 = values.top(); values.pop();
                char op = ops.top(); ops.pop();
                values.push(applyOp(val1, val2, op));
            }

            // 弹出左括号
            ops.pop();
        }

        // 当前字符是运算符
        else {
            while (!ops.empty() && precedence(ops.top()) >= precedence(expr[i])) {//记得考虑优先级，才能入栈
                int val2 = values.top(); values.pop();
                int val1 = values.top(); values.pop();
                char op = ops.top(); ops.pop();
                values.push(applyOp(val1, val2, op));
            }

            // 压入当前运算符
            ops.push(expr[i]);
        }
    }

  

    // 处理剩余的运算符
    while (!ops.empty()) {
        int val2 = values.top(); values.pop();
        int val1 = values.top(); values.pop();

        char op = ops.top(); ops.pop();
        values.push(applyOp(val1, val2, op));
    }

    // 表达式的最终结果
    return values.top();
}
```