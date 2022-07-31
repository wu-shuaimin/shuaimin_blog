---
layout: 'post'
title: 'OS CMU Note Machine Language' 
categories: 'OS'
permalink: '/:categories/:title'
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


### Assembly Code View

Programmer-Visible State

* PC: Program counter: Address of next instruction
* Memory: 
  * Byte addressable array
  * Code and user data
  * Stack to support procedures
* Register file: Heavily used program data
* Condition codes: 
  * Store status information about most recent arithmetic or logical operation
  * Used for conditional branching

Turning C into Object Code

1. text C programs: p1.c p2.c by compiler(gcc -Og -S) to next stage.
2. text Asm program: p1.s p2.s by Assembler(gcc or as) to next stage.
3. binary Object program: p1.o p2.o by Linker(gcc or ld) to next stage.
4. binary excutable program p


### Control: condition codes

#### Processor State (x86-64)

* Temporary data (%rax, ...)
* Location of runtime stack (%rsp) current stack top
* Location of current code control point (%rip, ...)
* Return value register: %eax

#### Status of condition codes by implicit setting
* Status of recent tests (condition codes):
  * CF: carry flag. The most recent operation generated a carry out of the MSB. Used to detect overflow. (for unsigned overflow)
  * ZF: Zero flag. The most recent operation yielded zero.
  * SF: Sign Flag: The most recent operation yielded a negative value.(for signed)
  * OF: Overflow Flag: The most recent operation caused a two's complement over-flow(either negative or postive) (for signed)

Example:$t = a+b $ <-> asm code: addq Src, Dest
* CF set: if carry/borrow out from most significant bit(unsigned overflow)
* ZF set: if t==0
* SF set: if t<0 (as signed)
* OF set: if two's complement (signed) overflow (a>0 && b>0 && t<0) || (a<0 && b<0 && t>=0)

#### Thoughts on condition codes

* CF set: This is used for reporting a carry or a borrow is required in arithmetic. Normally, for 32-bits the if there is a carry to the 33th bit, then there is an overflow. Similarly, if a borrow is required to complete the arithmetic an overflow is required. **This flag is for unsigned arithmetic reporting overflow**
* SF set: If the final result's sign bit is 1, then this flag is used to report the result is a negative number
* OF set: For signed arithmetic to report overflow.
* ZF set: when the result is all zeros

#### Condition codes by explicit setting: Compare

#### SetX Instructions

|SetX |Condition|Description|
|-----|---------|-----------|
|sete |ZF       | Equal/Zero|
|setne|~ZF      | Noe Equal/Not Zero|
|sets |SF       | Negative|
|setns|~SF      | Nonnegative|
|setg |~(SF^OF) & ~ZF| Greater(Signed)|
|setge|~(SF^OF) | Greater or Equal (Signed)| 
|setl |(SF^OF)  | Less(Signed) |
|setle|(SF^OF)& ZF | Less or Equal(Signed)|
|seta |~CF&~ZF   | Above (Unsigned)|
|setb |CF       | Below (unsigned) |

* cmpq: make comparisons and set condition codes
* %al: is the lower 8 bits in %eax
* movzbl: tell the cpu to fetch a byte from the memory and zero extend to 32-bit

### Conditional Branches

**Jump to different part of code depending on condition codes**

|jX |Condition       | Description|
|---|----------------|------------|
|jmp| 1 | Unconditional|
|je | ZF | Equal/Zero|
|jne| ~ZF| Not Equal/ Not Zero|
|js | SF | Negative|
|jns| ~SF| Nonnegative|
|jg |~(SF^OF) & ~ZF | Greater(Signed)|
|jge|~(SF^OF)|Greater or Equal(Signed)|
|jl |(SF^OF) | Less(Signed)|
|jle|(SF^OF)|ZF | Less or Equal(Signed)|
|ja |~CF&~ZF |Above(unsigned) |
|jb |CF |Below(unsigned) |

#### Goto Expression in C

```C
long absdiff(long x, long y)
{
  long result;
  if(x>y)
    result = x-y;
  else
    result = y-x;
  return result;
}

//Assembly
absdiff
  cmpq %rsi, %rdi #x,y
  jle .L4
  movq %rdi, %rax
  subq %rsi, %rax
  ret
.L4:  #x<=y
  movq %rsi, %rax
  subq %rdi, %rax
  ret

//Goto codes in C

long absdiff_j(long x, long y)
{
  long reuslt;
  int ntest = x <= y;
  if(ntest) goto Else;
  result = x-y;
  goto Done;
Else:
  result = y-x;
Done:
  return result;
}
//This goto style is more like assembly code, where we evaluate some condition if it's true, and then excuete the required code; otherwise we jump to the else code block accordingly.
```

General conditinal Expression Translation

> val = Test ? Then_expr : Else_Expr;

##### Conditional Moves

***Conditional moves excute all codes blocks and then choose the picece of result according to condition.***

Why?
* Branches are **disruptive** to instruction flow through pipelines
* Conditional moves do not require control transfer

#### Loops

##### Do While Loop

``` C
long pcount_do(unsigned longx)
{
  long result = 0;
  do{
    result += x & 0x1;
    x>>=1;
  }while(x);

  return result;
}

//goto version
long pcount_goto(unsigned long x)
{
  long result = 0;
  loop:
    result += x & 0x1;
    x>>=1;
    if(x) goto loop;
    
    return result;
}
```

The program above count the number of 1's in argument x ("popcount")

##### Do-while Loop Compilation

|Register| Uses|
|--------|-----|
|%rdi| argument x|
|%rax| result|

``` assembly
  movl $0, %eax # result = 0
.L2:
  movq %rdi, %rdx
  andl $1, %edx #%edx is the argument x
  addq %rdx, %rax #result += t
  shrq %rdi # x>>=1
  jne .L2   #if(x) goto loop
  rep; ret
```

##### While Loop Example

```C
long pcount_while(unsigned longx)
{
  long result = 0;
  while(x){
    result += x & 0x1;
    x>>=1;
  }

  return result;
}

long pcount_goto_jtm(unsigned long x)
{
  long result = 0;
  goto test;
  loop:
    reuslt += x & 0x1; 
    x>>=1;
  test:
    if(x) goto loop;
    return result;
}
```

##### For Loop form

```C
for(Init; Test; Update)
  Body
Init: i=0
Test: i<Wsize
Update: i++
Body
{
  unsigend bit = (x>>i) & 0x1;
  result += bit;
}

long pcount_for(unsigned long x)
{
  size_t i;
  long result = 0;
  for(i=0; i<Wsize; i++)
  {
    unsigned bit = (x>>i) & 0x1;
    result += bit;
  }
  return result;
}

```

##### For loop and While loop comparison

```C
//For version:
for(Init; Test; Update)
  Body

//while version
while(Test)
{
  Body

  Update;
}
```

#### Switch Statements

##### Jump Table Structure

switch form

```C
switch(x)
{
  case val_0:
    Block 0
  case val_1:
    Block 1
  ...
  case val_n-1:
    Block n-1
}
```

|Jump Table|
|----------|
|Targ0|
|Targ1|
|Targ2|
|...|
|Targn-1|

|Jump Targets|
|------------|
|Targ0: code block 0|
|Targ1: code block 1|
|Targ2: code block 2|
|...|
|Targ n-1: code block n-1|

> goto *JTab[x];

##### Assembly Setup Explanation

Table Structure

* Each target requires 8 bytes
* Base address at .L4

Jumping

* Direct: jmp .L8 (jump target is denoted by label .L8)
* Indirect: jmp *.L4(, %rdi, 8) 
  * start of jump table: .L4
  * must scale by factor of 8 (addresses are 8 bytes)
  * Fetch target from effective address .L4 + x*8
    * Only for 0<= x <=6

#### Thoughts from reading Controll

* jmp *%eax: Uses the value in register %eax as the jump target. Jump directly to the value
* jmp *(%eax): Uses in the register %eax as the read address, and use the address to get the jump target in the memory. This means use the register value as the address in the memory, and the address to get the jump target from the memory.

