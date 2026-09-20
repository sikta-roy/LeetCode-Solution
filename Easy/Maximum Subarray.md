### Solution 1: Dynamic Programming

<!-- thinking:start -->

> **Thinking**
>
> The first idea is to enumerate every pair of endpoints and sum, in $O(n^2)$ or $O(n^3)$. $n \le 10^5$ will time out.
>
> The waste is recomputing overlapping subarrays. With the right endpoint fixed at $i$, the best left end either continues the previous segment (if its sum is positive) or starts over at $i$.
>
> So let $f[i]$ be the best sum ending at $i$. It depends only on $f[i-1]$, so one rolling variable suffices; the answer is the max along the way.

<!-- thinking:end -->

We define $f[i]$ to represent the maximum sum of a contiguous subarray ending at element $\textit{nums}[i]$. Initially, $f[0] = \textit{nums}[0]$. The final answer we seek is $\max_{0 \leq i < n} f[i]$.

Consider $f[i]$ for $i \geq 1$. Its state transition equation is:

$$
f[i] = \max(f[i - 1] + \textit{nums}[i], \textit{nums}[i])
$$

That is:

$$
f[i] = \max(f[i - 1], 0) + \textit{nums}[i]
$$

Since $f[i]$ is only related to $f[i - 1]$, we can use a single variable $f$ to maintain the current value of $f[i]$ and perform the state transition. The answer is $\max_{0 \leq i < n} f$.

The time complexity is $O(n)$, where $n$ is the length of the array $\textit{nums}$. The space complexity is $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        ans = f = nums[0]
        for x in nums[1:]:
            f = max(f, 0) + x
            ans = max(ans, f)
        return ans
```

#### Java

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int ans = nums[0];
        for (int i = 1, f = nums[0]; i < nums.length; ++i) {
            f = Math.max(f, 0) + nums[i];
            ans = Math.max(ans, f);
        }
        return ans;
    }
}
```

### Solution 2: Kadane's Algorithm

#### Java

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int cur = nums[0], best = nums[0];
        for (int i = 1; i < nums.length; i++) {
            cur = Math.max(nums[i], cur + nums[i]);
            best = Math.max(best, cur);
        }
        return best;
    }
}
```

### Solution 3: Divide and Conquer

<!-- thinking:start -->

> **Thinking**
>
> Solution 1 is already $O(n)$ time and $O(1)$ space. The follow-up asks for divide and conquer, but DP's recurrence is linear in the right endpoint and does not split into disjoint halves.
>
> What it lacks is another observation: the best subarray lies entirely on the left, entirely on the right, or crosses the midpoint. Crossing means a max suffix on the left plus a max prefix on the right. Time becomes $O(n \log n)$; we use it for the follow-up, not for speed.

<!-- thinking:end -->

Split the array at the midpoint. The answer is the max of the left half, the right half, and the best subarray that crosses the midpoint (max suffix of the left plus max prefix of the right).

The time complexity is $O(n \log n)$ and the space complexity is $O(\log n)$, where $n$ is the length of the array.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        def crossMaxSub(nums, left, mid, right):
            lsum = rsum = 0
            lmx = rmx = -inf
            for i in range(mid, left - 1, -1):
                lsum += nums[i]
                lmx = max(lmx, lsum)
            for i in range(mid + 1, right + 1):
                rsum += nums[i]
                rmx = max(rmx, rsum)
            return lmx + rmx

        def maxSub(nums, left, right):
            if left == right:
                return nums[left]
            mid = (left + right) >> 1
            lsum = maxSub(nums, left, mid)
            rsum = maxSub(nums, mid + 1, right)
            csum = crossMaxSub(nums, left, mid, right)
            return max(lsum, rsum, csum)

        left, right = 0, len(nums) - 1
        return maxSub(nums, left, right)
```

#### Java

```java
class Solution {
    public int maxSubArray(int[] nums) {
        return maxSub(nums, 0, nums.length - 1);
    }

    private int maxSub(int[] nums, int left, int right) {
        if (left == right) {
            return nums[left];
        }
        int mid = (left + right) >>> 1;
        int lsum = maxSub(nums, left, mid);
        int rsum = maxSub(nums, mid + 1, right);
        return Math.max(Math.max(lsum, rsum), crossMaxSub(nums, left, mid, right));
    }

    private int crossMaxSub(int[] nums, int left, int mid, int right) {
        int lsum = 0, rsum = 0;
        int lmx = Integer.MIN_VALUE, rmx = Integer.MIN_VALUE;
        for (int i = mid; i >= left; --i) {
            lsum += nums[i];
            lmx = Math.max(lmx, lsum);
        }
        for (int i = mid + 1; i <= right; ++i) {
            rsum += nums[i];
            rmx = Math.max(rmx, rsum);
        }
        return lmx + rmx;
    }
}
```

