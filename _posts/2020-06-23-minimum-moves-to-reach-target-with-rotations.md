---
layout: single
title:  "Dungeon Game"
excerpt: ""
categories: []
tags: [leetcode, dynamic programming]
date:   2020-06-22 21:35:00 -0500
classes: wide
categories: leetcode update
---

[1210. Minimum Moves to Reach Target with Rotations](https://leetcode.com/problems/minimum-moves-to-reach-target-with-rotations/)

Solution: Each time the snake is horizontal, there are 3 possibilities: moving right, moving down, rotating clockwise. If the snake is vertical, the 3 possibilities become moving right, moving down, rotating counter-clockwise. This is reminiscent of problems 63, 64, 120, 174, and 931, which suggests that we may be able to use dynamic programming to solve the problem. However, care should be taken to ensure that the snake isn't stuck at the same place forever keeping rotating clockwise and counter-clockwise (stack overflow for recursion). So, in addition to the coordinates and orientation, we also need to keep track of whether the snake has already rotated. If it was previously rotated to the current position, we don't want the snake to rotate again as this would cause the snake to get stuck, which will never give us the minimum moves. Here, the last parameter in ```dp(r, c, hor = True, rotated = False)``` keeps track of that. The remaining part is easy, and the only tricky thing is checking if there's any obstacle blocking a move.

{% highlight python %}
class Solution:
    def minimumMoves(self, grid: List[List[int]]) -> int:
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        n = len(grid)
        if grid[n-1][n-2] == 1 or grid[n-1][n-1] == 1: 
            return -1
        
        cache = {(n-1, n-2, True, False): 0, (n-1, n-2, True, True): 0}
        
        def dp(r, c, hor = True, rotated = False):
            # Tail is at (r, c)
            # If hor is True: head is at (r, c+1)
            # if hor is False: head is at (r+1, c)
            tup = (r, c, hor, rotated)
            if tup in cache:
                return cache[tup]
            
            v1, v2, v3 = float("Inf"), float("Inf"), float("Inf")
            if hor:
                # Horizontal
                if c + 2 < n and grid[r][c+2] == 0:
                    # Move right
                    v1 = dp(r, c+1, True, False)
                if r + 1 < n and c + 1 < n and grid[r+1][c] == 0 and grid[r+1][c+1] == 0:
                    # Move down
                    v2 = dp(r+1, c, True, False)
                    # Rotate if it hasn't rotated yet
                    if not rotated:
                        v3 = dp(r, c, False, True)
            else:
                # Vertical
                if r+2 < n and grid[r+2][c] == 0:
                    # Move down
                    v1 = dp(r+1, c, False, False)
                if c + 1 < n and r + 1 < n and grid[r][c+1] == 0 and grid[r+1][c+1] == 0:
                    # Move right
                    v2 = dp(r, c+1, False, False)
                    # Rotate if it hasn't rotated yet
                    if not rotated:
                        v3 = dp(r, c, True, True)
            cache[tup] = min(v1, v2, v3) + 1
            return cache[tup]

        ret = dp(0, 0, True, False)
        return ret if ret != float("Inf") else -1
{% endhighlight %}
