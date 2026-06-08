---
tags:
  - Binary-Search
---
When we search for a word in dictionary, we don't look for it page by page. That's a long and hefty process. We instead open up a random page, and then relative to the first letter of the word we decide whether to search before the page we have opened or after that page. 

That's exactly how binary search works, instead of checking every element in the array to find the target, we first find the middle of the array and compare with the target. Note that binary search can only be applied in a sorted array. If the mid value in smaller that the target then we search in the left half of the array and mid value is greater than the target then we search in the right half of the array.

Binary search is more optimal than linear search because it significantly reduces the number of operations and performs the search with $O(log(n))$ time complexity.

**Note:** We find the middle using the formula $\frac{start + end}{2}$ but there's a catch. 
Both start and end indexes can take `INT_MAX` as their maximum value, and since we are adding two maximums, it will overflow from the int data type. We make a small tweak in the formula to fix this and we use $start + \frac{end - start}{2}$ instead. 

```cpp
int binarySearch (vector<int> nums, int target) {
    int n = nums.size();
    int st = 0, end = n-1;

    while(st <= end) { // search condition
        int mid = st + (end-st)/2; 

        if (nums[mid] > target) {
            end = mid -1; //searches in the first half
        }
        else if (nums[mid] < target) {
            st = mid +1; //searches in the second half
        } else {
            return mid;
        }
    }
}
```
