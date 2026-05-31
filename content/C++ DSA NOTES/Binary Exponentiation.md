#### Problem
To find the answer of $x^n$.

#### Brute force approach 
We run a simple loop and multiple the number by itself n time.
```cpp
int (int x, int n) {
	int ans = 1;
	for (int i = 0; i < n; i++) {
		ans *= x;
	}
	return ans;
}
```
But the time complexity of this is $O(n)$ which is not optimal for higher n values. 

#### Optimal Approach using Binary Exponentiation
Before the code, lets understand how can we calculate, say $2^5$, without multiplying $2$ five times. We know that the binary form of $5$ is $101$, also observe that-
	$2^5$ = $2^1 \times 2^0 \times 2^4$ 
See the powers, we are running through the squares of the number and only using it in the answer when the binary is $1.$ ==Note that the sequence of the binary form is to be taken right to left and not left to right for this algorithm.== 
Here is a better view at it -

| Binary | Squares | Use |
| ------ | ------- | --- |
| $1$    | $2^1$   | Yes |
| $0$    | $2^2$   | No  |
| $1$    | $2^4$   | Yes |

Final answer becomes $\rightarrow$ $2^5 = 2^1 \times 2^4$ 

> [!summary] Fact
> If there is a number $n$ in decimal form, then its binary form can have at most $log(n) + 1$ digits.

Let us take another example, say $3^{10}$, we know that the binary for of $10$ is $1010$.

| Binary | Squares | Use |
| ------ | ------- | --- |
| $0$    | $3$     | No  |
| $1$    | $3^2$   | Yes |
| $0$    | $3^4$   | No  |
| $1$    | $3^8$   | Yes |

The answer becomes $\rightarrow$ $3^{10} = 3^2 \times 3^8$ 
But what do we calculate $x^{-n}$ when $n$ becomes negative? Its simple because $x^{-n}$ is just $\left(\dfrac{1}{x}\right)^n$  
So finally the code for binary exponentiation is -
```cpp
double pow (double x, int n) {
	
	double ans = 1;
	
	if (n < 0) {
		x = 1/x;
		n = -n;
	}
	
	while(n > 0) {
		if (n % 2 == 1) {
			ans *= x;
		}
		x *= x;
	}
	return ans;
}
```
