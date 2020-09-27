#### 【剑指offer_65_不用加减乘除做加法】

##### 题目描述

+ 写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

##### 解题

```java
public class Solution {
    public int Add(int num1, int num2) {
        int turn1 = num1;
        int turn2 = num2;
        int and = (turn1 & turn2);
        int or = (turn1 | turn2);
        // 进位
        int c = 0;
        int i = 0;
        int temp1;
        int temp2;
        do {
            i = (i == 0) ? 1 : (i << 1);
            temp1 = (i & and);
            temp2 = (i & or);
            if (temp1 == 0 && temp2 == 0) {
                // 0 0
                // 检查是否有进位
                if (c == 1) {
                    or |= i;
                }
                c = 0;
            } else if (temp1 == temp2) {
                // 1 1
                if (c == 0) {
                    // 无进位
                    or ^= i;
                }
                // 有进位 do nothing
                c = 1;
            } else if (c == 1) {
                // 1 0 或者 0 1
                // 有进位
                // 这里注意：temp2 必须与 0 判断，而不是判断大小，因为temp2 可正也可能是负数！ 
                if (temp2 != 0) {
                    or ^= i;
                }
                c = 1;
            } else {
                // 1 0 或者 0 1
                // 无进位
                // 可直接置 1
                or |= i;
                c = 0;
            }
        } while (i != (1 << 31));
        return or;
    }

    // ---------------------------------------------------------------------
    // 下面段代码不需要了！
    // 计算补码：java 中 负数的表示就是补码！！！而不是底层操作系统的原码，所以不需要转换
    public int turn(int num) {
        if (((1 << 31) & num) == 0) {
            return num;
        }
        int k = (~num);
        return addOne((1 << 31) | (~num));
    }
    public int addOne(int num) {
        // 避免 num 为全1
        if (num == 0xFFFFFFFF) {
            return 0;
        }
        int k = 1;
        while (k != (1 << 31)) {
            if ((k & num) == 0) {
                num |= k;
                return num;
            }
            num ^= k;
            k = (k << 1);
        }
        return 0;
    }
    // ---------------------------------------------------------------------
}
```



##### 更优解

```java
/*
 ^ ：亦或：相当于各位相加，不带进位
 & ：求与：相当于各位上该向后一位的进位
 <<：左移
 
 将 整个进位 左移 1 位，就是下一位的进位，将该进位与 不带进位的值相加，便是最终值！
 
 但是 进位 与 不带进位的数相加后，还会产生进位，因此，又回到了 两数相加 的问题了！
 此时：重复上述步骤，直到 进位=0！
*/
```

