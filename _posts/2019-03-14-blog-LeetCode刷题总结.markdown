# Leetcodeˢ���ܽ�

## _984. String Without AAA or BBB_

�����£�
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

*�ܽ᣺*
1. ˼ά̫���ޣ�ֻ�����˾������������Ե��º����������֮����ֻ�ܴ�����������������������ԣ����������ͬ
2. �����⸴�ӻ�����û��ץס����ı���
3. �Ҿ�������㷨�������������ж���A>B,�ô����ڼ������˶����A���ֲ�����������̫���A������ʣ���˹����B
4. ���㷨�Ⱑ��һ����д�Ĵ��뿪ʼ��������������߱��ӷ���ˣ�˵��˼·���ˣ�������򵥵ķ���
