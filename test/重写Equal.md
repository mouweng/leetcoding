# 重写Equal

```java
package com.company;

import java.util.HashSet;
import java.util.Objects;

/**
 * @author: wengyifan
 * @description:
 * @date: 2022/3/6 5:30 下午
 */
public class Pair {
    int x, y;
    Pair(int _x, int _y) {
        x = _x;
        y = _y;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Pair pair = (Pair) o;
        return x == pair.x && y == pair.y;
    }

    @Override
    public int hashCode() {
        return Objects.hash(x, y);
    }

    @Override
    public String toString() {
        return "Pair{" +
                "x=" + x +
                ", y=" + y +
                '}';
    }

    public static void main(String[] args) {
        Pair p1 = new Pair(1,2);
        Pair p2 = new Pair(2, 3);
        Pair p3 = new Pair(1, 2);
        HashSet<Pair> s = new HashSet<>();
        s.add(p1);s.add(p2);s.add(p3);
        for (Pair p : s) {
            System.out.println(p);
        }
    }
}
```