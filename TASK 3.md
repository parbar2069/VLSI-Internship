#RISC-V INSTRUCTION TYPES
##THE RISC-V HAS VARIOUS TYPES OF INSTRUCTION FORMATSS, WHICH CAN BE GROUPED INTO TWO, WHICH ARE:
### 1) BASE FORMATS/TYPES
In the RV32I ISA, there are four core instruction types formats, which are R-type, I-type, S-type, U-type. Each of them are 32-bit in length. 
The base ISA has IALIGN=32, meaning that instructions must be aligned on a four-byte boundary in memory.
If there is an misalignment an exception is taken such it branches or there will occur an unconditional jump , techincally *instruction-address-misaligned exception.
**The various RISC-V instruction formats are as in the figure below:**
