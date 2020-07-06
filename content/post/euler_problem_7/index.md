---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Project Euler Problem 7: 10001st prime"
subtitle: ""
summary: ""
authors: [grae]
tags: [Euler]
categories: []
date: 2020-06-07T18:01:03-06:00
lastmod: 2020-06-07T18:01:03-06:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: true

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

Back to primes! So far we've been able to get away with being a little greedy with our compute when playing with primes. Now Euler is ratcheting up the difficulty and we'll have to focus on efficiency.

As usual, if you haven't spent time with [Problem 7](https://projecteuler.net/problem=7) yet, take a chance to play with it on your own and come back.

{{< figure library="true" lightbox="false" src="school_of_athens_euclid.png" caption="Euclid teaching his students, detail from The School of Athens by Raphael">}}


## Counting Primes

Let's start by looking at each integer, deciding whether it's prime, and counting it if it is until we get to the 10,001st prime.

Our first stab at an `is_prime(n)` function will be the simplest and we'll iterate into more optimized (and complicated) versions after. Here's the starting point:

```python
def is_prime(n):
    if n < 2:
        return False
    for x in range(2, n):
        if n % x == 0:
            return False
    return True
```

This checks every number less than $ n $ to see if it's a factor of $ n $. It's _almost_ good enough to solve the problem in under a minute. My laptop chugs through the first 10,001 primes in 68 seconds using this version of the `is_prime(n)` function (full code later). But the rules only give us a minute and we can do better.

## Cap the Search Space

Looking back at our [Problem 3 Solution](https://www.grae.io/post/euler_problem_3/) we optimized our `is_prime(n)` function by caping the space we search to find factors factors by checking only numbers up to $ \sqrt{n} $. Check out that post if you want to dig deep into why / how that works.


```python
def is_prime(n):
    if n < 2:
        return False
    for x in range(2, math.floor(math.sqrt(n)) + 1):
        if n % x == 0:
            return False
    return True
```

This runs a _lot_ faster. It finds the 10,001st prime in 0.29 seconds on my machine. But can we make it _even better_?

## Skip Through the Search Space

[Perhaps the most rediscovered result about primes numbers](https://primes.utm.edu/notes/faq/six.html) is the fact that every prime bigger than 3 is "next" to a multiple of 6. That is, for every prime number starting at 5 you can get a multiple of 6 by adding 1 or subtracting 1.

For example:
  * 5 is prime, add 1 and get 6
  * 13 is prime, subtract 1 and get (6 * 2)
  * 1,361 is prime, add 1 and get (6 * 227)

This works for every prime number.

We can use this property to skip potential factors we don't _need_ to check. When checking to see if a number $ n $ has factors we can get away with just looking for the prime factors, we don't _also_ need to know if it has any factors that are themselves composite. For example, we don't need to know that 24 is divisible by 8. We can stop as soon as we see it's divisible by 2. So we can skip every potential factor except for those which _might_ be prime.

In code:

```python
def is_prime(n):
    if n < 2:
        return False
    if n == 2 or n == 3:
        return True
    if n % 2 == 0 or n % 3 == 0:
        return False
    for x in range(6, math.floor(math.sqrt(n)) + 2, 6):
        if n % (x - 1) == 0 or n % (x + 1) == 0:
            return False
    return True
```

This version takes advantage of Python's "step" argument to `range()`. We're looking at every multiple of 6 (below our limit) and checking whether the number before or after it divides our target.

This optimizes things a bit more and, indeed, finds the 10,001st prime in 0.17 seconds on my machine.

## Putting it Together

Once we have an efficient `is_prime()` function the solution is a matter of counting primes with a while loop.

```python
seen = 0
n = 1
while seen < 10001:
    n += 1
    if is_prime(n):
        seen += 1

print(n)
```

## Going Further

There are ways to solve this problem even faster. You _could_ use the [Prime Number Theorem](https://en.wikipedia.org/wiki/Prime_number_theorem) to approximate an upper bound for a [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) and sieve out the answer. We'll deal with those concepts in coming problems so for now I'll leave that as an exercise for the reader.