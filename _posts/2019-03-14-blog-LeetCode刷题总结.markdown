---
layout: default
title:  "Leetcode刷题总结"
date:   2019-03-14 15:22:00
categories: main
---

# Leetcode刷题总结

## _984. String Without AAA or BBB_

答案如下：
```
class Solution(object):
    def strWithout3a3b(self, A, B):
        """
        :type A: int
        :type B: int
        :rtype: str
        """
        return self.strwithout3a3b1(A, B)
    def strwithout3a3b1(self, A, B, A_char = 'a', B_char = 'b', res = ''):
        if B > A:
            return self.strwithout3a3b1(B, A, 'b', 'a')
        while A > 0 or B > 0:
            if A > 0:
                res += A_char
                A -= 1
            if A > B:
                res += A_char
                A -= 1
            if B > 0:
                res += B_char
                B -= 1
        return res
```

*总结：*
1. 思维太局限，只考虑了具体的情况，所以导致后来情况复杂之后变得只能处理特殊情况，不具有普适性，钻进了死胡同
2. 简单问题复杂化，并没有抓住问题的本质
3. 我觉得这个算法的妙处妙就在妙在判断了A>B,好处在于既消耗了多余的A，又不至于消耗了太多的A以至于剩下了过多的B
4. 做算法题啊，一旦你写的代码开始处理特殊情况或者变得臃肿了，说明思路错了，不是最简单的方法
