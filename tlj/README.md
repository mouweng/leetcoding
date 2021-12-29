# tlj

### 2021-12-29

[1995. 统计特殊四元组](https://leetcode-cn.com/problems/count-special-quadruplets/)

```java
class Solution {
    public int countQuadruplets(int[] nums) {
        Map<Integer,Integer> count = new HashMap<>();
        int ans = 0;
        int n = nums.length;
        // nums[a]+nums[b]=nums[d]-nums[c]
        for(int c=2;c<n-1;c++){
            for(int a=0;a<c-1;a++){
                int b = c-1;
                count.put(nums[a]+nums[b],count.getOrDefault(nums[a]+nums[b],0)+1);
            }
            for(int d=c+1;d<n;d++){
                ans += count.getOrDefault(nums[d]-nums[c],0);
            }
        }
        return ans;
    }
}
```