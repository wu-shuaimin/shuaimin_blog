---
layout: 'post'
title: 'OS CMU Note Data Representation' 
categories: 'OS'
permalink: '/:categories/:title'
---

## Data Representation

#### Negation: Complement & Increment

> ~x + 1 = -x

### Representation in memory, pointers, and strings

* Programs refer data by address
  * think it as a big array of bytes conceptually
* An address is like an index into that array
* A pointer variable stores an address
* **System provides each address space to each "program"**

### Ordering

4-byte value: 01234567

* Little Endian: Least significant bits(LSB) at the left and Most significant bits(MSB) are at right. 67452301
* Big Endian: MSB at left and LSB at right. 01234567

### Floating Points representation

$$

(-1)^s M 2^E

$$

* s: sign bit
* M: significand (normally a fractional value in range[1.0-2.0])
* E: exponent weights value by power of 2

Precision Options

* Single Precision: 32-bits (s 1bit, exp 8 bits, frac 23 bits)
* Double Precision: 64-bits (s 1 bit, exp 11 bits, frac 52 bits)

#### Three kinds of floating point numbers

Resulting floating points representation s + exp + frac

* Normalized
  * When exp not equal 000000 and exp not equal 111111
  * Exponent encoded E = exp - Bias (Bias is a provided value)

Floating point 15213.0 in binary is 11101101101101 = 1.1101101101101 x 2^13
Significant: 

* M =   1.1101101101101 
* frac = 1101101101101 

Exponent: 

* E = 13
* Bias = 127
* exp = 140 = 1000 1100(binary)

**0 1000 1100 1101 1011 01101 0000000000**

* Denormalized
  * When exp = 000000 （exponent all zeros)
  * significand: M= 0.xxxx in binary
  
* Special:
  * When: exp=11111
  * Case 1 
    * exp = 11111, frac = 0000000
    * **represent infinity**
    * Used for operations that overflows
  * Case 2
    * exp = 11111, frac not = 00000
    * Not a Number (NaN)
    * **Represents the case when no numeric value can be determined**

Example: float: 0xC0A00000

1. convert hex to binary
2. decide the type of floating point, in this case Normalized
3. Use the formula: $v = (-1)^s M 2^E$ and  $E= exp - Bias$ to find v
4. $Bias = 2^(k-1) - 1 = 127$, where k is the number of bits for exp.

1. binary: 1100 0000 1010 0000 0000 0000 0000 0000
2. exp is 1000 0001, which is 129, and $Bias = 2^(8-1)-1=127$ 
3. $E = exp - Bias = 2 (decimal)$
4. S = 1 (negative number)
5. M = 1.010 0000 0000 0000 0000 0000 = 1 + 1/4 = 1.25
6. $v = (-1)^1 M 2^E = (-1)^1 * 1.25 * 2^2 = -5$

### Floating Point rounding, addition, multiplication

* $x + y = Round(x + y)$
* $x * y = Round(x * y) $

Basic idea

1. compute exact result.
2. make it fit into desired precision.
  * maybe overflow if exponent too large
  * maybe round to fit into frac

#### FP Multiplication

$(-1)^(s1) M1 2^(E1) x (-1)^(s2) M2 2^(E2) $

* Exact Result: (-1)^(s) M 2^(E)
  * Sign s: s1^s2
  * siginificand M: M1 x M2
  * Exponent E: E1 + E2

* Fixing
  * If M >= 2, shift M right, increment E
  * If E out of range, overflow
  * Round M to fit frac precision

Example: $1.010*2^2 x 1.110*2^3 = 10.0011*2^5 = 1.00011*2^6 = 1.001*w^6 $

#### FP Addition

* $(-1)^(s1) M1 2^(E1) + (-1)^(s2) M2 2^(E2) $
* Get binary points lined up. This means convert the smaller FP into the same exponent as the larger one, and then add them. Finally convert the result into FP again.
* Fixing
  * IF M>=2, shift M right, increment E
  * If M<2, shift M left k positions, decrement E by k 

#### Algebra of FP addition and multiplication comparing with Abelian Groups

* Not associative: Possibly overflow and inexactness of rounding
* FP Mul is not distributive over addition: possibly overflow, ineactness of rounding

### FP in C

* float: single precision
* double: double precision

Conversion/Casting

* Casting between int, float, double changes bit representation
* double/float -> int
  * Truncates fractional part
  * Like rounding toward zero
  * Not defined when out of range or NaN
  * int -> double: exact conversion
  * int -> float: will round according to rounding mode