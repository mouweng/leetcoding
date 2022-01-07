# tlj

[leetcode主页](https://leetcode-cn.com/u/lechrond/)

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
            int b = c-1;
            for(int a=0;a<b;a++){                
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

### 2021-12-30

#### [846. 一手顺子](https://leetcode-cn.com/problems/hand-of-straights/)

```java
class Solution {
    public boolean isNStraightHand(int[] hand, int groupSize) {
        int n = hand.length;
        if(n%groupSize!=0){
            return false;
        }
        Map<Integer,Integer> count = new HashMap<>();
        for(int num:hand){
            count.put(num,count.getOrDefault(num,0)+1);
        }
        Arrays.sort(hand);
        for(int num:hand){
            if(count.get(num)==0){
                continue;
            }
            for(int i=num;i<num+groupSize;i++){
                if(count.getOrDefault(i,0)==0){
                    return false;
                }
                count.put(i,count.get(i)-1);
            }
        }
        return true;
    }
}
```

### 2021-12-31

#### [507. 完美数](https://leetcode-cn.com/problems/perfect-number/)

```java
class Solution {
    public boolean checkPerfectNumber(int num) {
        if(num<4){
            return false;
        }
        int sum = 1;
        for(int i=2;i*i<=num;i++){
            if(num%i==0){
                sum+=i;
                if(i*i<num){
                    sum+=num/i;
                }
            }
        }
        return sum==num;
    }
}
```

### 2022-1-1

#### [2022. 将一维数组转变成二维数组](https://leetcode-cn.com/problems/convert-1d-array-into-2d-array/)

```java
class Solution {
    public int[][] construct2DArray(int[] original, int m, int n) {
        if(original.length!=m*n){
            return new int[0][0];
        }
        int[][] year2022 = new int[m][n];
        int idx = 0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                year2022[i][j]=original[idx++];
            }
        }
        return year2022;
    }
}
```

### 2022-1-2

#### [390. 消除游戏](https://leetcode-cn.com/problems/elimination-game/)

```java
class Solution {
    public int lastRemaining(int n) {
        // an实际用不上，但是为了等差数列的代码可读性还是加了
        int a1=1,an=n;
        int cnt=n,step=1;
        boolean falg = true;
        while(cnt>1){
            // 从左往右
            if(falg){
                if(cnt%2==0){
                    a1 = a1+step;
                    an = an;
                }else{
                    a1 = a1+step;
                    an = an-step;
                }
            }
            // 从右往左
            else{
                if(cnt%2==0){
                    a1 = a1;
                    an = an-step;
                }else{
                    a1 = a1+step;
                    an = an-step;
                }
            }
            cnt/=2;
            step*=2;
            falg=!falg;
        }
        return a1;
    }
}
```

### 2022-1-3

#### [1185. 一周中的第几天](https://leetcode-cn.com/problems/day-of-the-week/)

```java
import java.util.Calendar;

class Solution {
    public String dayOfTheWeek(int day, int month, int year) {
        String[] weekdays = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};
        Calendar calendar = Calendar.getInstance();
        // 由于月份下标从0开始赋值所以月份要-1
        calendar.set(year, month - 1, day);
        // Calendar.DAY_OF_WEEK返回从Sunday开始的1-7
        int week_index = calendar.get(Calendar.DAY_OF_WEEK) - 1;
        return weekdays[week_index];
    }
}
```

## 2022-1-7

#### [1614. 括号的最大嵌套深度](https://leetcode-cn.com/problems/maximum-nesting-depth-of-the-parentheses/)

```java
class Solution {
    public int maxDepth(String s) {
        int ans = 0;
        LinkedList<Character> stack = new LinkedList<>();
        for(char c:s.toCharArray()){
            if(c=='('){
                stack.offer(c);
                ans = Math.max(ans,stack.size());
            }else if(c==')'){
                stack.pollLast();
            }
        }
        return ans;
    }
}
```

