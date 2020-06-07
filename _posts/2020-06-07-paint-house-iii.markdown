---
layout: single
title:  "Paint House III"
excerpt: ""
categories: []
tags: [leetcode, programming, dp, dynamic programming]
date:   2020-06-07 14:35:00 -0500
classes: wide
categories: leetcode update
---

[1473. Paint House III](https://leetcode.com/problems/paint-house-iii/)

There is a row of `m` houses in a small city, each house must be painted with one of the `n` colors (labeled from `1` to `n`), some houses that has been painted last summer should not be painted again.

A neighborhood is a maximal group of continuous houses that are painted with the same color. (For example: houses = `[1,2,2,3,3,2,1,1]` contains 5 neighborhoods `[{1}, {2,2}, {3,3}, {2}, {1,1}]`).

Given an array `houses`, an `m * n` matrix cost and an integer `target` where:

`houses[i]`: is the color of the house `i`, 0 if the house is not painted yet.
`cost[i][j]`: is the cost of paint the house `i` with the color `j+1`.

Return the minimum cost of painting all the remaining houses in such a way that there are exactly `target` neighborhoods, if not possible return `-1`.

Solution:

We process houses one by one. Each time we process a new house, we have two possibilities: 

1. If it's already painted, we proceed to the next one. 
2. Otherwise, we try one of all possible choices of colors, set the the house to that color, and proceed to the next house (recursion). When we're back from the recursion, we need to reset the house to old color (backtracking). 

A key idea is identify cases of repeated computations (memoization). For a given house (`hid`: house id), a given number of neighborhoods from previous houses (`prev_neigh`) and the color of the previous house (`prev_color`), the cost only need to be computed *once*.

One easy way to do it is use `@functools.lrt_cache(None)` before the `dp` function and it will handle the memoization for us. Another way is to build a dictionary `cache` to memoize the results from previous computations.

```
class Solution:
    def minCost(self, houses: List[int], cost: List[List[int]], m: int, n: int, target: int) -> int:
        cache = {}
        
        def dp(hid, prev_neigh, prev_color):
            key = (hid, prev_neigh, prev_color)
            if key in cache:
                return cache[key]
            
            if hid == len(houses):
                return 0 if prev_neigh == target else float("Inf")            
            else:
                if houses[hid] != 0:
                    additional = 1 if houses[hid] != prev_color else 0
                    cache[key] = dp(hid+1, prev_neigh + additional, houses[hid])
                    return cache[key]
                else:
                    my_min = float("Inf")
                    for new_color in range(1, n+1):
                        houses[hid] = new_color
                        additional = 1 if houses[hid] != prev_color else 0
                        my_min = min(my_min, dp(hid+1, prev_neigh + additional, houses[hid]) + 
                                     cost[hid][new_color-1])
                        houses[hid] = 0
                    cache[key] = my_min
                    return my_min    
                            
        ret = dp(0, 0, -1)
        
        return -1 if ret == float("Inf") else ret
```


