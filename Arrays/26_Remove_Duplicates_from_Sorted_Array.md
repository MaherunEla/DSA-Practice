# Remove Duplicates from Sorted Array

**Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same.**

**Consider the number of unique elements in nums to be k. After removing duplicates, return the number of unique elements k.**

**The first k elements of nums should contain the unique numbers in sorted order. The remaining elements beyond index k - 1 can be ignored.**

**Example:**
**Input:**
nums = [1,1,2]
**Output:**
2, nums = [1,2,_]

**Explanation:**
Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

This is a sorted array, so duplicate numbers will be one after another. I can easily find duplicates by comparing a number with its next indexed number.
**But this has a problem, on the last element, i+1 goes out of bounds, which can cause a runtime error.**
**Also, I will miss the first occurrence of a duplicate sequence because I only push if current != next.**

**Example with nums[i] != nums[i+1]:**
nums = [1,1,2,3,3]
i=0: nums[0]!=nums[1]? 1==1 → skip
i=1: nums[1]!=nums[2]? 1!=2 → push 1
i=2: nums[2]!=nums[3]? 2!=3 → push 2
i=3: nums[3]!=nums[4]? 3==3 → skip
i=4: nums[4]!=??? → out of bounds!

Here the first 1 is skipped, wrong result.

So **the idea is:** only push a number if it’s the first element OR it’s different from the previous one.
This works because the array is sorted, so duplicates appear consecutively.

We can store all unique numbers in a temporary vector and then copy them back into the given array.
**Logically, it produces the same result as in-place.** But **technically, it’s not considered in-place because it takes extra space and doesn’t meet the O(1) space requirement.**
In-place means **using only constant extra space, not growing with input size.**

For traversing each element through the loop, **time complexity will be O(n)**, and for using an extra vector, **space complexity will be O(n)** because the temp vector grows with n.

```cpp
vector<int> temp;
for (int i = 0; i < n; i++) {
    if (i == 0 || nums[i] != nums[i - 1])
        temp.push_back(nums[i]);
}

for(int i = 0; i < temp.size(); i++){
    nums[i] = temp[i];
}
return temp.size();

```

For the **in-place solution**, I have to compare and change nums values at the same time without using an extra vector.
In the brute force solution, the first value was always pushed to temp, so here I’ll leave the first value as it is.

Start from index 1 and check with its previous number.
If not equal, then place the value at index j and increase j accordingly.
This is called the **two-pointer approach**.

**Time complexity: O(n) we traverse each element once.**
**Space complexity: O(1) no extra array or vector growing with n.**

```cpp
int removeDuplicates(vector<int>& nums) {
    int n = nums.size();
    if (n == 0) return 0;

    int j = 1; // pointer for next unique position
    for (int i = 1; i < n; i++) {
        if (nums[i] != nums[i - 1]) {
            nums[j] = nums[i];
            j++;
        }
    }
    return j; // new length
}

```

Since this array is sorted, using **two pointers gives a time complexity of O(n).**
If the array is not sorted, we can still use two pointers but only after sorting, which makes the total **time complexity O(n log n).**
To reduce time complexity further, we can think of a different approach.

If **an array has duplicate elements and we need only unique elements, we usually use a set.**
Here, we can also use a hash set approach for unsorted arrays.

```cpp
unordered_set<int> st(nums.begin(), nums.end());
vector<int> unique(st.begin(), st.end());

```

**Time complexity: O(n) on average**
**Space complexity: O(n) for using the set, not in-place.**

So the best approach is still the **Two Pointer Approach.**

**Edge Cases:**

- Empty array, all elements are the same, all elements are unique

- Two elements both same, two elements different

- Duplicates at the beginning, duplicates at the end

- Large input (performance edge case)
