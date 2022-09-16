---
layout: 'post'
title: 'OS CMU Note Linking' 
categories: 'OS'
permalink: '/:categories/:title'
---

Why we need to understand Linking?

1. Understnad linkers will help you build large programs
2. Understand linkers will help you avoid dangerous programming errors
3. Understand linkers will help you understand how language scoping rules are implemented
4. Understand linking will enable you to exploit shared libraries

## Compiler Drivers

![image](../pictures/compiling_proces.png)

The compiler driver first convert the main.c file to main.i (ascii intermediate value), and then uses assembler to translate main.i into main.s which is the assembly file, and then from assembly file into object file. Finally it runs the linker program to combine main.o and with other file to produce *excutable object file*.


## Static Linking

To generate an relocatable object files, linker program should run two steps

1. Symbol Resolution: Object files define and reference *symbols*, where each symbol represents a function, a global variable or a static variable. The purpose of symbol resolution is to associate symbol *reference* with only one symbol definition.
2. Relocation: The linker relocates these sections by associating a memory location with each symbol definition, and then modifying all references to those symbols so that they all point to the memory location.


## Object Files

* Relocatable Object File: contains binary code and data in a form that can be combined with other relocatable object files at compile time to create an excutable object file.
* Excutable Object File: contains binary code and data in a form that can be copied into main memory and excutable
* Shared Object File: A special type of relocatable object file that can be loaded into memory and linked dynamically, at either load time or run time.

object module: a sequence of bytes.

## Symbols and Symbol Tables

3 types of symbols:

* Global Symbols: defined by module m and can be referenced by other modules. Global linker symbols correspond *non-static* C functions and global variables
* Global symbols that are referenced by module m but defined by other modules. These are calld *externals*\
* Local symbols that are defined and referenced exclusively by module m. These are static C functions and global variables defined with keyword static. They can't be referenced by other modules.

## Relocatable Object Files

![image](../pictures/rel_obj_file.png)

ELF header begines with a 16-byte sequence that describes the word size and byte ordering of the system that generated the file. 

## Symbol Relocation

* Local symbols: defined in the same modules. The compiler allows only one definition of each local symbol per module.
* Global symbols: when referencing to global symbols, compilers   will only generate an entry in the symbol table and let linkers to find the module. If linkers can't find the module, it will raise an error.


## Symbol Resolutino

Strong Symbol: functions and initialized global variables are strong symboles

Weak Symbol: uninitialized global variables get weak symbols

3 Rules:

1. Multiple strong symbols with the same name are not allowed
2. Given a strong symbol and multiple weak symbol, the strong one will be selected
3. Given multiple weak symbol, one random weak symbol will be selected.

### Linking with static libraries

In practic, all compilation systems provide a machanism for packaging related object modules into a single file called *static library*

We create separate relocatable files for each standard function and storing them into a directory. But we need to explicitly link he appropriate object modules into their excutables.

```C
linux> gcc main.c /usr/lib/printf.o /usr/lib/scanf.o

```

In Linux system, static libraries are stored into a file called *archive*.

Archive: is a collection of concatenated relocatd object files, with a header that describes the *size* and *location* of each membner object file.

Steps to create a static library using AR tool

```C
linux> gcc -c addvec.c multvec.c
linux> ar rcs libvector.a addvec.o mulvec.o
```

To use the library, we can write a .c file which invokes addvec library routine.

```C
//below is linux comnmand
linux> gcc -c main2.c
linux> gcc -static -o prog2c main2.o ./libvector.a

#include <stdio.h>
#include "vector.h"

int x[2] = {1,2};
int y[2] = {3,4};
int z[2];

int main()
{
    addvec(x,y,z,2);
    printf("z = [%d %d]", z[0], z[1]);
    return 0;
}

```

![image](../pictures/linking_ex.png)

### How linkers use static libraries to resolve references

The linker will scan the file from left to right and it will maintain a set E of relocatabe object files, a set U of



