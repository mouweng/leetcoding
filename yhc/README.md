# yhc

## 2021.12.30
[846. 一手顺子](https://leetcode-cn.com/problems/hand-of-straights/)
```java
class Solution {
    public boolean isNStraightHand(int[] hand, int groupSize) {
        Arrays.sort(hand);
        int n = hand.length;
        if (n % groupSize != 0) return false;
        Map<Integer, Integer> mp = new HashMap<>();
        for (int d : hand) {
            if (mp.containsKey(d)) mp.put(d, mp.get(d) + 1);
            else mp.put(d, 1);
        }
        for (int h : hand) {
            if (mp.get(h) == 0) continue;
            int minVal = mp.get(h);
            for (int i = 1; i < groupSize; i++) {
                if (!mp.containsKey(h + i) || mp.get(h) == 0) return false;
                minVal = Math.min(mp.get(h + i), minVal);
            }
            for (int i = 0; i < groupSize; i++) {
                mp.put(h + i, mp.get(h + i) - minVal);
            }
        }
        return true;
    }
}
```
## 2021.12.31
[2113. Elements in Array After Removing and Replacing Elements](https://leetcode-cn.com/problems/elements-in-array-after-removing-and-replacing-elements/)
```java
class Solution {
    public int[] elementInNums(int[] nums, int[][] queries) {
        int n = nums.length;
        int nn = 2 * n;
        int[][] m = new int[nn][n];
        for (int i = 0; i < nn; i++) Arrays.fill(m[i], -1);
        for (int i  = 0; i < n; i++) for (int j = i; j < n; j++) m[i][j - i] = nums[j];
        for (int i = n + 1; i < nn; i++)  for (int j = 0; j < i - n; j++) m[i][j] = nums[j];
        int ansLen = queries.length;
        int[] ans = new int[ansLen];
        for (int i = 0; i < ansLen; i++) {
            int ind = queries[i][0] % nn;
            ans[i] = m[ind][queries[i][1]];
        }
        return ans;
    }
}
```
## 2022.1.1
[239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)
```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        Deque<Integer> win = new ArrayDeque<>();
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            while (!win.isEmpty() && nums[i] >= nums[win.getLast()]) {
                win.pollLast();
            }
            if (!win.isEmpty() && i - win.getFirst() + 1 > k) {
                win.pollFirst();
            }
            win.addLast(i);
            if (i >= k - 1) {
                ans.add(win.getFirst());
            }
        }
        return ans.stream().mapToInt(t -> nums[t]).toArray();
    }
}
```
## 2022.1.2
[390. 消除游戏](https://leetcode-cn.com/problems/elimination-game/)
```java
class Solution {
    public int lastRemaining(int n) {
        int step = 1;
        int first = 1;
        boolean flag = true;
        while (n > 1) {
            if (flag) {
                first += step;
            } else {
                first = first + (n % 2 == 0 ? 0 : step);
            }
            step *= 2;
            flag = !flag;
            n = n / 2;
        }
        return first;
    }
}
```
## 2022.1.3-2022.1.4
**唯唯诺诺2天啦，啥也不会。。。**
## 2022.1.5
[1576. 替换所有的问号](https://leetcode-cn.com/problems/replace-all-s-to-avoid-consecutive-repeating-characters/)
```java
class Solution {
    public String modifyString(String s) {
        StringBuilder sb = new StringBuilder(s);
        for (int i = 0; i < sb.length(); i++) {
            if (sb.charAt(i) != '?') continue;
            char left = '0', right = '0';
            if (i - 1 >= 0) {
                left = sb.charAt(i - 1);
            } 
            if (i + 1 < sb.length()) {
                right = sb.charAt(i + 1);
            }
            for (int j = 0; j < 26; j++) {
                char newVal = (char)(j + 'a');
                if (newVal != left && newVal != right) {
                    sb.setCharAt(i, newVal);
                    break;
                }
            }
        }
        return sb.toString();
    }
}
```
[146. LRU 缓存](https://leetcode-cn.com/problems/lru-cache/)
```java
class LRUCache {
    class Node {
        Node pre, next;
        int key;
        int val;
        Node(int key, int val) {
            this.key = key;
            this.val = val;
        }
    }
    int size = 0;
    int capacity = 0;
    Node head, tail;
    Map<Integer, Node> mp = new HashMap<>();
    
    public LRUCache(int capacity) {
        this.size = 0;
        this.capacity = capacity;
    }

    private void addNode(Node p) {
        if (tail == null) {
            head = p;
            tail = p;
        } else {
            p.pre = tail;
            tail.next = p;
            tail = p;
        }
    }

    private void removeHead() {
        if (head == null) return;
        Node next = head.next;
        if (next == null) {
            head = null;
            tail = null;
        } else {
            head.next = null;
            next.pre = null;
            head = next;
        }
    }

    private void removeTail() {
        if (tail == null) return;
        Node pre = tail.pre;
        if (pre == null) {
            head = null;
            tail = null;
        } else {
            pre.next = null;
            tail.pre = null;
            tail = pre;
        }
    }

    private void removeNode(Node p) {
        if (p == head) {
            removeHead();
        } else if (p == tail) {
            removeTail();
        } else {
            p.pre.next = p.next;
            if (p.next != null) {
                p.next.pre = p.pre;
            }
            p.pre = null;
            p.next = null;
        }
    }

    private void moveToTail(Node p) {
        removeNode(p);
        addNode(p);
    }
    
    public int get(int key) {
        if (!mp.containsKey(key)) return -1;
        Node p = mp.get(key);
        moveToTail(p);
        return p.val;
    }
    
    public void put(int key, int value) {
        if (mp.containsKey(key)) {
            get(key);
            Node p = mp.get(key);
            p.val = value;
            return;
        }
        Node p = new Node(key, value);
        if (size < capacity) {
            addNode(p);
            size++;
        } else {
            if (head == null) return;
            if (head != null) {
                mp.remove(head.key);
            }
            removeHead();
            addNode(p);
        }
        mp.put(key, p);
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```
[面试题 10.03. 搜索旋转数组](https://leetcode-cn.com/problems/search-rotate-array-lcci/)  
(面向测试用例编程)
```java
class Solution {
    public int search(int[] arr, int target) {
        int n = arr.length;
        int left = 0, right = n- 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (arr[left] == target) return left;
            else if (arr[mid] > arr[left]) {
                if (target > arr[mid]) left = mid + 1;
                else if (target <= arr[mid]) right = mid;
            } else if (arr[mid] < arr[left]) {
                if (target > arr[right]) right = mid - 1;
                else if (target < arr[right]) {
                    if (target >= arr[mid]) left = mid;
                    else if (target < arr[mid]) right = mid - 1;
                }
                else left++;
            } else {
                left++;
            }
        }
        return -1;
    }
}
```
[71. 简化路径](https://leetcode-cn.com/problems/simplify-path/)
```java
class Solution {
    public String simplifyPath(String path) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < path.length(); i++) {
            char c = path.charAt(i);
            if (c != '/' || i == 0) {
                sb.append(c);
            } else if (i != path.length() - 1 && sb.charAt(sb.length() - 1) != '/') {
                sb.append(c);
            }
        }
        Deque<String> dq = new ArrayDeque<>();
        String[] fs = sb.toString().split("/");
        for (String f : fs) {
            if (!f.equals("") && !f.equals(".")) {
                if (f.equals("..")) {
                    if (!dq.isEmpty()) dq.removeLast();
                } else {
                    dq.add(f);
                }
            } 
        }
        StringBuilder ans = new StringBuilder("");
        while (!dq.isEmpty()) {
            String s = dq.pollFirst();
            ans.append("/");
            ans.append(s);
        }
        return ans.length() == 0 ? "/" : ans.toString();
    }
}
```
## 2022.1.7
[1614. 括号的最大嵌套深度](https://leetcode-cn.com/problems/maximum-nesting-depth-of-the-parentheses/)
```java
class Solution {
    public int maxDepth(String s) {
        Stack<Character> st = new Stack<>();
        int ans = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') {
                st.push(c);
                ans = Math.max(ans, st.size());
            } else if (c == ')') {
                st.pop();
            }
        }
        return ans;
    }
}
```
[89. 格雷编码](https://leetcode-cn.com/problems/gray-code/)
```java
class Solution {
    List<Integer> ans = new ArrayList<>();
    int n;
    private void dfs(int p) {
        ans.add(p);
        int maxi = 0;
        while ((p >> maxi) > 0) {
            maxi++;
        }
        for (int i = maxi; i < n; i++) {
            dfs(p + (1 << i));
        }
    }
    public List<Integer> grayCode(int n) {
        this.n = n;
        dfs(0);
        return ans;
    }
}
```
## 2022.1.9
[1328. 破坏回文串](https://leetcode-cn.com/problems/break-a-palindrome/)
```java
class Solution {
    public String breakPalindrome(String palindrome) {
        int n = palindrome.length();
        if (n <= 1) return "";
        int mid = n / 2;
        StringBuilder sb = new StringBuilder(palindrome);
        boolean found = false;
        for (int i = 0; !found && i < n; i++) {
            if ((sb.charAt(i) != 'a') && (n % 2 == 0 || (n % 2 == 1 && i != mid))) {
                sb.setCharAt(i, 'a');
                found = true;
            }
        }
        if (!found) {
            sb.setCharAt(sb.length() - 1, 'b');
        }
        return sb.toString();
    }
}
```
[757. 设置交集大小至少为2](https://leetcode-cn.com/problems/set-intersection-size-at-least-two/)
```java
class Solution {
    public int intersectionSizeTwo(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] != b[0] ? a[0] - b[0] : b[1] - a[1]);
        int n = intervals.length;
        int[] remains = new int[n];
        Arrays.fill(remains, 2);
        int ans = 0;
        for (int i = n - 1; i >= 0; i--) {
            if (remains[i] == 0) continue;
            int t = remains[i];
            for (int j = intervals[i][0]; j < intervals[i][0] + t; j++) {
                ans++;
                for (int k = i; k >= 0 && j >= intervals[k][0]; k--) {
                    if (remains[k] > 0 && j <= intervals[k][1]) remains[k]--;
                }
            }
        }
        return ans;
    }
}
```
## 2022.1.10
[877. 石子游戏](https://leetcode-cn.com/problems/stone-game/)
```java
class Solution {
    public boolean stoneGame(int[] piles) {
        int n = piles.length;
        int[][][] dp = new int[n][n][2]; // 0-最后一步是Alice，1-最后一步是Bob，val=Alice-Bob
        for (int len = 1; len <= n; len++) {
            for (int i = 0; i < n; i++) {
                int j = i + len - 1;
                if (j >= n) break;
                if (i == j) {
                    dp[i][j][0] = piles[i];
                    dp[i][j][1] = -piles[i];
                } else {
                    dp[i][j][0] = Math.max(dp[i + 1][j][1] + piles[i], dp[i][j - 1][1] + piles[j]);
                    dp[i][j][1] = Math.max(dp[i + 1][j][0] - piles[i], dp[i][j - 1][0] - piles[j]);
                }
            }
        }
        return dp[0][n-1][1] >= 0;
    }
}
```