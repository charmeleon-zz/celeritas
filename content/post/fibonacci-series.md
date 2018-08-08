---
title: "Fibonacci series"
date: 2018-08-07T19:29:57-05:00
category: Algorithms
tags: fibonacci, recursion, memoization, dynamic programming
draft: false
---

I've recently been studying algorithm problems. I'm still in the early stages
of this trek, but I have come to the conclusion that coming up with the right
solution is not that important. I find it more valuable to be able to attack a
problem with multiple strategies (_code examples are written in JavaScript_).

As a first example, take the [Fibonacci series](https://en.wikipedia.org/wiki/Fibonacci_number)
where the sequence is defined by the recurrence relation
 `F(n) = F(n - 1) + F(n - 2)`, with seed values `F(0) = 1` and `F(1) = 1`.
It's a recurrence relation, and as far as algorithms go, I think it's the most
naturally recursive problem.

To write a function that is able to generate the n<sup>th</sup> Fibonacci number
`F(n)`, a recursive function is sufficient:
```javascript
function fibonacci(n) {
    if (n < 2) {
        return 1;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

It's evident that this function satisfies the recurrent relation and the seed
values: calling `fibonacci(0)` or `fibonacci(1)` will be trapped by the `if`
statement (since one and zero are less than 2), and the result is one, and
for numbers two and above, the result is the sum of the previous two Fibonacci
numbers.

Although mathematically correct, this solution is computationally inefficient.
You can read up some literature and discover that the runtime complexity is
_O(2<sup>n</sup>)_ (really _1.6<sup>n</sup>)_.

In an effort to be able to calculate larger Fibonacci numbers faster, we can
recognize that `fibonacci(3 - 1)` returns the same value as `fibonacci(4 - 2)`
and we can avoid computing the same Fibonacci number by storing previously
calculated values. This technique is called memoization:
```javascript
// Initialize our map with the seed values
var previous = {
    0: 1,
    1: 1
};
function fibonacci(n) {
    // This value was previously calculated
    if (n in previous) {
        return previous[n];
    }
    // First time calculating this result, we'll add it to our collection
    previous[n] = fibonacci(n - 1) + fibonacci(n - 2);
    return previous[n];
}
```

This way a Fibonacci number is calculated only once - our runtime complexity is
`O(n)`. Since we now have to store previously calculated values our storage
complexity is also `O(n)` (we store every result up to `F(n)`), which is not
a problem for small enough values of `n`.

There is one approach where we can keep our linear complexity without keeping
every previous Fibonacci number in memory, meaning. This approach requires a
little more analysis to realize that a non-recursive approach is possible:

```javascript
function fibonacci(n) {
    var res1 = 1;
    var res2 = 1;
    
    if (n < 2) {
        return 1;
    }

    var tmp = null;
    while (n > 2) {
        tmp = res1;
        // F(n - 1) becomes the sum of the two previous Fibonacci numbers
        res1 = res1 + res2;
        // Previous F(n - 1) becomes F(n - 2)
        res2 = tmp;
        // Decrease n
        n -= 1;
    }
    return res1 + res2;
}
```

This gives us an `O(n)` runtime complexity with an `O(1)` storage size, the
best of both worlds.

This is not a unique property of the Fibonacci series. There are many
algorithmic problems where a variety of solutions will satisfy the problem
exactly, and one approach is likely to be much better than others. The
unoptimized solution is typically straightforward enough to write the first time
encountering the problem, but the others require a more exposure and familiarity
with similar enough problems to approach in that fashion. I will write about
a few more problems where we can see this again. 