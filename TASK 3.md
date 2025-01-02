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
### 2) IMMEDIATE ENCODING VARIANTS
There are a further two variants of the instruction formats   based on the handling of immediates namely B-Type and J-Type:
![image](https://github.com/user-attachments/assets/8b017a64-2069-4943-a6de-2c182e3ff97b)
![image](https://github.com/user-attachments/assets/f5a74c32-3356-4b09-87c6-d9f5a5989c8a)
### **R-TYPE**
The R-Type is an instruction format in the RISC-V architecture to perform arithmetic and logical operations using registers.
From the first picture
opcode(6-0)  identifies the operation to be performed (add,subtract,etc),
rd(11-7) is the destination register  where the result is stored,
funct3(14-12) specifies the operation within the opcode category,
rs1(19-15) indicates the first source register that contains one of the operands for the operation,
rs2(24-20) indicates the 2nd source register that contains the other operand for the operation,
funct7(31-25) provides the operation, aloowing for more variations of instructions that share the same opcode and funct3 values.
### **I-Type** :
The S-Type instruction format in the RISC-V architecture is specifically designed for store operations, which involve writing data from registers to memory.
opcode(6-0) identifies the operation to be performed (SW - for store word),
imm[4:0] (11-7) these 5 bits of the immediate value specifies an offset *(an offset is an adjustment made to an address)* to be added to the address in rs1,
funct3(14-12) specifies the type of storage operation,
rs1(19-15) first source register that contains the base address where data will be stored,
rs2(24-20) 2nd source register that contains the data to be stored in memory,
imm[11:5] (31:25) 7 bits of upper immediate value which complete 12 bit immediate value used as an offset.
### **U-Type** :
The U-Type instruction format in the RISC-V architecture is designed for operations that involve a 20-bit immediate value, primarily used for loading upper immediate values into registers. 
opcode(6-0) identifies the operation to be performed (LUI - for loading an upper immediate value),
rd(11-7) destination register where the result of the operation will be stored,
imm[31:12] (31:12) 20 bit immediate value which is loaded into the upper part of the destination structure.
### **B-Type** :
The B-Type instruction format in the RISC-V architecture is primarily used for branch instructions, which control the flow of a program based on certain conditions.
opcode(6-0) identifies the operation to be performed (BEQ - equals to , BNE - not equal to),
funct3(14-12) specifies the type of branch operation,
rs1(19-15) is a first source register that contains one of the oprands for the comparision,
rs2(24-20) the 2nd register that contains the operand for the comparision,
imm[4:1] (11:7) the lower bits of the immediate value used as part of the offset,
imm[10:5] (30:25) the middle bits of the immediate value which are also part of the offset for branching,
imm(1 bit) sign bit of the immediate value, which is used to complete the 13-bit immediate offset for branching.
### **J-Type** :
the J-Type instruction format in the RISC-V architecture is specifically used for jump instructions, which allow for unconditionally transferring control to a new instruction address.
opcode(6-0) identifies the operation to be performed (JAL - jump and link),
rd(11-7) destination register where the return address will be stored,
imm[19:12] (31:12) middle bits of the immediate value which are part of the target address to jump to,
imm(1 bit) sign bit of the immediate value,which helps in determining the direction of the jump,
imm[10:1] (30:21) lower bits of the immediate value which are also part of the target address for jumping,
imm(1 bit) this bit is included to help with sign extension when calculating the jump address.
# Identifying atleast 15 of these instructions in a typical code

The C code is : 

          #include <stdio.h>

          int main() {
              int x = 5; // Number to compute the factorial
              int i;
              int factorial = 1; // Initialize factorial to 1
          
              for(i = 1; i <= x; i++) {
                  factorial *= i; // Multiply factorial by i in each iteration
              }
          
              printf("Factorial of %d is %d\n", x, factorial);
              return 0;
          }

Now we take the obj-dump of this code which is : 

![Screenshot 2024-12-27 141314](https://github.com/user-attachments/assets/0cb613ca-5116-44bd-ae6a-181309f1ebc1)

We will start to nalayze unique instructions ,

### 1)The first instruction

           10184: ff010113 addi sp, sp, -16

the number 10184 represents the address 


the opperation **addi** adds an immediate value to a register ,

**sp** is the stack pointer register (stack pointer is a special-purpose register that holds the memory address of the top of the stack)

and -16 indicates the immidiate value to add to the current value 

Thus we can conclude this is an I-Type instruction


These are  assembly language neumonics where the hexamdecimal instruction is given by **ff010113** which when conveted to binary is as follows :

Hex: `f f 0 1 0 1 1 3`

Binary: `1111 1111 0000 0001 0000 0001 0001 0011`

Code : `-16=111111110000, rs1=00010 ,funct3=000 ,rd=00010 ,opcode=0010011`


### 2)Now we move on to next instruction 

       10188: 00113423     sd     ra,8(sp) 

here the address is 10188 , 

**sd** is the store double which as the name suggests stores a double,

**ra** is the source register which contains the value to be stored ,
  
**8(sp)** indicates the offset of 8 to be  added to the surrent value in stack pointer register

Thus, we can conlude it is an s type of instruction 

These are  assembly language neumonics where the hexamdecimal instruction is given by **00113423** which when conveted to binary is as follows :

Hex: `0 0 1 1 3 4 2 3`

Binary: `0000 0000 0001 0001 0011 0100 0010 0011`

Code: `immediate = 0000000 ,rs2=00001 , rs1=00010 , funct3 =011, imm= 01000 ,Opcode = 0100011` 
       

### 3) The next instruction is 

            1018c: 03c00793 li a5,60
here the address is 1018c ,

**li** is a pseudo-instruction *(A pseudo-instruction is an assembly language command that does not correspond directly to a machine instruction. Instead, it simplifies programming by translating into one or more actual machine instructions during assembly.)* to load a constant value directly into a register

**a5** is a destination register where the immediate value is loaded

**60** is the immediate value

Thus we can conclude it is I-Type as the instruction involve one register as destination and one immediate value

These are  assembly language neumonics where the hexamdecimal instruction is given by **03c00793** which when conveted to binary is as follows :

Hex: `0 3 c 0 0 7 9 3`

Binary: `0000 0011 1100 0000 0000 0111 1001 0011`

Code : `immediate = 0000 0011 1100 ,rs1 = 00000,funct3 = 000,rd = 01111,Opcode = 0010011` 

### 4) The next instruction is 

            10190: fff7879b addiw a5,a5,-1  

here the address is 10190 ,

**addiw** indicates add immediate word,which adds a sign-extended 12-bit immediate to a 32-bit register

**a5** is both the source and destination register

**-1** is the immediate value to be added to the contents of register of  a5

Thus, it is a I-type instruction

These are  assembly language neumonics where the hexamdecimal instruction is given by **03c00793** which when conveted to binary is as follows :

Hex: `f f f 7 8 7 9 b`  

Binary: `1111 1111 1111 0111 1000 0111 1001 1011`

Code : `immediate = 1111 1111 1111 , rs1 = 01111 , funct3 = 000 , rd = 01111 , Opcode = 0011011` 


### 5) The next instruction is 

             10194: fe079ee3 bnez a5,10190 <main+0xc>

here the address is 10194 ,

**bnez** stands for branch not equal to zero which checks if the value in register a5 is non zero

if not equal to 0 it branches out to the address 10190 

**<main+0xc>** specifies an address that is 12 bytes beyond the start of the main function.

Thus , it is a B-type

Hex: `f e 0 7 9 e e 3`  

Binary: `1111 1110 0000 0111 1001 1110 1110 0011`

Code : `immmediate = 1 111111, rs2=00000 , rs1 = 01111 , funct3 = 001 ,imm = 1110 1  ,opcode = 1100011` 

### 6) The next unique instruction is 

             101a0: 00021537 lui a0,0x21

**lui** is used to load 20 bit immediate value into the upper 20 bits of a0 register 

**0x21** is the value which will be loaded 

Thus, its an U-type  instruction 

Hex: `0 0 0 2 1 5 3 7`  

Binary: `0000 0000 0000 0010 0001 0101 0011 0111`

Code : `imm = 0000 0000 0000 0010 0001 , rd = 01010  ,Opcode = 0110111 ` 

### 7) The next unique instruction is 

       101a8: 26c000ef jal ra,10414 <printf>

here the address is 101a8 ,

**jal** is "jump and link" , a instruction to jump to a specific address and simultaneously store the return address 

**ra** is the return address and 10414 is the target address(corresponding to the function printf)

Thus,it is a J-type instruction

Hex: `2 6 c 0 0 0 e f`  

Binary: `0010 0100 1100 0000 0000 0000 1110 1111`

Code : `imm = 0 0100100110 , imm = 0 0000 0000 ,rd = 00001 , Opcode = 110 1111 ` 

### 8) The next unique instruction is 

       101b0: 00813083 ld ra,8(sp)

here the address is 101b0 ,

**ld** is used to load a 64-bit value from memory into a specific refister

**ra** is the register where the value at address sp is at with offset of 8

This is I-Type instruction

Hex: `00813083`  

Binary: `0000 0000 1000 0001 0011 0000 1000 0011 `

Code : `imm = 0000 0000 1000, rs1 = 00010 , funct3 = 011 , rd = 00001 , Opcode = 0000011` 

### 9) The next unique code is 

          101b8: 00008067 ret 

here the address is 101b8 ,

**ret** is also an pseudo instruction , which is a short for jalr x0 , ra , 0 . This means that when the ret instruction is executed, it effectively jumps to the address stored in the return address register (ra, which is register x1) and sets the program counter (PC) to that address.

This is a J-Type instruction as it contains an immediate , destination register and an opcode .

Hex: `00008067`  

Binary: `0000 0000 0000 0000 1000 0000 0110 0111  `

Code : `immediate = 0 0000000000 0 00001000 , rd = 00000 , opcode =  1100111` 

### 10) The next unique code is 

              101bc: 00050593   mv  a1,a0

here the address is 101bc ,

The mv (move) instruction is a pseudo-instruction in RISC-V . 

This is an I - Type .

Hex: `0 0 0 5 0 5 9 3 `  

Binary: `0000 0000 0000  0101 0000 0101 1001 0011 `

Code : `immediate = 0000 0000 0000 , rs1 = 01010 , funct3 = 000 , rd = 01011 , Opcode = 0010011 ` 

### 11) The next unique code is 

             101c0: 00000693 li a3,0
here the address is 101c0 ,

**li** is again load immediate which loads 0 to register a3 .

This is an I - Type instruction.

Hex: `0 0 0 0 0 6 9 3 `  

Binary: `0000 0000 0000 0000 0000 0110 1001 0011 `

Code : `immmediate = 0000 0000 0000 , rs1 =  00000 , funct3 = 000 , rd = 01101 , opcode = 0010011` 


### 12) The unique instruction is 

           101cc: 4390206f j 12e04 <__register_exitproc>

The address 101cc .

**j** (jump) instruction is sed to perform an unconditional jump to a specificed address.

12e04 is the memory address where the function is .

<__register_exitproc> is the name of the function being refferd to .


This is an J-Type instruction .

Hex: `4 3 9 0 2 0 6 f`  

Binary: `0100 0011 1001 0000 0010 0000 0110 1111`

Code : `immmediate = 0 1000011100 1 00000010 , rd = 00000 , opcode = 110 1111 ` 

WE WILL NOW CHECK OTHER UNIQUE INSTRUCTIONS : 

![Screenshot 2025-01-02 151224](https://github.com/user-attachments/assets/691bcef8-01d2-43a3-bbe7-359cd13ee172)

### 13) The unique code in this is 

          100e4: ffff0797  auipc a5,0xffff0

The address is 100e4 
**auipc** is used to generate a PC(program counter) - relative address by adding 20-bit immediate value to the upper 20 bits of program couter 

**a5** is the register where the result is stored

**0xffff0** is the immediate value

This is an U-Type of instruction .

Hex: `f f f f 0 7 9 7`  

Binary: `1111 1111 1111 1111 0000 0111 1001 0111`

Code : `imm = 1111 1111 1111 1111 0000 , rd = 01111 , Opcode = 0010111 ` 

### 14) Another unique code is 

                 100ec: 00078863  beqz a5,100fc <register_fini+0x18>

the address here is 100ec ,

**beqz** (branch if equal to zero) is a pseudoinstruction that checks if the value of the given register is 0 or not , in this case a5

If 0 the PC is updated to the target address 100fc 

Thus it is an B-Type instruction.

Hex: `0 0 0 7 8 8 6 3`  

Binary: `0000 0000 0000 0111 1000 1000 0110 0011`

Code : `imm = 0 000000 , rs2 = 00000 , rs1 = 01111 , funct3 =  000 ,  imm =1000 0 ,Opcode = 1100011 ` 

### 15) Another unique code is 

              10114: 40a60633  sub a2,a2,a0

the address here is 10114 , 

**sub** is an instruction used to subtract the value of one register from another and store in a register , here a0 - a2 and the result is stored in a2 itself

Thus its an R-Type instruction .

Hex: ` 4 0 a 6 0 6 3 3 `  

Binary: `0100 0000 1010 0110 0000 0110 0011 0011`

Code : `funct7 = 0100000 , rs2 = 01010 , rs1 = 01100 , funct3 = 000 , rd =  01100 , Opcode =  011 0011 ` 

# **THAT WAS ALL FOR THE TASK 3 **
