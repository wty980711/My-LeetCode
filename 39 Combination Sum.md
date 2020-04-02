# 39. Combination Sum

## Description
Given a set of candidate numbers `(candidates)` (without duplicates) and a target number `(target)`, find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

**Note:**

All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.

## Code: Python
```python
class Solution(object):
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        rst = []
    
        def search(value, curr, candi):
            if value == target:
                rst.append(curr)
                return
            elif value > target: return
            for i in range(len(candi)):
                search(value + candi[i], curr + [candi[i]], candi[i:])

        search(0, [], candidates)
        return rst
```
