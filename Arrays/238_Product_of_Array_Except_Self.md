# Product of Array Except Self

## Type: Array, Prefix Sum

**Given an integer array `nums`, return an array answer such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.**
**The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.**

**You must write an algorithm that runs in O(n) time and without using the division operation.**

---

### Example 1:

Input: nums = [1,2,3,4]

Output: [24,12,8,6]

### Example 2:

Input: nums = [-1,1,0,-3,3]

Output: [0,0,9,0,0]

---

After reading this problem, the first solution that comes to mind is to **calculate the product of all elements** and then divide it by `nums[i] `for each index.
But the problem clearly states that division is not allowed, so this approach is invalid.

Another idea is to calculate the product of all elements except the current index by using two nested loops.

```cpp
 int n = nums.size();
        vector<int> ans(n,1);

        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                if(i!=j) ans[i]*=nums[j];
            }

        }
        return ans;
```

**Time Complexity:** O(n²)
**Space Complexity:** O(1) (excluding output array)

This is a **brute-force approach** and works logically, but it is too slow for large inputs. The problem clearly states that the algorithm should run in O(n) time, so **this approach does not satisfy the given constraints.**

**Optimized Approach (Prefix & Suffix Product)**

If we observe carefully, for every index i, we need:

- the product of elements before index i (**prefix**)
- the product of elements after index i (**suffix**)

Instead of recomputing these repeatedly, we can compute them separately using two loops.

**Step 1: Prefix Product**

```cpp
int n = nums.size();
vector<int> ans(n, 1);

int prefix = 1;
for (int i = 0; i < n; i++) {
    ans[i] = prefix;
    prefix *= nums[i];
}

```

At this point, `ans[i]` contains the product of all elements to the left of i.

**Step 2: Suffix Product**

```cpp
int suffix = 1;
for (int i = n - 1; i >= 0; i--) {
    ans[i] *= suffix;
    suffix *= nums[i];
}
return ans;

```

Now, each `ans[i]` is multiplied by the product of all elements to the right of i.

Final Complexity Analysis

**Time Complexity:** O(n)
**Space Complexity:** O(1) (output array not counted as extra space)
