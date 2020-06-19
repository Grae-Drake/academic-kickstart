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
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

Back to primes! So far we've been able to get away with being a little greedy with our compute when playing with primes. Now Euler is ratcheting up the difficulty and we'll have to focus on efficiency.

As usual, if you haven't spent time with [Problem 7](https://projecteuler.net/problem=7) yet, take a chance to play with it on your own and come back.

{{< figure library="true" lightbox="false" src="" caption="TODO">}}


## Counting Primes

Let's start by looking at each integer, deciding whether it's prime, and counting it if it is until we get to the 10,001st prime.

To begin, we'll use the `is_prime(n)` function we wrote back for our [Problem 3 Solution](https://www.grae.io/post/euler_problem_3/). This function is already pretty optimized because it checks only numbers up to $ \sqrt{n} $


```python
def is_prime(n):
    for x in range(2, n):
        if n % x == 0:
            return False
    return True

seen = 0
n = 1
while seen < 10001:
    n += 1
    if is_prime(n):
        seen += 1

print(n)
```