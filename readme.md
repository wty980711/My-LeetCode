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

[702. Search in a Sorted Array of Unknown Size](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/)

A smart way to define search boudaries

[33. Search in Rotated Sorted Array - LeetCode](https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/)

Make sure to consider the edge cases (<=, >=, <, >)

[\*81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

Check position of the target and mid first, and then move the pointers.

[\*4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

Use binary search to find partition positions. Remember the total number of elements of left sides of partitions are fixed.

[74. Search a 2D Matrix - LeetCode](https://leetcode.com/problems/search-a-2d-matrix/submissions/) Flat 2D matrix into 1D array.

[\*162. Find Peak Element - LeetCode](https://leetcode.com/problems/find-peak-element/solution/)

Check only one side is ok; Why we can definately find a peek?

[302. Smallest Rectangle Enclosing Black Pixels](https://leetcode.com/problems/smallest-rectangle-enclosing-black-pixels/) disgusting

[852. Peak Index in a Mountain Array](https://leetcode.com/problems/peak-index-in-a-mountain-array/)

Just care about `mid` and `mid + 1`, because there will always be `l <= peak <= r`.

[\*875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/submissions/)

[\*658. Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/)

Binary search with sliding window, interested. Don't use `abs()` in this question.

# Sort

## Bubble Sort

In each pass, swap two adjacent elements, so that we can move the largest element to the end.

```python
# bubble sort
def bubbleSort(nums):
    n = len(nums)
    for i in range(n):
        for j in range(n - 1 - i):
            if nums[j] > nums[j + 1]:
                nums[j], nums[j + 1] = nums[j + 1], nums[j]
```

## Inserttion Sort

Traverse the array, start from `i = 1` to `i = len(nums) - 1`. Each time we let `key = nums[i]`, and we assume that `nums[:i]` is already sorted.

So what we do here is to insert the `key` into the proper place in `nums[:i]`.

And we let `j = i - 1`, and compare `key` and `nums[j]`.

- `if key >= nums[j]`: there is no need to sort, key is already in the right place, and nums[:i+1] is sorted.
- `if key < nums[j]`: `key` is not in the right place, it should locate in somewhere in the front. So we take `key` out of the array, and let `nums[j+1] = nums[j]` to move nums[j] one place to the behind, so that there will be place to insert key in the front. Then we let `j -= 1` and keep comparing numbers in the front with `key`, until we find a place to insert `key` in.

```python
def insertionSort(nums):
    for i in range(1, len(nums)):
        key = nums[i]
        j = i - 1
        while j >= 0 and nums[j] > key:
            nums[j + 1] = nums[j]
            j -= 1
        nums[j + 1] = key
```

## Selection Sort

Each pass, find the least number and move it to the front of the array.

O(N^2)

```python
def selectionSort(nums):
    n = len(nums)
    for i in range(n):
        min_ind = i
        for j in range(i + 1, n):
            if nums[j] < nums[min_ind]:
                min_ind = j
        nums[i], nums[min_ind] = nums[min_ind], nums[i]
```

## Quick Sort

quickSort(nums, low, high):

For the range [low, high] of an array `nums`, let `pivot = nums[high]`, and move pivot to the position where all numbers before are less than of equal to it, and all numbers behind are larger than it.

Then we do quickSort(nums, low, pivot_ind - 1) and quickSort(nums, pivot_ind + 1, high) recursively.

O(nLogn), the worst case is O(n^2). QuickSort is faster in practice, because its inner loop can be efficiently implemented on most architectures, and in most real-world data. QuickSort can be implemented in different ways by changing the choice of pivot, so that the worst case rarely occurs for a given type of data. However, merge sort is generally considered better when data is huge and stored in external storage.

```python
def partition(nums, low, high):
    pivot = nums[high]
    i = low - 1
    for j in range(low, high):
        if nums[j] <= pivor:
            i += 1
            nums[i], nums[j] = nums[j], nums[i]
    nums[i + 1], nums[high] = nums[high], nums[i + 1]
    # [low, i] are numbers <= pivot
    # [i + 1, high] are numbers > pivot
    # so finally swap pivot and nums[i + 1]

    return i + 1

def quickSort(nums, low, high):
    if low < high:
        pivot_ind = partition(nums, low, high)
        quickSort(nums, low, pivot_ind - 1)
        quickSort(nums, pivot_ind + 1, high)

quickSort(nums, 0, len(nums) - 1)
```

[\*215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/submissions/)

Quick select, compare `pivot_idx` with `K`.

## Merge Sort

Partition the array into two subarrays, then sort two subarrays, and merge them together.

T: O(nLogn), depth of the tree is Logn, and for each level we need O(n) time to merge (linearly traverse all).

S: O(n)

```python
def mergeSort(nums):
    if len(nums) > 1:
        mid = len(nums) // 2
        L = nums[:mid]
        R = nums[mid:]
        mergeSort(L)
        mergeSort(R)
        i = j = k = 0

        while i < len(L) and j < len(R):
            if L[i] <= R[j]:
                nums[k] = L[i]
                i += 1
            else:
                nums[k] = R[j]
                j += 1
            k += 1

        while i < len(L):
            nums[k] = L[i]
            i += 1
            k += 1

        while j < len(R):
            nums[k] = R[j]
            j += 1
            k += 1
```

## Heap Sort

Time Complexity: O(n logn)

- Time complexity of heapify is O(Logn).
- Time complexity of createAndBuildHeap() is O(n)
- Hence the overall time complexity of Heap Sort is O(nLogn).

```python
def heapify(arr, n, i):
    largest = i  # Initialize largest as root
    l = 2 * i + 1     # left = 2*i + 1
    r = 2 * i + 2     # right = 2*i + 2

    # See if left child of root exists and is
    # greater than root
    if l < n and arr[largest] < arr[l]:
        largest = l

    # See if right child of root exists and is
    # greater than root
    if r < n and arr[largest] < arr[r]:
        largest = r

    # Change root, if needed
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]  # swap

        # Heapify the root.
        heapify(arr, n, largest)

# The main function to sort an array of given size
def heapSort(arr):
    n = len(arr)

    # Build a maxheap.
    for i in range(n//2 - 1, -1, -1):
        heapify(arr, n, i)

    # One by one extract elements
    for i in range(n-1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]  # swap
        heapify(arr, i, 0)
```

## Bucket Sort

Known number of elements, and elements distribut uniformly.

- Create n buckets `arr[n]`, where n is larger than the number of elements
- Align the elements to make them range from 0 to n
- For each element `num[i]`, do `arr[nums[i]].append(nums[i])`
- For each array element in `arr`, sort `arr[i]` with other sort method.
- then every number is sorted

# Array

[43. Multiply Strings](https://leetcode.com/problems/multiply-strings/)

Elementary math lol, and space complexity is O(m+n)

## Two Pointers

[\*42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

[\*160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

Really genius solution with 2 pointers, can also use hash table.

[\*234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

constant space solution: reverse half of the list right away, and compare

[328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)

[\*142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Intervals

[\*Lint-391. Number of Airplanes in the Sky](https://www.lintcode.com/problem/391/)

[\*253. Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)

Sweep line, separate start and end time and sort them.

[56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

Sort the intervals, and updating the result according the intersactions.

[57. Insert Interval](https://leetcode.com/problems/insert-interval/)

[\*986. Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/)

Very smart way to compare the boundaries of the intersections.

## Palindrom

[\*5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

Use two pointers to expand from the center; The center could be 1 or 2 characters.

[\*680. Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/)

Use two pointers. If `s[l] != s[r]`, delete s[l] or s[r], and then use two pointers to check these two situations again.

[125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

`isalnum()` is so useful!

## Sliding Window

[\*3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

Very intuitive way of reduce time complexity. Use hashmap to track previous same character's position. [solution](./0003.%20Longest%20Substring%20Without%20Repeating%20Characters.md)

[\*11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

The area is determined by the shorter edge, so each time we should move the pointer that points to the short edge.

[\*76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

Use sliding window and counter.

[209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

`76` and `209` share really similay idea: first expand the right boundary to satisfy the target, then shrink the left boudary until the target is no more satisfied to find the min subarray.

[\*239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

Sliding window + mono deque. Maintain a mono decreasing deque, so that the largest element is in the head. Each time sliding window moves, update the largest element and remove the elements that are no longer in the windwow.

[\*713. Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)

Really hard to think of how to count new subarrays.
