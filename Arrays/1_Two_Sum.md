# Two Sum Problem

**Given an array of integers nums and an integer target, return the indices of the two numbers such that they add up to the target. You can return the answer in any order.**

**Example:**
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

This means I have to choose a number from the array and sum it with the rest of the elements until I find the target. For this, I have to use two nested loops. Using nested loops, the **time complexity** in the worst case will be O(n²) because for each number it will loop over n elements. The **space complexity** is O(1) — constant extra space — no matter whether you use a vector to store the result or return the two indices directly, because the extra storage is always constant (2 integers) and does not grow with n.

```cpp
for(int i=0; i<nums.size(); i++){
for(int j=i+1; j<nums.size(); j++){
if(nums[i]+nums[j]==target){
result.push_back(i);
result.push_back(j);
return result;
}
}
}
return result;

```

But this **brute force approach** takes too much time, so I need to decrease its **time complexity** to O(n). If we think manually, to find the target we would choose one big number and one small number and sum them. If the sum is bigger than the target, we would choose a smaller big number than the previous one; if the sum is smaller than the target, we would choose a bigger small number. This is called the **two-pointer** approach in programming. But two pointers only work for **sorted arrays**, because if **sum < target or sum > target**, we need to know which pointer to move. In an unsorted array, we cannot predict the direction.

In this problem, the array is not sorted, so I have to sort it first. Sorting takes **O(n log n) time complexity**. Then I can use a while loop to scan until the two pointers cross or the answer is found. Scanning takes O(n), so the total **time complexity is O(n log n)**, and the **space complexity** is O(1). If the array is already sorted, the **time complexity** will be O(n).

```cpp
sort(nums.begin(), nums.end());
int left = 0, right = n-1;
while(left < right){
int sum = nums[left] + nums[right];
if(sum == target) return {left, right};
else if(sum < target) left++; // increase sum
else right--; // decrease sum
}

```

This solution is still not optimal. It can take more time. Now think mathematically: in school, we always solve problems like **x + 5 = 10**. What do we do to find x? We calculate **x = 10 - 5 = 5**. In this problem, we use the same idea but add a trick: subtract each number from the target and check if the result is present in the array. If it is present, then both numbers sum to the target.

To check if a number exists in the array efficiently, I will use a **hashmap/unordered_map**. There is no need to sort the array, and we only traverse each element once. The **time complexity** will be O(n) and **space complexity** will be O(n) (for the hashmap).

```cpp
unordered_map<int,int> mp;
for(int i=0; i<n; i++){
if(mp.find(target-arr[i]) != mp.end())
return {mp[target-arr[i]], i};
mp[arr[i]] = i;
}

```

**Worst case:** O(n) amortized — unordered_map can have collisions, but lookups are generally considered O(1).
