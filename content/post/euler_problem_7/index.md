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
draft: true

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
    for x in range(2, n):
        if n % x == 0:
            return False
    return True
```

This is _almost_ good enough to solve the problem in under a minute. My laptop chugs through the first 10,001 primes in 68 seconds using that version of the `is_prime(n)` function (full code later). But we can do better.

Looking back at our [Problem 3 Solution](https://www.grae.io/post/euler_problem_3/) we optimized our `is_prime(n)` function by checking only numbers up to $ \sqrt{n} $. Check out that post if you want to dig deep into why / how that works.


```python
def is_prime(n):
    for x in range(2, math.floor(math.sqrt(n)) + 1):
        if n % x == 0:
            return False
    return True
```

This runs a _lot_ faster. It finds the 10,001st prime in 0.37 seconds. But can we make it _even better_?

[Perhaps the most rediscovered result about primes numbers](https://primes.utm.edu/notes/faq/six.html) is the fact that every prime bigger than 3 is "next" to a multiple of 6. That is, every prime number is either 1 greater than or 1 less than 6 * k for some k. Using that property we can skip the 2/3rds of the integers. In code:

```python
def is_prime(n):
    if n == 2 or n == 3:
      return True
    for x in range(6, math.floor(math.sqrt(n), 6) + 1):
        if n % x == 0:
            return False
    return True
```

That definition takes advantage of Python's "step" argument to the `range()` function. We're looking at every multiple of 6 (below our limit) and checking whether the number before and after it is prime




That optimi
```python
seen = 0
n = 1
while seen < 10001:
    n += 1
    if is_prime(n):
        seen += 1

print(n)
```