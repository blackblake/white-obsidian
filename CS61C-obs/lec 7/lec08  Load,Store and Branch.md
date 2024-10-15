1. 8 bit chunk is called a byte (1 word = 4 bytes)

2. Data typically smaller than 32 bits, but rarely smaller than 8 bits

3.RISC-V遵循小端法规则。整个字(word)的起始地址就是它最右边(最低有效位)的内存地址

| 15  | 14  | 13  | 12  |
| --- | --- | --- | --- |
| 11  | 10  | 9   | *8* |
| 7   | 6   | 5   | *4* |
| 3   | 2   | 1   | *0* |

4. lw  x10,12(x15):"从右向左传输"：把右边的内存地址中存储的值传输到左边的寄存器中

5. sw   x10,40(x15):"从左向右传输"：把左边的寄存器中存储的值传输到右边的内存地址中

6. lw、sw都是传输整个32bit的word(字)，而lb、sb是挑选字的某几个字节传输

7. 但是只传输字的几个bit，会导致最高有效位（符号位）的丢失；为了把正负性一同传输过去，我们会给要传输的bits做 **符号扩展**。

	![[Pasted image 20241013203011.png]]

---

8. **"w"和"b"的区别**
   ![[Pasted image 20241013203436.png]]
	地址为 ”\[x11所存储的地址值加3]“ 的内容被加载到x10的**低字节位置** 

E.g.
	![[Pasted image 20241013203717.png]]
	s**w**是把存储在x11里的整个**w**ord都存入\[x5所存储的地址值]的地址；
	l**b**只把 ”\[x5存储的地址值]+1“ 这个地址上**一字节**的内容加载到x12的**低字节**位置；

**↑上图对于理解sw和lb指令很重要！**

---
### Branch分支指令

#### (1) if-statement
`beq reg1,reg2,L1`
\# go to statement labeled L1 if (value in reg1) == (value in reg2)

#### (2) Loop

![[Pasted image 20241013205331.png]]

当i每次循环+1时，通过使x9每次+4来实现”下标递增“，但其实i并没有参与到`下标->地址`的计算中，而是只充当一个循环计数(类似cnt)的角色