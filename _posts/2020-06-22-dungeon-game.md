---
layout: single
title:  "Dungeon Game"
excerpt: ""
categories: []
tags: [leetcode, programming, dp, dynamic programming]
date:   2020-06-22 21:35:00 -0500
classes: wide
categories: leetcode update
---

[174. Dungeon Game](https://leetcode.com/problems/dungeon-game/)

The demons had captured the princess (P) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid. Our valiant knight (K) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.

Some of the rooms are guarded by demons, so the knight loses health (negative integers) upon entering these rooms; other rooms are either empty (0's) or contain magic orbs that increase the knight's health (positive integers).

In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.


Write a function to determine the knight's minimum initial health so that he is able to rescue the princess.

For example, given the dungeon below, the initial health of the knight must be at least 7 if he follows the optimal path RIGHT-> RIGHT -> DOWN -> DOWN.

| |Col0|Col1|Col2|
| --- | --- | --- | --- |
|Row0| -2(K)| -3| 3|
|Row1|-5|-10|1|
|Row2|10|30|-5 (P)|

Note:

The knight's health has no upper bound.
Any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.



Solution:

For the iterative method, the trick is that we need to process from the bottom-right to the upper-left corner for the iterative method. If we do it the other way around (upper-left to bottom-right), we won't be able to determine the minimum health at current position [i][j] given [i-1][j] (U) and [i][j-1] (L). In some cases, it's tempting the select one of U and L as the correct direction just to get a good minimum health, it is entirely possible that the other direction---which gives worse minimum health—--makes the knight to have more health to lead to a better global minimum health.

1. Iterative:

```
class Solution(object):
    def calculateMinimumHP(self, dungeon):
        """
        :type dungeon: List[List[int]]
        :rtype: int
        """
        nrow, ncol = len(dungeon), len(dungeon[0])
        
        for r in range(nrow-1, -1, -1):
            for c in range(ncol-1, -1, -1):
                if r == nrow-1 and c == ncol-1:
                    dungeon[r][c] = max(1, 1-dungeon[r][c])
                elif r == nrow-1:
                    dungeon[r][c] = max(1, dungeon[r][c+1] - dungeon[r][c])
                elif c == ncol-1:
                    dungeon[r][c] = max(1, dungeon[r+1][c] - dungeon[r][c])
                else:
                    # print(r, c)
                    dungeon[r][c] = min(max(1, dungeon[r][c+1] - dungeon[r][c]),
                                        max(1, dungeon[r+1][c] - dungeon[r][c]))
                    
        return dungeon[0][0]
```

2. Recursive:

```
class Solution(object):
    def calculateMinimumHP(self, dungeon):
        """
        :type dungeon: List[List[int]]
        :rtype: int
        """
        nrow, ncol = len(dungeon), len(dungeon[0])
        cache = {}
        
        def dp(r, c):
            tup = (r, c)
            if tup in cache:
                return cache[tup]
            
            if r == nrow - 1 and c == ncol - 1:
                minhealth = max(1, 1 - dungeon[r][c])
            elif r == nrow - 1:
                minhealth = max(1, dp(r, c+1) - dungeon[r][c])
            elif c == ncol - 1:
                minhealth = max(1, dp(r+1, c) - dungeon[r][c])
            else:
                minhealth = min(max(1, dp(r, c+1) - dungeon[r][c]), 
                                max(1, dp(r+1, c) - dungeon[r][c]))
            cache[tup] = minhealth
            return cache[tup]
        
        return dp(0, 0)            
```




