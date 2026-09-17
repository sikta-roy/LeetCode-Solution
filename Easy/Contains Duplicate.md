Solution 1: Sorting
Sort the array, then check whether any two neighbors are equal.
#### Java

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        for(int i=0;i<nums.length-1;i++)
        {
            if(nums[i]==nums[i+1])
                return true;
        }
        return false;
        
    }
}
```
Solution 2: Hash Table
A value that is already in the set is a duplicate.
#### Java

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> a=new HashSet<>();
        for(int num:nums)
        {
            if(!a.add(num))
            {
                return true;
            }
        }
        return false;
        
    }
}
```
