---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Project Euler Problem 5: Smallest Multiple"
subtitle: ""
summary: ""
authors: [grae]
tags: [Euler]
categories: []
date: 2020-06-03T10:33:17-06:00
lastmod: 2020-06-03T10:33:17-06:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---


[Problem 5](https://projecteuler.net/problem=5) is a lot of fun (well, "fun") because (1) there's a very simple program requiring no math that calculates the answer, but (2) that program would need impossible amounts of compute to actually run, and (3) you can figure the answer with pen and paper super fast if you think about the math a bit. That's what normal people consider fun, right?

Since this problem leans much more on number theory than on programming we'll use it as an excuse to talk about math more than usual. Our first real step into number theory was [Problem 3](https://projecteuler.net/problem=3) ([blog post](grae.io/post/euler_problem_3/)) with prime factors. Here we'll talk about least common multiples and prime factorization. This will be useful: as we get deeper into Project Euler we'll get much deeper into number theory.

As always, spend some time with [Problem 5](https://projecteuler.net/problem=5) on your own if you haven't already.

{{< figure library="true" src="ulam_spiral.jpeg" caption="An Ulam Spiral visualizing prime factorization. Source: <a href='https://en.wikipedia.org/wiki/Number_theory'>Wikipedia</a>" lightbox="false" >}}


## Brute Force?

Each Project Euler problem has basically two parts: an example and a problem statement. In this case the example is:

>2520 is the smallest number that can be divided by each of the numbers from 1 to 10 without any remainder.

These examples are crazy useful. They aren't just there to define the problem: they let us work through the problem with simpler, smaller inputs. Testing and tinkering with small examples can yield insights, concepts, and strategies that help solve the problem with tougher inputs. They're hints; if you look at them hard enough.

Now, as a programmer your instinct might be to throw piles of compute at this problem. The example number is pretty small (just 2,520). Maybe we can just light up a few nested loops to chew through this?

```python
found = False
n = 1
factors = range(1, 11)
while not found:
    found = True
    for x in factors:
        if n % x != 0:
            found = False
    if found:
        print(i)
    n += 1
print(n)
```

That program gives `2520` with no appreciable delay. Let's crank our range up to 20 and try it out!

...

Yeah so I pulled the plug after taking more than a minute to iterate all the way up to `19034074` without finding the answer. Clearly the number we're looking for is too big to find with this brute force approach. Looks like we're going to _have_ to think about the math a bit.


## Break Out the Pen and Paper

If we can't brute force our way, there must be a more direct solution. This is where Project Euler's hints are crazy useful.

One way to calculate a number that's divisible by a set of factors (a "**common multiple**") is to just multiply the factors all together. In our example case that's:

$$ 1 * 2 * 3 * 4 * 5 * 6 * 7 * 8 * 9 * 10 $$

That's cumbersome to write out, so instead I'll use $ ! $, which is short hand for the [factorial operation](https://en.wikipedia.org/wiki/Factorial).

We see that $ 10! $ is $ 3{,}628{,}800 $. That's MUCH bigger than $ 2{,}520 $. Exactly $ 1{,}440 $ times bigger in fact. What's so special about $ 1{,}440 $? And why can you evenly divide it by 2 and 3 so many times?

Now, _you know_ that every integer is either prime or the product of prime factors. You _may not_ know that's called the "unique factorization theorem", or, with more gravitas, the "[fundamental theorem of arithmetic](https://en.wikipedia.org/wiki/Fundamental_theorem_of_arithmetic)".

Finding the prime factors of a number is called **prime factorization** and it can take a long time with big numbers. But you can factorize all the numbers you need to solve this problem in your head because they're all so small.

Tinkering with a calculator and without needing to write any code, we can see that the prime factors of $ 1{,}440 $ are:

$$ [2{,\ }2{,\ }2{,\ }2{,\ }2{,\ }3{,\ }3{,\ }5] $$

You could say that each of those is an "extra" factor and that $ 10! $ has a bunch of extra factors of $ 2 $ and $ 3 $ and $ 5 $ compared to the least common multiple. This is even clearer if we set the prime factorization of the least common multiple ("LCM") and $ 10! $ next to each other:

  $$ LCM = 2{,}520 = 2^3 * 3^2 * 5^1 * 7^1 $$
  $$ 10! = 3{,}628{,}800 = 2^8 * 3^4 * 5^2 * 7^1 $$ 

Progress! Is there a way we can figure out (1) which prime numbers are factors of our answer and (2) how many of each prime is "enough"?


### Which Prime Numbers?

Looking at the example we see none of the prime factors are larger than 10. And that every prime number less than 10 is represented at least once.

This makes sense. The next largest prime number, 11, isn't a factor of any of the numbers 1-10. And _every_ number, _including each prime number_, less than 10 is a factor.

So we know that our answer has every prime number less than 20 as a factor at least once, and none of the prime numbers larger than 20 as a factor.

### How many of each?

Some of the primes from 2 to 19 have to be represented more than once. We know that because _that number_ (9,699,690) would have been found with our brute force approach if it were correct. We also know that because a number like 4, which has two factors of 2, wouldn't evenly divide it. And 8, which has _three_ factors of two _also_ wouldn't evenly divide it.

Turns out that we need "enough" of each prime to make every one of the factors (1 to 20). Looking at the answer to the example, it needs _three_ factors of 2 because it needs to be divisible by 8. And it needs _two_ factors of 3 because it needs to be divisible by 9. But only _one_ factor of the larger primes.


## Coding a solution

At this point it might actually be easier to break out a pen and tally the answer. You could do that by looking at each number from 1 to 20, calculating the prime factors for each number, keeping the largest count of each prime factor, and then multiplying out everything. But instead, let's write some code to do that!

```python
from math import floor

# A simple function to test whether a number is prime.
def is_prime(n):
    for x in range(2, floor(n ** 0.5) + 1):
        if n % x == 0:
          return False
    return True

# The number we'll check up to. Set to 10 to test the code then change to 20.
limit = 100

# A list of primes we know show up at least once.
primes = [x for x in range(2, limit + 1) if is_prime(x)]

# A simple function to calculate the prime factors of a number.
def prime_factors(n):
    result = {prime: 0 for prime in primes}
    i = 0
    for prime in primes:
        while n % prime == 0:
            result[prime] += 1
            n = n / prime
    return result

# Initialize factors. We know each prime below limit shows up at least once.
factors = {prime: 1 for prime in primes}

# Look at the prime counts for each number in our range and keep the highest.
for x in range(2, limit + 1):
    for prime, count in prime_factors(x).items():
        factors[prime] = max(count, factors[prime])

# Calculate and print the result.
result = 1
for prime, count in factors.items():
    result *= prime ** count

print(result)
```

There are ways to further optimize this solution. For example, you could sidestep the need for a `prime_factors()` function altogether with witty application of some logarithms and the observation that 3 * 3 * 3 is bigger than 20. But that feels a bit silly. Even bumping up our limit to 100 and running this program it calculates the 41-digit answer without noticeable delay.
