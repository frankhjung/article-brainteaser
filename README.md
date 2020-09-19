---
title: 'Magic Triangle Brainteaser Solution using Haskell'
author: Frank Jung
geometry: margin=25mm
header-includes:
  - \usepackage{fancyhdr}
  - \usepackage{graphicx}
  - \pagestyle{fancy}
  - \fancyfoot[L]{© Frank H Jung  2020}
date: '18 September 2020'

---

I first saw this puzzle in [CSIRO](https://www.csiro.au/)'s [Double
Helix](https://doublehelixshop.csiro.au/)
[here](https://blog.doublehelix.csiro.au/a-magic-triangle-brainteaser/).

The problem we are solving is described as:

You are given a triangle with circle on each point and on each side:

![Magic Triangle](files/magic-triangle.png)

Then, using the numbers from 1 to 6, arrange them in a triangle with three
numbers on each side. Swap them around until the sides all add up to the same
number.

Finally, sum each side to 10.

## Solution

Let's label the triangle: starting from any vertex label the nodes:

![Labelled Magic Triangle](files/magic-triangle-labelled.png)

The method to solve this problem is broken into the following steps:

  - get all permutations of numbers 1 to 6 as a, b, c, d, e, f

  - filter permutation to satisfy conditions:

    - `a + b + c == c + d + e == e + f + a`

    - and final condition: `a + b +c == 10`

### Using Haskell

All permutations of numbers 1 to 6:

```haskell
import Data.List

permutations [1..6]
```

_This will give `6! = 720` permutations._

Filter on sides summing up to the same value:

```haskell
[
  [(a,b,c), (c,d,e), (e,f,a)] | [a,b,c,d,e,f] <- permutations [1..6],
    a+b+c == c+d+e &&
    c+d+e == e+f+a
]
```

Which gives all solutions where the sides are equal sums:

```text
  [(3,2,5),(5,4,1),(1,6,3)]
  [(2,4,3),(3,5,1),(1,6,2)]
  [(1,5,3),(3,4,2),(2,6,1)]
  [(1,4,5),(5,2,3),(3,6,1)]
  [(5,3,4),(4,2,6),(6,1,5)]
  [(6,2,4),(4,3,5),(5,1,6)]
  [(4,5,2),(2,3,6),(6,1,4)]
  [(6,3,2),(2,5,4),(4,1,6)]
  [(2,3,6),(6,1,4),(4,5,2)]
  [(4,1,6),(6,3,2),(2,5,4)]
  [(3,4,2),(2,6,1),(1,5,3)]
  [(1,6,2),(2,4,3),(3,5,1)]
  [(5,4,1),(1,6,3),(3,2,5)]
  [(4,3,5),(5,1,6),(6,2,4)]
  [(6,1,5),(5,3,4),(4,2,6)]
  [(3,6,1),(1,4,5),(5,2,3)]
  [(5,2,3),(3,6,1),(1,4,5)]
  [(2,6,1),(1,5,3),(3,4,2)]
  [(3,5,1),(1,6,2),(2,4,3)]
  [(1,6,3),(3,2,5),(5,4,1)]
  [(5,1,6),(6,2,4),(4,3,5)]
  [(2,5,4),(4,1,6),(6,3,2)]
  [(6,1,4),(4,5,2),(2,3,6)]
  [(4,2,6),(6,1,5),(5,3,4)]
```

Filter on sides summing to 10:

```haskell
[
  [(a,b,c), (c,d,e), (e,f,a)] | [a,b,c,d,e,f] <- permutations [1..6],
    a+b+c == 10 &&
    c+d+e == 10 &&
    e+f+a == 10
]
```

Which gives our final list of solutions:

```text
  [(3,2,5),(5,4,1),(1,6,3)]
  [(1,4,5),(5,2,3),(3,6,1)]
  [(5,4,1),(1,6,3),(3,2,5)]
  [(3,6,1),(1,4,5),(5,2,3)]
  [(5,2,3),(3,6,1),(1,4,5)]
  [(1,6,3),(3,2,5),(5,4,1)]
```

Note here that the solutions aren't unique: there are repetitions if you
consider rotations or node reversals. Can we filter these out to get the only
unique solution?

Try ordering:

```haskell
[
  [(a,b,c), (c,d,e), (e,f,a)] | [a,b,c,d,e,f] <- permutations [1..6],
    a+b+c == 10 &&
    c+d+e == 10 &&
    e+f+a == 10 &&
    a > c &&
    c > e
]
```

The idea here is that as the nodes are unique, we can order them. This yields
our final solution:

```text
  [(5,2,3),(3,6,1),(1,4,5)]
```

Using one other Haskell refinement we can write this as:

```haskell
[
  [(a,b,c), (c,d,e), (e,f,a)] | [a,b,c,d,e,f] <- permutations [1..6],
    all (==10) [a+b+c, c+d+e, e+f+a] &&
    a > c &&
    c > e
]
```

Check your answer on CSIRO page
[here](https://blog.doublehelix.csiro.au/a-magic-triangle-brainteaser/#answer).

## Using Python

To do ...
