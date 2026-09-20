

## Solutions

<!-- solution:start -->

### Solution 1: Memoization Search

<!-- thinking:start -->

> **Thinking**
>
> Adjacent houses cannot both be robbed; maximize the take. Subset enumeration must enforce the gap, and overlapping suffixes repeat. From index $i$, rob and skip to $i+2$, or skip to $i+1$, and take the better. Memoization evaluates each index once.

<!-- thinking:end -->

We design a function $\textit{dfs}(i)$, which represents the maximum amount of money that can be stolen starting from the $i$-th house. Thus, the answer is $\textit{dfs}(0)$.

The execution process of the function $\textit{dfs}(i)$ is as follows:

- If $i \ge \textit{len}(\textit{nums})$, it means all houses have been considered, and we directly return $0$;
- Otherwise, consider stealing from the $i$-th house, then $\textit{dfs}(i) = \textit{nums}[i] + \textit{dfs}(i+2)$; if not stealing from the $i$-th house, then $\textit{dfs}(i) = \textit{dfs}(i+1)$.
- Return $\max(\textit{nums}[i] + \textit{dfs}(i+2), \textit{dfs}(i+1))$.

To avoid repeated calculations, we use memoization search. The result of $\textit{dfs}(i)$ is saved in an array or hash table. Before each calculation, we first check if it has been calculated. If so, we directly return the result.

The time complexity is $O(n)$, and the space complexity is $O(n)$, where $n$ is the length of the array.

<!-- tabs:start -->


#### Java

```java
class Solution {
    private Integer[] f;
    private int[] nums;

    public int rob(int[] nums) {
        this.nums = nums;
        f = new Integer[nums.length];
        return dfs(0);
    }

    private int dfs(int i) {
        if (i >= nums.length) {
            return 0;
        }
        if (f[i] == null) {
            f[i] = Math.max(nums[i] + dfs(i + 2), dfs(i + 1));
        }
        return f[i];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Dynamic Programming

<!-- thinking:start -->

> **Thinking**
>
> Solution 1 is recursive. $f[i]$ is the best total among the first $i$ houses: rob house $i$ and add $f[i-2]$, or take $f[i-1]$. Fill the table bottom-up.

<!-- thinking:end -->

We define $f[i]$ as the maximum total amount that can be robbed from the first $i$ houses, initially $f[0]=0$, $f[1]=nums[0]$.

Consider the case where $i \gt 1$, the $i$th house has two options:

- Do not rob the $i$th house, the total amount of robbery is $f[i-1]$;
- Rob the $i$th house, the total amount of robbery is $f[i-2]+nums[i-1]$;

Therefore, we can get the state transition equation:

$$
f[i]=
\begin{cases}
0, & i=0 \\
nums[0], & i=1 \\
\max(f[i-1],f[i-2]+nums[i-1]), & i \gt 1
\end{cases}
$$

The final answer is $f[n]$, where $n$ is the length of the array.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Where $n$ is the length of the array.

<!-- tabs:start -->



```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        int[] f = new int[n + 1];
        f[1] = nums[0];
        for (int i = 2; i <= n; ++i) {
            f[i] = Math.max(f[i - 1], f[i - 2] + nums[i - 1]);
        }
        return f[n];
    }
}
```



<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 3: Dynamic Programming (Space Optimization)

<!-- thinking:start -->

> **Thinking**
>
> $f[i]$ in Solution 2 depends only on the previous two values, so two rolling variables cut space to $O(1)$.

<!-- thinking:end -->

We notice that when $i \gt 2$, $f[i]$ is only related to $f[i-1]$ and $f[i-2]$. Therefore, we can use two variables instead of an array to reduce the space complexity to $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        f = g = 0
        for x in nums:
            f, g = max(f, g), f + x
        return max(f, g)
```

#### Java

```java
class Solution {
    public int rob(int[] nums) {
        int f = 0, g = 0;
        for (int x : nums) {
            int ff = Math.max(f, g);
            g = f + x;
            f = ff;
        }
        return Math.max(f, g);
    }
}
```



<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
