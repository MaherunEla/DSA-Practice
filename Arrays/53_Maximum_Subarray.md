# Maximum Subarray

**Given an integer array `nums`, find the subarray with the largest sum and return its sum.**

---

### Example 1

**Input:** `nums = [-2,1,-3,4,-1,2,1,-5,4]`  
**Output:** `6`

**Explanation:**  
The subarray `[4, -1, 2, 1]` has the largest sum, which is `6`.

---

A subarray is a **continuous part of an array** and can be of any size.  
In a subarray, all elements appear in the same order as in the original array, with **no elements skipped**.

For example:

- `[-2, 1, -3, 4]` is a valid subarray
- `[-2, 1, 4]` is not valid because `-3` is skipped

---

## Brute-Force Approach

To return the largest sum, we can manually calculate the sum of **all possible subarrays** and return the maximum one.  
For this, we need to use **nested loops**.

```cpp
int Maxsum = INT_MIN;

for (int i = 0; i < nums.size(); i++) {
    int sum = 0;
    for (int j = i; j < nums.size(); j++) {
        sum += nums[j];
        Maxsum = max(Maxsum, sum);
    }
}
return Maxsum;

```

**Time Complexity:** O(n²)
**Space Complexity:** O(1)

This is a **brute-force approach**, but it is too slow for large inputs.

---

## Optimized Approach (Kadane’s Algorithm)

If we observe carefully, the array contains both **positive and negative numbers.**
For negative values, the sum will definitely decrease, but we are looking for the **maximum sum.**

So whenever the current sum becomes **less than 0,** it means continuing the current subarray will not give a better result.
In that case, we start a **new subarray** by resetting `sum = 0`, while always keeping track of the maximum sum.

```cpp
int sum = 0, maxsum = INT_MIN;

for (int i = 0; i < nums.size(); i++) {
    sum += nums[i];
    if (sum > maxsum) maxsum = sum;
    if (sum < 0) sum = 0;
}
return maxsum;

```

**Time Complexity:** O(n)
**Space Complexity:** O(1)

This is a much better solution and is the optimal approach for this problem.

---
