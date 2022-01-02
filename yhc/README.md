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