---
layout: post
title: "Fermat's Theorem and Inverse factorials!"
excerpt: "Understanding modular multiplication (and division?)."
categories: [algorithm]
image:
    feature: 
comments: true
---

Hello people!

I've been competing in programming contests for nearly (maybe even more than?) 2 years, and I've seen my fair share of esoteric, or fairly "rare" algorithms or approaches -- that don't really have explicit documentation on most public forums.

This is one of them.

Fermat's Theorem is an integral component of number theory, and is **heavily** involved in cryptography.

Not to mention, it's in a TON of algorithmic challenges that you might encounter.

So let's jump in!

### Fermat's Theorem

Fermat's theorem is a specific application of the more general Euler's theorem.

I'll explain the latter in a moment, but here's Fermat's theorem.

For a natural number `a` and a prime number `p`, the following equation will always be satisfied. 

<img src="https://latex.codecogs.com/gif.latex?a^{p-1}&space;{\equiv}&space;1&space;{mod}&space;{(p)}" title="a^{p-1} {\equiv} 1 {mod} {(p)}" />

In code, this looks like -

```python

a**(p-1) % p == 1

```

This is a very specific application of Euler's Theorem, which is far more general.

We'll revisit this in a moment.

### Euler's Theorem

Euler's theorem states that for any natural number `a` and any natural number `n`, the following equation is always satisfied:

<img src="https://latex.codecogs.com/gif.latex?a^{\varphi(n)}&space;{\equiv}&space;1&space;{mod}&space;{(n)}" title="a^{\varphi(n)} {\equiv} 1 {mod} {(n)}" />

Here, you'll notice the exponent isn't `p-1`.

The exponent here is called **the totient function**.

### Totient Function

The totient function, briefly put, is a function that denotes how many natural numbers exist below a certain number, that are co-prime with this number.

That is, 

```python

phi(n) = len( {x where x>0 and x<n and gcd(x, n)==1} )

```

I won't dive into how to effectively compute this for a given number. That's left as an exercise for the reader.

I want to point out WHY Fermat's theorem is a specific case of this general theorem.

Note that in the case of Fermat's theorem, we know that `n` is a prime.

Now logically speaking, gcd(a prime number, any other number) = 1.

Therefore, the totient function of any prime number `n`, is always equal to `n-1`.

That's why, Fermat's theorem is - 

<img src="https://latex.codecogs.com/gif.latex?a^{p-1}&space;{\equiv}&space;1&space;{mod}&space;{(p)}" title="a^{p-1} {\equiv} 1 {mod} {(p)}" />

The `p-1` in the exponent, is the totient function of `p`.

### Modular Multiplicative Inverses

So now that we know Euler's theorem, let's dive into multiplicative inverses.

Modular multiplicative inverses (I'll just call them inverses, for the rest of this article) are a concept introduced in modular arithmetic, that basically define for a number, another number which when multiplied with the initial number results in a value of 1 after taking the modulo.

For example, let M = 11 and let A = 3 (where M is the modulo number, and A is your number on the LHS.)

The multiplicative inverse of A under modulo M, is 4.

As 4 * A = 4 * 3 = 12.

12 mod 11 = 1.

Similarly, the multiplicative inverse for 5 under modulo 11, is 9.

9 * 5 = 45.

45 mod 11 = 1.

See?

### How this is used in programming contests

Often times, you'll need to divide large numbers despite under modular calculations.

For example, try calculating C(n, k) where n goes as high as 10^6.

Modular arithmetic doesn't support division under modulo.

Obviously, you can't calculate factorial(n) and then divide it by it's denominator since you'll run into overflow issues.

So, we use multiplicative inverses.

**Multiplicative inverses act in the same manner as dividing the initial number.**

This is because modular arithmetic supports multiplication.

(a * b) mod c = ((a mod c) * (b mod c)) mod c

So, to divide a number `Y` by `X`, for example, we multiply `Y` with the multiplicative inverse of `X`.

And that's it!

Except I haven't yet told you how to compute the multiplicative inverse -- and now I shall.

### Computing Multiplicative Inverses

This is very simple.

We know Fermat's theorem. Most programming contests provide a modulo value = 10^9 + 7, or another such prime. And this is the reason why.

<img src="https://latex.codecogs.com/gif.latex?a^{&space;p&space;-&space;1}&space;{\equiv}&space;1&space;{mod}&space;{(p)}" title="a^{ p - 1} {\equiv} 1 {mod} {(p)}" />

I'll rewrite this, as -

<img src="https://latex.codecogs.com/gif.latex?a&space;*&space;a^{&space;p&space;-&space;2&space;}&space;{\equiv}&space;1&space;{mod}&space;{(p)}" title="a * a^{ p - 2 } {\equiv} 1 {mod} {(p)}" />

And that's it!

This is of the form -

<img src="https://latex.codecogs.com/gif.latex?a&space;*&space;i&space;{\equiv}&space;1&space;{mod}&space;{(p)}" title="a * i {\equiv} 1 {mod} {(p)}" />

And this form implies that `i` is the modular multiplicative inverse of `a`.

And thus, to determine the modular multiplicative inverse of any number `a` where the modulo value is prime, we simply need to raise `a` to the power of `mod - 2`.

```python

def inv(a, mod):
	inverse = pow(a, mod-2, mod) 	# binary exponentiation to calculate a**(mod-2) % mod 
	return inverse 					# time complexity of O(log(mod-2))

```

NOTE: This technique is only valid when M is prime. The generalization would be to raise `a` to the power of `totient_function(M) - 1`, however it's also worth noting that the multiplicative modular inverse can only exist if `a` and `M` are co-prime.

Good luck!

---

~ Aditya Ramesh
