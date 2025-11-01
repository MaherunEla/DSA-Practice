# Rotate Array

**Given an integer array nums, rotate the array to the right by k steps, where k is non-negative.**

**Example 1:**

**Input:** nums = [1,2,3,4,5,6,7], k = 3
**Output:** [5,6,7,1,2,3,4]

**Explanation:**
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

In this problem, imagine the array as a circle: after the last element comes the first element. So when we shift the last element one step to the right, it goes to the start and the other elements shift accordingly.

        0  1  2  3  4  5  6

nums = [1, 2, 3, 4, 5, 6, 7] , k = 3

        0  1  2  3  4  5  6

output [5, 6, 7, 1, 2, 3, 4]

**Compare indices:** nums[0] = 1 and output[3] = 1. Observe that 0 + k = 3. Similarly, 1 + k = 4 output[4] = 2, 2 + k = 5 output[5] = 3, 3 + k = 6 output[6] = 4. For 4 + k = 7, which is outside the array, use (4 + k) % n = 7 % 7 = 0, so output[0] = 5. Likewise, (5 + k) % n = 8 % 7 = 1 output[1]= 6, (6 + k) % n = 9 % 7 = 2 output[2] = 7. So always **Use (i + k) % n so the index loops back to the start if it exceeds the array size.**

You cannot apply this directly on nums **in-place** by assigning **nums[(i + k) % n] = nums[i]** because you would overwrite values you still need. One simple approach is to use a temporary array, copy every element to its rotated position, then copy the temporary array back to nums. The **time complexity** will be O(n) because we traverse all the elements in the array, and the **space complexity** will be O(n) since we use an extra array that grows with the input size.

```cpp
vector<int> temp(n);
for (int i = 0; i < n; i++) {
    temp[(i + k) % n] = nums[i];
}
nums = temp;

```

But this is not in-place. Now think differently. Consider reversing:

nums = [1, 2, 3, 4, 5, 6, 7] → reverse → [7, 6, 5, 4, 3, 2, 1]

Divide into two parts: the first k elements [7, 6, 5] and the remaining elements [4, 3, 2, 1]. Reverse both parts: [5, 6, 7] and [1, 2, 3, 4]. Concatenate them to get [5, 6, 7, 1, 2, 3, 4], which is the desired output.

```cpp
void rotate(vector<int>& nums, int k) {
    int n = nums.size();
    k %= n;
    reverse(nums.begin(), nums.end());
    reverse(nums.begin(), nums.begin() + k);
    reverse(nums.begin() + k, nums.end());
}

```

This is the **in-place reverse** method.**Time complexity** is O(n) because we reverse n elements in total, and **space complexity** is O(1). This is an optimal approach for this problem.

**Edge Cases:**

- k = 0, No rotation happens.

- k = n, The array remains the same after rotation.

- k > n, Only rotate k % n times.

- n = 1, Single element array; rotation has no effect.

- Empty array, Nothing to rotate; handle carefully to avoid division by zero.

- Large k, Use modulo (k % n) to keep rotation efficient.
