# 复原 IP 地址

## 题目

- [93. 复原 IP 地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

## 代码

```java
class Solution {
    List<String> res = new ArrayList<>();
    StringBuilder sb = new StringBuilder();

    public List<String> restoreIpAddresses(String s) {
        bfs(s, 0);
        return res;
    }
    void bfs(String s, int n) {
        if (n > 4) return;
        if (n == 4 && s.length() == 0) {
            res.add(sb.toString());
            return;
        }
        for (int j = 0; j < s.length() && j < 3; j ++) {
            if (s.charAt(0) == '0' && j != 0) break;
            String part = s.substring(0, j + 1);
            if (Integer.valueOf(part) <= 255) {
                if (sb.length() != 0) part = "." + part;
                sb.append(part);
                bfs(s.substring(j + 1), n + 1);
                sb.delete(sb.length() - part.length(), sb.length());
            }
        }
    }
}
```

