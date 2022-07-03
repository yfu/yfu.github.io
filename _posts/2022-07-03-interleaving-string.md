---
layout: single
title:  "Interleaving String"
excerpt: ""
categories: []
tags: [leetcode]
date:   2022-07-03 04:27:00 -0700
classes: wide
categories: leetcode update
---

[97. Interleaving String](https://leetcode.com/problems/interleaving-string/)

### 1D dynamic programming

Note that there is no need to have another memo array as it is updated exactly from left to right.

{% highlight python %}
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        l1, l2, l3 = len(s1), len(s2), len(s3)
        if l3 != l1 + l2:
            return False
        if l1 == 0:
            return s2 == s3
        if l2 == 0:
            return s1 == s3
        memo = []
        memo = [False for _ in range(l2+1)]
        memo[0] = True
        for i2 in range(1, l2+1):
            memo[i2] = memo[i2-1] and s2[i2-1] == s3[i2-1]
        for i1 in range(1, l1+1):
            memo[0] = memo[0] and s1[i1-1] == s3[i1-1]
            for i2 in range(1, l2+1):
                i3 = i1 + i2 
                memo[i2] = (memo[i2] and s1[i1-1]==s3[i3-1]) or (memo[i2-1] and s2[i2-1]==s3[i3-1])
        return memo[-1]
{% endhighlight %}
