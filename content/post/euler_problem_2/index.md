---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Project Euler Problem 2: Even Fibonacci numbers"
subtitle: ""
summary: ""
authors: [grae]
tags: [Euler]
categories: []
date: 2020-05-24T15:13:26-07:00
lastmod: 2020-05-24T15:13:26-07:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: "center"
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

Our first, and certainly not our last, encounter with the Fibonacci Sequence on Project Euler. Before we dive into Problem 2 together take some time to chew on it yourself if you haven't already. Did you find a solution? If so have you been able to improve or streamline your first one? If not can you clearly describe to yourself what you're stuck on?

{{< figure library="true" src="Leonardo_Fibonacci.jpeg" caption="Modern mathematicians just aren't this _imposing_. Image: <a href='https://commons.wikimedia.org/wiki/File:Leonardo_da_Pisa.jpg#/media/File:Leonardo_Fibonacci.JPG'>Wikipedia</a>" lightbox="false" >}}


## The Straightforward Approach
The most straightforward way to solve this problem is to generate every Fibonacci number below four million, then look at each one, check whether it's even and add it to our total if it is:

```python
fibs = [1, 1]
while fibs[-1] + fibs[-2] < 4000000:
    fibs.append(fibs[-1] + fibs[-2])

evens = []
for n in fibs:
    if n % 2 == 0:
        evens.append(n)

print(sum(evens))

```

## Saving Some Memory
The solution above works and runs plenty quick. We don't _need_ to simplify. But _can_ we? You might notice that we're creating a pretty big list of fibonacci numbers. Is there a way we can avoid using all that memory? What if, instead of a list, we just kept track of the two most recent numbers and checked for evenness at the same time we generate each new number?

```python
result = 0
a = 1
b = 1
while a + b < 4000000:
    next = a + b
    a = b
    b = next
    
    if b % 2 == 0:
        result += b

print(result)
```

## Saving Some Lines
Mathematicians and programmers coming from other languages might get weirded out by an amazing Python feature called **multiple assignment**. It lets us do things like this:

```python
x = 'ham'
y = 'eggs'

x, y = y, x

print(x)
>>> 'eggs'
print(y)
>>> 'ham'
```

Multiple assignment lets us swap two variables in a single line and do other fun things with assigning to more than one variable at a time. Check out how we can use multiple assignment to compress our solution above in two places:

```python
result = 0
a, b = 1, 1
while a + b < 4000000:
    a, b = b, a + b
    if b % 2 == 0:
        result += b

print(result)
```

This is super useful and can make for much more concise code. Trey Hunter has a great [tutorial on multiple assignment](https://treyhunner.com/2018/03/tuple-unpacking-improves-python-code-readability/) if you want to learn more.


## Skipping Odds
In our solutions so far we've been calculating every fibonacci number and then checking if it's even. What if we could just skip the fibonacci numbers we don't want and just calculate the ones we need?

Look at the sequence below. Do you see a pattern with even numbers?

1, 1, **2**, 3, 5, **8**, 13, 21, **34**, 55, 89, ...

Every third term in the sample above is even. Can you see why it's not just a coincidence and is true for the entire sequence?

Is there a way to calculate _only_ the 3rd, 6th, 9th, etc. items so we don't waste precious CPU cycles on lame odd numbers like `89`? Yes.

{{< figure library="true" src="computer_fire.gif" caption="He must have used more compute than absolutely necessary" lightbox="false" >}}

I won't run through the steps here, but you can start with the fibonacci series definition:

$$ F\_{n} = {\color{red}F\_{n-2}} + \color{blue}F\_{n-1} $$

where $ F\_{n} $ is the nth term in the sequence, and algebraically derive the equation:

$$ F\_{n} = {\color{red}F\_{n-6}} + {\color{blue}F\_{n-3}} + {\color{green}(3 * F\_{n-3})} $$

So normally to get the next term we add the previous two terms. For the next _even_ term we add the previous two _even_ terms, and then add the previous even term three more times. In code:

```python
# Seed with first two even terms.
a, b = 2, 8
result = 10
while a + 4 * b < 4000000:
    a, b = b, a + b + (3 * b)
    result += b

print(result)
```

It's always nice when a more optimal algorithm _also_ makes for less code. However, we've lost some clarity compared to our previous solutions. It's not clear from the code itself where the hard-coded **magic numbers** `2`, `8`, and `10` are coming from. [Magic numbers](https://en.wikipedia.org/wiki/Magic_number_(programming)#Unnamed_numerical_constants) aren't self-explanatory in the way named variables are and they can make a program harder to understand and maintain. That's why I added a comment at the top: I did't think the code alone made it obvious enough to you what it was doing.

## A Golden Solution
Let's get _real_ funky with it. Is there a way to calculate each term directly from the single term before it? For example, how could we look at $ 34 $ and calculate $ 55 $ without knowing or caring that the previous term was 21?

Our buddy Fibonacci discovered way back in 1202 that the ratio between successive terms in the fibonacci sequence converges on $ \phi $, the **golden ratio**. So if you take $ F\_n $ and multiply it by $ \phi $ you get alllllmost $ F\_{n+1} $. Using the example above you get:

$$ 34 * \phi = 55.0131556175... $$

That's _super_ close to the right answer: $ 55 $.

Multiply by $ \phi $ again and you get $ \approx{89} $, and multiply by $ \phi $ one last time to get $ \approx{144} $. Each time we multiply by $ \phi $ we step to the next fibonacci number. We can take three steps at once by multiplying by $ \phi^3 $. More formally:

$$ F\_{n+3} \approx F\_{n} * \phi^3 $$

Ok this trick gets us an _approximate_ answer, but how do we turn that into an _exact_ answer? It turns out the approximation is so good and the error so small that you can just round the result to the nearest integer. That's it:

$$ F\_{n+3} = \\|(F\_{n} * \phi^3)\\| $$

Let's code that up:

```python
# Define phi because it isn't predifined in the Python standard library.
phi = (1 + 5 ** 0.5) / 2
a = result = 2
while a * phi ** 3 < 4000000:
    a = round(a * phi ** 3)
    result += a

print(result)
```

Now ain't that a shiny solution.


## Analytic Approximation (not Solution)
This is a nice solution but it still relies on a `while` loop to calculate items one by one. I can almost, but not _quite_ get to a direct calculation. The approximation of $ \phi $, which was small enough between terms to round away in the solution above, compounds in this stab at an analytic solution and so only gives an approximation.

For the specific problem inputs this gives a result about 5% off from the true answer. Here's my approximation.

If we ignore rounding for now, we can write out the sequence of terms we generate above like this:

$$ 2,\\: 2\phi^3,\\: 2\phi^6,\\: 2\phi^9,\\: 2\phi^{12},\\: ... $$

If you squint real hard, you can see that's a [geometric series](https://en.wikipedia.org/wiki/Geometric_series). It's easier to see if we replace $ \phi^3 $ with the symbol $ r $:

$$ 2r^0,\\: 2r^1,\\: 2r^2,\\: 2r^3,\\: 2r^4,\\: ... $$

where $ r = \phi^3 \approx{4.2360679775} $

Since this is a geometric series we can use the [formula](https://en.wikipedia.org/wiki/Geometric_progression#Geometric_series) for the sum of the first $ n $ terms of a geometric series:

$$ Geometric{\ }Sum = \frac{a(1-r^n)}{1 - r} $$

Where $ a $ is the start term (in our case: $ 2 $), $ r $ is the ratio between terms (in our case $ \phi^3 $ or about $ 4.2360679775 $), and $ n $ is the number of terms.

All we're missing now is $ n $. We can get that by taking the log base $ r $ of our limit (spoiler: it's $ 11 $). Let's code it up:

```python
import math
phi = (1 + 5 ** 0.5) / 2
r = phi ** 3
a = 2
n = 11
print((a * (1 - r ** n)) / (1 - r))
```

Unfortunately the approximations, which were small enough to ignore last time, are now compounding. This attempt overshoots the right answer by about 5%.

I don't know whether there's a way to improve the accuracy of this approach or if there's a way to tweak it to sidestep the approximation issues. If you see something I'm overlooking please reach out and let me know!