
### Intro

- Unlike HLL like C or Java, assembly  **cannot use variables**.
	Why not? Keep Hardware Simple
	Benefit: Assembly operands are  **registers**. They're very **fast**.
	Drawback: predetermined number of regisrers.
- **32 registers** in RISC-V
	Registers are numbered from **0 to 31**
	Each RISC-V register is **32 bits** wide

### Add/Sub
- sub x3, x4, x5   \# x3=x4+x5
- There's **no `subi` instruction**, cause it can be **replaced by`addi x3, x4, -10`**
 - There's **no `equal`  instruction**,  cause it can be **replaced by `add x3, x4, x0`** 