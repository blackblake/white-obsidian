1. Each RISC-V register is 32 bits wide
2. RV32中，32bit叫做1“字(word)”
3.  x0存储的值一定是0
4. 不存在"subi"指令，因为可以在addi指令中把常数变成它的相反数，就等效于subi
5. 编译器翻译赋值语句“a=b”也是用的add指令：x0存储的值无论如何一定是0，所以add x3,x4,x0就等效于x3=x4+0=x4
6. 66666
7. 666666
8. 88888888