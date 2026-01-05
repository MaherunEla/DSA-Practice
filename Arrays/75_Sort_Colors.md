# Sort Colors

Given an array `nums` with `n` objects colored red, white, or blue, sort them **in-place** so that objects of the same color are adjacent, with the colors in the order **red, white, and blue**.

We use the integers `0`, `1`, and `2` to represent the color red, white, and blue respectively.

You must solve this problem **without using the library's sort function**.

---

### Example 1

Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]

### Example 2

Input: nums = [2,0,1]
Output: [0,1,2]

---

This is a sorting problem. We could easily solve it using a built-in sort function, but the problem clearly states that we **must not use it**, so we need a different approach.

---

## Better Approach (Counting Method)

If we observe carefully, the array contains only **three values: 0, 1, and 2**.  
If we count how many times each value appears, we can simply overwrite the array in the correct order.

---

```cpp
int cnt0 = 0, cnt1 = 0, cnt2 = 0;

for (int i : nums) {
    if (i == 0) cnt0++;
    else if (i == 1) cnt1++;
    else cnt2++;
}

int j = 0;
while (cnt0--) {
    nums[j++] = 0;
}
while (cnt1--) {
    nums[j++] = 1;
}
while (cnt2--) {
    nums[j++] = 2;
}

```

**Time Complexity:** O(n)
**Space Complexity:** O(1)

This solution works efficiently but requires **two passes** over the array (one for counting and one for overwriting), so it is **not a one-pass solution.**

---

## Optimal Approach (Dutch National Flag)

If we observe carefully, the array contains only three values: `0`, `1`, and `2`.

Our goal is:

- all `0s` should come first

- all `1s` should stay in the middle

- all `2s` should go to the end

Instead of counting first and then rewriting the array, we can **sort the array in a single pass** by placing each element in its correct position while traversing the array.

For this, we use **three pointers:**

- `low` position where next `0` should go

- `mid` current index

- `high` position where next `2` should go

Initially:

- `low = 0`

- `mid = 0`

- `high = n - 1`

Now we iterate while `mid <= high` and check the value at `nums[mid]`.

- If `nums[mid] == 0`
  We swap it with `nums[low]`, then move both `low` and `mid` forward.

- If `nums[mid] == 1`
  It is already in the correct position, so we just move `mid` forward.

- If `nums[mid] == 2`
  We swap it with `nums[high]` and move `high` backward.
  We do **not** move `mid` here, because the swapped element still needs to be checked.

```cpp

int low = 0, mid = 0, high = nums.size() - 1;

while (mid <= high) {
    if (nums[mid] == 0) {
        swap(nums[low], nums[mid]);
        low++;
        mid++;
    }
    else if (nums[mid] == 1) {
        mid++;
    }
    else {
        swap(nums[mid], nums[high]);
        high--;
    }
}

```

**Time Complexity:** O(n)
**Space Complexity:** O(1)

This is the **optimal solution** because it sorts the array in **one pass** and does not use any extra space.
