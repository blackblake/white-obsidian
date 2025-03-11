
## sleep.c
---
1. **Look at some of the other programs in user/ (e.g., user/echo.c, user/grep.c, and user/rm.c) to see how you can obtain the command-line arguments passed to a program.**

```c
int main(int argc, char *argv[])
```
main函数接受两个命令行参数：
- `argc`：命令行参数的数量（包括程序名称本身，如echo）；
- `argv`：一个字符串数组，包含所有的命令行参数。`argv[0]`是程序名称，`argv[1]`是第一个参数，以此类推；

2. **If the user forgets to pass an argument, sleep should print an error message.**

```c
if(argc <= 1){//如果只输入了程序名称argv[0]或者连程序名称都没有
	fprintf(2, "usage: sleep seconds\n");//打印错误信息
	exit(1);
}
```

3. **The command-line argument is passed as a string; you can convert it to an integer using `atoi`.**
	
	`char *argv[]`就是字符串数组的形式。


## pingpong.c
---
1. **怎么指定管道的读端p\[0]和写端p\[1]？**
	
- `p[0]`和`p[1]`是通过`pipe(p)`函数自动分配的文件描述符，你无需手动指定它们的值（注意不要忘了写pipe(p)）。
- 调用`pipe(p)`时，系统会创建一个管道，并将两个文件描述符放入数组`p`中作为端p\[0]和写端p\[1]


2. **write(p\[1], ,1);我只想写一个字节，那缓冲区要设置为什么？**
	
	可以这样写：`write(p[1], "A", 1);`

3. **初始化**
```c
	int pid = fork();//用于在之后的if分支语句中区分 父进程/子进程/fork错误

    char buf[1]={'a'};//管道的缓冲区，按本题规定只有1个字节 //记得要初始化

    int p1[2];//p数组的元素就是读端和写端的[文件描述符]
    pipe(p1);//从parent向child写入的管道

    int p2[2];
    pipe(p2);//从child向parent写入的管道
```

4. **父进程**
```c
if(pid>0){//parent process
    close(p1[0]);// 关闭不需要的读端
    close(p2[1]);// 关闭不需要的写端

    write(p1[1],buf,1);

    // 等待子进程结束，确保顺序(wait必须写在write之后，read之前！)
    wait(0);

    read(p2[0],buf,1);
    printf("%d: received pong\n", getpid());

    close(p1[1]);//写完之后要把所写入的写端关闭
    close(p2[0]);//读完之后要把所读入的读端关闭

    exit(0);
}
```

==**关闭端口：**==
- 读写之前，要把本进程没用到的管道端口全都关闭；
- 读写之后，要把本进程用到过的管道端口全都关闭；
- 总之，只要是在这个程序中之前创建过的管道，所有管道的端口都必须在每个进程(包括父/子进程)中被关闭一次；

==**等待子进程**==
第一次试运行pingpong程序的时候，我发现输出混乱如下：
```zsh
56::  rreecceeiivveedd p ipnogn g
```
- 这是因为父进程和子进程的运行混到一起了，原因是忘了在父进程中用`wait(0)`等待子进程；
- 并且，`wait(0);`必须写在父进程想要让子进程先运行的那个时间点，在本题中就是父进程write之后、read之前，**若`wait(0;`的地方不对，那么输出仍然是混乱的**！

==**子进程别忘了exit(0);**==

5. **fork错误情况**
```c
else { printf("fork error\n"); exit(1); }
```
