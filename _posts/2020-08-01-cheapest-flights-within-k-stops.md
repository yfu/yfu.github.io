---
layout: single
title:  "Cheapest Flights Within K Stops"
excerpt: ""
categories: []
tags: [leetcode]
date:   2020-08-01 02:09:00 -0700
classes: wide
categories: leetcode update
---

[787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

### BFS

{% highlight python %}
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
        
        g = collections.defaultdict(list)
        for u, v, w in flights:
            g[u].append([v, w])
            
        costs = collections.defaultdict(lambda: float("inf"))
        ret = float("inf")

        q = collections.deque()
        q.append([0, -1, src])
        ret = float("inf")
        
        while q:
            cur_cost, cur_stop, cur_city = q.popleft()
            # print(cur_cost, cur_stop, cur_city)
            if cur_stop > K:
                break

            if costs[cur_city] <= cur_cost:
                continue
            else:
                costs[cur_city] = cur_cost
            if cur_city == dst:
                ret = min(ret, cur_cost)
            for _neigh, _cost in g[cur_city]:
                q.append([cur_cost+_cost, cur_stop+1, _neigh])
        # print(ret)        
        return ret if ret != float("inf") else -1
{% endhighlight %}

### DP (top-down), Bellman-Ford's
{% highlight python %}
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
        
        g2 = collections.defaultdict(list)
        for u, v, w in flights:
            g2[v].append([u, w])
            
        @functools.lru_cache(None)
        def dp(stops, city):
            if stops < -1:
                return float("inf")
            if city == src:
                return 0
            
            ret = dp(stops-1, city)
            for prev, w in g2[city]:
                ret = min(ret, dp(stops-1, prev) + w)
                
            return ret
        
        ret = dp(K, dst)
        return ret if ret != float("inf") else -1
{% endhighlight %}

### DP with rolling arrays (bottom-up)
{% highlight python %}
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
        
        g2 = collections.defaultdict(list)
        for u, v, w in flights:
            g2[v].append([u, w])
            
        dp, dp2 = [float("inf")] * n, [float("inf")] * n        
        dp[src] = 0
        
        for k in range(1, K+2):
            for u, v, w in flights:
                dp2[v] = min(dp2[v], dp[u] + w)
            dp = [i for i in dp2]
            
        return dp[dst] if dp[dst] != float("inf") else -1
                
{% endhighlight %}

