**题目描述**： 给定一个按非递减顺序排序的整数数组 `A`，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。 

**示例**

```
输入：[-4,-1,0,3,10]
输出：[0,1,9,16,100]

输入：[-7,-3,2,3,11]
输出：[4,9,9,49,121]
```

**解答**：**借用有序特性，使用双指针，直接判断首尾元素平方后的值即可。**优化：判断首尾元素时，不需要将其平方，只需要将首部元素 * -1 再与尾部元素做大小判断即可。

```java
class Solution {
    // 双指针：借助有序，判断头尾元素即可
    public int[] sortedSquares(int[] A) {
        int[] result = new int[A.length];
        int pre = 0;
        int end = A.length - 1;
        int idx = A.length - 1;
        // 注意：pre <= end 必须要有 pre == end 的判断，才能够包含所有元素
        while(pre <= end){
            // 头尾元素做判断，首部元素 * -1 再与尾部元素做大小判断
            if(A[end] >= (-1 * A[pre])){
                result[idx] = A[end] * A[end];
                end--;
            } else {
                result[idx] = A[pre] * A[pre];
                pre++;
            }
            idx--;
        }
        return result;
    }
}
```

