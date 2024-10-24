
### 4.1  
---
考虑如下指令
and rd, rs1, rs2
解释：Reg\[rd]=Reg\[rs1] AND Reg\[rs2]

**4.1.1　对于上述指令，图4-10中的控制信号各是什么数值？**
![[Pasted image 20241024204005.png]]

###### ==**在控制信号(control signals)中，值的表示方法如下：**==
- 对于ALUop：如果不记得00, 01, 10对应什么运算，可以直接写[and], [or], 
[add],[ sub]；
	
- 读/写信号(read/write)用[true] ,[false]表示；
	
- MUX的控制信号用[0],[1]表示 

**4.1.2　对于上述指令，将用到哪些功能单元？**
Registers, ALUsrc mux, ALU, and the MemToReg mux.
###### ==**`mux`也是功能单元，不要忽略**==

**4.1.3　对于上述指令，哪些功能单元不产生任何输出？哪些功能单元的输出不会被用到？**
All blocks produce some output. The outputs of DataMemory and ImmGen are not used.
###### ==无论什么指令类型，所有的功能单元都会产生输出==，只不过如果没用到的unit若没有输入，就会输出默认值



### 4.3
---
![[Pasted image 20241024205027.png]]
4.3.1　发生数据访存的指令所占比例？
**25 + 10 = 35%. Only Load and Store use Data memory**

4.3.2　发生指令访存的指令所占比例？
**100%. Every instruction must be fetched from instruction memory before it can be executed.**

4.3.3　使用符号扩展的指令所占比例？
**28 + 25 + 10 + 11 + 2 = 76%. Only R-type instructions do not use the sign extender.**

4.3.4　当不需要符号扩展的结果时，符号扩展单元的行为是什么？
**Th e sign extend produces an output during every cycle. If its output is not needed, it is simply ignored.**


### 4.4
在制造硅芯片时，材料（例如硅）缺陷和制造错误会导致电路故障。一个非常常见的故障是，信号线发生“断路”​，导致总是保持逻辑“0”​。这被称为“固定为0”故障。

**4.4.1　如果MemToReg信号发生以上故障，哪些指令会执行错误？**
Only loads are broken. MemToReg is either 1 or “don’t care” for all other instructions.

**4.4.2　如果ALUSrc信号发生以上故障，哪些指令会执行错误？**
I-type, loads , stores are all broken.


### 4.5
---
假设本周期处理器取来指令：0x00c6ba32

**4.5.1　此时ALU控制单元的输入值是多少？**
Th e encoded instruction is sd x12, 20(x13)
![[Pasted image 20241024205630.png]]
[详见真值表](obsidian://open?vault=Obsidian%20Vault&file=Images%2FPasted%20image%2020241024160153.png)

**4.5.2　该指令执行结束后新的PC地址是多少？**
Th e new PC is `the old PC + 4`.

**4.5.3　对于每一个多选器，给出执行该指令时的各个输入和输出值，列出寄存器Reg\[xn]中的数值。**
ALUsrc : 
	Inputs: Reg\[x12] and 0x0000000000000014 ; 
	Output: 0x0000000000000014

MemToReg : 
	Inputs: Reg\[x13] + 0x14 and \<undefined> ; 
	output:\<undefined>

Branch: Inputs: PC+4 and 0x000000000000000A

**4.5.4　此时ALU和另两个加法单元的输入数值是多少？**
	-ALU inputs: Reg\[x13] and 0x0000000000000014
	-PC + 4 adder inputs: PC and 4
	-Branch adder inputs: PC and 0x0000000000000028

> [!NOTE] Title
> 1）RISC-V datapath中只有 **2个**Add加法器，而且都是用于PC地址计算的：一个用来PC+4，一个用来PC+offset；
> 2）至于load/store指令的**offset(rs2)**的计算，是借助 **ALU** 的（通过一个**ALUSrs MUX**）


### 4.6
---
**4.6.1　如果要基于图4-25添加I型指令，需要额外的什么逻辑单元？**
No additional logic blocks are needed.

**4.6.2　为addi指令列出控制单元产生的信号值。**
Branch: false
MemRead: false
MemToReg: 0
**ALUop: 10 (or simply saying “[add]” is suffi cient for this problem)**
MemWrite: false
ALUsrc: 1
RegWrite: 1


### 4.7
---
![[Pasted image 20241024210924.png]]
4.7.1　R型指令的延迟是多少？（如果想让这类指令工作正确，时钟周期最少为多少？）
R-type : [30 (read PC)] + 250 + 150 + [25 (ALUrcs mux)] + 200 
\+ [25 (MemtoReg mux)] + [20 (write rd)] = 700ps

4.7.2　lw指令的延迟是多少？
lw : 30 + 250 + 150 + 25 + 200 + [250 (read Data-Memo)] + 25 + 20 = 950 ps

4.7.3　sw指令的延迟是多少？
sw : 30 + 250 + 150 + 200 + 25 + 250 = 905

4.7.4　beq指令的延迟是多少？
beq : 30 + 250 + 150 + 25 + 200 + [5 (branch信号和ALU的零输出相与)] + 25 + 20 = 705

4.7.5　算术、逻辑或者移位指令（I型，非load指令）的延迟是多少？
I-type : 30 + 250 + 150 + 25 + 200 + 25 + 20 = 700ps

4.7.6　该CPU的最小时钟周期是多少？
950ps
###### ==CPU的最小时间周期=耗时最长的一个指令所需的时间==


### 4.8
---
![[Pasted image 20241024214754.png]]

Using the results from Problem 4.7, we see that the average time per
instruction is 0.52\*700 + 0.25\*950 +0.11\*905 + 0.12\* 705 = 785.6ps

In contrast, a single-cycle CPU with a “normal” clock would require a
clock cycle time of 950.

Thus, the speedup would be 925/787.6 = 1.174

