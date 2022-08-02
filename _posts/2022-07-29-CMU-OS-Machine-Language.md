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


#### Machine Level Programming: Procedures

Mechanisms in Procedures

* Passing control
  * To beginning of procedure code
  * Back to return point
* Passing Data
  * Procedure arguments
  * Return value
* Memory Management
  * Allocate during procedure excution
  * Deallocate upon return

##### Stack Structure

x86-64 Stack
 
* Region of memory managed with stack discipline.
* The address grows toward lower address
* High address is the bottom of the stack, whereas lower address is the top of the stack with **Stack Pointer** %rsp

Instruction: 

* pushq Src
  * Fetch operand at Src
  * Decrement %rsp by 8
  * Write operand at address given by %rsp
* popq Dest
  * Read value at address given by %rsp
  * Increment %rsp by 8
  * Store value at Dest (usually a register)

**Thoughts**: The value is not discarded from the memory but simply the pointer to the top of the stack has decremented by 8, and the poped out value is not in the concern of the stack anymore.s

##### Passing Control

* Proedure Call: call *label*
  * push return address on stack
  * jump to *label*
* Return Address:
  * Address of the next instruction right after call
* Procedure return: ret
  * Pop Address from stack
  * Jump to address

```C
//Assembly

//leaf(long y) y in %rdi
400540 lea 0x2(%rdi), %rax //z+2
400544 retq //return

//top(long x) x in %rdi
400545 sub $0x5, %rdi //x-5
400549 callq 400540<leaf> //call leaf(x-5)
40054e add %rax, %rax  //double result
400551 retq //Return

//main
400545 sub $0x5, %rdi //Call top(100)
400560 mov %rax, %rdx //Resume

```
Trace table of the code

|Label| PC| Instruction|%rdi|%rax|%rsp|*%rsp|Description|
|-----|---|------------|----|----|----|-----|-----------|
|M1|0x40055b|callq|100|-|0x7fffffffe820|-|call top(100)|
|T1|0x400545|sub|100|-|0x7fffffffe818|0x400560|entry of top|
|T2|0x400549|callq|95|-|0x7fffffffe818|0x400560|call leaf(95)|
|L1|0x400540|lea|95|97|0x7fffffffe810|0x40054e|entry of leaf|
|L2|0x400544|retq|-|97|0x7fffffffe810|0x40054e|return 97 from leaf|
|T3|0x40054e|add|-|97|0x7fffffffe818|0x400560|resume top|
|T4|0x400551|retq|-|194|0x7fffffffe818|0x400560|return 194 from top|
|M1|0x400560|mov|-|194|0x7fffffffe820|-|resume main|

**Thoughts on the trace table**:
The main insight for me to this table is the value of %rsp and *%rsp. The former has the stack like structure for nested call it will decrement its address by 8 bytes (see the change on %rsp from M1 to T1). The latter points the address to execute after return from a call, and this address is the next line after the call instruction.

### Passing Data

Data Flow: 

First 6 argments

* %rdi
* %rsi
* %rdx
* %rcx
* %r8
* %r9

Return value

* %rax

##### Managing Local Data

Stack-Based Languages

* Stack Discipline
  * State for given procedure needed for limited time
    * From when called to when return
  * Callee returns before caller does
* Stack allocatd in *Frames*

Stack Frames

* Contents
  * Return information
  * Local storage (if needed)
  * Temporary spack (if needed)
  * **Each call to a function will create a stack frame**. This is similar to java method which have access to its local variables.
* Management
  * Space allocated when enter procedure
    * "Set-up" code
    * Includes push by *call* instruction
  * Deallocated when return
    * "Finish" code
    * Includes pop by *ret* instruction.

x86-64/Linus Stack Frame

* Current Stack Frame("Top" to Bottom)
  * "Argument build": parameters for function about to call
  * Local variables: if can't keep in register
  * Saved register context
  * Old frame pointer (optional)
* Caller Stack Frame
  * Return address: pushed by call instruction
  * Arguments for this call

**Note**: This Linux stack frame, I need to read further reading on CSAPP

```C
long incr(long *p, long val)
{
  long x = *p;
  long y = x+val;
  *p = y;
  return x;
}

//assembly

inc:
  movq (%rdi), %rax
  addq %rax, %rsi
  movq %rsi, (%rdi)
  ret

long call_incr()
{
  long v1 = 15213;
  long v2 = incr(&v1, 3000);
  return v1+v2;
}

//assembly

call_incr:
  subq $16, %rsp
  movq $15213, 8(%rsp)
  movl $3000, %esi
  leaq 8(%rsp), %rdi
  call incr
  addq 8(%rsp), %rax
  addq $16, %rsp
  ret
```

**Note**: movl %exx zeros out high order 32 bits, and movl is 1 byte shorter than movq

##### Register Saving Convention

* who calls who is caller
* who is called who is callee

* Caller saved: caller saves temporar values in its frame before using
* Callee restores them before returning to caller

##### x86-64 Linux register usage

* %rax
  * return value
  * caller-saved
  * can be modified by procedure
* %rdi, ...,%r9
  * arguments
  * also caller-saved
  * can be modified by procedure
* %r10, %r11
  * caller-saved
* %rbx, %r12, %r13, %r14
  * Callee-saved
  * Callee must save & restore
* %rbp
  * Callee-saved
  * Callee must save & restore
  * May be used as frame pointer
  * Can mix & match
* %rsp
  * Special form of callee save
  * Restored to original value upon exit from procedure

|Register| Usages|
|--------|-------|
|%rax| Return value|
|%rdi|Arguments|
|%rsi|Arguments|
|%rdx|Arguments|
|%rcx|Arguments|
|%r8|Arguments|
|%r9|Arguments|
|%r10| Caller-saved Temporary|
|%r11|Caller-saved Temporary|
|%rbx|Callee-saved Temporaries|
|%r12|Callee-saved Temporaries|
|%r13|Callee-saved Temporaries|
|%r14|Callee-saved Temporaries|


