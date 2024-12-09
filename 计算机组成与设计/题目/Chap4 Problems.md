
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
所有块都会产生一些输出，但不使用datammemory和ImmGen的输出。
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
Branch: false（**除了mux的0和1之外的控制信号都用==false==和==true==来表示是否有效，因为不一定低电平有效还是高电平有效**）
MemRead: false
MemToReg: 0（MemToReg是**从ALU输出和DataMem输出中二选一**写回寄存器的mux）
**ALUop: 10 (or simply saying “==add==” is suffi cient for this problem)**
MemWrite: false
ALUsrc: 1
RegWrite: 1


### 4.7
---
![[Pasted image 20241024210924.png]]
4.7.1　R型指令的延迟是多少？（如果想让这类指令工作正确，时钟周期最少为多少？）
R-type : [30 (read PC)] + 250 + 150 + [25 (ALUrcs mux)] + 200 
\+ [25 (MemtoReg mux)] + [20 (write rd)] = 700ps
`1）注意不要忘了mux的延迟`
`2）ImmGen和Adder的延迟为什么不加进来算？`

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


### 4.9
---
如果在图4-25的CPU中添加一个乘法器，这将给ALU增添300ps的延迟。但是，由于不再需要对乘法指令进行模拟，指令数量将会减少5%。
4.9.1　改动前后，时钟周期分别是多少？
before: 950; 
after: 1250

4.9.2　通过这个改动，将获得多少加速比？
Th e running time of a program on the original CPU is 950\*n.
The running time on the improved CPU is 1250\*(0.95)\*n = 1187.5. Th us, the “speedup” is 0.8.

4.9.3　在保证提升性能的条件下，新ALU最低频率是多少？
Because adding a multiply instruction will remove 5% of the instructions,
the cycle time can grow to as much as 950/(0.95) = 1000. Th us, the time
for the ALU can increase by up to 50 (from 200 to 250)

### 4.10
---
设计者对处理器数据通路进行改造，通常会根据性价比做出方案折中。在下面三个问题中，若以图4-25为数据通路的改造基础，各单元延迟参考练习4.7，成本如下：
![[Pasted image 20241025171904.png]]
4.10.1　增加这样的改进后，获得的加速比为多少？
Th e additional registers will allow us to remove 12% of the loads and
stores, or (0.12)\*(0.25 + 0.1) = 4.2% of all instructions. Thus, the time to
run n instructions will decrease from (950\*n) to (960\*0.958\*n) = 919.68*n.
That corresponds to a speedup of 950/919.68 = 1.03.

4.10.2　比较性能上的变化和成本上的变化。
  The cost of the original CPU is 4507; the cost of the improved CPU is 4707.
PC: 5
I-Mem: 1000
Register file: 200
ALU: 100
D-Mem: 2000
Sign Extend: 1002
Controls: 10002
adders: 30\*24
muxes: 4\*102
single gates: 2\*1
  Thus, for a 3% increase in performance, the cost of the CPU increases by
about 4.4%.


### 4.11
---
4.11　尝试添加RISC-V中的指令：lwi.d rd, rs1, rs2（地址自增的load指令）​。指令释义：Reg\[rd]=Mem\[Reg\[rs1]+Reg\[rs2]​]
4.11.1　对于这条指令，需要添加的新功能部件是什么？
No new functional blocks are needed.

4.11.2　现有的哪些功能部件需要改造？
Only the control unit needs modifi cation.

4.11.3　对于这条指令，需要新添加的数据通路是什么？
No new data paths are needed.

4.11.4　为支持这条指令，为控制单元新添加的控制信号有哪些？
No new signals are needed.


### 4.12
---
尝试添加RISC-V中的指令：swap rs1, rs2。指令释义：Reg\[rs2]=Reg\[rs1]​；Reg\[rs1]=Reg\[rs2]
4.12.1　对于这条指令，需要添加的新功能部件是什么？
No new functional blocks are needed.

4.12.2　现有的哪些功能部件需要改造？
The register fi le needs to be modifi ed so that it can write to two registers
in the same cycle. Th e ALU would also need to be modifi ed to allow read
data 1 or 2 to be passed through to write data 1.

4.12.3　对于这条指令，需要新添加的数据通路是什么？
Th e answer depends on the answer given in 4.12.2: whichever input was
not allowed to pass through the ALU above must now have a data path to
write data 2.

4.12.4　为支持这条指令，为控制单元新添加的控制信号有哪些？
There would need to be a second RegWrite control wire.


### 4.13
---
尝试添加RISC-V中的指令：ss rs1, rs2, imm（存储两数之和）​。指令释义：Mem\[Reg\[rs1]​]=Reg\[rs2]+immediate
4.13.1　对于这条指令，需要添加的新功能部件是什么？
We need some additional muxes to drive the data paths discussed in 4.13.3.

4.13.2　现有的哪些功能部件需要改造？
No functional blocks need to be modifi ed.

4.13.3　对于这条指令，需要新添加的数据通路是什么？
Th ere needs to be a path from the ALU output to data memory’s write
data port. Th ere also needs to be a path from read data 2 directly to Data
memory’s Address input.

4.13.4　为支持这条指令，为控制单元新添加的控制信号有哪些？
Th ese new data paths will need to be driven by muxes. Th ese muxes will
require control wires for the selector.

![[Pasted image 20241105190145.png]]


### 4.14
---
对于哪些指令，立即数产生单元(Imm Gen block)处于关键路径上？
None: all instructions that use sign extend also use the register file, which
is slower.


### 4.15
---
4.15　从4.4节可知，lw是CPU中延迟最长的一条指令。如果修改lw和sw的功能，去掉地址偏移，比如lw或sw指令的访存地址只能使用计算后存放于rs1中的数值，那么就不会有指令同时使用ALU和数据存储，这将缩短时钟周期。但是这也会增多指令数目，因为很多lw和sw指令将会被lw/add或者sw/add组合取代。

4.15.1　修改后的时钟周期是多少？
Th e new clock cycle time would be 750. ALU and Data Memory will now
run in parallel, so we have effectively removed the faster of the two (the
ALU with time 200) from the critical path.

4.15.2　在这款新CPU上运行如练习4.7中的指令组合，将会变快还是变慢？大概变化多少？（为简化起见，假设每条lw和sw指令将被两条指令的组合取代）
Slower. The original CPU takes 950\*n picoseconds to run n instructions.
The same program will have approximately 1.35\*n instructions when
compiled for the new machine. Th us, the time on the new machine will be
750\*1.35n = 1012.5\*n. Th is represents a “speedup” of 0.93.

4.15.3　在新款CPU上使程序运行速度变快或者变慢的主要因素是什么？
The number of loads and stores is the primary factor. How the loads and
stores are used can also have an effect. For example, a program whose
loads and stores tend to be to only a few diff erent address may also run
faster on the new machine.

### 4.19
---
问x15的最终值是多少，而x15只在第四条指令中被写入；x15的值取决于x11的值，而第一条和第四条指令可能存在数据冒险，所以看图：
![[Pasted image 20241105193156.png]]
 重点在**CC 5**即第五周期，在这个周期里**同时发生**了 “***第一条指令将x11的新值写入Reg***(寄存器堆)” 和 “***第四条指令从Reg中读取x11的值***” 这两件事，而题目说 **“先写后读”**，所以当第四条指令读取x11的时候，x11已经被第一条指令写入新的值了，所以没有发生数据冒险。