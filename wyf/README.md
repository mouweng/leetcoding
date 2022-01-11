# 👨🏻‍💻Mouweng给我冲！
📟[[LEETCODE主页](https://leetcode-cn.com/profile/)]

- 🔖1月份目标：LEETCODE数量达到350题

### 2021-12-29
#### [1995. 统计特殊四元组](https://leetcode-cn.com/problems/count-special-quadruplets/)
```java
class Solution {
    public int countQuadruplets(int[] nums) {
        int n = nums.length;
        int ans = 0;
        Map<Integer, Integer> cnt = new HashMap<Integer, Integer>();
        for (int b = n - 3; b >= 1; --b) {
            for (int d = b + 2; d < n; ++d) {
                cnt.put(nums[d] - nums[b + 1], cnt.getOrDefault(nums[d] - nums[b + 1], 0) + 1);
            }
            for (int a = 0; a < b; ++a) {
                ans += cnt.getOrDefault(nums[a] + nums[b], 0);
            }
        }
        return ans;
    }
}
```

### 2021-12-29
#### [846. 一手顺子](https://leetcode-cn.com/problems/hand-of-straights/)
```java
class Solution {
    public boolean isNStraightHand(int[] hand, int groupSize) {
        int len = hand.length;
        if (len % groupSize != 0) {
            return false;
        }
        Arrays.sort(hand);
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int x : hand) {
            map.put(x, map.getOrDefault(x, 0) + 1);
        }
        for (int x : hand) {
            if (!map.containsKey(x)) {
                continue;
            }
            for (int j = 0; j < groupSize; j++) {
                int num = x + j;
                if (!map.containsKey(num)) {
                    return false;
                }
                map.put(num, map.get(num) - 1);
                if (map.get(num) == 0) {
                    map.remove(num);
                }
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
        if (num <= 4) return false;
        int cnt = 1;
        for (int i = 2; i <= num / 2; i ++) {
            if (num % i == 0) {
                cnt += i;
            }
        }
        return num == cnt ? true : false;
    }
}
```

### 2022-01-01
> 新年快乐🎉🎊🎈
#### [2022. 将一维数组转变成二维数组](https://leetcode-cn.com/problems/convert-1d-array-into-2d-array/)
```java
class Solution {
    public int[][] construct2DArray(int[] original, int m, int n) {
        int[][] res = new int[m][n];
        int len = original.length;
        if (len != m * n) return new int[][]{};
        for (int i = 0; i < m; i ++) {
            for (int j = 0; j < n; j ++) {
                res[i][j] = original[i * n + j];
            }
        }
        return res;
    }
}
```

### 2022-01-03
#### [1185. 一周中的第几天](https://leetcode-cn.com/problems/day-of-the-week/)
- 1970年1月1号是星期四
- 闰年的定义是：年份不是100的整数且是4的整数，或者年份是400的整数
```java
class Solution {
    public String dayOfTheWeek(int day, int month, int year) {
        String[] WeekDay = {"Monday", "Tuesday", "Wednesday", "Thursday","Friday", "Saturday", "Sunday"};
        int[] Monthday = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        // 计算过去的年有多少天
        int days = 365 * (year - 1971) + (year - 1969) / 4;
        // 计算过去的月有多少天
        for (int i = 0; i < month - 1; i ++) {
            days += Monthday[i];
        }
        // 计算今年是否事闰年
        if (month >= 3 && (year % 400 == 0 || (year % 4 == 0 && year % 100 != 0))) {
            days ++;
        }
        days += day;
        return WeekDay[(days + 3) % 7];
    }
}
```

### 2022-01-04
#### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)
- 递归解法
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode p = head.next;
        ListNode q = reverseList(p);
        head.next = null;
        p.next = head;
        return q;
    }
}
```

### 2022-01-05
#### [1576. 替换所有的问号](https://leetcode-cn.com/problems/replace-all-s-to-avoid-consecutive-repeating-characters/)
```java
class Solution {
    public String modifyString(String s) {
        s = "#" + s + "#";
        StringBuilder res = new StringBuilder();
        for (int i = 0; i < s.length(); i ++) {
            if (s.charAt(i) == '?') {
                // 替换, 从a到c中选一个
                for (int j = 0; j < 3; j ++) {
                    char c = (char)('a' + j);
                    if (c != res.charAt(res.length() - 1) && c != s.charAt(i + 1)){
                        res.append(c);
                        break;
                    }
                }
            } else {
                res.append(s.charAt(i));
            }
        }
        return res.substring(1, res.length() - 1);
    }
}
```

#### [146. LRU 缓存](https://leetcode-cn.com/problems/lru-cache/)
- 哈希+双向链表实现
```java
class LRUCache {
    
    class ListNode {
        int key,value;
        ListNode prev, next;
        ListNode(int k, int v) {
            this.key = k;
            this.value = v;
        }
    }

    ListNode head, tail;
    Map<Integer, ListNode> m;
    int size, cap;

    public LRUCache(int capacity) {
        cap = capacity;
        size = 0;
        head = new ListNode(-1, -1);
        tail = new ListNode(-1, -1);
        head.next = tail;
        tail.prev = head;
        m = new HashMap<Integer, ListNode>();
    }
    
    public int get(int key) {
        if (!m.containsKey(key)) return -1;
        else {
            // 删除 
            ListNode n = m.get(key);
            delNode(n);
            addHead(n);
            return n.value;
        }
    }
    
    public void put(int key, int value) {
        // key是否存在
        if (!m.containsKey(key)) {
            // 插入到头部
            ListNode nn = new ListNode(key, value);
            addHead(nn);
            m.put(key, nn);
            if (size == cap) {
                // 删除尾部
                m.remove(tail.prev.key);
                delNode(tail.prev);
            } else {
                size ++;
            }
        } else {
            ListNode n = m.get(key);
            n.value = value;
            delNode(n);
            addHead(n);
        }
    }

    public void addHead(ListNode node) {
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
        node.prev = head;
    }
    public void delNode(ListNode node) {
        ListNode next = node.next;
        ListNode prev = node.prev;
        prev.next = next;
        next.prev = prev;
    }
}
```

### 2022-01-06
#### [71. 简化路径](https://leetcode-cn.com/problems/simplify-path/)
```java
class Solution {
    public String simplifyPath(String path) {
        String[] strs = path.split("/");
        List<String> res = new ArrayList<>();
        for (String s : strs) {
            if (s.equals("") || s.equals(".")) continue;
            else if (s.equals("..")) {
                if (!res.isEmpty()) res.remove(res.size() - 1);
            }
            else res.add(s);
        }
        if (res.size() == 0) return "/";
        StringBuilder sb = new StringBuilder();
        for (String s : res) {
            sb.append("/");
            sb.append(s);
        }
        return sb.toString();
    }
}
```

#### [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // 滑动窗口实现
        Map<Character, Integer> m = new HashMap<>();
        int i = 0, res = 0;
        for (int j = 0; j < s.length(); j ++) {
            char c = s.charAt(j);
            if (m.containsKey(c) && m.get(c) >= i) {
                i = m.get(c) + 1;
            }
            m.put(c, j);
            res = Math.max(res, j - i + 1);
        }
        return res;
    }
}
```

### 2022-01-07
#### [1614. 括号的最大嵌套深度](https://leetcode-cn.com/problems/maximum-nesting-depth-of-the-parentheses/) 
```java
class Solution {
    public int maxDepth(String s) {
        int depth = 0, res = 0;
        for (int i = 0; i < s.length(); i ++) {
            if (s.charAt(i) == '(') {
                depth ++;
                res = Math.max(res, depth);
            }
            else if (s.charAt(i) == ')') depth --;
            else continue;
        }
        return res;
    }
}
```
#### [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

- 优先队列
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>((a, b)->{return a - b;});
        for (int i : nums) {
            pq.offer(i);
            if (pq.size() > k) pq.poll();
        }
        return pq.poll();
    }
}
```

- 快速/归并排序
```java
class Solution {
    void quickSort(int[] nums, int l, int r) {
        if (l >= r) return;

        int q = nums[l], i = l, j = r;
        while (i < j) {
            while (i < j && nums[j] >= q) j --;
            nums[i] = nums[j];
            while (i < j && nums[i] <= q) i ++;
            nums[j] = nums[i];
        }
        nums[i] = q;
        quickSort(nums, l, i - 1);
        quickSort(nums, i + 1, r); 
    }

    public int findKthLargest(int[] nums, int k) {
        quickSort(nums, 0, nums.length - 1);
        return nums[nums.length - k];
    }
}
```

### 2022-01-10
#### [306. 累加数](https://leetcode-cn.com/problems/additive-number/)
```java
class Solution {
    public boolean isAdditiveNumber(String num) {
        int n = num.length();
        char[] numsChar = num.toCharArray();
        for (int i = 0 ; i< n-1;i++){
            if (numsChar[0] == '0' && i > 0)return false;
            for (int j = i+1;j< n-1;j++){
                if (numsChar[i+1] == '0' && j > i+1)continue;
                Long pre = Long.parseLong(num.substring(0,i+1));
                Long cur = Long.parseLong(num.substring(i+1,j+1));
                if (dfs(numsChar,pre,cur,num,n,j+1))return true;
            }
        }
        return false;
    }
    public boolean dfs (char[] numsChar,Long pre,Long cur ,String num,int n,int now){
        if (now == n)return true;
        for (int i = now;i<n;i++){
            if (numsChar[now] == '0' && i > now)return false;
            Long next = Long.parseLong(num.substring(now,i+1));
            if (next > pre + cur)return false;
            if (next == pre + cur){
                if (dfs(numsChar,cur,next,num,n,i+1))
                    return true;
                break;
            }
        }
        return false;
    }
}
```
#### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        int len = nums.length;
        if (len < 3) return res; 
        Arrays.sort(nums);
        for (int i = 0; i < len - 2 && nums[i] <= 0; i ++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            int j = i + 1, k = len - 1;
            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                if (sum < 0) {
                    j ++;
                } else if (sum > 0) {
                    k --;
                } else {
                    List<Integer> temp = new ArrayList<>();
                    temp.add(nums[i]);temp.add(nums[j]);temp.add(nums[k]);
                    res.add(temp);
                    j ++;k --;
                    while (j < len&&nums[j] == nums[j - 1]) j ++;
                    while (k > i&& nums[k] == nums[k + 1]) k --;
                }
            }  
        }
        return res;
    }
}
```

### 2022-01-11
#### [53. 最大子数组和](https://leetcode-cn.com/problems/maximum-subarray/)
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int len = nums.length, max = nums[0];
        int dp = nums[0];
        for (int i = 1; i < len; i ++) {
            dp = Math.max(nums[i] + dp, nums[i]);
            max = Math.max(dp, max);
        }
        return max;
    }
}
```