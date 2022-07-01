# My LeetCode Notebook

Hi guys! I'm starting to update my LeetCode solution in Github.

# Binary Search

## Find the first

e.g. for the array `nums = [0, 0, 1, 1, 2, 2]`, find the position of the first 1

```python
# target = 1
l = 0
r = len(nums) - 1
while l < r:
    mid = (l + r) // 2
    if nums[mid] < target:
        l = mid + 1
    elif nums[mid] == target:
      	r = mid
    else:
      	r = mid - 1
ans = l
```

**Why let mid = (l + r) // 2 ?**

Consider when nums[l] == target and nums[r] == target. Since we want to find the left bound, we let mid = l rather than mid = r.

**Why write if-else like this?**

- `nums[mid] != target`

Apparently, current `mid` is definately not what we want, so move `l` to the right or move `r` to the left.

- `nums[mid] == target`

Now we find one of the target value, but we are not sure whether it is the first one. Meanwhile, one thing we are quite sure about is that all elements on the **right** of `mid` are useless. Therefore, we need to **keep it in our searching range but rule out all elements on its right**. That's why we let `r = mid` 

## Find the last

e.g. for the array `nums = [0, 0, 1, 1, 2, 2]`, find the position of the last 1

```python
# target = 1
l = 0
r = len(nums) - 1
while l < r:
    mid = (l + r + 1) // 2
    if nums[mid] <= target:
        l = mid
    else:
        r = mid - 1
ans = l
```

**Why let mid = (l + r + 1) // 2 ?**
Consider when nums[l] == target and nums[r] == target. Since we want to find the right bound, we let mid = r rather than mid = l.

**Why write if-else like this?**

- `nums[mid] != target`

Just like above.

- `nums[mid] == target`

Now we find one of the target value, but we are not sure whether it is the last one. Meanwhile, one thing we are quite sure about is that all elements on the **left** of `mid` are useless. Therefore, we need to **keep it in our searching range but rule out all elements on its left**. That's why we let `l = mid` 

## 'while' condition

- When I use `while l < r`

I want to find the **index/ position** of the target

- When I use `while l <= r`

I want to find the **value** of the target

## Exercise

[34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

[702. Search in a Sorted Array of Unknown Size]([(1) Search in a Sorted Array of Unknown Size - LeetCode](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/))

A smart way to define search boudaries

[33. Search in Rotated Sorted Array]([(1) Search in Rotated Sorted Array - LeetCode](https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/))

Make sure to consider the edge cases (<=, >=, <, >)

[*81. Search in Rotated Sorted Array II]([(1) Search in Rotated Sorted Array II - LeetCode](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/))

Check position of the target and mid first, and then move the pointers.

[*4. Median of Two Sorted Arrays]([(1) Median of Two Sorted Arrays - LeetCode](https://leetcode.com/problems/median-of-two-sorted-arrays/))
