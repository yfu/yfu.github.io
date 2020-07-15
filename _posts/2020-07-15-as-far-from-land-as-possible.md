---
layout: single
title:  "1162 As Far from Land as Possible"
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


{% highlight java %}
class Solution {
    int gridSize;
    public int maxDistance(int[][] grid) {
        
        this.gridSize = grid.length;
        
        Queue<Triplet> q = new ArrayDeque<Triplet>();
            
        for (int i = 0; i < gridSize; ++i) {
            for (int j = 0; j < gridSize; ++j) {
                if (grid[i][j] == 1) {
                    for (List<Integer> l: this.getNeighbors(i, j)) {
                        q.add(new Triplet(l.get(0), l.get(1), 1));
                    }
                }
            }
        }
        int ret = -1;
        while (!q.isEmpty()) {
            Triplet t = q.remove();
            int i = t.arr[0], j = t.arr[1], dist = t.arr[2];
            if (grid[i][j] != 0) continue;
            grid[i][j] = dist;
            for (List<Integer> tmp: this.getNeighbors(i, j)) {
                q.add(new Triplet(tmp.get(0), tmp.get(1), dist + 1));
                ret = Math.max(ret, dist);
            }
        }
        
        return ret;
        
    }
    private List<List<Integer>> getNeighbors(int i, int j) {
        List<List<Integer>> ret = new ArrayList<>();
        if (i + 1 < gridSize)
            ret.add(Arrays.asList(i+1, j));
        if (i - 1 >= 0)
            ret.add(Arrays.asList(i-1, j));
        if (j + 1 < gridSize)
            ret.add(Arrays.asList(i, j+1));
        if (j - 1 >= 0)
            ret.add(Arrays.asList(i, j-1));
        return ret;
    }
}

class Triplet {
    public int[] arr;
    public Triplet(int a, int b, int c) {
        arr = new int[]{a, b, c};
    }
}
{% endhighlight %}
