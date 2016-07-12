=============
Documentation
=============

Made By:
Mansi Goel – 2014062
Taneea S Agrawaal - 2014166

FORKED FROM: https://github.com/mansigoel/ARM-Simulator
By: Grant Slape

The ARMSim has been designed in the following manner:

Design Document: Functional Simulator for Subset of ARM instruction set
-----------------------------------------------------------------------

The document describes the design aspect of myARMSim, a functional simulator for subset of ARM instruction set.

Input
~~~~~

Input to the simulator is MEM file that contains the encoded instruction and the corresponding address at which instruction is supposed to be stored, separated by space.   For example:

- 0x0 0xE3A0200A
- 0x4 0xE3A03002
- 0x8 0xE0821003

Functional Behavior and output
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The simulator reads the instruction from instruction memory, decodes the instruction, read the register, execute the operation, and write back to the register file. The instruction set supported is same as given in the lecture notes. 
The execution of instruction continues till it reaches instruction “swi 0x11”. In other words as soon as instruction reads “0xEF000011”, simulator stops and writes the updated memory contents on to a memory text file. 
The simulator also prints messages for each stage, for example for the third instruction above following messages are printed:

1.	Fetch prints:
	- “FETCH:Fetch instruction 0xE3A0200A from address 0x0” 
2.	Decode
	- “DECODE: Operation is ADD, first operand R2, Second operand R3, destination register R1”
	- “DECODE:  Read registers R2 = 10, R3 = 2”
3.	Execute
	- “EXECUTE: ADD 10 and 2”
4.	Memory
	-	“MEMORY:No memory  operation”
5.	Writeback
	- “WRITEBACK: write 12 to R1”

Design of Simulator
-------------------

Data structure
~~~~~~~~~~~~~~

Registers, memories, intermediate output for each stage of instruction execution are declared as global static. Being static, the variables are not visible outside the file, thus, make the data encapsulated in the myARMSim.cpp.

Simulator flow:
~~~~~~~~~~~~~~~

1.	First memory is loaded with input memory file.
2.	Simulator executes instruction one by one.
	- For the second step, there is infinite loop, which simulates all the instruction till the instruction sequence reads “SWI 0x11”.

Test plan:
~~~~~~~~~~

We test the simulator with following assembly programs:
-	Fibonacci Program
-	Sum of the array of N elements. Initialize an array in first loop with each element equal to its index. In second loop find the sum of this array, and store the result at Arr[N].   

Data Processing Instructions Implemented:
-----------------------------------------

+--------+--------+-------+--------+-------+--------+--------+----------+
| Cond   | F      | I     | Opcode | S     | Rn     | Rd     | Operand2 |
+========+========+=======+========+=======+========+========+==========+
| 4 bits | 2 bits | 1 bit | 4 bits | 1 bit | 4 bits | 4 bits | 12 bits  |
+--------+--------+-------+--------+-------+--------+--------+----------+

-	ADD Rd, Rn, Rm | ADD Rd, Rn, #imm
-	SUB Rd, Rn, Rm | SUB Rd, Rn, #imm
-	ADD Rd, Rn, Rm | ADD Rd, Rn, #imm 
-	SUB Rd, Rn, Rm | SUB Rd, Rn, #imm 
-	AND Rd, Rn, Rm | AND Rd, Rn, #imm 
-	ORR Rd, Rn, Rm | ORR Rd, Rn, #imm 
-	ADC Rd, Rn, Rm | ORR Rd, Rn, #imm 
-	CMP Rn, Rm | CMP Rn, #imm 
-	MOV Rd, Rm | MOV Rd, #imm 
-	MNV Rd, Rm | MNV RD, #imm

+--------+--------+--------+-------+--------+----------+
| Cond   | F      | Opcode | Rn    | Rd     | Offset12 |
+========+========+========+=======+========+==========+
| 4 bits | 2 bits | 6 bits |4 bits | 4 bits | 12 bits  |
+--------+--------+--------+-------+--------+----------+

Data Transfer Instructions Implemented:
---------------------------------------

-	LDR Rd [Rn, #imm] 
-	STR Rd [Rn, #imm]

+--------+--------+--------+---------+
| Cond   | F      | Opcode |  Offset |
+========+========+========+=========+
| 4 bits | 2 bits | 2 bits | 24 bits |
+--------+--------+--------+---------+

Branch Instructions Implemented:
--------------------------------

-	BEQ
-	BNE 
-	BLT 
-	BLE 
-	BGT 
-	BGE 
-	BAL

FETCH:
------

1.	The instruction is fetched from the .mem file which is inputted at the time of execution. The .mem file would contain the address as well as the instruction (in hexadecimal format) of the ARM assembly command.
2.	This instruction is written into the MEM array.

DECODE:
-------

1.	This function uses bit-masking to calculate the flag, the opcode, the condition bits and the immediate of the given instruction or one that is fed into the code. 
2.	We know that if immediate=0, the second operand is a register, and if immediate =1, the second operand is a constant value. Using this, and the opcode, we determine the operation in the instruction as well as operands 1 and 2.

EXECUTE:
--------

1.	This function executes the instruction decoded, with the instruction’s operands and the destination register. 
2.	If the instruction is ADD, this function computes operand1 plus operand2 and places it in a temporary variable called result. 
3.	For instructions like LDR, STR, there are different arrays for each register, in order to store or load values from memory(which are registers, in this case)     we take/place values from respective registers and put them in respective registers.
4.	For instructions like BRANCH, we  sign extend the 24 bits of the offset to 32 bit, and then increment the PC or R[15], by using the formula R[15] = R[15] + 8 + SignExt32(Offset*4) to compute the correct Branch address.

MEMORY:
-------

1.	This function elaborates what is to be written to memory, or loaded from memory.  
2.	In all instructions except LDR, STR, there's no memory function being carried out. Thus, it only prints statements for LDR and STR printing out the stored/loaded values and their destinations.

WRITEBACK:
----------

1.	This function writes back to the destination register, what is stored in the temporary variable ("result").
2.	If there is no writeback for example in case of branch and cmp instruction, it prints that “No writeback operation is required”.
