
##### **`fork()`**  
---
1）作用
	用于创建新进程。当 `fork()` 被调用时，操作系统会复制当前进程，生成一个几乎一模一样的子进程。

2）返回值
1. 如果 `fork()` 成功：    
    - 在父进程中，返回新建的子进程的PID
    - 在子进程中，返回值为 0
2. 如果 `fork()` 失败，返回一个负值


#### 父进程和子进程的区别
---
1. **`fork()` 的返回值不同**
2. 子进程不会从main()函数开始执行（因此hello world 信息只输出了一次），而是直接从fork()系统调用返回，就好像是它自己调用了fork()；
3. CPU 调度程序（scheduler）决定了某个时刻哪个进程被执行，有时候父进程先执行，有时子进程先执行。


##### **`wait()`**  
---
**1）作用**
  父进程调用wait()，延迟自己的执行，直到子进程执行完毕。
  当子进程结束时，wait()才返回父进程。
  
**2）代码**

```c
int main(int argc, char *argv[])
{
    printf("hello world (pid:%d)\n", (int) getpid());
    
    int rc = fork();
    
    if (rc < 0) { // fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
    } else if (rc == 0) { // child (new process)
        printf("hello, I am child (pid:%d)\n", (int) getpid());
    } else { // parent goes down this path (main)
        int wc = wait(NULL);
        printf("hello, I am parent of %d (wc:%d) (pid:%d)\n",
               rc, wc, (int) getpid());
    }
    
    return 0;
}
```

**注意：这个程序是如何让子程序与父程序执行不同的操作的？**
答：通过`rc`，获取`fork()`的返回值，根据返回值的不同确认现在是处于哪个程序，然后用`if`语句来执行不同操作。

输出：
```
//父进程的输出,在fork调用之前,所以子进程不会输出
hello world (pid:29266) 
//父进程调用wait,先执行子进程
hello, I am child (pid:29267)
//子进程执行完毕,执行父进程
hello, I am parent of 29267 (wc:29267) (pid:29266)
```


#### 进程映像
---
进程映像（Process Image）是进程在内存中的完整表示，包括：

1. 内存组成部分
    
    - 代码段（Text Segment）：程序的机器指令
    - 数据段（Data Segment）：全局变量和静态变量
    - 堆（Heap）：动态分配的内存
    - 栈（Stack）：局部变量、函数调用信息
    - 命令行参数和环境变量
2. 关键特征
    
    - 唯一标识一个正在运行的程序实例
    - 包含程序运行所需的所有内存资源
    - 由操作系统管理和维护
3. 映像变化
    
    - `fork()` 会复制进程映像
    - `exec()` 系列函数会完全替换进程映像

简单类比：

- 进程映像 = 程序的"内存快照"
- 类似于计算机中程序的"运行状态"


#### `exec()`
---
1. 作用
exec() 系列函数用新的进程映像替换当前进程映像。 它将程序加载到当前进程空间，并从入口点开始运行。对 exec()的成功调用永远不会返回。


