1. 只能从**桌面快捷方式**打开vscode, 否则找不到插件安装目录
2. mingw(gcc, g++, gdb)安装位置："D:\MSYS2\ucrt64\bin"
3. 报错如下
```
PS D:\mycodes> g++ hello.cpp Assembler messages: Fatal error: can't create C:\Users\
```
**解决方案：**`g++` 在编译时会创建临时文件，通常存放在 `C:\Users\<你的用户名>\AppData\Local\Temp` 目录。如果该目录不存在，或者你没有写入权限，可能会导致错误。

- 运行以下命令，显式指定一个可写的 `TEMP` 目录：
```
$env:TMP="D:\Temp"
$env:TEMP="D:\Temp"
g++ hello.cpp
```
然后再运行：
```
g++ -o hello.exe hello.cpp

.\ hello.exe
```


---
##### Note: D盘里原来就有一个Temp文件夹,里面有一个BT_DFU_FW文件夹, 上述修改后可能对C, D盘都有影响

 **1. 对 `D:\Temp\BT_DFU_FW` 的影响**

✅ **不会影响已有文件**：

- 修改 `TEMP` 和 `TMP` 只是让 Windows 和应用程序在 `D:\Temp` 存放临时文件，不会删除原来的 `BT_DFU_FW` 目录。
- 但是，如果某些程序会清理整个 `TEMP` 目录（例如某些清理工具），可能会误删 `BT_DFU_FW`，所以 **建议先备份 `BT_DFU_FW` 目录**。

📌 **解决方案**：

- **最安全的方法**：在 `D:\Temp` 里面新建一个专门的临时文件夹，比如 `D:\Temp\WinTemp`，然后把 `TEMP` 和 `TMP` 设置为它：
    
    ```powershell
    [System.Environment]::SetEnvironmentVariable("TEMP", "D:\\Temp\\WinTemp", "User")
    [System.Environment]::SetEnvironmentVariable("TMP", "D:\\Temp\\WinTemp", "User")
    ```
    
    这样，`D:\Temp\BT_DFU_FW` 目录不会受到影响。

---

### **2. 对 C 盘临时文件的影响**

✅ **C 盘不会再积累新的临时文件，但已有的不会自动删除**：

- 你修改 `TEMP` 和 `TMP` 后，**新创建的临时文件会存放到 `D:\Temp`，但 C 盘已有的临时文件不会自动转移**。
- Windows 和一些软件（如浏览器、解压缩工具）仍可能使用 `C:\Windows\Temp` 作为某些特殊任务的临时存储，但大多数软件会遵循 `TEMP` 变量的设定。

📌 **解决方案**：

- **手动清理 C 盘已有的临时文件**： 在 **PowerShell（管理员模式）** 下运行：
    
    ```powershell
    Remove-Item -Path "C:\Users\你的用户名\AppData\Local\Temp\*" -Force -Recurse
    ```
    
    或者使用 Windows 自带的 **磁盘清理**（Win + R → 输入 `cleanmgr` → 选择 `C:` → 勾选“临时文件”）。

---

### **3. 对系统稳定性的影响**

✅ **大多数情况下不会有问题**，但如果某些软件或 Windows 更新强制依赖 `C:\Users\XXX\AppData\Local\Temp`，可能会导致问题，比如：

- 某些旧软件硬编码了 `C:\Users\XXX\AppData\Local\Temp`，如果它们不遵循 `TEMP` 变量，可能会报错。
- Windows 更新时可能会短暂使用 `C:\Windows\Temp`，但影响不大。

📌 **解决方案**：

- 如果遇到问题，可以临时把 `TEMP` 变量改回 `C:\Users\XXX\AppData\Local\Temp`：
    
    ```powershell
    [System.Environment]::SetEnvironmentVariable("TEMP", "C:\\Users\\你的用户名\\AppData\\Local\\Temp", "User")
    [System.Environment]::SetEnvironmentVariable("TMP", "C:\\Users\\你的用户名\\AppData\\Local\\Temp", "User")
    ```
    

---

## **结论**

|影响|详细情况|解决方案|
|---|---|---|
|✅ **对 `BT_DFU_FW` 目录**|**不会直接删除**，但清理工具可能误删|**建议把 `TEMP` 设为 `D:\Temp\WinTemp`**|
|✅ **对 C 盘临时文件**|**不会自动转移旧文件，新文件不会存 C 盘**|**可以手动清理 `C:\Users\XXX\AppData\Local\Temp`**|
|⚠️ **对某些软件**|**极少数旧软件可能仍依赖 `C:\Users\XXX\AppData\Local\Temp`**|**如有问题，可临时改回原路径**|

📌 **推荐做法**：

4. **使用 `D:\Temp\WinTemp` 而不是 `D:\Temp`**，避免影响已有文件：
    
    ```powershell
    [System.Environment]::SetEnvironmentVariable("TEMP", "D:\\Temp\\WinTemp", "User")
    [System.Environment]::SetEnvironmentVariable("TMP", "D:\\Temp\\WinTemp", "User")
    ```
    
5. **手动清理 `C:\Users\XXX\AppData\Local\Temp`** 以节省空间：
    
    ```powershell
    Remove-Item -Path "C:\Users\你的用户名\AppData\Local\Temp\*" -Force -Recurse
    ```
    
6. **如果遇到问题，恢复默认 `TEMP` 路径**：
    
    ```powershell
    [System.Environment]::SetEnvironmentVariable("TEMP", "C:\\Users\\你的用户名\\AppData\\Local\\Temp", "User")
    [System.Environment]::SetEnvironmentVariable("TMP", "C:\\Users\\你的用户名\\AppData\\Local\\Temp", "User")
    ```
    

这样修改后，你可以放心使用 `D:\Temp` 作为临时目录，同时不会影响 C 盘已有文件，也不会影响 `BT_DFU_FW` 目录。🚀