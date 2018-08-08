---
title: "Fibonacci series"
date: 2018-08-07T19:29:57-05:00
category: Algorithms
tags: fibonacci, recursion, memoization, dynamic programming
draft: false
---

I've been studying algorithms lately. I'm still in the early stages
of this trek, but I have concluded that coming up with the right
solution is not that important. It's far more valuable to be able to attack a
problem with multiple strategies (_note: code examples are written in JavaScript_).

As an example, take the [Fibonacci series](https://en.wikipedia.org/wiki/Fibonacci_number)
where the n<sup>th</sup> Fibonacci number is defined by the recurrence relation
 `F(n) = F(n - 1) + F(n - 2)`, and the definitions `F(0) = 1` and `F(1) = 1`.
It's a recurrence relation, and as far as algorithms go, I find it the most
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

It's evident that this function satisfies the recurrence relation and the
definitions: calling `fibonacci(0)` or `fibonacci(1)` will return `1` (since
one and zero are less than 2); and for numbers two and above, the result is the
sum of the previous two Fibonacci numbers.

Although mathematically correct, this solution is computationally inefficient.
You can read some of the literature to discover that the runtime complexity is
_O(2<sup>n</sup>)_ (really _1.6<sup>n</sup>)_.

In an effort to be able to calculate larger Fibonacci numbers, we
recognize that `fibonacci(3 - 1)` returns the same value as `fibonacci(4 - 2)`
and we can avoid computing the same Fibonacci number by storing previously
calculated values. This technique is called memoization:
```javascript
// Initialize our map object with the seed values
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
`O(n)`. We now have to store all previously calculated values so our storage
complexity is also `O(n)` (we store every result up to `F(n)`), which is not
a problem for small enough values of `n`.

There is one approach where we can still calculate in linear time without
keeping every previous Fibonacci number in memory. This approach requires
careful analysis to realize that a non-recursive approach is possible by
retaining `F(n-1)` and `F(n - 2)` in variables:

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

This gives us an `O(n)` runtime complexity and our storage does not need to
grow regardless of how large `n` is.

There are many problems where a variety of solutions will satisfy the problem
exactly, and one approach is likely to be much better than others. The
unoptimized solution is typically straightforward enough to write the first time
encountering the problem, but the others require more exposure and familiarity
with similar problems to approach in more creative ways. I will continue to
write about such problems in the next blog post. 