---
title:  "Number Theory 1 -- Divisibility and Modular Arithmetic"
tags: Number Theory
---

## Division

### Definition 1: 

If a and b are integers with a ≠ 0, we say that a divides b if there is an integer c such that
b = ac, or equivalently, if b/a is an integer. When a divides b we say that a is a factor or divisor of b, and that b is a multiple of a. The notation a | b denotes that a divides b. We write a ∤ b when a does not divide b.

### Theorem 1

Let a, b, and c be integers, where a ≠ 0. Then

1. if a \| b and a \| c, then a \| (b + c); 
2. if a \| b, then a \| bc for all integers c;
3. if a \| b and b \| c, then a \| c.

### Corollary 1

If a, b, and c are integers, where a ≠ 0, such that a | b and a | c, then a | mb + nc whenever
m and n are integers

## The Division Algorithm

### Theorem 2

THE DIVISION ALGORITHM Let a be an integer and d a positive integer. Then there
are unique integers q and r, with 0 ≤ r < d, such that a = dq + r.

### Definition 2

In the equality given in the division algorithm, d is called the divisor, a is called the dividend, q is called the quotient, and r is called the remainder. This notation is used to express the quotient and remainder:

q = a **div** d, r = a **mod** d.

## Modular Arithmetic

### Difinition 3

If a and b are integers and m is a positive integer, then a is *congruent to b modulo m* if
m divides a − b. We use the notation a ≡ b (mod m) to indicate that a is congruent to
b modulo m. We say that a ≡ b (mod m) is a **congruence** and that m is its **modulus** (plural
**moduli**). If a and b are not congruent modulo m, we write a &NotCongruent; b (mod m).  

### Theorem 3

Let a and b be integers, and let m be a positive integer. Then a ≡ b (mod m) if and only
if a *mod* m = b *mod* m.

### Theorem 4

Let m be a positive integer. The integers a and b are congruent modulo m if and only if there
is an integer k such that a = b + km.

### Theorem 5

Let m be a positive integer. If a ≡ b (mod m) and c ≡ d (mod m), then

a + c ≡ b + d (mod m) and ac ≡ bd (mod m).

### Corollary 2

Let m be a positive integer and let a and b be integers. Then

(a + b) *mod* m = ((a *mod* m) + (b *mod* m)) *mod* m

and

ab *mod* m = ((a *mod* m)(b *mod* m)) *mod* m.