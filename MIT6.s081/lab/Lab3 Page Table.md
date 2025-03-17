
1. 启动`qemu`和`gdb`: `make CPUS=1 qemu-gdb`，当前的这个终端窗口我们暂且称呼其为窗口1
2. 打开另一个终端窗口2，还是在xv6-labs-2021目录下，执行`gdb kernel/kernel`
3. 在gdb中输入`tui enable`可以进入源代码展示窗口，可以用`先按下Ctrl+x，然后按下Ctrl+a`来退出
4. 如果您想完全退出GDB程序：在GDB提示符下输入 `quit` 或简写 `q
5. 还记得窗口1吗？`vmprint`函数会在这个窗口进行输出，因为这个窗口1显示的是**QEMU的控制台输出**
	 ![[Pasted image 20250317143335.png|500]]
	 我们来看一下这里的输出。第一行是最高一级page directory的地址，这就是存在SATP或者将会存在SATP中的地址。第二行可以看到最高一级page directory只有一条PTE序号为0，它包含了中间级page directory的物理地址。第三行可以看到中间级的page directory只有一条PTE序号为128，它指向了最低级page directory的物理地址。第四行可以看到最低级的page directory包含了PTE指向物理地址。你们可以看到最低一级 page directory中PTE的物理地址就是0x10000000，对应了UART0（因为到此为止程序只执行了一条kvmmap语句`kvmmap(UART0, UART0, PGSIZE, PTE_R | PTE_W);`）

