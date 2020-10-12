**题目描述**

```
给你一棵所有节点为非负值的二叉搜索树，请你计算树中任意两节点的差的绝对值的最小值。
```

**示例**

```
输入：

   1
    \
     3
    /
   2

输出：
1

解释：
最小绝对差为 1，其中 2 和 1 的差的绝对值为 1（或者 2 和 3）。
```

**解答**：**二叉搜索树：中序遍历，升序有序**

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    private int min;
    public int getMinimumDifference(TreeNode root) {
        this.min = Integer.MAX_VALUE;
        if(root != null){
            getMinimumAbs(root, -1);
        }
        return this.min;
    }
    // 中序遍历
    private int getMinimumAbs(TreeNode node, int father){
        if(min == 0){ // 减枝
            return node.val;
        }
        // 深入左结点
        if(node.left != null){
            father = getMinimumAbs(node.left, father);
        }
        // 判断差值，第二个结点开始判断 father == -1 是第一个结点的标志
        if(father >= 0){
            min = Math.min(min, node.val - father);
        }
        if(min == 0){ // 减枝
            return node.val;
        }
        // 将当前结点作为 右子树的 前继，深入右子树判断差值
        // 若存在右子树，则右子树的最右结点为 父结点的前驱，否则当前结点为父结点的前驱
        if(node.right != null){
            return getMinimumAbs(node.right, node.val);
        }
        return node.val;
    }
}
```

