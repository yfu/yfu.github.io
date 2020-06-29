---
layout: single
title:  "Longest Increasing Subsequence"
excerpt: ""
categories: []
tags: [leetcode, dynamic programming]
date:   2020-06-29 16:00:00 -0500
classes: wide
categories: leetcode update
---

[1048. Longest String Chain](https://leetcode.com/problems/longest-string-chain/)

Solution: bottom-up dynamic programming

{% highlight python %}
class Solution:
    def longestStrChain(self, words: List[str]) -> int:        
        def is_predecessor(s, l):
            # Returns True is s is a predecessor of l
            if len(l) - len(s) != 1:
                return False
            i, j = 0, 0
            while i < len(s) and j < len(l):
                if s[i] == l[j]:
                    i += 1
                    j += 1
                else:
                    j += 1
                    return s[i:] == l[j:]
            return j == len(l) - 1
        
        dp = {w: 1 for w in words}
        
        lengths = collections.defaultdict(list)
        lengths[0] = []
        for w in words:
            lengths[len(w)].append(w)
        
        ls = sorted(lengths.keys())
        for l in ls:
            for w in lengths[l]:
                for w2 in lengths[l-1]:
                    if is_predecessor(w2, w):
                        dp[w] = max(dp[w], dp[w2]+1)
        
        return max(dp[i] for i in dp)     
{% endhighlight %}
