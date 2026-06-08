---
tags:
  - Two-pointer
---
#### Problem 
Given a target $n$, we have to find two elements in an sorted array such that its sum add up to $n$.

#### Brute force approach
We run a double loop, check the sum of every elements and if the sum equals the target we return the index of the elements that adds up to $n.$ 
```cpp
vector<int> pairSum1(vector<int> nums, int target ) {
	vector<int> ans;
	int n = nums.size();

    for(int i = 0; i < n; i++) {
        for(int j = i+1; j < n; j++) {
            if (nums[i] + nums[j] == target) {
                ans.push_back(i);
                ans.push_back(j);
                return ans;
            }
        }
    }
    return ans;
}
```
Time complexity - $O(n^2)$
#### Better approach
A better approach is to use a two pointer at the start and the end of the array and check its sum. There can be three possible cases after that - 
1) sum $<$ target $\Rightarrow$ increase the pointer at the start by $1$.
2) sum $>$ target $\Rightarrow$ decrease the pointer at the start by $1$.
3) sum $=$ target $\Rightarrow$ repeat the above cases until this is case is reached.
```cpp
vector<int> pairSum2(vector<int> nums, int target) {

    vector<int> ans;
    int n = nums.size();
    int i = 0, j = n-1; //i is the starting pointer and j is the endind pointer

    while (i < j) {
        int pairSum = nums[i] + nums[j]; 
        if(pairSum > target) {
            j--;
        } else if(pairSum < target) {
            i++;
        } else {
            ans.push_back(i);
            ans.push_back(j);
            return ans;
        }
    }
    return ans;
}
```
Time complexity - $O(n)$
