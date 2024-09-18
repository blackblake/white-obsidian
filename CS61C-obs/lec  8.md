1. 8 bit chunk is called a byte (1 word = 4 bytes)
2. Data typically smaller than 32 bits, but rarely smaller than 8 bits
3. Word address is same as address of rightmost byte–least-significant byte
(i.e. Little-endian convention)
   `RISC-V遵循小端法规则。整个字(word)的起始地址就是它最右边(最低有效位)的内存地址`

| 15  | 14  | 13  | *12* |
| --- | --- | --- | ---- |
| 11  | 10  | 9   | *8*  |
| 7   | 6   | 5   | *4*  |
| 3   | 2   | 1   | *0*  |

4. lw  x10,12(x15):"从右向左传输"：把右边的内存地址中存储的值传输到左边的寄存器中
5. sw   x10,40(x15):"从左向右传输"：把左边的寄存器中存储的值传输到右边的内存地址中

6. lw、sw都是传输整个的word(字)，而lb、sb是挑选字的某几个字节传输
7. 但是只传输字的几个bit，会导致最高有效位（符号位）的丢失；为了把正负性一同传输过去，我们会给要传输的bits做 **符号扩展**。
   
   
