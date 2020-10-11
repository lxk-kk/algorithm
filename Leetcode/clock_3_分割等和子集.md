**题目描述**

```
给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

注意:
每个数组中的元素不会超过 100
数组的大小不会超过 200
```

**示例**

```
输入: [1, 5, 11, 5]
输出: true
解释: 数组可以分割成 [1, 5, 5] 和 [11].

输入: [1, 2, 3, 5]
输出: false
解释: 数组不能分割成两个元素和相等的子集.
```

**解答**：**背包问题：子集合之和为 总集合和的一半**

```java
class Solution {
    int[] arr;
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int a : nums){
            sum += a;
        }
        if((sum & 1 ) == 1){
            return false;
        }
        int target = sum >> 1;
        this.arr = nums;
        return dpSumTarget(target);
    } 

    // 转换为背包问题：空间 O(target) 时间 O(n*target)
    public boolean dpSumTarget(int target){
        boolean[] dp = new boolean[target + 1];
        dp[0] = true;
        // dp[i] ：arr 中是否存在和为 i 的集合
        // dp[i] = dp[i] || dp[i - arr[k]]
        for(int i = 0; i < arr.length; ++i){
            int a = arr[i];
            for(int j = target; j >= a; --j){
                dp[j] |= dp[j - arr[i]];
                if(dp[target]){
                    return dp[target];
                }
            }
        }
        return dp[target];
    }
}
```

