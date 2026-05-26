# Array_Full_Problems



## 📋 Problems Solved

| # | Problem | Difficulty | Approach |
|---|---------|------------|----------|
| 1 | [Two Sum](#1-two-sum) | 🟢 Easy | HashMap |
| 2 | [Three Sum](#2-three-sum) | 🟡 Medium | Two Pointer |
| 3 | [Rotate Array](#3-rotate-array) | 🟡 Medium | Reversal |
| 4 | [Find the Missing Number](#4-find-the-missing-number) | 🟢 Easy | XOR / Math |
| 5 | [Sort Colors](#5-sort-colors) | 🟡 Medium | Dutch National Flag |
| 6 | [Linear Search](#6-linear-search) | 🟢 Easy | Traversal |
| 7 | [Binary Search](#7-binary-search) | 🟢 Easy | Divide & Conquer |
| 8 | [Second Largest Number](#8-second-largest-number) | 🟢 Easy | Single Pass |
| 9 | [Move Zeros](#9-move-zeros) | 🟢 Easy | Two Pointer |
| 10 | [Add Two Numbers (Arrays as Subsets)](#10-add-two-numbers) | 🟡 Medium | Simulation |
| 11 | [Union of Two Sorted Arrays](#11-union-of-two-sorted-arrays) | 🟢 Easy | Two Pointer |

---

## 1. Two Sum

**Problem:** Find two indices such that `nums[i] + nums[j] == target`.

### 💡 Intuition
For every element, we need another element = `target - current`. If we store visited elements in a HashMap, we can look up the complement in O(1).

### 🔴 Brute Force — O(n²) Time | O(1) Space
```python
def twoSum(nums, target):
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            if nums[i] + nums[j] == target:
                return [i, j]
```

### ✅ Optimal — O(n) Time | O(n) Space
```python
def twoSum(nums, target):
    seen = {}  # value -> index
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
```

---

## 2. Three Sum

**Problem:** Find all unique triplets in the array that sum to zero.

### 💡 Intuition
Sort the array. Fix one element and use two pointers on the rest to find pairs that sum to its negative. Skip duplicates after fixing.

### 🔴 Brute Force — O(n³) Time | O(n) Space
```python
def threeSum(nums):
    nums.sort()
    result = set()
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            for k in range(j + 1, n):
                if nums[i] + nums[j] + nums[k] == 0:
                    result.add((nums[i], nums[j], nums[k]))
    return list(result)
```

### 🟡 Better — O(n²) Time | O(n) Space
```python
def threeSum(nums):
    nums.sort()
    result = []
    n = len(nums)
    for i in range(n):
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        seen = set()
        j = i + 1
        while j < n:
            complement = -nums[i] - nums[j]
            if complement in seen:
                result.append([nums[i], complement, nums[j]])
                while j + 1 < n and nums[j] == nums[j + 1]:
                    j += 1
            seen.add(nums[j])
            j += 1
    return result
```

### ✅ Optimal — O(n²) Time | O(1) Space
```python
def threeSum(nums):
    nums.sort()
    result = []
    n = len(nums)
    for i in range(n - 2):
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        left, right = i + 1, n - 1
        while left < right:
            total = nums[i] + nums[left] + nums[right]
            if total == 0:
                result.append([nums[i], nums[left], nums[right]])
                while left < right and nums[left] == nums[left + 1]: left += 1
                while left < right and nums[right] == nums[right - 1]: right -= 1
                left += 1
                right -= 1
            elif total < 0:
                left += 1
            else:
                right -= 1
    return result
```

---

## 3. Rotate Array

**Problem:** Rotate array to the right by `k` steps.

### 💡 Intuition
Rotating by k is the same as: reverse all → reverse first k → reverse rest k. This is the in-place trick.

### 🔴 Brute Force — O(n×k) Time | O(1) Space
```python
def rotate(nums, k):
    n = len(nums)
    k = k % n
    for _ in range(k):
        last = nums[-1]
        for i in range(n - 1, 0, -1):
            nums[i] = nums[i - 1]
        nums[0] = last
```

### 🟡 Better — O(n) Time | O(n) Space
```python
def rotate(nums, k):
    n = len(nums)
    k = k % n
    temp = nums[-k:] + nums[:-k]
    nums[:] = temp
```

### ✅ Optimal (Reversal) — O(n) Time | O(1) Space
```python
def rotate(nums, k):
    n = len(nums)
    k = k % n

    def reverse(l, r):
        while l < r:
            nums[l], nums[r] = nums[r], nums[l]
            l += 1
            r -= 1

    reverse(0, n - 1)   # Reverse entire array
    reverse(0, k - 1)   # Reverse first k elements
    reverse(k, n - 1)   # Reverse remaining elements
```

---

## 4. Find the Missing Number

**Problem:** Given `n` numbers from `0` to `n`, find the missing one.

### 💡 Intuition
Sum from `0` to `n` is `n*(n+1)/2`. Subtract actual sum. Or use XOR — every number XOR'd with itself cancels out.

### 🔴 Brute Force — O(n²) Time | O(1) Space
```python
def missingNumber(nums):
    n = len(nums)
    for i in range(n + 1):
        if i not in nums:
            return i
```

### 🟡 Better — O(n) Time | O(n) Space
```python
def missingNumber(nums):
    return set(range(len(nums) + 1)) - set(nums)  # returns a set with 1 element
```

### ✅ Optimal (Math) — O(n) Time | O(1) Space
```python
def missingNumber(nums):
    n = len(nums)
    expected = n * (n + 1) // 2
    return expected - sum(nums)
```

### ✅ Optimal (XOR) — O(n) Time | O(1) Space
```python
def missingNumber(nums):
    xor = 0
    n = len(nums)
    for i in range(n + 1):
        xor ^= i
    for num in nums:
        xor ^= num
    return xor
```

---

## 5. Sort Colors

**Problem:** Sort array of 0s, 1s, and 2s in-place without using sort().

### 💡 Intuition
Dutch National Flag algorithm by Dijkstra. Maintain 3 pointers: `low`, `mid`, `high`. Keep 0s at start, 2s at end, 1s in the middle.

### 🔴 Brute Force — O(n log n) Time | O(1) Space
```python
def sortColors(nums):
    nums.sort()
```

### 🟡 Better (Counting) — O(n) Time | O(1) Space
```python
def sortColors(nums):
    count = [0, 0, 0]
    for num in nums:
        count[num] += 1
    i = 0
    for color in range(3):
        for _ in range(count[color]):
            nums[i] = color
            i += 1
```

### ✅ Optimal (Dutch National Flag) — O(n) Time | O(1) Space | Single Pass
```python
def sortColors(nums):
    low, mid, high = 0, 0, len(nums) - 1
    while mid <= high:
        if nums[mid] == 0:
            nums[low], nums[mid] = nums[mid], nums[low]
            low += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:
            nums[mid], nums[high] = nums[high], nums[mid]
            high -= 1
```

---

## 6. Linear Search

**Problem:** Find the index of a target in an unsorted array.

### 💡 Intuition
Go through each element one by one. No shortcut when array is unsorted.

### ✅ Solution — O(n) Time | O(1) Space
```python
def linearSearch(nums, target):
    for i in range(len(nums)):
        if nums[i] == target:
            return i
    return -1
```

---

## 7. Binary Search

**Problem:** Find target in a sorted array. Return index or -1.

### 💡 Intuition
In a sorted array, if the middle element is greater than target, the target must be in the left half — and vice versa. Eliminate half the search space each time.

### 🔴 Brute Force — O(n) Time | O(1) Space
```python
def search(nums, target):
    for i in range(len(nums)):
        if nums[i] == target:
            return i
    return -1
```

### ✅ Optimal (Iterative) — O(log n) Time | O(1) Space
```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = left + (right - left) // 2  # Avoids overflow
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

### ✅ Optimal (Recursive) — O(log n) Time | O(log n) Space
```python
def search(nums, target, left=0, right=None):
    if right is None: right = len(nums) - 1
    if left > right: return -1
    mid = left + (right - left) // 2
    if nums[mid] == target: return mid
    elif nums[mid] < target: return search(nums, target, mid + 1, right)
    else: return search(nums, target, left, mid - 1)
```

---

## 8. Second Largest Number

**Problem:** Find the second largest element in the array.

### 💡 Intuition
Keep track of the largest and second largest in a single pass. Update accordingly.

### 🔴 Brute Force — O(n log n) Time | O(1) Space
```python
def secondLargest(nums):
    nums = sorted(set(nums), reverse=True)
    return nums[1] if len(nums) >= 2 else -1
```

### ✅ Optimal — O(n) Time | O(1) Space
```python
def secondLargest(nums):
    first = second = float('-inf')
    for num in nums:
        if num > first:
            second = first
            first = num
        elif num > second and num != first:
            second = num
    return second if second != float('-inf') else -1
```

---

## 9. Move Zeros

**Problem:** Move all zeros to the end while maintaining the relative order of non-zero elements.

### 💡 Intuition
Use two pointers. `j` tracks where the next non-zero element should go. Whenever we find a non-zero, swap it with position `j`.

### 🔴 Brute Force — O(n) Time | O(n) Space
```python
def moveZeroes(nums):
    non_zero = [x for x in nums if x != 0]
    zeros = [0] * (len(nums) - len(non_zero))
    nums[:] = non_zero + zeros
```

### ✅ Optimal (Two Pointer) — O(n) Time | O(1) Space
```python
def moveZeroes(nums):
    j = 0  # pointer for next non-zero position
    for i in range(len(nums)):
        if nums[i] != 0:
            nums[i], nums[j] = nums[j], nums[i]
            j += 1
```

---

## 10. Add Two Numbers

**Problem:** Add two numbers represented as arrays (digits), return result as array.

### 💡 Intuition
Start from the last digit (units place). Add digit by digit and carry over just like school-level addition.

### 🔴 Brute Force — O(n) Time | O(n) Space
```python
def addTwoNumbers(nums1, nums2):
    num1 = int("".join(map(str, nums1)))
    num2 = int("".join(map(str, nums2)))
    return list(map(int, str(num1 + num2)))
```

### ✅ Optimal (Simulation) — O(max(n,m)) Time | O(max(n,m)) Space
```python
def addTwoNumbers(nums1, nums2):
    i, j = len(nums1) - 1, len(nums2) - 1
    carry = 0
    result = []
    while i >= 0 or j >= 0 or carry:
        a = nums1[i] if i >= 0 else 0
        b = nums2[j] if j >= 0 else 0
        total = a + b + carry
        carry = total // 10
        result.append(total % 10)
        i -= 1
        j -= 1
    return result[::-1]
```

---

## 11. Union of Two Sorted Arrays

**Problem:** Return the union (unique elements) of two sorted arrays in sorted order.

### 💡 Intuition
Use two pointers. Since both arrays are sorted, compare the current elements of each and add the smaller one. Skip duplicates.

### 🔴 Brute Force — O((n+m) log(n+m)) Time | O(n+m) Space
```python
def unionSortedArrays(a, b):
    return sorted(set(a) | set(b))
```

### ✅ Optimal (Two Pointer) — O(n+m) Time | O(n+m) Space
```python
def unionSortedArrays(a, b):
    i, j = 0, 0
    result = []
    while i < len(a) and j < len(b):
        if a[i] < b[j]:
            if not result or result[-1] != a[i]:
                result.append(a[i])
            i += 1
        elif b[j] < a[i]:
            if not result or result[-1] != b[j]:
                result.append(b[j])
            j += 1
        else:
            if not result or result[-1] != a[i]:
                result.append(a[i])
            i += 1
            j += 1
    while i < len(a):
        if not result or result[-1] != a[i]:
            result.append(a[i])
        i += 1
    while j < len(b):
        if not result or result[-1] != b[j]:
            result.append(b[j])
        j += 1
    return result
```

---

## 📊 Complexity Summary

| Problem | Brute Time | Optimal Time | Optimal Space |
|---------|-----------|-------------|---------------|
| Two Sum | O(n²) | O(n) | O(n) |
| Three Sum | O(n³) | O(n²) | O(1) |
| Rotate Array | O(n×k) | O(n) | O(1) |
| Missing Number | O(n²) | O(n) | O(1) |
| Sort Colors | O(n log n) | O(n) | O(1) |
| Linear Search | — | O(n) | O(1) |
| Binary Search | O(n) | O(log n) | O(1) |
| Second Largest | O(n log n) | O(n) | O(1) |
| Move Zeros | O(n) | O(n) | O(1) |
| Add Two Numbers | O(n) | O(n) | O(n) |
| Union Sorted Arrays | O((n+m) log n) | O(n+m) | O(n+m) |

---

## 🧠 Key Takeaways

- **Two Pointer** is powerful for sorted array problems (Three Sum, Union, Sort Colors)
- **HashMap** trades space for time — turns O(n²) into O(n) for lookup problems
- **XOR trick** is a gem for finding missing/duplicate numbers in O(1) space
- **Reversal algorithm** is the cleanest way to rotate arrays in-place
- **Dutch National Flag** solves 3-way partition in a single pass

---

## 

---

*Day 3 of DSA grind 🔥 | Connected on [LinkedIn](https://linkedin.com/in/your-profile)*
