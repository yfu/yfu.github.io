---
layout: single
title:  "1162. As Far from Land as Possible"
excerpt: ""
categories: []
tags: [leetcode]
date:   2020-07-15 01:01:00 -0500
classes: wide
categories: leetcode update
---

[1162. As Far from Land as Possible](https://leetcode.com/problems/as-far-from-land-as-possible/)

### BFS


{% highlight python %}
class Solution:
    def maxDistance(self, grid: List[List[int]]) -> int:
        
        N = len(grid)
        q = collections.deque()
        
        def get_neighbors(x, y):
            ret = []
            for xx, yy in ((-1, 0), (1, 0), (0, -1), (0, 1)):
                if 0 <= x + xx < N and 0 <= y + yy < N:
                    ret.append((x + xx, y + yy))
            return ret
            
        for i in range(N):
            for j in range(N):
                if grid[i][j] == 1:
                    for ii, jj in get_neighbors(i, j):
                        if grid[ii][jj] == 0:
                            q.append((ii, jj, 1))
        
        ret = -1        
        while q:
            i, j, dist = q.popleft()
            if grid[i][j] != 0:
                continue
            grid[i][j] = dist
            ret = max(ret, dist)
            
            for ii, jj in get_neighbors(i, j):
                q.append((ii, jj, dist+1))
                
        return ret
            
{% endhighlight %}
