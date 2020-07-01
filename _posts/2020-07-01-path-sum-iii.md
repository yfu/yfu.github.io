---
layout: single
title:  "Path Sum III"
excerpt: ""
categories: []
tags: [leetcode, dynamic programming]
date:   2020-07-01 19:25:00 -0500
classes: wide
categories: leetcode update
---

[437. Path Sum III](https://leetcode.com/problems/path-sum-iii/)

### Solution 1: two recursive functions. The time complexity is O(N^2) and space complexity is O(N).

{% highlight python %}
class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> int:
        
        def dfs(root, sum):
            if not root: return 0
            return pathSumFromRoot(root, sum) + dfs(root.left, sum) + dfs(root.right, sum)
            
        def pathSumFromRoot(root, sum):
            if not root: return 0
            return int(root.val == sum) + pathSumFromRoot(root.left, sum-root.val) + pathSumFromRoot(root.right, sum-root.val)
        
        return dfs(root, sum)    
{% endhighlight %}

### Solution 2: prefix sums. For each of the paths from the root to a node, we can maintain the prefix sums we've seen so far. When we encounter a new node, we can quickly look up to see if `current node's value` + `previous prefix sum` - `sum` is already in the prefix sums. If so, we add that to the counter. Both the time and space complexity is O(N).

{% highlight python %}
class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> int:
        
        prefixSums = collections.defaultdict(int)
        count = 0
        prefixSums[0] = 1
        
        def helper(root, last=0):
            nonlocal count
            
            if not root: return 0
            
            ps = last+root.val
            # if prefixSums[ps - sum] > 0:
            #     count += prefixSums[ps - sum]
            count += prefixSums[ps - sum]
            prefixSums[ps] += 1
            helper(root.left, ps)
            helper(root.right, ps)
            prefixSums[ps] -= 1
            
        helper(root)
        return count
{% endhighlight %}
