（一）C++
1. 运行方法：Ctrl+Alt+N
2. 编译方法：Ctrl + Shift + B
3. 调试方法：F5
4. 终端：编译g++ -g `filename`.cpp -o `filename`.exe
	     运行.\\`filename`.exe
	     调试 gdb`filename`.exe
### 调试如果案件都是灰的，关闭窗口再打开试试
### 如果终端无响应，点击小垃圾筒图标重新启动新的终端


（二）Java
1. java文件的src是source code源代码的意思
2. 终端
	编译javac `filename`.java
	运行java `filename`
![[Pasted image 20240713152402.png]]

（三）如果出现“Permission Denied“：
![[Pasted image 20240821221640.png]]
这是因为进程占用，上一次的调试/运行尚未关闭就开始下一次导致的，所以在上方关闭当前进程：
![[Pasted image 20240821221757.png]]


> [!NOTE] 如何实现调试时输入？
> 只需把launch.json的"externalConsole“设置为true即可
