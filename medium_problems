# 🚀 DSA — Arrays & Binary Search Deep Dive


## 📋 Problems Covered

| # | Problem | Difficulty | Key Technique |
|---|---------|------------|---------------|
| 1 | [Subarray Sum Equals K](#1-subarray-sum-equals-k) | 🟡 Medium | Prefix Sum + HashMap |
| 2 | [Kadane's Algorithm — Max Subarray](#2-kadanes-algorithm--maximum-subarray) | 🟡 Medium | Dynamic Programming |
| 3 | [Subarray with XOR = K](#3-subarray-with-xor--k) | 🔴 Hard | Prefix XOR + HashMap |
| 4 | [Merge Sorted Arrays (Fixed Space)](#4-merge-sorted-arrays-with-fixed-space) | 🔴 Hard | Gap Algorithm |
| 5 | [Maximum Product Subarray](#5-maximum-product-subarray) | 🔴 Hard | Prefix / Suffix |
| 6 | [Lower Bound](#6-lower-bound) | 🟢 Easy | Binary Search |
| 7 | [Upper Bound](#7-upper-bound) | 🟢 Easy | Binary Search |
| 8 | [Find Greatest Element (Floor/Ceil)](#8-greatest-element--floor-and-ceil) | 🟡 Medium | Binary Search |
| 9 | [Next Permutation](#9-next-permutation) | 🔴 Hard | In-place Reversal |
| 10 | [Leaders in an Array](#10-leaders-in-an-array) | 🟢 Easy | Right-to-Left Scan |

---

## 1. Subarray Sum Equals K

**Problem:** Count the number of subarrays whose sum equals `k`.

### 💡 Intuition
If prefix sum at index `j` minus prefix sum at index `i` equals `k`, then subarray `i+1 to j` sums to `k`.  
So for every index, check if `prefixSum - k` was seen before. That count = valid subarrays ending here.

### 🔴 Brute Force — O(n³) Time | O(1) Space
```python
def subarraySum(nums, k):
    count = 0
    n = len(nums)
    for i in range(n):
        for j in range(i, n):
            total = sum(nums[i:j+1])  # recompute every time
            if total == k:
                count += 1
    return count
```

### 🟡 Better — O(n²) Time | O(1) Space
```python
def subarraySum(nums, k):
    count = 0
    n = len(nums)
    for i in range(n):
        total = 0
        for j in range(i, n):
            total += nums[j]       # running sum, no recompute
            if total == k:
                count += 1
    return count
```

### ✅ Optimal — O(n) Time | O(n) Space
```python
def subarraySum(nums, k):
    prefix_count = {0: 1}   # prefix_sum -> frequency
    total = 0
    count = 0
    for num in nums:
        total += num
        # If (total - k) was seen before, those subarrays sum to k
        count += prefix_count.get(total - k, 0)
        prefix_count[total] = prefix_count.get(total, 0) + 1
    return count
```
> **Example:** `nums = [1, 2, 3]`, `k = 3` → subarrays `[1,2]` and `[3]` → answer = **2**

---

## 2. Kadane's Algorithm — Maximum Subarray

**Problem:** Find the contiguous subarray with the largest sum.

### 💡 Intuition
At each position, decide: is it better to extend the previous subarray, or start fresh?  
If running sum goes negative, discard it — a negative prefix only hurts future sums.

### 🔴 Brute Force — O(n³) Time | O(1) Space
```python
def maxSubArray(nums):
    max_sum = float('-inf')
    n = len(nums)
    for i in range(n):
        for j in range(i, n):
            max_sum = max(max_sum, sum(nums[i:j+1]))
    return max_sum
```

### 🟡 Better — O(n²) Time | O(1) Space
```python
def maxSubArray(nums):
    max_sum = float('-inf')
    n = len(nums)
    for i in range(n):
        total = 0
        for j in range(i, n):
            total += nums[j]
            max_sum = max(max_sum, total)
    return max_sum
```

### ✅ Optimal (Kadane's) — O(n) Time | O(1) Space
```python
def maxSubArray(nums):
    max_sum = nums[0]
    current = nums[0]
    for num in nums[1:]:
        current = max(num, current + num)   # extend or restart
        max_sum = max(max_sum, current)
    return max_sum
```

### ✅ Optimal + Print the Subarray
```python
def maxSubArray(nums):
    max_sum = nums[0]
    current = nums[0]
    start = end = temp_start = 0
    for i in range(1, len(nums)):
        if nums[i] > current + nums[i]:
            current = nums[i]
            temp_start = i
        else:
            current += nums[i]
        if current > max_sum:
            max_sum = current
            start = temp_start
            end = i
    print("Subarray:", nums[start:end+1])
    return max_sum
```

---

## 3. Subarray with XOR = K

**Problem:** Count the number of subarrays whose XOR equals `k`.

### 💡 Intuition
XOR has a special property: `A XOR B = C` → `A = B XOR C`.  
So if prefix XOR up to `j` is `xr`, and `xr XOR k` was seen before at some `i`, then subarray `i+1 to j` has XOR = `k`.  
This mirrors the prefix sum logic — just replace addition with XOR.

### 🔴 Brute Force — O(n³) Time | O(1) Space
```python
def subarrayXorK(nums, k):
    count = 0
    n = len(nums)
    for i in range(n):
        for j in range(i, n):
            xor = 0
            for x in nums[i:j+1]:
                xor ^= x
            if xor == k:
                count += 1
    return count
```

### 🟡 Better — O(n²) Time | O(1) Space
```python
def subarrayXorK(nums, k):
    count = 0
    n = len(nums)
    for i in range(n):
        xor = 0
        for j in range(i, n):
            xor ^= nums[j]
            if xor == k:
                count += 1
    return count
```

### ✅ Optimal — O(n) Time | O(n) Space
```python
def subarrayXorK(nums, k):
    prefix_count = {0: 1}   # xor_value -> frequency
    xr = 0
    count = 0
    for num in nums:
        xr ^= num
        # We need xr ^ k to have been seen before
        count += prefix_count.get(xr ^ k, 0)
        prefix_count[xr] = prefix_count.get(xr, 0) + 1
    return count
```
> **Why XOR k?** Because `xr ^ (xr ^ k) = k`. If `xr ^ k` is in our map, those subarrays XOR to exactly `k`.

---

## 4. Merge Sorted Arrays with Fixed Space

**Problem:** Merge two sorted arrays `a[]` and `b[]` in-place without extra space such that `a[]` has the smaller elements and `b[]` has the larger.

### 💡 Intuition
The optimal trick is the **Gap Algorithm** (Shell Sort idea).  
Start with a gap = ceil((n+m)/2). Compare elements that are `gap` apart and swap if out of order. Keep halving the gap until it becomes 0.

### 🔴 Brute Force — O((n+m) log(n+m)) Time | O(n+m) Space
```python
def mergeArrays(a, b):
    combined = sorted(a + b)
    n, m = len(a), len(b)
    a[:] = combined[:n]
    b[:] = combined[n:]
```

### 🟡 Better — O(min(n,m) * log(min(n,m))) Time | O(1) Space
```python
def mergeArrays(a, b):
    n, m = len(a), len(b)
    # Compare last of a with first of b; bubble the smaller into a
    for i in range(n - 1, -1, -1):
        if a[i] > b[0]:
            a[i], b[0] = b[0], a[i]
            # Re-sort b to maintain order
            j = 0
            while j + 1 < m and b[j] > b[j + 1]:
                b[j], b[j + 1] = b[j + 1], b[j]
                j += 1
```

### ✅ Optimal (Gap Algorithm) — O((n+m) log(n+m)) Time | O(1) Space
```python
import math

def mergeArrays(a, b):
    n, m = len(a), len(b)
    total = n + m
    gap = math.ceil(total / 2)

    def get(i):
        return a[i] if i < n else b[i - n]

    def set_val(i, val):
        if i < n: a[i] = val
        else: b[i - n] = val

    while gap > 0:
        left, right = 0, gap
        while right < total:
            l_val, r_val = get(left), get(right)
            if l_val > r_val:
                set_val(left, r_val)
                set_val(right, l_val)
            left += 1
            right += 1
        gap = math.ceil(gap / 2) if gap > 1 else 0
```

---

## 5. Maximum Product Subarray

**Problem:** Find the subarray with the largest product.

### 💡 Intuition
Two negative numbers multiply to positive — so a negative can flip and become the maximum later.  
**Key insight:** Scan from both left-to-right (prefix) and right-to-left (suffix). Reset product to 1 when it hits 0. The answer is the max across both passes.

### 🔴 Brute Force — O(n³) Time | O(1) Space
```python
def maxProduct(nums):
    max_prod = float('-inf')
    n = len(nums)
    for i in range(n):
        for j in range(i, n):
            prod = 1
            for k in range(i, j + 1):
                prod *= nums[k]
            max_prod = max(max_prod, prod)
    return max_prod
```

### 🟡 Better — O(n²) Time | O(1) Space
```python
def maxProduct(nums):
    max_prod = float('-inf')
    n = len(nums)
    for i in range(n):
        prod = 1
        for j in range(i, n):
            prod *= nums[j]
            max_prod = max(max_prod, prod)
    return max_prod
```

### ✅ Optimal (Prefix-Suffix) — O(n) Time | O(1) Space
```python
def maxProduct(nums):
    n = len(nums)
    max_prod = float('-inf')
    prefix = suffix = 1
    for i in range(n):
        if prefix == 0: prefix = 1
        if suffix == 0: suffix = 1
        prefix *= nums[i]
        suffix *= nums[n - 1 - i]
        max_prod = max(max_prod, prefix, suffix)
    return max_prod
```

### ✅ Optimal (Track Min & Max) — O(n) Time | O(1) Space
```python
def maxProduct(nums):
    max_prod = nums[0]
    cur_max = cur_min = nums[0]
    for num in nums[1:]:
        candidates = (num, cur_max * num, cur_min * num)
        cur_max = max(candidates)
        cur_min = min(candidates)
        max_prod = max(max_prod, cur_max)
    return max_prod
```

---

## 6. Lower Bound

**Problem:** Find the first index where `nums[i] >= target`. If not found, return `n`.

### 💡 Intuition
Binary search variant. We're not looking for exact match — we want the leftmost position where the condition `>= target` becomes true.

### 🔴 Brute Force — O(n) Time | O(1) Space
```python
def lowerBound(nums, target):
    for i in range(len(nums)):
        if nums[i] >= target:
            return i
    return len(nums)
```

### ✅ Optimal — O(log n) Time | O(1) Space
```python
def lowerBound(nums, target):
    left, right = 0, len(nums)
    while left < right:
        mid = left + (right - left) // 2
        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid      # potential answer, but check further left
    return left
```
> **Example:** `nums = [1,3,5,7]`, `target = 4` → index **2** (nums[2]=5 is first ≥ 4)

---

## 7. Upper Bound

**Problem:** Find the first index where `nums[i] > target`. If not found, return `n`.

### 💡 Intuition
Same as lower bound but condition changes from `>= target` to `> target`. One character difference in code.

### 🔴 Brute Force — O(n) Time | O(1) Space
```python
def upperBound(nums, target):
    for i in range(len(nums)):
        if nums[i] > target:
            return i
    return len(nums)
```

### ✅ Optimal — O(log n) Time | O(1) Space
```python
def upperBound(nums, target):
    left, right = 0, len(nums)
    while left < right:
        mid = left + (right - left) // 2
        if nums[mid] <= target:
            left = mid + 1
        else:
            right = mid      # potential answer, but check further left
    return left
```
> **Example:** `nums = [1,3,5,7]`, `target = 5` → index **3** (nums[3]=7 is first > 5)

---

## 8. Greatest Element — Floor and Ceil

**Problem:**  
- **Floor:** Largest element ≤ target  
- **Ceil:** Smallest element ≥ target (same as Lower Bound)

### 💡 Intuition
Binary search while tracking the last valid answer for floor/ceil. Shrink search space each step.

### 🔴 Brute Force — O(n) Time | O(1) Space
```python
def floorCeil(nums, target):
    floor = ceil = -1
    for num in nums:
        if num <= target:
            floor = max(floor, num)   # largest that's still <= target
        if num >= target:
            ceil = num if ceil == -1 else min(ceil, num)  # smallest >= target
    return floor, ceil
```

### ✅ Optimal Floor — O(log n) Time | O(1) Space
```python
def findFloor(nums, target):
    left, right = 0, len(nums) - 1
    floor = -1
    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] <= target:
            floor = nums[mid]    # valid candidate, try to go right for bigger
            left = mid + 1
        else:
            right = mid - 1
    return floor
```

### ✅ Optimal Ceil — O(log n) Time | O(1) Space
```python
def findCeil(nums, target):
    left, right = 0, len(nums) - 1
    ceil = -1
    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] >= target:
            ceil = nums[mid]     # valid candidate, try to go left for smaller
            right = mid - 1
        else:
            left = mid + 1
    return ceil
```

### ✅ Both Together
```python
def floorAndCeil(nums, target):
    floor = findFloor(nums, target)
    ceil = findCeil(nums, target)
    return floor, ceil
```
> **Example:** `nums = [1,3,5,7,9]`, `target = 6` → floor = **5**, ceil = **7**

---

## 9. Next Permutation

**Problem:** Rearrange numbers into the lexicographically next greater permutation. If no such permutation, return the smallest (sorted ascending).

### 💡 Intuition
Three steps:
1. From the right, find the first index `i` where `nums[i] < nums[i+1]` (the "dip")
2. From the right, find the first index `j` where `nums[j] > nums[i]`, and swap them
3. Reverse everything after index `i`

If no dip found (array is fully descending), just reverse the whole array.

### 🔴 Brute Force — O(n! × n) Time | O(n!) Space
```python
from itertools import permutations

def nextPermutation_brute(nums):
    perms = sorted(set(permutations(nums)))
    idx = perms.index(tuple(nums))
    result = perms[(idx + 1) % len(perms)]
    nums[:] = list(result)
```

### ✅ Optimal — O(n) Time | O(1) Space
```python
def nextPermutation(nums):
    n = len(nums)
    i = n - 2

    # Step 1: Find the first "dip" from the right
    while i >= 0 and nums[i] >= nums[i + 1]:
        i -= 1

    if i >= 0:
        # Step 2: Find the element just greater than nums[i]
        j = n - 1
        while nums[j] <= nums[i]:
            j -= 1
        nums[i], nums[j] = nums[j], nums[i]

    # Step 3: Reverse from i+1 to end
    left, right = i + 1, n - 1
    while left < right:
        nums[left], nums[right] = nums[right], nums[left]
        left += 1
        right -= 1
```
> **Example:** `[1,3,2]` → Step 1: dip at index 1 (3→2). Step 2: swap 1 with 2 → `[2,3,1]`. Step 3: reverse after 0 → `[2,1,3]`

---

## 10. Leaders in an Array

**Problem:** An element is a **leader** if it is greater than all elements to its right. The rightmost element is always a leader.

### 💡 Intuition
Scan from right to left. Keep track of the current maximum from the right. Any element that is ≥ current max is a leader.

### 🔴 Brute Force — O(n²) Time | O(1) Space
```python
def findLeaders(nums):
    n = len(nums)
    leaders = []
    for i in range(n):
        is_leader = True
        for j in range(i + 1, n):
            if nums[j] >= nums[i]:   # something to the right is bigger
                is_leader = False
                break
        if is_leader:
            leaders.append(nums[i])
    return leaders
```

### ✅ Optimal — O(n) Time | O(1) Space (excluding output)
```python
def findLeaders(nums):
    n = len(nums)
    leaders = []
    max_from_right = nums[-1]
    leaders.append(nums[-1])    # rightmost is always a leader

    for i in range(n - 2, -1, -1):
        if nums[i] >= max_from_right:
            leaders.append(nums[i])
            max_from_right = nums[i]

    return leaders[::-1]        # reverse to maintain original order
```
> **Example:** `nums = [16, 17, 4, 3, 5, 2]` → Leaders: **17, 5, 2**

---

## 📊 Complexity Summary

| Problem | Brute Time | Better Time | Optimal Time | Optimal Space |
|---------|-----------|------------|-------------|---------------|
| Subarray Sum = K | O(n³) | O(n²) | O(n) | O(n) |
| Kadane's (Max Subarray) | O(n³) | O(n²) | O(n) | O(1) |
| Subarray XOR = K | O(n³) | O(n²) | O(n) | O(n) |
| Merge Sorted (Fixed Space) | O((n+m)log) | O(n·m) | O((n+m)log) | O(1) |
| Maximum Product Subarray | O(n³) | O(n²) | O(n) | O(1) |
| Lower Bound | O(n) | — | O(log n) | O(1) |
| Upper Bound | O(n) | — | O(log n) | O(1) |
| Floor & Ceil | O(n) | — | O(log n) | O(1) |
| Next Permutation | O(n!·n) | — | O(n) | O(1) |
| Leaders in Array | O(n²) | — | O(n) | O(1) |

---

## 🧠 Key Patterns to Remember

| Pattern | Problems It Solves |
|---------|-------------------|
| **Prefix Sum / XOR + HashMap** | Subarray Sum = K, Subarray XOR = K |
| **Kadane's (extend or restart)** | Max Subarray, Max Product (variant) |
| **Binary Search on answer** | Lower Bound, Upper Bound, Floor, Ceil |
| **Right-to-left scan** | Leaders in Array |
| **Find dip + reverse suffix** | Next Permutation |
| **Gap Algorithm** | Merge with Fixed Space |

---


```

---

*Keep grinding 🔥 | [LeetCode Profile](https://leetcode.com/your-username) | [LinkedIn](https://linkedin.com/in/your-profile)*
