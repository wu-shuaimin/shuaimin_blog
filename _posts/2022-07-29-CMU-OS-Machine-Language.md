---
layout: 'post'
title: 'OS CMU Note Machine Language' 
---



## Machine Level Language Basics

Intel x86 Processors and ARM are the two dominant players in the market.

* Architecture (Instruction Set Architecture ISA): The part of a processor design that one needs to understands to write assembly code.
* Microarchitecture: Implementation of architecture.
* Machine Code: The bit level code that a machine excutes
* Assembly Code: The text representation of machine code.

### Assembly/Machine Code

Programmer visible state:

* Program Counter(PC): Address of next instruction
* Register File: A Number of memory location given by **Name**
* Condition Code: Store status information about recent logical arithmetic
* Memory: An array of bytes.

compile with command: gcc -Og -S p1.c

#### Procetures turning Programms into Machine Code

1. C programs p1.c by Compile
2. Asm programs p1.s by Assembler
3. Object programs p1.o by Linker
4. Excutable program

##### Compiling into Assembly Code

``` c
{
    // c code
long plus(long x, long y);

void sumstore(long x, long y, long *dest)
{
    long t = plus(x, y);
    *dest = t;
}

}

{
// x86-64 assembly
pushq  %rbx
movq   %rdx, %rbx
call   plus
moveq  %rax, (%rbx)
popq   %rbx
ret    

}

gcc -Og -S sum.c
```

This code produces sum.s

##### Assembly Characteristics

Data Types

* Integer: 1,2,4,8 bytes
* Floating point: 4,8,10 bytes
* Code: Byte sequences encoding series of instructions
* No aggregate types such as arrays or structures. Only gondiguously allocated bytes in memory.

Operations

* Perform arithmetic function on register or memory data, but the operations are very limited.
* Transfer data between memory and register: 
  * load data from memory into register
  * store register data into memory
* Transfer Control: This depends on low level features of design.
  * Unconditional jumps
  * Conditional branches

#### From Assembly Code to Object Code

* Assembler:
  * Translates .s into .o
  * Binary encoding of each instruction
  * Nearly-complete emage of executable code
  * Missing linkages between code in different files
* Linker
  * Resolves references between files
  * Combines with static run-time libraries
  * Some libraries are dynamically linked
  * Linker links the requested files with the current program

##### Disassembling Object Code

Disassembler: objdump -d sum

* Useful for examining object code
* Analyzes bit pattern of series of instructions

* You can also use **gdb** for disassembling

### Assembly Basics: Registers, operands, move

#### x86-64 Integer Registers

16 registers for 64 bit:

* %rax
* %rbx
* %rcx
* %rdx
* %rsi
* %rdi
* %rsp
* %rbp
* %r8
* %r9
* %r10
* %r11
* %r12
* %r13
* %r14
* %r15

#### Moving Data

* movq Source, Dest: (<- this is the assembly code)
* Operand Types:
  * Immediate: constant integer data
    * \$0x400, $-533
  * Register: one of 16 integer registers
    * %rax, %r13
    * **%rsp** is reserved for special use
    * Others have special uses for particular instructions
  * Memory: 8 consecutive bytes of memory at address given by register
    * %rax

**moveq moves three types of information constant, registers, and memory.**

* Source: Immediate:
  * Destination:
  * Register: movq \$0x4, %rax ---C  temp = 0x4;
  * Memoery: movq \$-147, (%rax) ---C *p = -147;
  * Move from register to memory or vice versa
* Source: Register:
  * Destination:
  * Register: movq %rax, %rdx ---C temp2 = temp1;
  * Memoery: movq %rax, (%rdx) ---C *p = temp;
  * Move from register to memory or vice versa
* Source: Memory
  * Destination:
  * Register: movq (%rax), %rdx ---C temp = *p;

#### Simple Memory Addressing Modes

* Normal: movq (%rcx), %rax 
  * The parenthesis indicates that use the register's memory location
* Displacement: movq 8(%rbp), %rdx
  * Access the memory location of the value that offset 8 by the given register.

#### Code Demonstration

**Remember**: %rdi will be the register of the first argument, and %rsi will be the register of the second argument.

``` C
void swap (long *xp, long *yp)
{
  long t0 = *xp;
  long t1 = *yp;
  *xp = t1;
  *yp = t0;
}

// Machine Level Code

swap:
movq (%rdi), %rax # t0 = *xp Use $rdi as an address, copy that memory location and store the result and register %rax
movq (%rsi), %rdx # t1 = *yp Copy the value at the memory location %rsi to the register %rax
movq %rdx, (%rdi) # *xp = t1 Copy the value in the register %rax back to the memory %rdi
movq %rax, (%rsi) # *yp = to Copy the value in the register %rdx back to the memory %rsi
ret
```

Complete Memory Addressing Modes

* Most General Form: D(Rb, Ri, S) / Mem[Reg[Rb] + S*Reg[Ri] + D]
  * D: Displacement 1,2, or 4 bytes
  * Rb: Base register: any of 16 integer regsiter
  * Ri: Index register: any, excep for %rsp
  * S: Scale: 1,2,4, or 8 (why these numbers?)
* Special Cases
  * (Rb, Ri): Mem[Reg[Rb] + Reg[Ri]]
  * D(Rb, Ri): Mem[Reg[Rb] + Reg[Ri] + D]
  * (Rb, Ri, S): Mem[Reg[Rb] + S*Reg[Ri]]

Address Computation Instruction

* leaq Src, Dst
  * Src is address mode expression
  * Set Dst to address denoted by expression
* Uses
  * Computing address without a memory reference
    * p = &x[i];
  * computing arithmetic expression of the form x+k*y (k=1,2,4, or 8)

``` C
long m12(long x)
{
  return x*12;
}

ASM by compiler

leaq (%rdi, %rdi, 2), %raw  # t <- x+2*x
salq $2, %rax  # return 
```

#### Arithmetic Operations

Two Operand Instructions

* addq Src, Dest # Dest = Dest + Src
* subq Src, Dest # Dest = Dest - Src
* imulq Src, Dest # Dest = Dest * Src
* salq Src, Dest # Dest = Dest << Src  also called shlq
* sarq Src, Dest # Dest = Dest >> Src  Arithmetic
* shrq Src, Dest # Dest = Dest >> Src  Logical
* xorq Src, Dest # Dest = Dest ^ Src
* andq Src, Dest # Dest = Dest & Src
* orq Src, Dest # Dest = Dest | Src


One Operand Instructions

* incq Dest # Dest = Dest + 1
* decq Dest # Dest = Dest - 1
* negq Dest # Dest = -Dest
* notq Dest # Dest = ~Dest

**See CSAPP for more instructions**

## Machine Level Language: Control

### Control: condition codes

* Temporary data (%rax, ...)
* Location of runtime stack (%rsp) current stack top
* Location of current code control point (%rip, ...)
* Status of recent tests:
  * CF: carry flag. The most recent operation generated a carry out of the MSB. Used to detect overflow. (for unsigned)
  * ZF: Zero flag. The most recent operation yielded zero.
  * SF: Sign Flag: The most recent operation yielded a negative value.(for signed)
  * OF: Overflow Flag: The most recent operation caused a two's complement over-flow(either negative or postive) (for signed)


