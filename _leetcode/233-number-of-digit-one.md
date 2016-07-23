---
layout: leetcode
date: 2016-07-18
title: Number of Digit One
tags: [Math]
---

* Contents
{:toc #toc_of_keans_blog}

## Question

Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.

For example:

Given n = `13`,

Return 6, because digit 1 occurred in the following numbers: `1, 10, 11, 12, 13`.

**Hint:**

1. Beware of overflow.



***

## Solution

**Result:** Accepted **Time:**  0 ms

Here should be some explanations.

```c
int countDigitOne(int n) {
    long long ret = 0;
    int base = 1, last = 0, t;
    while(n > 0)
    {
        t = n % 10;
        n /= 10;
        if( t == 1)
            ret += n*base + last + 1;
        else if(t == 0)
            ret += n*base;
        else
            ret += (n+1)*base;
        last = t * base + last;
        base *= 10;
    }
    return ret;
}
```

**Complexity Analytics**

- Time Complexity: $$O(log(n))$$
- Space Complexity: $$O(1)$$
