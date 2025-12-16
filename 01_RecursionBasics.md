# DSA 01: Recursion Basics & Visualization

**Topic:** Introduction to Recursion, Call Stack, Time & Space Complexity
**Focus:** Building the recursive thought process ("Recursive Leap of Faith")

---

## 1. Iteration vs Recursion

### Iterative Approach

* Uses loops (`for`, `while`) to repeat tasks.
* **Memory Usage:** O(1) auxiliary space (constant).
* **Mechanism:** Single function call with internal repetition.

```cpp
void func1(int n) {
    for(int i = n; i > 0; i--) {
        cout << i << endl;
    }
}
```

### Recursive Approach

* A function calls itself to solve a problem.
* **Memory Usage:** Uses stack memory for each function call.
* **Mechanism:** Multiple function calls stored in call stack.

```cpp
void func2(int n) {
    if (n == 0) return;   // Base Condition
    cout << n << endl;
    func2(n - 1);         // Recursive Call
}
```

### Key Concepts

1. **Recursive Function:** Function that calls itself.
2. **Call Stack:** Memory structure that stores function calls (LIFO).
3. **Recursive Tree:** Diagram showing how recursive calls branch.

---

## 2. Stack Overflow (The Danger)

* Occurs when recursion never stops.
* Stack memory fills up due to infinite calls.
* Program crashes with **Stack Overflow Error**.

### Cause

* Missing or incorrect base condition.

### Solution

* Always define a **Base / Terminating Condition**.

Example:

* Calling `func(10)` without `if(n==0) return;` leads to infinite recursion.

---

## 3. The 5-Step Framework for Solving Recursion

1. **Identify the Actual Problem**
   What is required for input `n`?

2. **Identify the Sub-problem**
   Smaller version of the same problem (usually `n-1`, `n-2`, etc.).

3. **Recursive Leap of Faith (Trust)**
   Assume the function works correctly for the sub-problem.

4. **Link Actual Problem to Sub-problem**
   Combine results to form the recurrence relation.

5. **Base Condition**
   Decide when recursion must stop.

---

## 4. Visualizing Recursion: The Sandwich Pattern

### Problem

Print: `3 2 1 1 2 3` using recursion (`n = 3`).

### Logic

1. Print `n` (before recursive call).
2. Trust `func(n-1)` prints inner pattern.
3. Print `n` again (after recursive call).

```cpp
void func(int n) {
    if (n == 0) return;
    cout << n << " ";
    func(n - 1);
    cout << n << " ";
}
```

### Call Stack Flow

* func(3) → print 3 → func(2)
* func(2) → print 2 → func(1)
* func(1) → print 1 → func(0)
* func(0) returns
* func(1) prints 1
* func(2) prints 2
* func(3) prints 3

---

## 5. LeetCode Recursion Examples

### A. Fibonacci Number (LC 509)

**Formula:**
F(n) = F(n-1) + F(n-2)

**Base Cases:**
F(0) = 0, F(1) = 1

```C++
public int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}
```
![Untitled design](https://github.com/user-attachments/assets/5a0cfdd2-f90b-4f50-b3f0-83d838155664)


**Complexity:**

* Time: O(2^n)
* Space: O(n) (maximum recursion depth)

---

### B. Tribonacci Number (LC 1137)

**Formula:**
T(n) = T(n-1) + T(n-2) + T(n-3)

```C++
public int trib(int n) {
    if(n==2)return 1;
    if (n <= 1) return n;
    return trib(n - 1) + trib(n - 2) + trib(n-3);
}
```
![Untitled design(1)](https://github.com/user-attachments/assets/a5d7aa53-5557-457b-8ccc-bff01e76eb2a)


**Complexity:**

* Time: O(3^n)
* Space: O(n)

---

### C. Climbing Stairs (LC 70)

**Problem:** Number of ways to reach step `n` using 1 or 2 steps.

### Thought Process

* Reach `n` from:

  * `n-1` (1 step)
  * `n-2` (2 steps)

**Recurrence:**
Ways(n) = Ways(n-1) + Ways(n-2)

**Base Cases:**

* n = 1 → 1 way
* n = 2 → 2 ways

```java
public int climbStairs(int n) {
    if (n <= 2) return n;
    return climbStairs(n - 1) + climbStairs(n - 2);
}
```

![Untitled design](https://github.com/user-attachments/assets/5a0cfdd2-f90b-4f50-b3f0-83d838155664)


Note: This solution causes TLE but is correct for understanding recursion.

---

## 6. Time & Space Complexity in Recursion

| Aspect           | Key Idea       | Rule                     |
| ---------------- | -------------- | ------------------------ |
| Time Complexity  | Recursive Tree | (Branches)^(Height)      |
| Space Complexity | Call Stack     | Height of recursion tree |

### Examples

* Fibonacci → Time: O(2^n), Space: O(n)
* Tribonacci → Time: O(3^n), Space: O(n)

---


