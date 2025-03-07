# The x86 Architecture
## Instruction Set Architecture (ISA)
“Logical” specification of a computer architecture
Concerned with programming concepts
- instructions, registers, interrupts, memory architecture, . . .
May differ (widely) from the actual microarchitecture
Examples:
- x86 (IA-32 and x86 64)
- ARM (mobile devices)
- MIPS (embedded devices, e.g., consumer routers)
- AVR, SPARC, Power, RISC V, . . .
A set of instruction that let you program one architecture
## Von Neumann Architecture
![](https://i.imgur.com/hG9sUXj.png)

**CPU** with  a lot of registers
**BUS** to move data
**MEMORY** to store the results from the CPU
We use registers to reduce the computation time as we don't need to transfer data from memory to the CPU.
## Memory
![](https://i.imgur.com/1PTfO2K.png)

Each cell store up to 255 if need more you can use multiple cells.
We use exadecimal as it is easy to move to bits/bytes.
## IA-32: Registers
General-purpose registers
- EAX(E first 16 bytes, AX as the last 16 bytes), EBX, ECX, EDX
- ESI, EDI (source and destination index for string operations)
- EBP (base pointer)
- ESP (stack pointer)
Instruction pointer: EIP
- No explicit access
- Modified by jmp, call, ret
- Read through the stack (saved IP)
Program status and control: EFLAGS
(segment registers)

![](https://i.imgur.com/m9orMeg.png)

### EFLAGS register
 32-bits register, boolean flags
Program status: overflow, sign, zero, auxiliary carry (BCD), parity, carry
- Indicate the result of arithmetic instructions
- Extremely important for control flow
Program control: direction flag
- controls string instructions (auto-increment or auto-decrement)
System: control operating-system operations

![](https://i.imgur.com/fyGvHQb.png)

The CPU will set these bits automatically.
## Fundamental data types
**byte** 8 bits
**word** 2 bytes
**dword** Doubleword, 4 bytes (32 bits)
**qword** Quadword, 8 bytes (64 bits)
## Assembly and Machine Code
![](https://i.imgur.com/agyCAWh.png)

Assembly language: specific to each ISA, mapped to binary code
For simplicity, we don’t deal with the linking process.
The assembler transform the assembly code in bytes understandable from the cpu. This process looses a lot of information(ex: the name you give to the variables, the datatype, the name of the functions, the comments in the source code,...) so going back to the source code from the assembler output is really hard.
### Assembly: Syntax
Two main syntaxes:
- Intel: default in most Windows programs (e.g., IDA)
- AT&T: default in most UNIX tools (e.g., gdb, objdump)
Beware: The order of the operands is **different**. We will use the Intel syntax

![](https://i.imgur.com/eg9K6bn.png)

Memory to memory is an invalid combination(except in some instructions, such as movs(move from string to string)).
Beware: in x86, instructions have variable length.
### Basic instructions
**Data Transfer**: mov, push, pop, xchg, lea
**Integer Arithmetic**: add, sub, mul, imul, div, idiv, inc, dec
**Logical**: and, or, not, xor
**Control Transfer**: jmp, jne, call, ret
and lots more. . .
We will use the intel synthax
#### Data Transfer: mov
mov destination, source
- source: immediate, register, memory location
- destination: register or memory location
Basic load/store operations
- Register to register, register to memory, immediate to register, immediate to memory
- Memory to memory is INVALID (in every instruction)
#### Integer Arithmetics: add and sub
![](https://i.imgur.com/QuqQ3BD.png)

Addressing:
- source: immediate, register, memory location
- destination: register or memory location (the destination has to be at least as large as the source)
Negate a value: neg [op]
Bitwise operations: and, or, xor, not work similarly
#### Integer Arithmetics: unsigned multiply (mul)
mul source
- source: register or memory location
- dest ← implied op × source
**Implied operands** according to the size of source
- First operand: AL, AX, or EAX
- Destination: AX, DX:AX, EDX:EAX (double the size of source)
Signed multiply: imul
**Example**
mul ebx: EDX:EAX ← EAX * EBX
- most significant bits of the result in EDX
- least significant bits of the result in EAX
mul cx: DX:AX ← AX * CX
mul cl: AX ← AL * CL
Doing so create the problem that the previous values in EDX are lost 
#### Integer Arithmetics: unsigned divide (div)
div source
- source: register or a memory location
Computes quotient and remainder
Implied operand: EDX:EAX (according to the size of **source**)
Signed divide: idiv
**Examples**
div ebx (4 bytes)
- EAX ← EDX:EAX / EBX
- EDX ← EDX:EAX % EBX
div bx (2 bytes)
- AX ← DX:AX / BX         DX = DX:AX % BX
div bl (1 byte)
- AL ← AX / BX                AH = AX % BX
#### Integer Arithmetics: cmp and test
![](https://i.imgur.com/gLcYAxL.png)

Sets the flags (ZF,CF, OF, . . . )
Discards the result
Result is negative if it is 1, positive if it is 0 and then you can jump to the address.
#### Control-Flow Instructions: conditional jumps
j$<cc>$ **address** or **offset**
Jump to **address** if and only if a certain condition is verified

![](https://i.imgur.com/rYIohqi.png)

Reference: http://www.unixwiz.net/techtips/x86-jumps.html
#### Control-Flow Instructions: unconditional jump jmp
![](https://i.imgur.com/domgBss.png)

**Examples:**
Translate the following C code in assembly x86. Assume EBX ← b, ECX ← c. Finally, a goes in EAX
```C
if (c == 0)
	a = b;
else
	a = -b;
```

```Assembly
mov edx, 0
cmp ecx, edx
jne ELSE
mov eax, ebx
jmp ENDIF
ELSE:
mov eax, 0
sub eax, ebx
ENDIF:
nop
```

Translate the following C code in assembly x86. The variable a goes in EAX.
```C
a = 0;
for(i = 0; i < 10; i++)
	a += i;
```

```Assembly
mov eax, 0
mov ebx, 0
mov ecx, 10
LOOP:
cmp ebx, ecx
jge END
add eax, ebx
inc ebx
jmp LOOP
END:
nop
```

Assume that the input is in registers: ECX and EDX; output: EAX

```Assembly
mov eax, ecx
mov ebx, edx
cmp ebx, 0
jz LABEL
LOOP:
cmp ebx, 1
jle RET
mul ecx
sub ebx, 1
jmp LOOP
LABEL:
mov eax, 1
RET:
```

```C
a = c;
b = d;
if(b != 0){
	while(b > 1){
		a *= c;
		b--;
	}
}else{
	a = 1;
}
```

#### Load effective address (lea)
 lea destination, source
- source: memory location
- destination: register
Like a mov, but it is storing the pointer, not the value
**It does NOT access memory**

![](https://i.imgur.com/x464d7G.png)

lea eax, [ebx + 8]
	   =
mov eax, ebx
add eax, 8	

We use lea as it is a single instruction, it is the only difference(alias of instructions).
#### Basic Instructions: nop
nop = No Operation. Just move to next instruction.
The opcode is pretty famous and is 0x90
Really useful in exploitation (we will see!)
#### Interrupts and Syscalls
int value
- value: software interrupt number to generate (0-255)
- Every OS has its set of interrupt numbers (e.g., 80h for Linux system calls)
syscall used for Linux 64-bit
sysenter used by Microsoft Windows
Used to do privilege operations(like printf, scanf as you are using the screen/getting inputs from the keyboard, you are using a periferical).
## The x86 64 ISA
![](https://i.imgur.com/MBAKSlC.png)

This way has only more registers/bits, the operations are working like before. The difference is that you are using 64 bits instead of 32.
## Endianness
**Endianness**: convention that specifies in which order the bytes of
a data word are lined up sequentially in memory.

![](https://i.imgur.com/PkHpqww.png)

# Program Layout and Functions
## How an executable is mapped to memory in Linux (ELF)
![](https://i.imgur.com/pAI4JL0.png)

Each section has its own mean.
## Simplified program memory layout
![](https://i.imgur.com/WQsrPJO.png)

This is the convention we will use and we can see the various sections of the layout. 
### The Stack
LIFO (last in first out) data structure
Used to manage functions
- local variables
- return addresses
- ...
Handled through the register ESP (stack pointer)
Remember: the stack grows toward lower addresses
	(downward the address space)
