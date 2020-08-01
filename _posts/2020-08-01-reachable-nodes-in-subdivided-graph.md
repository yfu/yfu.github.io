---
layout: single
title:  "Reachable Nodes In Subdivided Graph"
excerpt: ""
categories: []
tags: [leetcode]
date:   2020-08-01 02:09:00 -0700
classes: wide
categories: leetcode update
---

[882. Reachable Nodes In Subdivided Graph](https://leetcode.com/problems/reachable-nodes-in-subdivided-graph/)

### DFS (Time Limit Exceeded)

{% highlight python %}
class Solution:
    def reachableNodes(self, edges: List[List[int]], M: int, N: int) -> int:
        g = collections.defaultdict(list)
        reachables = collections.defaultdict(int)
        for i, j, n in edges:
            g[i].append([j, n])
            g[j].append([i, n])
        
        memo = collections.defaultdict(int)
        
        def dfs(node, steps):
            if memo[node] >= steps:
                return
            memo[node] = steps
            for neigh, dist in g[node]:
                if steps <= dist:
                    reachables[(node, neigh)] = steps
                else:
                    reachables[(node, neigh)] = dist
                    dfs(neigh, steps-dist-1)
                    
        dfs(0, M)
        ret = len(memo)
        for i, j, n in edges:
            ret += min(n, reachables[(i, j)] + reachables[(j, i)])
        return ret

{% endhighlight %}

### Dijkstra
{% highlight python %}
class Solution:
    def reachableNodes(self, edges: List[List[int]], M: int, N: int) -> int:
        g = collections.defaultdict(list)
        counts = collections.defaultdict(int)
        for i, j, n in edges:
            g[i].append([j, n])
            g[j].append([i, n])
        
        h = []
        h.append([0, 0])
        visited_edges = set()
        visited_nodes = set()
        
        while h:
            cur_steps, cur_node = heapq.heappop(h)
            visited_edges.add(cur_node)
            visited_nodes.add(cur_node)
            for neigh, weight in g[cur_node]:
                if (cur_node, neigh) in visited_edges:
                    continue
                visited_edges.add((cur_node, neigh))
                if M - cur_steps <= weight:
                    counts[(cur_node, neigh)] = M - cur_steps
                else:
                    counts[(cur_node, neigh)] = weight
                    heapq.heappush(h, [cur_steps + weight + 1, neigh])
        ret = len(visited_nodes)
        for i, j, n in edges:
            ret += min(n, counts[(i, j)] + counts[(j, i)])            
        return ret
{% endhighlight %}
