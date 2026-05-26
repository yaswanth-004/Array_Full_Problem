# 🧠 Dynamic Programming — Complete Guide + Array DP Problems

> Everything you need to understand DP from scratch — what it is, why it exists, how to think in DP, and 6 classic problems solved in Java with all 3 approaches.

---

## 📋 Table of Contents

| Section | Content |
|---------|---------|
| [What is DP?](#-what-is-dynamic-programming) | Definition, intuition, real-life analogy |
| [Why DP?](#-why-dp--when-to-use-it) | When to use it, how to identify |
| [DP Methods](#-the-three-dp-methods) | Recursion, Memoization, Tabulation, Space Opt |
| [Basic Steps](#-basic-steps-to-solve-any-dp-problem) | The 5-step framework |
| [Problem 1](#1-frog-jump--1-or-2-steps) | Frog Jump — 1 or 2 steps |
| [Problem 2](#2-frog-jump-with-k-steps) | Frog Jump — K steps |
| [Problem 3](#3-staircase--count-ways-1-or-2-steps) | Count Ways to climb stairs |
| [Problem 4](#4-house-robber--non-adjacent-sum) | House Robber |
| [Problem 5](#5-house-robber-ii--circular-array) | House Robber II (Circular) |
| [problem 6](#6-Ninja's Training — DP on Arrays) | find the maximum points |

---

## 🤔 What is Dynamic Programming?

**Dynamic Programming (DP)** is a technique to solve problems by breaking them into **smaller overlapping subproblems**, solving each subproblem **only once**, and **storing the result** so you never compute it again.

### The Core Idea

Imagine you're climbing stairs and someone asks:
> *"How many ways can you reach step 10?"*

A naive approach would recompute the ways to reach step 8 and step 9 — over and over again — every time it's needed. DP says:

> **"Compute it once. Remember it. Reuse it."**

### Real-Life Analogy

Think of DP like a **smart student** preparing for exams:
- A **dumb approach** = re-reads the same chapter from scratch every time a question about it appears.
- A **DP approach** = reads the chapter once, writes notes, refers to notes when needed.

The "notes" in DP are called a **memo table** or **DP array**.

### The Two Conditions for DP

A problem is solvable by DP if it has:

1. **Optimal Substructure** — The optimal solution to the problem can be built from optimal solutions of its subproblems.
2. **Overlapping Subproblems** — The same subproblems are solved multiple times in a naive recursive solution.

---

## 🎯 Why DP? — When to Use It

### Ask yourself these questions:

| Question | If YES → |
|----------|----------|
| Does the problem ask for **minimum / maximum** of something? | Likely DP |
| Does it ask to **count the number of ways**? | Likely DP |
| Does it ask if something is **possible / achievable**? | Likely DP |
| Can the problem be expressed as **"choices at each step"**? | Likely DP |
| Does a recursive solution have **repeated subproblems**? | Definitely DP |

### Problems that are NOT DP

- Sorting an array → No overlapping subproblems
- Finding an element → Direct traversal
- Graph traversal (BFS/DFS) → No repeated state usually

### Classic DP Problem Areas

```
Arrays        → Frog Jump, House Robber, Max Subarray
Strings       → LCS, Edit Distance, Palindrome
Grids         → Unique Paths, Min Path Sum
Subsequences  → LIS, Knapsack
Intervals     → Matrix Chain, Burst Balloons
```

---

## 🔧 The Three DP Methods

Every DP problem can be solved in multiple ways. Here's the full progression:

```
Plain Recursion
      ↓  (add memoization)
Top-Down DP (Memoization)
      ↓  (convert to iterative)
Bottom-Up DP (Tabulation)
      ↓  (reduce dp array to variables)
Space Optimized DP
```

---

### Method 1 — Plain Recursion (No DP)

Just write the recursive definition of the problem. No memory storage.

**Pros:** Easy to write, matches the problem definition  
**Cons:** Exponential time — recomputes same subproblems

```
Time:  O(2^n) or worse
Space: O(n) — recursion stack
```

**When to write this first:** ALWAYS. Write recursion first, then optimize.

---

### Method 2 — Top-Down DP (Memoization)

Take the recursive solution and add a **memo array** (or HashMap). Before computing, check if the answer is already stored. If yes, return it directly.

**Direction:** Starts from the original problem → breaks down → reaches base case (top to bottom)

```
Time:  O(n) — each subproblem solved once
Space: O(n) recursion stack + O(n) memo array = O(n)
```

**Template:**
```java
int[] memo = new int[n];
Arrays.fill(memo, -1);  // -1 = not computed yet

int solve(int n) {
    if (base case) return answer;
    if (memo[n] != -1) return memo[n];      // already computed
    memo[n] = /* recursive calls */;
    return memo[n];
}
```

---

### Method 3 — Bottom-Up DP (Tabulation)

Build the solution **iteratively** from the smallest subproblems up to the answer. No recursion stack.

**Direction:** Starts from base case → builds up → reaches the original problem (bottom to top)

```
Time:  O(n)
Space: O(n) — only the dp array, no recursion stack
```

**Template:**
```java
int[] dp = new int[n + 1];
dp[0] = base_case_0;
dp[1] = base_case_1;

for (int i = 2; i <= n; i++) {
    dp[i] = /* formula using dp[i-1], dp[i-2] ... */;
}
return dp[n];
```

---

### Method 4 — Space Optimization

Look at the tabulation solution. If `dp[i]` only depends on the last 1 or 2 values, replace the entire array with just those variables.

```
Time:  O(n)
Space: O(1) ✅ — constant space!
```

**Template:**
```java
int prev2 = base_case_0;
int prev1 = base_case_1;

for (int i = 2; i <= n; i++) {
    int curr = /* formula using prev1, prev2 */;
    prev2 = prev1;
    prev1 = curr;
}
return prev1;
```

---

## 📐 Basic Steps to Solve Any DP Problem

Follow these 5 steps every time — no exceptions:

```
Step 1: Express the problem in terms of index/state
        → "What does dp[i] represent?"

Step 2: Write the recursive relation (try all choices)
        → "What are my options at step i?"

Step 3: Identify base cases
        → "What is the smallest input I can answer directly?"

Step 4: Code recursion + memoization (Top-Down)
        → Add memo array to plain recursion

Step 5: Convert to tabulation (Bottom-Up)
        → Fill dp array iteratively from base case

Bonus:  Space optimize if dp[i] only needs last few values
```

---

## 1. Frog Jump — 1 or 2 Steps

**Problem:** A frog is at index 0. It can jump 1 or 2 steps. The cost of jumping from index `i` to `j` is `|height[i] - height[j]|`. Find the **minimum energy** to reach the last index.

### 💡 Intuition

At every stone, the frog has 2 choices:
- Jump 1 step from `i-1`
- Jump 2 steps from `i-2`

Pick whichever costs less. This is a classic "make a choice, take the minimum" DP pattern.

**DP definition:** `dp[i]` = minimum energy to reach stone `i`

**Recurrence:**
```
dp[i] = min(
    dp[i-1] + |height[i] - height[i-1]|,   // came from 1 step back
    dp[i-2] + |height[i] - height[i-2]|    // came from 2 steps back
)
```

---

### 🔴 Recursion — O(2^n) Time | O(n) Space

```java
import java.util.*;

public class FrogJump {

    // Plain recursion — no memoization
    // At each stone, try both 1-step and 2-step jumps
    static int solve(int[] height, int n) {
        // Base case: already at stone 0, no cost
        if (n == 0) return 0;

        // Jump from 1 step back
        int oneStep = solve(height, n - 1) + Math.abs(height[n] - height[n - 1]);

        // Jump from 2 steps back (only valid if n >= 2)
        int twoStep = Integer.MAX_VALUE;
        if (n >= 2) {
            twoStep = solve(height, n - 2) + Math.abs(height[n] - height[n - 2]);
        }

        return Math.min(oneStep, twoStep);
    }

    public static void main(String[] args) {
        int[] height = {10, 20, 30, 10};
        int n = height.length - 1;
        System.out.println("Min Energy (Recursion): " + solve(height, n));
        // Output: 20
    }
}
```

---

### 🟡 Memoization (Top-Down) — O(n) Time | O(n) Space

```java
import java.util.*;

public class FrogJumpMemo {

    static int solve(int[] height, int n, int[] memo) {
        if (n == 0) return 0;

        // Return cached result if already computed
        if (memo[n] != -1) return memo[n];

        int oneStep = solve(height, n - 1, memo) + Math.abs(height[n] - height[n - 1]);

        int twoStep = Integer.MAX_VALUE;
        if (n >= 2) {
            twoStep = solve(height, n - 2, memo) + Math.abs(height[n] - height[n - 2]);
        }

        // Store result before returning
        memo[n] = Math.min(oneStep, twoStep);
        return memo[n];
    }

    public static void main(String[] args) {
        int[] height = {10, 20, 30, 10};
        int n = height.length - 1;
        int[] memo = new int[n + 1];
        Arrays.fill(memo, -1);   // -1 means not yet computed
        System.out.println("Min Energy (Memo): " + solve(height, n, memo));
    }
}
```

---

### ✅ Tabulation (Bottom-Up) — O(n) Time | O(n) Space

```java
public class FrogJumpTabulation {

    static int solve(int[] height) {
        int n = height.length;
        int[] dp = new int[n];

        dp[0] = 0;   // base case: no cost at starting stone

        for (int i = 1; i < n; i++) {
            // Cost of coming from 1 step back
            int oneStep = dp[i - 1] + Math.abs(height[i] - height[i - 1]);

            // Cost of coming from 2 steps back
            int twoStep = Integer.MAX_VALUE;
            if (i >= 2) {
                twoStep = dp[i - 2] + Math.abs(height[i] - height[i - 2]);
            }

            dp[i] = Math.min(oneStep, twoStep);
        }

        return dp[n - 1];
    }

    public static void main(String[] args) {
        int[] height = {10, 20, 30, 10};
        System.out.println("Min Energy (Tabulation): " + solve(height));
    }
}
```

---

### ✅ Space Optimized — O(n) Time | O(1) Space

```java
public class FrogJumpOptimized {

    static int solve(int[] height) {
        int n = height.length;
        // dp[i] only needs dp[i-1] and dp[i-2]
        // Replace entire dp array with just 2 variables
        int prev2 = 0;   // dp[i-2]
        int prev1 = 0;   // dp[i-1]

        for (int i = 1; i < n; i++) {
            int oneStep = prev1 + Math.abs(height[i] - height[i - 1]);

            int twoStep = Integer.MAX_VALUE;
            if (i >= 2) {
                twoStep = prev2 + Math.abs(height[i] - height[i - 2]);
            }

            int curr = Math.min(oneStep, twoStep);
            prev2 = prev1;   // shift window forward
            prev1 = curr;
        }

        return prev1;
    }

    public static void main(String[] args) {
        int[] height = {10, 20, 30, 10};
        System.out.println("Min Energy (Space Optimized): " + solve(height));
    }
}
```

**Complexity:**
| Approach | Time | Space |
|----------|------|-------|
| Recursion | O(2^n) | O(n) stack |
| Memoization | O(n) | O(n) |
| Tabulation | O(n) | O(n) |
| Space Optimized | O(n) | **O(1)** ✅ |

---

## 2. Frog Jump with K Steps

**Problem:** Same as above but the frog can jump up to **K steps** instead of just 1 or 2. Find the minimum energy.

### 💡 Intuition

Same idea, but now at each stone `i`, try **all jumps from 1 to K**. Pick the one with minimum total cost. We can't space-optimize this one because we need the last K values, not just 2.

**Recurrence:**
```
dp[i] = min over j from 1 to K:
    dp[i - j] + |height[i] - height[i - j]|   (if i - j >= 0)
```

---

### 🔴 Recursion — O(K^n) Time | O(n) Space

```java
public class FrogJumpK {

    static int solve(int[] height, int n, int k) {
        if (n == 0) return 0;

        int minEnergy = Integer.MAX_VALUE;

        // Try all jumps from 1 to k
        for (int jump = 1; jump <= k; jump++) {
            if (n - jump >= 0) {
                int cost = solve(height, n - jump, k)
                           + Math.abs(height[n] - height[n - jump]);
                minEnergy = Math.min(minEnergy, cost);
            }
        }

        return minEnergy;
    }

    public static void main(String[] args) {
        int[] height = {10, 40, 30, 10, 20};
        int k = 3;
        System.out.println("Min Energy K-Jump (Recursion): "
                           + solve(height, height.length - 1, k));
    }
}
```

---

### 🟡 Memoization (Top-Down) — O(n * K) Time | O(n) Space

```java
import java.util.*;

public class FrogJumpKMemo {

    static int solve(int[] height, int n, int k, int[] memo) {
        if (n == 0) return 0;
        if (memo[n] != -1) return memo[n];

        int minEnergy = Integer.MAX_VALUE;

        for (int jump = 1; jump <= k; jump++) {
            if (n - jump >= 0) {
                int cost = solve(height, n - jump, k, memo)
                           + Math.abs(height[n] - height[n - jump]);
                minEnergy = Math.min(minEnergy, cost);
            }
        }

        memo[n] = minEnergy;
        return memo[n];
    }

    public static void main(String[] args) {
        int[] height = {10, 40, 30, 10, 20};
        int k = 3;
        int n = height.length - 1;
        int[] memo = new int[n + 1];
        Arrays.fill(memo, -1);
        System.out.println("Min Energy K-Jump (Memo): "
                           + solve(height, n, k, memo));
    }
}
```

---

### ✅ Tabulation (Bottom-Up) — O(n * K) Time | O(n) Space

```java
public class FrogJumpKTabulation {

    static int solve(int[] height, int k) {
        int n = height.length;
        int[] dp = new int[n];
        dp[0] = 0;

        for (int i = 1; i < n; i++) {
            int minEnergy = Integer.MAX_VALUE;

            // Try all K jumps ending at stone i
            for (int jump = 1; jump <= k; jump++) {
                if (i - jump >= 0) {
                    int cost = dp[i - jump]
                               + Math.abs(height[i] - height[i - jump]);
                    minEnergy = Math.min(minEnergy, cost);
                }
            }

            dp[i] = minEnergy;
        }

        return dp[n - 1];
    }

    public static void main(String[] args) {
        int[] height = {10, 40, 30, 10, 20};
        int k = 3;
        System.out.println("Min Energy K-Jump (Tabulation): " + solve(height, k));
    }
}
```

> **Note:** No further space optimization here — `dp[i]` depends on the last K values, so we need the full array.

**Complexity:**
| Approach | Time | Space |
|----------|------|-------|
| Recursion | O(K^n) | O(n) |
| Memoization | O(n·K) | O(n) |
| Tabulation | O(n·K) | O(n) |

---

## 3. Staircase — Count Ways (1 or 2 Steps)

**Problem:** You're at the bottom of a staircase with `n` steps. You can climb 1 or 2 steps at a time. Count the **total number of distinct ways** to reach the top.

### 💡 Intuition

To reach step `n`, you could have come from:
- Step `n-1` (took 1 step), or
- Step `n-2` (took 2 steps)

So: `ways(n) = ways(n-1) + ways(n-2)` — this is just **Fibonacci**!

**Base cases:**
- `dp[0] = 1` → one way to stay at ground (do nothing)
- `dp[1] = 1` → only one way to reach step 1

---

### 🔴 Recursion — O(2^n) Time | O(n) Space

```java
public class Staircase {

    // Count all distinct ways to reach step n
    static int countWays(int n) {
        // Base cases
        if (n == 0) return 1;   // 1 way: stay (empty path)
        if (n == 1) return 1;   // 1 way: single 1-step jump

        // Either came from n-1 (1-step) or n-2 (2-step)
        return countWays(n - 1) + countWays(n - 2);
    }

    public static void main(String[] args) {
        int n = 5;
        System.out.println("Ways to climb " + n + " stairs (Recursion): "
                           + countWays(n));
        // Output: 8
    }
}
```

---

### 🟡 Memoization (Top-Down) — O(n) Time | O(n) Space

```java
import java.util.*;

public class StaircaseMemo {

    static int countWays(int n, int[] memo) {
        if (n == 0) return 1;
        if (n == 1) return 1;

        if (memo[n] != -1) return memo[n];   // already solved

        memo[n] = countWays(n - 1, memo) + countWays(n - 2, memo);
        return memo[n];
    }

    public static void main(String[] args) {
        int n = 5;
        int[] memo = new int[n + 1];
        Arrays.fill(memo, -1);
        System.out.println("Ways (Memo): " + countWays(n, memo));
    }
}
```

---

### ✅ Tabulation (Bottom-Up) — O(n) Time | O(n) Space

```java
public class StaircaseTabulation {

    static int countWays(int n) {
        if (n <= 1) return 1;
        int[] dp = new int[n + 1];
        dp[0] = 1;   // base
        dp[1] = 1;   // base

        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];   // Fibonacci pattern
        }

        return dp[n];
    }

    public static void main(String[] args) {
        System.out.println("Ways (Tabulation): " + countWays(5));   // 8
        System.out.println("Ways (Tabulation): " + countWays(6));   // 13
    }
}
```

---

### ✅ Space Optimized — O(n) Time | O(1) Space

```java
public class StaircaseOptimized {

    static int countWays(int n) {
        if (n <= 1) return 1;
        int prev2 = 1;   // dp[0]
        int prev1 = 1;   // dp[1]

        for (int i = 2; i <= n; i++) {
            int curr = prev1 + prev2;
            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }

    public static void main(String[] args) {
        System.out.println("Ways (Optimized): " + countWays(5));   // 8
    }
}
```

**Complexity:**
| Approach | Time | Space |
|----------|------|-------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n) | O(n) |
| Tabulation | O(n) | O(n) |
| Space Optimized | O(n) | **O(1)** ✅ |

---

## 4. House Robber — Non-Adjacent Sum

**Problem:** Given an array of houses with money, rob houses to maximize total money. You **cannot rob two adjacent houses** (alarm triggers).

### 💡 Intuition

At each house `i`, you have exactly **two choices**:
- **Rob it:** Take `nums[i]` + best from `i-2` onwards (can't take `i-1`)
- **Skip it:** Take best from `i-1` onwards

**DP definition:** `dp[i]` = max money robbing from house 0 to house i

**Recurrence:**
```
dp[i] = max(
    nums[i] + dp[i-2],    // rob current house
    dp[i-1]               // skip current house
)
```

---

### 🔴 Recursion — O(2^n) Time | O(n) Space

```java
public class HouseRobber {

    // Returns max money from house index 0..n
    static int rob(int[] nums, int n) {
        if (n == 0) return nums[0];
        if (n < 0)  return 0;

        // Choice 1: Rob house n → add nums[n] and jump 2 back
        int pick = nums[n] + rob(nums, n - 2);

        // Choice 2: Skip house n → solve for n-1
        int skip = rob(nums, n - 1);

        return Math.max(pick, skip);
    }

    public static void main(String[] args) {
        int[] nums = {2, 7, 9, 3, 1};
        System.out.println("Max Robbery (Recursion): "
                           + rob(nums, nums.length - 1));
        // Output: 12 (rob houses 0, 2, 4 → 2+9+1=12)
    }
}
```

---

### 🟡 Memoization (Top-Down) — O(n) Time | O(n) Space

```java
import java.util.*;

public class HouseRobberMemo {

    static int rob(int[] nums, int n, int[] memo) {
        if (n == 0) return nums[0];
        if (n < 0)  return 0;
        if (memo[n] != -1) return memo[n];

        int pick = nums[n] + rob(nums, n - 2, memo);
        int skip = rob(nums, n - 1, memo);

        memo[n] = Math.max(pick, skip);
        return memo[n];
    }

    public static void main(String[] args) {
        int[] nums = {2, 7, 9, 3, 1};
        int n = nums.length - 1;
        int[] memo = new int[n + 1];
        Arrays.fill(memo, -1);
        System.out.println("Max Robbery (Memo): " + rob(nums, n, memo));
    }
}
```

---

### ✅ Tabulation (Bottom-Up) — O(n) Time | O(n) Space

```java
public class HouseRobberTabulation {

    static int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];

        int[] dp = new int[n];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);   // pick best of first two houses

        for (int i = 2; i < n; i++) {
            int pick = nums[i] + dp[i - 2];   // rob current + best 2 back
            int skip = dp[i - 1];              // skip current, take best so far
            dp[i] = Math.max(pick, skip);
        }

        return dp[n - 1];
    }

    public static void main(String[] args) {
        int[] nums = {2, 7, 9, 3, 1};
        System.out.println("Max Robbery (Tabulation): " + rob(nums));   // 12
    }
}
```

---

### ✅ Space Optimized — O(n) Time | O(1) Space

```java
public class HouseRobberOptimized {

    static int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];

        int prev2 = nums[0];
        int prev1 = Math.max(nums[0], nums[1]);

        for (int i = 2; i < n; i++) {
            int curr = Math.max(nums[i] + prev2, prev1);
            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }

    public static void main(String[] args) {
        int[] nums = {2, 7, 9, 3, 1};
        System.out.println("Max Robbery (Optimized): " + rob(nums));   // 12
    }
}
```

**Complexity:**
| Approach | Time | Space |
|----------|------|-------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n) | O(n) |
| Tabulation | O(n) | O(n) |
| Space Optimized | O(n) | **O(1)** ✅ |

---

## 5. House Robber II — Circular Array

**Problem:** Same as House Robber, but the houses are arranged in a **circle** — first and last house are now adjacent. You still cannot rob adjacent houses.

### 💡 Intuition

The circle constraint means: **you can't rob both house 0 and house n-1 together.**

**Key trick:** Split the circular problem into two linear problems:
- **Case 1:** Rob from index `0` to `n-2` (exclude last house)
- **Case 2:** Rob from index `1` to `n-1` (exclude first house)

Answer = `max(Case 1, Case 2)`

This reuses the exact same House Robber logic on a subarray!

---

### 🔴 Recursion — O(2^n) Time | O(n) Space

```java
public class HouseRobberII {

    // Reuse plain House Robber logic on a subarray
    static int robLine(int[] nums, int start, int end) {
        if (start == end) return nums[start];
        if (start > end)  return 0;
        return Math.max(
            nums[start] + robLine(nums, start + 2, end),   // rob start
            robLine(nums, start + 1, end)                   // skip start
        );
    }

    static int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        if (n == 2) return Math.max(nums[0], nums[1]);

        // Case 1: skip last house (0 to n-2)
        int case1 = robLine(nums, 0, n - 2);
        // Case 2: skip first house (1 to n-1)
        int case2 = robLine(nums, 1, n - 1);

        return Math.max(case1, case2);
    }

    public static void main(String[] args) {
        int[] nums = {2, 3, 2};
        System.out.println("Circular Robbery (Recursion): " + rob(nums));
        // Output: 3

        int[] nums2 = {1, 2, 3, 1};
        System.out.println("Circular Robbery (Recursion): " + rob(nums2));
        // Output: 4
    }
}
```

---

### 🟡 Memoization (Top-Down) — O(n) Time | O(n) Space

```java
import java.util.*;

public class HouseRobberIIMemo {

    static int robLine(int[] nums, int n, int[] memo) {
        if (n < 0)  return 0;
        if (n == 0) return nums[0];
        if (memo[n] != -1) return memo[n];

        int pick = nums[n] + robLine(nums, n - 2, memo);
        int skip = robLine(nums, n - 1, memo);
        memo[n] = Math.max(pick, skip);
        return memo[n];
    }

    static int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        if (n == 2) return Math.max(nums[0], nums[1]);

        // Case 1: houses 0..n-2 (exclude last)
        int[] memo1 = new int[n];
        Arrays.fill(memo1, -1);
        int[] sub1 = Arrays.copyOfRange(nums, 0, n - 1);
        int case1 = robLine(sub1, sub1.length - 1, memo1);

        // Case 2: houses 1..n-1 (exclude first)
        int[] memo2 = new int[n];
        Arrays.fill(memo2, -1);
        int[] sub2 = Arrays.copyOfRange(nums, 1, n);
        int case2 = robLine(sub2, sub2.length - 1, memo2);

        return Math.max(case1, case2);
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 3, 1};
        System.out.println("Circular Robbery (Memo): " + rob(nums));   // 4
    }
}
```

---

### ✅ Tabulation (Bottom-Up) — O(n) Time | O(n) Space

```java
public class HouseRobberIITabulation {

    // Standard house robber on a linear array segment
    static int robLinear(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];

        int[] dp = new int[n];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);

        for (int i = 2; i < n; i++) {
            dp[i] = Math.max(nums[i] + dp[i - 2], dp[i - 1]);
        }
        return dp[n - 1];
    }

    static int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        if (n == 2) return Math.max(nums[0], nums[1]);

        // Case 1: exclude last house → rob nums[0..n-2]
        int case1 = robLinear(Arrays.copyOfRange(nums, 0, n - 1));

        // Case 2: exclude first house → rob nums[1..n-1]
        int case2 = robLinear(Arrays.copyOfRange(nums, 1, n));

        return Math.max(case1, case2);
    }

    public static void main(String[] args) {
        int[] nums1 = {2, 3, 2};
        System.out.println("Circular (Tabulation): " + rob(nums1));   // 3

        int[] nums2 = {1, 2, 3, 1};
        System.out.println("Circular (Tabulation): " + rob(nums2));   // 4
    }
}
```

---

### ✅ Space Optimized — O(n) Time | O(1) Space

```java
import java.util.Arrays;

public class HouseRobberIIOptimized {

    static int robLinear(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];

        int prev2 = nums[0];
        int prev1 = Math.max(nums[0], nums[1]);

        for (int i = 2; i < n; i++) {
            int curr = Math.max(nums[i] + prev2, prev1);
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }

    static int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        if (n == 2) return Math.max(nums[0], nums[1]);

        // Split into two linear problems
        int case1 = robLinear(Arrays.copyOfRange(nums, 0, n - 1));
        int case2 = robLinear(Arrays.copyOfRange(nums, 1, n));

        return Math.max(case1, case2);
    }

    public static void main(String[] args) {
        int[] nums = {1, 2, 3, 1};
        System.out.println("Circular (Space Opt): " + rob(nums));   // 4

        int[] nums2 = {2, 7, 9, 3, 1};
        System.out.println("Circular (Space Opt): " + rob(nums2));  // 11
    }
}
''''
# 🥷 Ninja's Training — DP on Arrays

> Added to the DSA DP Series | All 4 approaches in Java with full intuition

---

## Problem Statement

A Ninja has to train for `n` days. Each day he can perform one of **3 activities**:
- Activity 0 — Running
- Activity 1 — Fighting Practice  
- Activity 2 — Learning New Moves

Each activity on each day gives some **merit points** stored in a 2D array `points[day][activity]`.

**Rule:** He **cannot do the same activity on two consecutive days.**

Find the **maximum merit points** the ninja can earn over all `n` days.

**Example:**
```
points = [
  [10, 40, 70],   // day 0
  [20, 50, 80],   // day 1
  [30, 60, 90]    // day 2
]

Best path: 70 (day0, act2) → 50 (day1, act1) → 90 (day2, act2)
           Wait — act2 repeated! Not allowed.

Best valid: 70 (day0, act2) → 50 (day1, act1) → 90 (day2, act2)
           Still same. Try: 70 → 20 → 90 = 180? act2 on day0 and day2 is fine (not consecutive).
           Actually: 70 (act2) → 20 (act0) → 90 (act2) = 180 ✅

Answer: 210  →  70 (day0,act2) + 50 (day1,act1) + 90 (day2,act2) = 210
         Wait: day1=act1, day2=act2 → different ✅, day0=act2, day1=act1 → different ✅
         So 210 is valid. ✅
```

---

## 💡 Intuition

At every day, you pick one of the 3 activities — but you can't pick the **same one you did yesterday**.

This is a **"try all choices, skip the one that's blocked"** DP pattern.

**DP definition:**
> `dp[day][last]` = max points from day 0 to `day`, given that `last` was the activity done on `day`

**Recurrence:**
```
For each day and each activity `task` (where task != last):
    dp[day][task] = points[day][task] + max over previous day with last != task
```

**Base case (day 0):**
```
dp[0][0] = points[0][0]
dp[0][1] = points[0][1]
dp[0][2] = points[0][2]
```

**Key trick — the `last` parameter:**  
Pass `last = 3` as a special "no last activity" flag for day 0 (nothing was done before day 0).

---

## 🔢 Basic Steps Applied

```
Step 1: State definition
        → solve(day, last) = max points from day 0..day
          where `last` = activity done on previous day

Step 2: Recurrence
        → At each day, try all 3 tasks
        → Skip the task == last
        → Take max of all valid choices

Step 3: Base case
        → day == 0: try all tasks != last, return max valid

Step 4: Top-Down → add memo[day][last]

Step 5: Bottom-Up → fill dp table from day 0 to n-1

Bonus:  dp[day] only needs dp[day-1] → space optimize to 1D array
```

---

## 🔴 Recursion — O(3^n) Time | O(n) Space

```java
public class NinjaTraining {

    // day  = current day we're deciding activity for
    // last = activity done on the previous day (3 means no previous)
    static int solve(int day, int last, int[][] points) {

        // Base case: on day 0, try all activities except 'last'
        if (day == 0) {
            int maxPoints = 0;
            for (int task = 0; task < 3; task++) {
                if (task != last) {
                    maxPoints = Math.max(maxPoints, points[0][task]);
                }
            }
            return maxPoints;
        }

        int maxPoints = 0;

        // Try all 3 activities for today
        for (int task = 0; task < 3; task++) {
            if (task != last) {
                // Points today + best we can get from day 0..(day-1)
                // with 'task' as the new last activity
                int todayPoints = points[day][task]
                                  + solve(day - 1, task, points);
                maxPoints = Math.max(maxPoints, todayPoints);
            }
        }

        return maxPoints;
    }

    public static void main(String[] args) {
        int[][] points = {
            {10, 40, 70},
            {20, 50, 80},
            {30, 60, 90}
        };
        int n = points.length;

        // Start from last day, no previous activity (last = 3)
        System.out.println("Max Points (Recursion): "
                           + solve(n - 1, 3, points));
        // Output: 210
    }
}
```

**Why O(3^n)?** Every day branches into up to 3 choices, and we go n days deep. Same subproblems get recomputed — that's the problem memoization fixes.

---

## 🟡 Memoization (Top-Down) — O(n × 4) Time | O(n × 4) Space

```java
import java.util.Arrays;

public class NinjaTrainingMemo {

    // memo[day][last] stores the answer for that state
    // last can be 0,1,2 (activity) or 3 (no previous) → size 4
    static int solve(int day, int last, int[][] points, int[][] memo) {

        if (day == 0) {
            int maxPoints = 0;
            for (int task = 0; task < 3; task++) {
                if (task != last) {
                    maxPoints = Math.max(maxPoints, points[0][task]);
                }
            }
            return maxPoints;
        }

        // Return cached answer if already computed
        if (memo[day][last] != -1) return memo[day][last];

        int maxPoints = 0;
        for (int task = 0; task < 3; task++) {
            if (task != last) {
                int todayPoints = points[day][task]
                                  + solve(day - 1, task, points, memo);
                maxPoints = Math.max(maxPoints, todayPoints);
            }
        }

        // Cache before returning
        memo[day][last] = maxPoints;
        return memo[day][last];
    }

    public static void main(String[] args) {
        int[][] points = {
            {10, 40, 70},
            {20, 50, 80},
            {30, 60, 90}
        };
        int n = points.length;

        // memo dimensions: n days × 4 possible 'last' values (0,1,2,3)
        int[][] memo = new int[n][4];
        for (int[] row : memo) Arrays.fill(row, -1);

        System.out.println("Max Points (Memo): "
                           + solve(n - 1, 3, points, memo));
        // Output: 210
    }
}
```

**Why n × 4?** `day` goes 0..n-1, `last` goes 0..3. That's all unique states. Each state computed only once.

---

## ✅ Tabulation (Bottom-Up) — O(n × 4 × 3) Time | O(n × 4) Space

```java
public class NinjaTrainingTabulation {

    static int solve(int[][] points) {
        int n = points.length;

        // dp[day][last] = max points from day 0..day
        //                 when 'last' is the activity done on day 'day'
        int[][] dp = new int[n][4];

        // Base case: fill day 0
        // dp[0][last] = best activity on day 0 that is NOT 'last'
        dp[0][0] = Math.max(points[0][1], points[0][2]); // can't do task 0
        dp[0][1] = Math.max(points[0][0], points[0][2]); // can't do task 1
        dp[0][2] = Math.max(points[0][0], points[0][1]); // can't do task 2
        dp[0][3] = Math.max(points[0][0],
                   Math.max(points[0][1], points[0][2])); // no restriction

        // Fill from day 1 to n-1
        for (int day = 1; day < n; day++) {
            for (int last = 0; last <= 3; last++) {
                dp[day][last] = 0;

                // Try all 3 tasks for today
                for (int task = 0; task < 3; task++) {
                    if (task != last) {
                        int todayPoints = points[day][task] + dp[day - 1][task];
                        dp[day][last] = Math.max(dp[day][last], todayPoints);
                    }
                }
            }
        }

        // Answer: last day, no restriction on last (last = 3)
        return dp[n - 1][3];
    }

    public static void main(String[] args) {
        int[][] points = {
            {10, 40, 70},
            {20, 50, 80},
            {30, 60, 90}
        };
        System.out.println("Max Points (Tabulation): " + solve(points));
        // Output: 210
    }
}
```

---

## ✅ Space Optimized — O(n × 4 × 3) Time | O(4) Space

```java
public class NinjaTrainingOptimized {

    static int solve(int[][] points) {
        int n = points.length;

        // dp[day] only needs dp[day-1]
        // Replace 2D array with a single 1D array of size 4
        int[] prev = new int[4];

        // Base case: day 0
        prev[0] = Math.max(points[0][1], points[0][2]);
        prev[1] = Math.max(points[0][0], points[0][2]);
        prev[2] = Math.max(points[0][0], points[0][1]);
        prev[3] = Math.max(points[0][0],
                  Math.max(points[0][1], points[0][2]));

        // Iterate from day 1 to n-1
        for (int day = 1; day < n; day++) {
            int[] curr = new int[4];   // current day's dp values

            for (int last = 0; last <= 3; last++) {
                curr[last] = 0;

                for (int task = 0; task < 3; task++) {
                    if (task != last) {
                        int todayPoints = points[day][task] + prev[task];
                        curr[last] = Math.max(curr[last], todayPoints);
                    }
                }
            }

            prev = curr;   // shift: current day becomes previous for next day
        }

        // Answer with no restriction → prev[3]
        return prev[3];
    }

    public static void main(String[] args) {
        int[][] points = {
            {10, 40, 70},
            {20, 50, 80},
            {30, 60, 90}
        };
        System.out.println("Max Points (Space Optimized): " + solve(points));
        // Output: 210

        // Test 2
        int[][] points2 = {
            {1, 2, 5},
            {3, 1, 1},
            {3, 3, 3}
        };
        System.out.println("Max Points (Space Optimized): " + solve(points2));
        // Output: 11  →  5 + 3 + 3 = 11
    }
}
```

---

## 📊 Complexity Summary

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| Recursion | O(3^n) | O(n) stack | Recomputes everything |
| Memoization | O(n × 4) | O(n × 4) | Each state once |
| Tabulation | O(n × 4 × 3) | O(n × 4) | No recursion stack |
| Space Optimized | O(n × 4 × 3) | **O(4) = O(1)** ✅ | Only prev row needed |

> Time for Memoization and Tabulation is effectively **O(n)** since 4 × 3 = 12 is a constant.

---

## 🔍 State Transition Diagram

```
Day 0      Day 1          Day 2
------     ----------     ----------
act0 ──────→ act1, act2
act1 ──────→ act0, act2
act2 ──────→ act0, act1

At each arrow: add points[day][chosen_task]
Goal: maximize total across all days
```

---

## 🧠 Why the `last = 3` Trick?

Instead of handling day 0 as a special case *outside* the function, we use `last = 3` as a **sentinel value** meaning "no previous activity."

```java
// Without sentinel → messy special case outside
int ans = Math.max(
    Math.max(points[n-1][0], points[n-1][1]),
    points[n-1][2]
);

// With sentinel → clean, uniform call
int ans = solve(n - 1, 3, points);  // 3 = "no last activity"
```

This keeps the recursion clean — when `last = 3`, all 3 tasks are valid on day 0.

---

## 🔑 Key Pattern Recognition

This problem is the template for all **"choose from K options each step, can't repeat last"** DP problems:

```
Same pattern used in:
→ Paint Houses (K colors, can't use same color adjacent)
→ K-colored fence problems
→ Job scheduling with cooldown periods
```

Whenever you see **"N days × K choices × no consecutive repeat"** — this is your blueprint.

---

*Keep grinding 🔥 | [LeetCode](https://leetcode.com/your-username) | [LinkedIn](https://linkedin.com/in/your-profile)*

**Complexity:**
| Approach | Time | Space |
|----------|------|-------|
| Recursion | O(2^n) | O(n) |
| Memoization | O(n) | O(n) |
| Tabulation | O(n) | O(n) |
| Space Optimized | O(n) | **O(1)** ✅ |

---

## 📊 Full Complexity Table

| Problem | Recursion | Memoization | Tabulation | Space Opt |
|---------|-----------|-------------|------------|-----------|
| Frog Jump (1/2 steps) | O(2^n) / O(n) | O(n) / O(n) | O(n) / O(n) | O(n) / **O(1)** |
| Frog Jump K steps | O(K^n) / O(n) | O(nK) / O(n) | O(nK) / O(n) | ❌ needs K values |
| Staircase (ways) | O(2^n) / O(n) | O(n) / O(n) | O(n) / O(n) | O(n) / **O(1)** |
| House Robber | O(2^n) / O(n) | O(n) / O(n) | O(n) / O(n) | O(n) / **O(1)** |
| House Robber II | O(2^n) / O(n) | O(n) / O(n) | O(n) / O(n) | O(n) / **O(1)** |

---

## 🧠 Summary — DP Cheat Sheet

```
┌─────────────────────────────────────────────────────┐
│              DP DECISION TREE                       │
│                                                     │
│  Problem asks min/max/count/possible?               │
│         ↓ YES                                       │
│  Can you define dp[i] = answer for subproblem i?    │
│         ↓ YES                                       │
│  Write recursion with base cases                    │
│         ↓                                           │
│  Add memo[] array → Top-Down DP                     │
│         ↓                                           │
│  Convert to loop → Bottom-Up DP                     │
│         ↓                                           │
│  dp[i] needs only last 1-2 values?                  │
│         ↓ YES                                       │
│  Replace array with variables → O(1) Space          │
└─────────────────────────────────────────────────────┘
```

### When to Use Each Method

| You want... | Use |
|-------------|-----|
| Quick working solution | Memoization (Top-Down) |
| No recursion stack overhead | Tabulation (Bottom-Up) |
| Best possible space | Space Optimization |
| Interview — show understanding | Write all 3 progressively |



---

