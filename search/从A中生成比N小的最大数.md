# 从A中生成比N小的最大数

- [leetcode讨论](https://leetcode.cn/circle/discuss/fbhhev/view/m0Tt1l/)

```
数组A中给定可以使用的1~9的数，返回由A数组中的元素组成的小于n的最大数。
例如x=2533，A=[1，2,4,9]返回2499
例如N=4412345，A=[2,4,9]
例如N=99999，A=[2,4,9]
例如N=23132，A=[2,4,9]
例如N=1111, A=[2,4,9]
```

 这道题边界条件很多

## 方法一：遍历法

```java
import java.util.*;

class Main {
    public static void main(String[] args) {
        String s = "1111";// 4412345\99999\23132
        int[] nums = new int[]{1,2,4,9};
        System.out.println(findMaximunNum(s, nums));
    }

    public static String findMaximunNum(String s, int[] nums) {
        Arrays.sort(nums);
        StringBuilder sb = new StringBuilder();
        boolean flag = false;
        int prel = -1;
        for (int i = 0; i < s.length(); i ++) {
            int target = s.charAt(i) - '0';
            if (!flag) {
                int l = bs(nums, target);
                System.out.println(l);
                if (nums[l] > target) {
                    if (prel > 0) {
                        sb.deleteCharAt(sb.length() - 1);
                        sb.append(nums[prel - 1]);
                    } else {
                        i ++;
                    }
                    for (int j = s.length() - i; j > 0; j --) sb.append(nums[nums.length - 1]);
                    return sb.toString();
                } 
                if (nums[l] < target) flag = true;
                if (i == s.length() - 1 && nums[l] == target && l == nums.length - 1) {
                     l --;
                }
                sb.append(nums[l]);
                prel = l;
            } else {
                sb.append(nums[nums.length - 1]);
            }
        }
        return sb.toString();
    } 

    public static int bs(int[] nums, int target) {
        int l = 0, r = nums.length - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] <= target) l = mid;
            else r = mid - 1;
        }
        return l;
    }

}
```

##  方法二：回溯法

```java
import java.util.Arrays;

public class Solution {

    /**
     * 数组A中给定可以使用的1~9的数，返回由A数组中的元素组成的小于n的最大数。
     * 例如A={1, 2, 4, 9}，x=2533，返回2499
     *
     * @param args
     */
    public static void main(String[] args) {
        Solution solution = new Solution();
        // String s = "23132"; // 4412345\99999\23132
        int ans = solution.getMaxAns(1111, new Integer[]{1, 2, 4, 9}); //
        System.out.println(ans);
    }

    public int getMaxAns(int n, Integer[] candidate) {
        Arrays.sort(candidate, ((a, b) -> b - a));
        String s = String.valueOf(n);
        int len = s.length();
        // 兜底
        int backup = 0;
        for (int i = 0; i < len - 1; i++) {
            backup *= 10;
            backup += candidate[0];
        }
        StringBuilder ans = new StringBuilder();
        boolean res = dfs(s, ans, candidate, false);
        return res ? Integer.parseInt(ans.toString()) : backup;
    }

    /**
     *
     * @param s n的字符串形式
     * @param sb 答案
     * @param candidate 候选集
     * @param less sb中是否已经有前面的数比n的高位数小了
     */
    private boolean dfs(String s, StringBuilder sb, Integer[] candidate, boolean less) {
        if (sb.length() == s.length()) {
            // 如果能到这里，说明已经找到了符合条件的结果
            return true;
        }
        // 当前的下标
        int index = sb.length();
        for (Integer integer : candidate) {
            char c = s.charAt(index);
            char cur = (char) (integer + '0');
            // 和n当前位一样大
            if (c == cur && sb.length() < s.length() - 1 || less) {
                sb.append(cur);
                boolean res = dfs(s, sb, candidate, less);
                if (res) {
                    return true;
                }
                sb.deleteCharAt(sb.length() - 1);
            } else if (c > cur) { // 从这里开始比n的当前位小，后续直接取最大
                sb.append(cur);
                dfs(s, sb, candidate, true);
                return true;
            }
        }
        return false;
    }

}
```

