# RISC-V INSTRUCTION TYPES
## THE RISC-V HAS VARIOUS TYPES OF INSTRUCTION FORMATSS, WHICH CAN BE GROUPED INTO TWO, WHICH ARE:
### 1) BASE FORMATS/TYPES
In the RV32I ISA, there are four core instruction types formats, which are R-type, I-type, S-type, U-type. Each of them are 32-bit in length. 
The base ISA has IALIGN=32, meaning that instructions must be aligned on a four-byte boundary in memory.
If there is an misalignment an exception is taken such it branches or there will occur an unconditional jump , techincally *instruction-address-misaligned exception.
**The various RISC-V instruction formats are as in the figure below:**
![Screenshot 2025-01-02 205446](https://github.com/user-attachments/assets/c6638638-85e6-4a38-85ff-998bbf609245)
In the illustration, we observe source registers labeled as rs (these registers temporarily store input values required for specific operations or instructions) and destination registers identified as rd (this register holds the computed result of the instruction execution). The field funct3 is a 3-bit segment (segments are defined portions of an instruction containing vital data for the processor's execution process), while funct7 comprises a 7-bit segment. The opcode (a command indicating the specific operation the processor should execute) and imm[x:y] (an immediate value embedded directly in the instruction, derived from bits spanning positions y to x) are essential parts of the instruction.

Notably, the RISC-V Instruction Set Architecture (ISA) maintains the source registers (rs1, rs2) and destination register (rd) at consistent positions across all instruction formats, which facilitates straightforward decoding.
