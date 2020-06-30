---
layout: single
title:  "Edit Distance"
excerpt: ""
categories: []
tags: [leetcode, dynamic programming]
date:   2020-06-29 21:25:00 -0500
classes: wide
categories: leetcode update
---

[72. Edit Distance](https://leetcode.com/problems/edit-distance/)

Solution: Dynamic programming. 

This is basically a simplified [global sequence alignment](https://en.wikipedia.org/wiki/Sequence_alignment#Global_and_local_alignments) commonly used in bioinformatics. In bioinformatics, different operations are usually assigned different weights to reflect the likelihood. For example, in some situations, mutations are more likely than insertions due to the properties of DNA/RNA, so replacement has lower penalty. 

### 1. Naïve method
This uses a matrix as big as R x C where R is the length of word1 and C is the length of word2.

{% highlight python %}
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        nrow, ncol = len(word1), len(word2)
        if nrow == 0: return ncol
        if ncol == 0: return nrow
        
        dp = []
        for i in range(nrow+1):
            dp.append([])
            for j in range(ncol+1):
                dp[i].append(0)
                
        for r in range(nrow+1):
            for c in range(ncol+1):
                if r == 0 and c == 0:
                    dp[r][c] = 0
                elif r == 0:
                    dp[r][c] = dp[r][c-1] + 1
                elif c == 0:
                    dp[r][c] = dp[r-1][c] + 1
                else:
                    score = 0 if word1[r-1] == word2[c-1] else 1
                    dp[r][c] = min(dp[r-1][c] + 1, dp[r][c-1] + 1, dp[r-1][c-1] + score)
        # print(dp)            
        return dp[-1][-1]
{% endhighlight %}

### 2. Lower space complexity

{% highlight python %}
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        nrow, ncol = len(word1), len(word2)
        if nrow == 0: return ncol
        if ncol == 0: return nrow
        
        dp = list(range(0, ncol + 1))
        dp2 = [0] * (ncol + 1)
        
        for r in range(nrow+1):
            for c in range(ncol+1):
                if r == 0 and c == 0:
                    dp2[c] = 0
                elif r == 0:
                    dp2[c] = dp2[c-1] + 1
                elif c == 0:
                    dp2[c] = dp[c] + 1
                else:
                    score = 0 if word1[r-1] == word2[c-1] else 1
                    dp2[c] = min(dp[c] + 1, dp2[c-1] + 1, dp[c-1] + score)
            dp, dp2 = dp2, [0] * (ncol+1)
        # print(dp)            
        return dp[-1]
{% endhighlight %}

### 3. Some more tweaks
If we set up the initial arrays right, we can skip some of the statements

{% highlight python %}
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        nrow, ncol = len(word1), len(word2)
        if nrow == 0: return ncol
        if ncol == 0: return nrow
        
        dp = list(range(0, ncol + 1))
        dp2 = [0] * (ncol + 1)
        
        for r in range(1, nrow+1):
            for c in range(ncol+1):
                if c == 0:
                    dp2[c] = dp[c] + 1
                else:
                    score = 0 if word1[r-1] == word2[c-1] else 1
                    dp2[c] = min(dp[c] + 1, dp2[c-1] + 1, dp[c-1] + score)
            dp, dp2 = dp2, [0] * (ncol+1)

        return dp[-1]

{% endhighlight %}

