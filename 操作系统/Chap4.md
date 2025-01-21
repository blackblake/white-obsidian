
#### 使用bash运行.py文件
---
`floydwhite@DESKTOP-81VN84D:/mnt/d/OStep$ python3 ./process-run.py -l 5:100,5:100 -c`

#### 命令行参数：-h
---
   `-h` 是一个常用的命令行参数，代表 "help"，用于
- 显示程序的使用说明
- 列出可用的命令行选项
- 提供简要的使用指南

例如
```
$ ./process-run.py -h
Usage: process-run.py [options]

Options:
  -h, --help            show this help message and exit
  -s SEED, --seed=SEED  the random seed
  -P PROGRAM, --program=PROGRAM
                        more specific controls over programs
  -l PROCESS_LIST, --processlist=PROCESS_LIST
                        a comma-separated list of processes to run, in the
                        form X1:Y1,X2:Y2,... where X is the number of
                        instructions that process should run, and Y the
                        chances (from 0 to 100) that an instruction will use
                        the CPU or issue an IO (i.e., if Y is 100, a process
                        will ONLY use the CPU and issue no I/Os; if Y is 0, a
                        process will only issue I/Os)
  -L IO_LENGTH, --iolength=IO_LENGTH
                        how long an IO takes
  -S PROCESS_SWITCH_BEHAVIOR, --switch=PROCESS_SWITCH_BEHAVIOR
                        when to switch between processes: SWITCH_ON_IO,
                        SWITCH_ON_END
  -I IO_DONE_BEHAVIOR, --iodone=IO_DONE_BEHAVIOR
                        type of behavior when IO ends: IO_RUN_LATER,
                        IO_RUN_IMMEDIATE
  -c                    compute answers for me
  -p, --printstats      print statistics at end; only useful with -c flag
                        (otherwise stats are not printed)
```


#### 运行两个程序的指令
---
`prompt> ./process-run.py -l 5:100,5:100`即给出两个5:100

#### 命令行参数-c
---
`-c` 即compute，该"计算"可能包括：
1. 进程执行序列模拟
2. CPU和IO时间分配
3. 进程切换开销
4. 总体系统执行时间
5. 各进程执行统计

例如，对于参数 `-l 3:50,4:30`：
- 第一个进程：3条指令，50%为CPU指令
- 第二个进程：4条指令，30%为CPU指令
`-c`会计算并展示这些进程的具体执行过程和统计结果。

#### CPU和IO
---
互补关系，- CPU像厨师快速烹饪，IO像服务员取食材和上菜；
指令可以粗略地分为：CPU指令（计算）与 IO指令（等待）

#### PID
---
Process ID (PID) 是操作系统中唯一标识进程的编号，系统用它区分不同进程；
- 范围：0-32768或更大
- 可重用
- 顺序分配
- 还有跟踪进程状态、控制进程、CPU调度等功能

![[Pasted image 20250114160752.png]]
运行两个进程，使用-l、-c参数，两个进程的PID分别顺序分配为0和1，注意看其状态

在C语言中可以用getpid()函数得到当前进程的PID值

#### -l和-L的区别
---
- `-L`是指定每次IO操作需要的时间，比如`-L 5` 表示每次IO操作持续5个时间单位

- 如果不使用 -L 参数指定IO长度，程序会使用预设的默认IO时长，通常是5个时间单位，这是硬编码在脚本中的默认值

#### IO会导致进程阻塞等待（blocked）
---
进程需要IO获取/存储数据，而IO由进程发起，会导致进程暂停等待。

![[Pasted image 20250114162533.png]]
比如上例运行一条指令，它只发起IO，CPU处于空闲，可以观察其进程状态，只在发起IO和IO结束时为RUN，中间为BLOCKED。


#### -p
---
- 打印一些stats，如total time、CPU busy time（CPU利用率）、 IO Busy time
- 

