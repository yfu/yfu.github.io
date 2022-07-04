---
layout: single
title:  "Walls and Gates"
excerpt: ""
categories: []
tags: [leetcode]
date:   2022-07-03 10:06:00 -0700
classes: wide
categories: leetcode update
---

[286. Walls and Gates](https://leetcode.com/problems/walls-and-gates/)

### BFS

Use a tuple (row, col, dist) as elements in the queue

{% highlight python %}
from collections import deque

class Solution:
    def wallsAndGates(self, rooms: List[List[int]]) -> None:
        """
        Do not return anything, modify rooms in-place instead.
        """        
        INF = 2147483647
        # -1 A wall, 0 A gate, INF Infinity means an empty room. 
        queue = deque()
        rows, cols = len(rooms), len(rooms[0])
        for r in range(rows):
            for c in range(cols):
                if rooms[r][c] == 0:
                    queue.append((r, c, 0))
        while queue:
            r, c, dist = queue.popleft()
            for r_delta, c_delta in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                r2, c2 = r + r_delta, c + c_delta
                if r2 >= 0 and r2 < rows and c2 >= 0 and c2 < cols and rooms[r2][c2] == INF:
                    rooms[r2][c2] = dist + 1
                    queue.append((r2, c2, dist+1))
{% endhighlight %}

BFS layer by layer
{% highlight python %}
from collections import deque

class Solution:
    def wallsAndGates(self, rooms: List[List[int]]) -> None:
        """
        Do not return anything, modify rooms in-place instead.
        """
        INF = 2147483647
        queue = deque()
        rows, cols = len(rooms), len(rooms[0])
        for r in range(rows):
            for c in range(cols):
                if rooms[r][c] == 0:
                    queue.append((r, c))
        dist = 0
        while queue:
            dist += 1
            queue2 = deque()
            while queue:
                r, c = queue.popleft()
                for r_delta, c_delta in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                    r2, c2 = r + r_delta, c + c_delta
                    if r2 >= 0 and r2 < rows and c2 >= 0 and c2 < cols and rooms[r2][c2] == INF:
                        rooms[r2][c2] = dist
                        queue2.append((r2, c2))
            queue = queue2
{% endhighlight %}
