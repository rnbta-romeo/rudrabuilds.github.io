#### Problem 
We are given an array with each element representing the height of the walls of a container. We have to find the combination of two elements that gives the maximum amount of water. 
#### Brute Force Approach
We calculate the area, taking the smaller of two elements as the length (because only the smaller one limits the water stored) and a unit width of 1 between two consecutive elements as breath. We run a double loop and check the largest possible area.
```cpp
int maxWater(int arr[]) {
	int size = 4;
	int maxW = INT_MIN;
	
	for (int i = 0; i < n; i++) {
		int currW = 0; 
		for (int j = i + 1; j < n; j++){
			int l = min(arr[i],arr[j]);
			int b = j - i;
			currW = l * b;
			
			if (currW > maxW) {
				maxW = currW;
			}
		}
	}
	return maxW;
}
```
Time complexity $\rightarrow O(n^2)$ 

#### Optimal approach using two pointers
Instead of running two loops, we can just run one that starts checking for the area from the start and end of the array.
```cpp
int maxWater(int arr[]) {
	int size = 4;
	int maxW = INT_MIN;
	int st = 0, end = n-1;
	
	while (end > st){
		int currW = 0;
		int l = min(arr[end],arr[st]);
		int b = end - start;
		int currW = l * b;
		
		if (currW > maxW) {
			maxW = currW;
		}
		if (arr[st] < arr[end]) {
			st++;
		} else {
			end--;
		}
	}
	return maxW;
}
```
Time complexity $\rightarrow O(n)$ 