### Solution 1: Stack

<!-- thinking:start -->

> **Thinking**
>
> The first idea is to keep stripping $()$, $[]$, and $\{\}$ until nothing changes. Correct, but worst-case $O(n^2)$. $n\le 10^4$ might pass, yet the writing is clumsy.
>
> Matching is last-opened-first-closed, so we need LIFO. A left bracket waits for its right counterpart; a right bracket must pair with the nearest unmatched left. The stack should be empty at the end, or some left bracket never closed.
>
> So we push left brackets and, on a right bracket, pop and compare.

<!-- thinking:end -->

Traverse the bracket string $s$. When encountering a left bracket, push the current left bracket into the stack; when encountering a right bracket, pop the top element of the stack (if the stack is empty, directly return `false`), and judge whether it matches. If it does not match, directly return `false`.

Alternatively, when encountering a left bracket, you can push the corresponding right bracket into the stack; when encountering a right bracket, pop the top element of the stack (if the stack is empty, directly return `false`), and judge whether they are equal. If they do not match, directly return `false`.

> The difference between the two methods is only the timing of bracket conversion, one is when pushing into the stack, and the other is when popping out of the stack.

At the end of the traversal, if the stack is empty, it means the bracket string is valid, return `true`; otherwise, return `false`.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the bracket string $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stk = []
        d = {'()', '[]', '{}'}
        for c in s:
            if c in '({[':
                stk.append(c)
            elif not stk or stk.pop() + c not in d:
                return False
        return not stk
```

#### Java

```java
class Solution {
    public boolean isValid(String s) {
        Deque<Character> stk = new ArrayDeque<>();
        for (char c : s.toCharArray()) {
            if (c == '(' || c == '{' || c == '[') {
                stk.push(c);
            } else if (stk.isEmpty() || !match(stk.pop(), c)) {
                return false;
            }
        }
        return stk.isEmpty();
    }

    private boolean match(char l, char r) {
        return (l == '(' && r == ')') || (l == '{' && r == '}') || (l == '[' && r == ']');
    }
}
```
