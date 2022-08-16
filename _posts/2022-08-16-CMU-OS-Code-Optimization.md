---
layout: 'post'
title: 'OS CMU Note Code Optimization' 
categories: 'OS'
permalink: '/:categories/:title'
---

# Questions:

1. How to code optimization is estabilished in the bubble sort example?´
2. 

# Overview

## Performance Realities

There is more to performance than asymptotic complexity, and constant factors matter too!
In general, code optimization consists of four stages: algorithm, data representations, procedures, and loops.

**Important:**: We need to understand system to optimize performance. It consists of following questions:

* How programs are compiled and excuetd?
* How modern processrs + memory systems operate?
* How to measure program performance and identify bottlenecks?
* How to improve performance without destroying code modularity and generality?

# Useful Optimizations

## Code Motion

Reduce frequence with which computation performed: If it will always produce same result

```C
void set_row(double *a, double *b, long i, long n)
{
    //original version
    long j;
    for(j=0; j<n; j++)
        a[n*i + j] = b[j];

    // improved version
    long j;
    int ni = n*i;  // improvement
    for(j=0; j<n; j++)
        a[ni+j] = b[j];

    // assembly A in %rdi,  B in %rsi, n in %rcx, j in %rax
    set_row:
        testq %rcx, %rcx  //Test n
        jle .L1   // If <= 0, goto done
        imulq %rcx, %rdx //ni = n*i
        leaq (%rdi, %rcx, 8), %rdx //rowp = A + ni*8
        movl $0, %eax  //j=0
    .L3:
        movsd (%rsi, %rax, 8), %xmm0 //t=b[j]
        movsd %xmm0, (%rdx, %rax,8)  //M[A+ni*8 + j*8] = t
        addq $1, %rax //j++
        cmpq %rcx, %rax // check the result n-j
        jne .L3  // If the comparison above is not euqal goto L3
    .L1 //done
        rep; ret

}
```

## Reduction in Strength

* Use simple addtion and subtraction instead of multiplication and division.
* Use ***shift, add*** instead of mltiply or divide. $16*x --> x << 4$
* R
ecognize sequence of products

```C
//original version

for (i=0; i<n; i++)
{
    int ni = n*i;
    for(j=0; j<n; j++)
    {
        a[ni+j] = b[j];
    }
}

//improved version
int ni = 0;
for (i=0; i<n; i++)
{   
    for(j=0; j<n; j++)
    {
        a[ni+j] = b[j];
    }
    ni += n;
}
```
In this example, we recoginized the repeated multiplication in ni, so we changed it
to a simpler addition. The idea is that, in each iteration of i, ni is obtained by multiply the fixed n with the varied i. We can instead add n after iteration of all j.

## Shared Common Subexression

Reuse portions of expressions

```C
/**sum neighbors of i,j*/

//original version
up = val[(i-1)*n + j];
down = val[(i+1)*n +j];
left = val[i*n + j-1];
right = val[i*n + j+1];
sum = up + down + left + right;

//improved version
long inj = i*n + j;
up = val[inj - n];
down = val[inj + n];
left = val[inj - 1];
right = val[inj + 1];
sum = up + down + left + right;

```

# Optimization Blockers

# Exploiting Instruction-Level Parallelism

# Dealing with Conditionals

