---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Project Euler Problem 1: Multiples of 3 and 5"
subtitle: ""
summary: ""
authors: [grae]
tags: [Euler]
categories: []
date: 2020-05-22T16:12:38-07:00
lastmod: 2020-05-22T16:12:38-07:00
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

This is a lovely [problem](projecteuler.net/problem=1) to start with. It has a straightforward brute-force loop solution as well as a nice analytic solution where you can calculate the solution directly without the need for much programming. And it's [fizzbuzz](https://en.wikipedia.org/wiki/Fizz_buzz#Programming)! What a great way to dive in.


## Who is this for?
A quick note: this series of posts is meant for people who already have a development environment set up and are familiar with the very basics of Python. If you've never written a line of Python before I recommend [Data Camp's Python introduction](https://learn.datacamp.com/courses/intro-to-python-for-data-science) or another free Python 3 tutorial that covers the basics of **variables**, **basic types** (ints, floats, strings, booleans), **operations**, **lists**, **dictionaries**, and **functions**. If you're an experienced programmer but new to Python check out the "official" [Python Tutorial](https://docs.python.org/3/tutorial/). And if you're comfy writing Python but don't have a good local development environment set up yet check out [Problem 0: Getting Started](/) [TODO].

Also, this is meant for people who enjoy solving or reading about [Project Euler](projecteuler.net) problems. PE has this to say about sharing solutions: 

> We hope that you enjoyed solving this problem. Please do not deprive others of going through the same process by publishing your solution outside of Project Euler. Members found to be spoiling problems beyond #100 will have their accounts locked (see note).
>
> Note: The rule about sharing solutions outside of Project Euler does not apply to the first 100 problems, as long as any discussion clearly aims to instruct methods, not just provide answers, and does not directly threaten to undermine the enjoyment of solving later problems. Problems 1 to 100 provide a wealth of helpful introductory teaching material and if you are able to respect our requirements, then we give permission for them to be discussed elsewhere.

I take PE's requirements about sharing information seriously and hope you will too. I love that so much of the programming world happens in the open with public repos and such, but you won't find my work on problems 101+ here, on GitHub, or anywhere else public. 



## The Problem
Ok, let's talk about threes and fives. Before we analyze it together, [take a few minutes alone with Problem 1](https://projecteuler.net/problem=1). Set up a Project Euler account if you haven't already. Consider how you _might_ solve it. Take a stab at writing a solution before moving on. I'll wait.

...


## Iterative Solutions
Great, you're back. Let's dive in. The most straightforward way to solve this problem is to look at every number from one to a thousand, test whether it's divisible by 3 or 5, and add it to our running total if it is:

```python {linenostart=199}
result = 0
integers = range(1, 1000)
for x in integers:
  if x % 3 == 0 or x % 5 == 0:
    result += x

print(result)
```

In this solution we initialize our result to `0`, use a `for` loop to iterate over every integer from `1` through `1000`, test whether it's divisible by `3` or `5` using the modulo operator (`%`) and add it to our `result` variable if it is. Bam.


### Using List Comprehensions
[List comprehensions](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions) can be an elegant and "pythonic" way to solve problems. Here's the same iterative solution above using a list comprehension rather than a loop.

```python
print(sum([x for x in range(1, 1000) if x % 3 == 0 or x % 5 == 0]))
```

Now isn't that short and sweet. I love list comprehensions. They can get ugly fast, and it's possible to overuse them, but comprehensions excel in cases like this where you can express a few lines of procedural code as a single thought.


### A More Functional Approach
Python isn't known for being a naturally functional language, but it's certainly possible to use it that way. You can use `filter()` in place of the comprehension above:

```python
print(sum(filter(lambda x: x % 3 == 0 or x % 5 == 0, range(1, 1000))))
```

Looking at this approach next to the comprehension above it's easy to see why the comprehension is more idiomatic: it just reads easier.

And of course it's no fun to talk about functional programming without contorting `reduce()` into a solution: 

```python
from functools import reduce
print(reduce(
    lambda x, y: x + y if y % 3 == 0 or y % 5 == 0 else x,
    range(1, 1000),
    0
))
```

With Python 3 `reduce()` is no longer a built-in function and instead needs to be imported from the `functools` library. That makes me sad. I can understand why (a comprehension is _almost_ always more practical), but speaking as an apostle of [_Reduce, God of Parentheses_](https://www.youtube.com/watch?v=Wl98eZpkp-c&feature=youtu.be&t=18) I have to say I miss having it an my fingertips.


# Analytic solution
The iterative solutions above all rely on generating a list of integers, checking them one by one, and adding them up. That works just fine when you're only counting to a thousand and you have modern computing resources at your fingertips. But things aren't always so easy. What if we were solving this for all integers up to a quadrillion? What if we were working in an environment with limited memory or compute? What if we needed to run this solution gajillions of times every second?

You might have heard a [famous anecdote about Gauss](https://en.wikipedia.org/wiki/Carl_Friedrich_Gauss#Anecdotes) and how as a young kid he summed the numbers from 1 to 100 in just a few seconds. He didn't actually add all the numbers together, he recognized a pattern in the arithmetic progression 1, 2, 3, ... 99, 100 and used a formula instead. Specifically, you can "fold" the sequence and match pairs like 1 + 100, 2 + 99, 3 + 98, ... 49 + 52, 50 + 51. There are exactly 50 such pairs and each pair equals 101, so the total sum is 101 * 50 = 5,050.

This approach works with _any_ arithmetic progression. We could sum all multiples of 5 below a hundred by "folding" the sequence 5, 10, 15, ... 95, 100 and getting 10 pairs that equal 105 for a total sum of 1,050.

In general, the formula to calculate the [sum of an arithmetic progression](https://en.wikipedia.org/wiki/Arithmetic_progression#Sum) is: `number_of_terms * (first_term + last_term) / 2`.

How does this apply to our fizzbuzz problem? The integers we're summing don't form a nice arithmetic progression. And we can't just add the sum of multiples of three to the sum of multiples of five, because that would double-count numbers like 15 and 30. And 45. And 60. See a pattern?

The trick is in recognizing that we're dealing with _three_ arithmetic progressions.  If we want the sum of all numbers that are a multiple of three _or_ a multiple of five, we can find that by adding `sum(multiples_of_three` to `sum(multiples_of_five)` and then _subtracting_ `sum(multiples_of_fifteen)`.

```python
def simple_arithmetic_series(step, limit):
    first = step
    count = (limit - 1) // step
    last = step * count
    return count * (first + last) / 2

multiples_of_3 = simple_arithmetic_series(3, 1000)
multiples_of_5 = simple_arithmetic_series(5, 1000)
multiples_of_15 = simple_arithmetic_series(15, 1000)

print(multiples_of_3 + multiples_of_5 - multiples_of_15)
```


# Evil optimization
The analytic solution involves a lot more typing and it isn't as simple to read and understand, so why is it better?

Well, it _isn't_ better. There are tradeoffs between the analytic solution and the iterative solution. Neither is inherently better. Deciding which approach is better for you depends on your context.

In the context of this specific problem, _the iterative solution works just fine_. It's faster to conceive and implement, it's easier to understand, and less code is less chance for things to break. With our crazy strong computers and the small numbers involved the analytic solution is just a bunch of optimization that was (maybe) fun to do but doesn't buy us anything valuable.

This is an example of **premature optimization**, and as Donald Knuth famously said, "[premature optimization is the root of all evil](https://en.wikipedia.org/wiki/Program_optimization#When_to_optimize)". Spending time optimizing things we don't need to isn't just a waste of time better spent elsewhere, it also makes our programs worse: harder to understand and maintain.

That said, it's easy to think of cases where the tradeoffs might play out the other way. What if we were working with much larger numbers? Try plugging in bigger numbers to the iterative and analytic solutions and watch where the difference in performance starts to get noticeable. My laptop struggles to run the iterative solution above about a hundred million. Even the world's biggest supercomputer, [Summit](https://en.wikipedia.org/wiki/Summit_(supercomputer)) doesn't have enough memory to store a list of all integers up to a quadrillion. But my dinky laptop can calculate `simple_arithmetic_series(3, 1000000000000000)` with no noticeable delay. With the right algorithm, my laptop can do something that's literally impossible for the world's best supercomputer.

Those numbers seem ridiculous but spoiler alert: Project Euler is going to start throwing hefty numbers at us pretty quick. As we dig deeper performance, resources, and [computational complexity](https://en.wikipedia.org/wiki/Computational_complexity_theory) are going to get critically important.

See an issue on this page? Report a typo, coding error, or general feedback on GitHub.