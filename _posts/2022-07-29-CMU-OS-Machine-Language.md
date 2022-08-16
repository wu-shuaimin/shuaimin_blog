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

Regsiters for passing function arguments

|Operand Size(bits)|1|2|3|4|5|6|
|------------------|-|-|-|-|-|-|
|64|%rdi|%rsi|%rdx|%rcx|%r8|%r9|
|32|%edi|%esi|%edx|%ecx|%e8d|%e9d|
|16|%di|%si|%dx|%cx|%r8w|%r9w|
|8|%dil|%sil|%dl|%cl|%r8b|%r9b|

#### Managing Local Data

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

#### Machine Language: Data

Basic Principles: T A[N];

1. It allocates a contiguous region of L . N bytes in memory, where L is the size (in bytes) of data type T
2. It introduces an identifier A that can be used as a pointer to the beginning of the array.

#### Array Access

The array accessing A[i], will be encoded into assembly code, where i is stored in the register %rcx

```C
movl (%rdx, %rcx, 4), %eax
```

and the instruction compute $x_A + 4i$.

**Note** 4 is a scalling factor, which includes 1,2,4,8.

The memory access formula is

```C
int P[5];
short Q[2];
int **R[9];
double *S[10];
short *T[2];
```

|Array|Element Size|Total Size|Start Address|Element i|
|-----|------------|----------|-------------|---------|
|P|4|20|$x_p$|$x_p+4i$|
|Q|2|4|$x_Q$|$x_Q+2i$|
|R|8|72|$x_R$|$x_R+8i$|
|S|8|80|$x_S$|$x_S+8i$|
|T|8|80|$x_T$|$x_T+8i$|

#### Multi-dimensional Array (Nested)

```C
int A[5][3]

```

The array elements are ordered in memory in row-major order, meaning all elements in a row are stored contiguously. For example, elements of 1st row will be stored contiguously, and then followed by the 2nd row, so on.

A[i][j] is element of type T, which requires K bytes.

Address $A + (i *C + j)* K$ or $A + i*K*C + K*j$



**This is how you compute the address for accesing element in the array**

* K is the size of the data type
* C is the number of columns in a row
* i and j are the row and column you want to access

$C*K$ computes the number of bytes required for a row, and $i*C*K$ directs the pointer to the specific row. $j*K$ moves the pointer to the element you want.

The computation in assembly starts at $i*C$(line 1 in asm), and then compute $A + 4*i*C$ by ***leaq*** instruction. Finally, compute $A+4*i*C + 4*j$ by ***movl*** 

```C
int fix_ele(fix_matrix A, size_t i, size_t j)
{
    return A[i][j];
}

//Assembly
// A in %rdi, i in %rsi, j in %rdx
sqlq $6, %rsi     //64*i
addq %rsi, %rdi   //A + 64*i
movl (%rdi, %rdx, 4), %eax  //M[A + 64*i +4*j]
ret

//general case n in %rdi, A in %rsi, i in %rdx, j in %rcx
imulq $rdx, $rdi  //n*i
leaq  ($rsi, %rdi, 4), %rax  //A+4*n*i
movl  (%rax, %rcx, 4), %eax 
ret
```

Recall the formula $A + K(i*C + j)$

### Data Alignment

* 1 byte: char
* 2 byte: short
* 4 byte: int, float
* 8 byte: double, long, char *

Within the structure, for example, data type int must start with address of multiple of 4, and for double and pointers the address must start with multiple of 8.

The overall data alignment of the structure must ends wit multiple of the largest data type K byte. For example, if there is a pointer in the structure, the overall structure must ends with multiple of 8.

Array of structure: This is similar to the overall alignment above. The array has length multiple of the structure, so if the structure satisfies the overall alignment, the array will satisfy too. For exmaple, if the structure has overall length of 24 bytes, then the array will be multiple of 24 and within the structure the alignment is the same as the individual structure

To save space in a program: Write larger fields first

## Machine level programming: Advanced Topics

### Memory Layout

x86-64 Linux Memory Layout

* Stack: Runtime stack (8mb limit) stores local variables
* Heap: Dynamically alloated as needed
* Data: Statically allocated data
* Text/ Shared Libraries: Excutable machine instructions, with read only mode.

So in x86-64 linux memory, it consits of 4 components Stack, Heap, Data, Text and Shared Libraries

```C
//Memoery allocation example in C

char big_array[1L];
char huge_array[1L<<31];

int global = 0;

int useless(){return 0;}

int main()
{
    void *p1, *p2, *p3, *p4
    p1 = malloc(1L << 28); //256 MB
    p2 = malloc(1L << 8); //256 B
    p3 = malloc(1L << 32); //4 GB
    p4 = malloc(1L << 8); //256 B

}
```
|Shared Libraries|Stack|Heap|Data|Text|
|----------------|-----|----|----|----|
||local|p1,p2,p3,p4|big_array, huge_array|main(),useless()|

### Buffer Overfow

When you access some location in the memory, there might be an issue of out of bounds. In java, it's called Null pointer exception, in C it's called segmentation faults.

Most common form

* Unchecked lengths on string inputs
* Particularly for bounded character arrays on the stack

```C
//C programming pointers

++*p //increment the value stored at the pointer *p by 1 and return the value

p*++ //return the value stored at the pointer *p and increment the pointer by 1

*++p // increment the pointer by 1 and return the value stored at the incremented pointer
```

**Note:** In many string library code, such as gets(), strcpy(), strcat(), we have no way to limit on number of characters to read. This results in the buffer overflow

A vulnerability is something that can cause the program to misbehave. As a hacker, the interest is to exploit these vulnerabilities to gain access to systems.

Stack smashing is a situation that the data in the buffer region corrupt the region of stack so that the return address stored in the stack is corrupted to other address instead of the original correct address, and this will cause a segmentation faults. when programming in C, we need to be aware of the memory space allocated for stack, heap, and manage these resources efficiently.

#### Code Injection Attack

A program calls a function which requries input, and typically the input string will contain byte representation of excutable code. On machine level, the return address of the function will be stored in stack, i.e the %rsp (stack register). The attack string will be contain some paddings to fill and overflow the heap so that the return  address in stack %rsp will be overwrriten by the injected code. Now the return address might be changed to the address of the code that helps attackers to exploit the system or the application.

#### Ways to Avoid Buffer Overflow Attacks

1. Avoid buffer overflow vulnerabilities in **Code**. Use function that limit number of character.

* use fgets() and other f functions that bound the characters

2. Employ system-level protection

* Randomized Stack Offsets:
  * At start of the program allocate random amount of space on stack
  * Shift stack address for entire program
  * Stack repositioned in each program excution
  * The idea of stack randomization is that in each excution of program, the size and position of the stack will be varied making it difficult for the attacker to predict the injecting address. Furthermore, x86-64 added explicitly excute permission, and stack is non-excutable region making injected code in the stack not possible to be excuted.

3. Have compiler to use *stack canary*

* Placing a special value (canary) just beyond the buffer, and check for corruption before exiting function. If the canary were corrupted, then there will be an attack in the input code.
* The idea is that will can set randomly and differently a number at 8(%rsp) before the call of each function, and after return from a function we can check if the number still equal; otherwise, we report stack check fails if not the same.

```C
//assembly code for protected buffer disassembly

sub $0x18, %rsp
mov %fs:0x28, %rax //move the randomized canary to %rax
mov %rax, 0x8(%rsp) //move the canary value just beyond the stack at offset 8 from stack register %rsp
xor %eax, %eax //check if they equal
mov %rsp, %rdi
callq 4006e0<gets>
mov %rsp, %rdi
callq 400570<puts@plt>
mov 0x8(%rsp), %rax  
xor %fs:0x28, %rax //check if after excution of functions the canary value still equals
je 400768<echo+0x29>
callq 400580 <_stack_chk_fail@plt>
add $0x18, %rsp
retq

```

The assembly code above is a demonstration of how the stack canary is implemented in machien level.

#### Return Oriented Programming Attacks

ROF attacks intergrate fragments of existing functions in libraries such as stdlib to achieve overall desired output

In x86 the retq will encoded as c3, and anything before that can potentially be a gadget.

**Gadget:** A sequence of byte that ends with c3
### Unions

**Union:** is an user defined datatype assigning the largest space for its fields, but at a single time only a field can contain data.

