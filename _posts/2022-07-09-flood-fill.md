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

### Using DFS
{% highlight python %}
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:
        r, c = len(image), len(image[0])
        
        def dfs(ir, ic):
            cur_color = image[ir][ic]
            if cur_color != color:
                image[ir][ic] = color
            for jr, jc in ((ir-1, ic), (ir+1, ic), (ir, ic-1), (ir, ic+1)):
                if 0 <= jr < r and 0 <= jc < c and image[jr][jc] == cur_color and image[jr][jc] != color:
                    dfs(jr, jc)
        
        dfs(sr, sc)
        
        return image
{% endhighlight %}

### Using a stack

{% highlight python %}
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:
        nrows, ncols = len(image), len(image[0])
        
        stack = []
        stack.append((sr, sc))
        
        while stack:
            cur_r, cur_c = stack.pop()
            cur_color = image[cur_r][cur_c]
            if cur_color != color:
                image[cur_r][cur_c] = color
                for r, c in ((cur_r-1, cur_c), (cur_r+1, cur_c), (cur_r, cur_c-1), (cur_r, cur_c+1)):
                    if 0 <= r < nrows and 0 <= c < ncols and image[r][c] == cur_color:
                        stack.append((r, c))
                        
        return image
{% endhighlight %}
