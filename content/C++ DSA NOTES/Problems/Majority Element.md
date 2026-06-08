---
tags:
  - Moores-voting-algorithm
---
 #### Problem 
 We are given an array of size $n$ with some repeated elements, we have to find the majority i.e. element that appears more than $\lfloor n/2 \rfloor$ times.
#### Brute force approach 
We run a double loop, check the frequency of each elements one by one. If there is any element whose frequency exceeds $\lfloor n/2 \rfloor$, we return that element.
```cpp
int solution (vector<int> nums) {
    int n = nums.size(); //checking size of vector
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
Time complexity - $O(n^2)$
#### Better approach using Sorting
We first sort the array for example $\{1,1,1,1,2,2,3\}$, now we know that repeated elements occurs consecutively, now we run just one loop that checks the frequency of elements that are same in consecutive.
```cpp
int solution(vector<int> nums) {

    int n = nums.size();

    //sort
    sort(nums.begin(), nums.end()); //sort is defined in <algorithm> so include that in the header file

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
Time complexity - $O(n\log(n))$ 
#### Optimal approach using Moore's Voting Algorithm
The basic intuition behind Moore's Algorithm is that if there exists a majority element, say $a$, then no other element can have a frequency that exceeds the frequency of $a.$ 
How it actually works is that we run a single loop and check the frequency of each elements, if we encounter a same element then the frequency is increased by 1, if we encounter a different element the we decrease the frequency by 1. The frequency of $a$ is so high in the array that in the overall frequency count it will always win.
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
Time complexity - $O(n)$ 