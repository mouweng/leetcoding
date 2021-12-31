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