### 02: Advanced Recursion & Backtracking

**Focus:** Mastering recursion, understanding backtracking (the "include/exclude" pattern), time/space complexity analysis, and avoiding common pitfalls.

---

### 5-Step Framework for Recursive Problems

1. Identify the **Actual Problem**.
2. Identify the **Sub-problem**.
3. **Trust/Faith:** Assume the sub-problem works correctly.
4. **Link:** Connect the actual problem and sub-problem (Recurrence Relation).
5. **Base Condition:** Define when the recursion stops to prevent Stack Overflow.

---

## 2. Deep Dive: Min Cost Climbing Stairs (LeetCode 746)

### Problem Statement

You are given an integer array `cost` where `cost[i]` is the cost of the *iᵗʰ* step on a staircase. Once you pay the cost, you can either climb one or two steps. You can start from step `0` or step `1`. Find the minimum cost to reach the top of the floor.

### Why Greedy Fails

A greedy approach (always choosing the cheaper next step) fails because a cheaper immediate step might lead to a very expensive path later. We must explore **all possible paths** to find the global minimum.

---

### Recursive Approach 1: Bottom-Up (0 → N)

**Helper Function Definition:**
`helper(index)` returns the minimum cost to reach the top starting from `index`.

**Recurrence Relation:**

* Take 1 step: `cost[index] + helper(index + 1)`
* Take 2 steps: `cost[index] + helper(index + 2)`

Result:

```
min(cost[index] + helper(index + 1), cost[index] + helper(index + 2))
```

**Base Case:**
If `index >= n`, we have reached or crossed the top, so cost = `0`.

### C++ Code (Bottom-Up Logic)

```cpp
class Solution {
public:
    int helper(vector<int>& cost, int index) {
        // Base Condition
        if (index >= cost.size()) {
            return 0;
        }
        
        int step1 = helper(cost, index + 1);
        int step2 = helper(cost, index + 2);
        
        return cost[index] + min(step1, step2);
    }

    int minCostClimbingStairs(vector<int>& cost) {
        return min(helper(cost, 0), helper(cost, 1));
    }
};
```

---

### Recursive Approach 2: Top-Down (N → 0)

**Helper Function Definition:**
`helper(index)` returns the minimum cost to reach `index` from the bottom.

**Recurrence Relation:**

* From `index - 1`: `helper(index - 1) + cost[index - 1]`
* From `index - 2`: `helper(index - 2) + cost[index - 2]`

**Base Case:**
If `index <= 1`, cost = `0` (we can start there for free).

---

## 3. House Robber (LeetCode 198) – Include / Exclude Pattern

### Problem Statement

You are a robber planning to rob houses along a street. Adjacent houses have security systems connected, so you cannot rob two adjacent houses. Given an array `nums`, return the maximum amount of money you can rob.

---

### Include / Exclude (Pick / Don’t Pick) Strategy

For every house, you have two choices:

1. **Include (Rob):**

   * Add `nums[index]`
   * Skip the next house → move to `index + 2`

2. **Exclude (Don’t Rob):**

   * Skip current house
   * Move to `index + 1`

---

### Recurrence Relation

```
rob = nums[index] + helper(index + 2)
dontRob = helper(index + 1)
answer = max(rob, dontRob)
```

**Base Condition:**
If `index >= nums.size()`, return `0`.

---

### Complexity Analysis

* **Time Complexity:** O(2ⁿ)
* **Space Complexity:** O(n) (recursive call stack)

---

### C++ Code (House Robber)

```cpp
class Solution {
public:
    int helper(vector<int>& nums, int index) {
        if (index >= nums.size()) {
            return 0;
        }
        
        int rob = nums[index] + helper(nums, index + 2);
        int dontRob = helper(nums, index + 1);
        
        return max(rob, dontRob);
    }

    int rob(vector<int>& nums) {
        return helper(nums, 0);
    }
};
```

---

## 4. Subsets (LeetCode 78) – Backtracking & State Management

### Problem Statement

Given an integer array `nums` of unique elements, return all possible subsets (the power set).

---

### Backtracking Approach

For every element:

1. **Exclude it** from the subset
2. **Include it** in the subset

We build a `current` subset and move forward. When we reach the end of the array, we store the subset.

---

### Key Concept: Backtracking

When passing `current` by reference in C++:

* We **must undo changes** after recursion
* This is done using `pop_back()`
* It ensures the state remains clean for the next branch

---

### Correct C++ Code (Backtracking)

```cpp
class Solution {
public:
    void helper(int index, vector<int>& nums, vector<int>& current, vector<vector<int>>& ans) {
        if (index == nums.size()) {
            ans.push_back(current);
            return;
        }
        
        // Exclude
        helper(index + 1, nums, current, ans);
        
        // Include
        current.push_back(nums[index]);
        helper(index + 1, nums, current, ans);
        
        // Backtrack
        current.pop_back();
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> current;
        helper(0, nums, current, ans);
        return ans;
    }
};
```

---

### Common Pitfall: Missing `pop_back()`

If `pop_back()` is omitted while passing by reference:

* The state of `current` leaks into other branches
* Results become incorrect
* This is a classic backtracking bug

---

### Pass-by-Value Alternative

Passing `current` by value avoids backtracking errors:

```
helper(index + 1, nums, current);
```

But this is inefficient due to repeated copying and higher space usage.

---

## 5. Summary & Key Takeaways

1. **Greedy vs Recursion:**

   * Greedy → local optimal
   * Recursion / Backtracking → global optimal

2. **Include / Exclude Pattern:**

   * Core idea behind subsets, combinations, knapsack, house robber

3. **Backtracking:**

   * Explicitly undo state changes
   * Essential when passing by reference

4. **Reference vs Value:**

   * Prefer reference for efficiency
   * Must backtrack correctly

5. **Base Conditions:**

   * Always handle edge cases
   * Prevent stack overflow

