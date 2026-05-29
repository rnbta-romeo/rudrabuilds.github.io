---
tags:
  - Moores-voting-algorithm
---
#### Problem 
#### Brute force approach 
```cpp
int solution (vector<int> nums) {
    int n = nums.size();
    for(int val : nums) {
        int freq = 0;
        for (int el : nums) {
            if(el == val) {
                freq++;
            }
        }
        if (freq > n/2) {
            return val;
        }
    }
    return -1;
}
```
#### Better approach
```cpp
int solution(vector<int> nums) {

    int n = nums.size();

    //sort
    sort(nums.begin(), nums.end()); //sort is defined in <algorithm>

    //freq count
    int freq = 1, ans = nums[0];
    for (int i=1; i < n; i++) {
        if(nums[i] == nums[i-1]) {
            freq++;
        } else {
            freq = 1;
            ans = nums[i];
        }
        if(freq > n/2) {
            return ans;
        }
    }
    return ans;
}
```
#### Optimal approach using Moore's Voting Algorithm
```cpp
int solution(vector<int> nums) {

    int n = nums.size();
    int freq = 0, ans = 0;

    for(int i=0; i<n; i++) {
        if(freq == 0) {
            ans = nums[i];
        }
        if(ans == nums[i]) {
            freq++;
        } else{
            freq--;
        }
    }
    return ans;
}
```
