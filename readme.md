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

## Insertion Sort

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

Really hard to think of how to count new subarrays. [solution](./0713.%20Subarray%20Product%20Less%20Than%20K.md)

[727. Minimum Window Subsequence](https://leetcode.com/problems/minimum-window-subsequence/)

DP, you idiot

[\*480. Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)

Two heaps + hashmap; use heap to track median, and use hashmap to track outliers.

## Stream

[\*295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)

Same as 480, use two heaps to keep track of medians.

[352. Data Stream as Disjoint Intervals](https://leetcode.com/problems/data-stream-as-disjoint-intervals/)

SortedDict + Binary Search

## Prefix Sum

[\*238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

O(1) space is awsome! Use the result to store left prefix sum, and update the result again. [solution](./0238.%20Product%20of%20Array%20Except%20Self.md)

[303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

[\*325. Maximum Size Subarray Sum Equals k](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/)

Prefix Sum + Two Sum (hashtable)!!!

[560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

Exactly the same as 325

[528. Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)

Prefix Sum + Binary Search

## Two Sum and his family

[\*18. 4Sum](https://leetcode.com/problems/4sum/)

I think this is all you need...

[\*653. Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)

[1099. Two Sum Less Than K](https://leetcode.com/problems/two-sum-less-than-k/)

Sort + Two pointers

[259. 3Sum Smaller](https://leetcode.com/problems/3sum-smaller/)

Similar to 1099. If for a sorted array, if `nums[i] + nums[j] < k`, then there are (j - i) pairs of numbers whoes sum is less than k. They are: `[i, i + 1], [i, i + 2], ..., [i, j - 1], [i, j]`

# Tree

[\*102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

BFS or DFS

## DFS

[\*297. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

DFS + pre-order to serialize the tree

## BFS

# TOPO

[\*207. Course Schedule](https://leetcode.com/problems/course-schedule/)

[269. Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)

[\*444. Sequence Reconstruction](https://leetcode.com/problems/sequence-reconstruction/)

Always check if `len(queue) == 1` to make sure there is only one possible supersequence.

# Matrix

[200. Number of Islands](https://leetcode.com/problems/number-of-islands/)

Iterate all cells in the matrix while useing bfs/dfs

[\*490. The Maze](https://leetcode.com/problems/the-maze/)

[\*542. 01 Matrix](https://leetcode.com/problems/01-matrix/)

imagine water flood from 0 to 1

[\*994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

Rember to update the status after appending to the queue.

[\*https://leetcode.com/problems/sliding-puzzle/](https://leetcode.com/problems/sliding-puzzle/)

Matrix serilize + BFS

# Graph

## BFS & DFS

[\*133. Clone Graph](https://leetcode.com/problems/clone-graph/)

Use memoization to void cycle. bfs/dfs

[\*127. Word Ladder](https://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=678970&extra=&authorid=682747&page=2)

Bidirenctional DFS + template graph

[\*261. Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/solution/)

Valid tree of `n` nodes = `n - 1` edges + the graph is full connected

[841. Keys and Rooms](https://leetcode.com/problems/keys-and-rooms/)

Similar to above

[\*323. Number of Connected Components in an Undirected Graph](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)

We can not use `in_degree` because there can be cycles. We just need to select every node and use BFS/DFS to mark its neighbors and neighbors' neighbors in a hashset `visited`.

Later when we find a new node hasn't been marked, we know it is on a different component, so we let `rst += 1` and then mark its neighbors again.

[1306. Jump Game III](https://leetcode.com/problems/jump-game-iii/)

# Binary Tree

## Traversal

Generally, we use `recursive function` to traverse, but we can also use `stack` to traverse `iteratively`. The T is O(n), and S is O(n).

For inorder traversal, we can use `Moris Traversal` to optimize S to reach O(1).

[\*94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Construct Binary Tree

[105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

[106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

To construct a unique binary tree, `inorder` traversal is a must.

When using `preorder` + `inorder`, the first element of `preorder` is the root. Then, we find this value in `inorder`, and it can split `inorder` into two parts. The left part is the left subtree, and the right part is the right subtree.

When using `postorder` + `inorder`, it is similiar. But now the last element of `postorder` is the root. We can also find this value in `inorder`, and split it into two parts.

[889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

Without `inorder`, the binary tree may not be unique. But it's ok, we can return any of them. We can recursively create root based on either `preorder` or `postorder`, and split the other one.

## Iterator

[\*173. Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

No need to flat the tree, use stack to iterate the tree in real time.

[173. Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

Same as above, inorder traversal + counter. We can use either recursive or iterative traversal. But use iterative traveral can save T to O(h + k).

[\*285. Inorder Successor in BST](https://leetcode.com/problems/inorder-successor-in-bst/)

Very smart to use BST properties.

[\*510. Inorder Successor in BST II](https://leetcode.com/problems/inorder-successor-in-bst-ii/)

Considering the successor of a `node`, there can be two cases:

- `node` has a right child: then the successor must be **the smallest element in its right branch**.
- `node` doesn't have a right child: then the successor must be its parent of greater parent. And the node must **in the left branch of its successor**.

[\*270. Closest Binary Search Tree Value](https://leetcode.com/problems/closest-binary-search-tree-value/)

We don't need to search both larger closest and smaller closest number. Just need to do one pass search with T = O(H).

Let's assume the start value is smaller than target. Then we will search the right child to get closer to target. And finally we'll pass the target and reach to a node whose value is larger than the target. Then, we'll search the left child.

Since we can not tarverse from childen to parents in BST, so we actually searching around the target and never look back, and get closer and closer until we reach a Null value. We just need to update the closest reuslt whenever we meet a new value.

[\*98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

- Solution1: For each node, we can keep track of its upper bound and lower bound, we compare its value with the bounds, and then update the bound to check its children.
- Solution2: We use inorder traversal to check all nodes, which is `left->root->right`, and we keep track of value of previous checked node. So each time we check if the node's value is larger than the previous value. Because BST should have `left < root < right`.

[\*333. Largest BST Subtree](https://leetcode.com/problems/largest-bst-subtree/)

Check if is BST first, if yes, then count nodes and return; if no, then check left subtree and right subtree

## LCA

[\*236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

Remember that `p`/`q` could be the ancestor of the other one.

## Others

[199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

Variant of BFS

[513. Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/)

Variant of DFS

[\*449. Serialize and Deserialize BST](https://leetcode.com/problems/serialize-and-deserialize-bst/)

Enhanced version of `lc297`. Use BST feature to optimize space and use binary search to optimize time.

[\*114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

In light of Morris Traversal....

# Trie

```python
class TrieNode:
    def __init__(self):
        self.end = False
        self.nextLetter = {}

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        node = self.root
        for c in word:
            if c not in node.nextLetter:
                node.nextLetter[c] = TrieNode()
            node = node.nextLetter[c]
        node.end = True

    def search(self, word: str) -> bool:
        node = self.root
        for c in word:
            if c not in node.nextLetter:
                return False
            node = node.nextLetter[c]
        return node.end

    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for c in prefix:
            if c not in node.nextLetter:
                return False
            node = node.nextLetter[c]
        return True
```

# Union Find

```python
class UnionFind:
    def __init__(self, size):
        self.root = [i for i in range(size)]
        self.rank = [1] * size
        self.count = size

    # The find function here is the same as that in the disjoint set with path compression.
    def find(self, x):
        if x == self.root[x]:
            return x
        self.root[x] = self.find(self.root[x])
        return self.root[x]

    # The union function with union by rank
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            if self.rank[rootX] > self.rank[rootY]:
                self.root[rootY] = rootX
            elif self.rank[rootX] < self.rank[rootY]:
                self.root[rootX] = rootY
            else:
                self.root[rootY] = rootX
                self.rank[rootX] += 1
        self.count -= 1

    def connected(self, x, y):
        return self.find(x) == self.find(y)

    def rootCount(self):
        return self.count

```

# LRU

```python
class Node:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.prev = None
        self.next = None

class DLL:
    def __init__(self):
        self.head = Node(-1, -1)
        self.tail = Node(-1, -1)
        self.head.next = self.tail
        self.tail.prev = self.head

    def _add(self, node):
        p = self.tail.prev
        p.next = node
        self.tail.prev = node
        node.prev = p
        node.next = self.tail

    def _remove(self, node):
        p = node.prev
        n = node.next
        p.next = n
        n.prev = p

    def _pop_first(self):
        p = self.head.next
        self._remove(p)
        return p

class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.size = 0
        self.dll = DLL()
        self.mp = {}

    def get(self, key: int) -> int:
        if key in self.mp:
            node = self.mp[key]
            self.dll._remove(node)
            self.dll._add(node)
            # self.dll.print()
            return node.val
        else:
            return -1

    def put(self, key: int, value: int) -> None:
        node = Node(key, value)
        if key in self.mp:
            self.dll._remove(self.mp[key])
            self.dll._add(node)
            self.mp[key] = node
        else:
            if self.size == self.capacity:
                p = self.dll._pop_first()
                self.mp.pop(p.key)
                self.size -= 1

            self.dll._add(node)
            self.mp[key] = node
            self.size += 1
```
