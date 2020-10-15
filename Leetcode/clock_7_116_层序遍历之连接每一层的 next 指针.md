**题目描述**：填充每个节点的下一个右侧节点指针

```
给定一个完美二叉树，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。
初始状态下，所有 next 指针都被设置为 NULL。
```

**示例**

![](image/完美二叉树之层序 next 指针.png)

```
输入：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":null,"right":null,"val":4},"next":null,"right":{"$id":"4","left":null,"next":null,"right":null,"val":5},"val":2},"next":null,"right":{"$id":"5","left":{"$id":"6","left":null,"next":null,"right":null,"val":6},"next":null,"right":{"$id":"7","left":null,"next":null,"right":null,"val":7},"val":3},"val":1}

输出：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":{"$id":"4","left":null,"next":{"$id":"5","left":null,"next":{"$id":"6","left":null,"next":null,"right":null,"val":7},"right":null,"val":6},"right":null,"val":5},"right":null,"val":4},"next":{"$id":"7","left":{"$ref":"5"},"next":null,"right":{"$ref":"6"},"val":3},"right":{"$ref":"4"},"val":2},"next":null,"right":{"$ref":"7"},"val":1}

解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如上图所示。
```

**提示**

```
你只能使用常量级额外空间。
使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。
```

**解答**：借用 next 指针已经连接好的父级链表，通过父级链表连接其所有子结点的链表 **时间 O(N)；空间 O(1)**

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    // 法1：层序遍历：队列
    // 法2：层序遍历，借用父级已经连接好的 链表，连接下一层结点的链表
    public Node connect(Node root) {
        Node head = root;
        Node list = head;
        Node pre;
        while(list != null && list.left != null){
            // 保存下一层链表的头节点
            head = list.left;
            // 头链表的 pre 为 null
            pre = null;
            // 遍历 list 链表，根据 list 链表将链表中的各个结点的左右子结点相连
            while(list != null){
                // 连接结点的 left 结点
                if(pre == null){
                    pre = list.left;
                } else {
                    pre.next = list.left;
                    pre = pre.next;
                }
                // 连接结点的 right 结点
                pre.next = list.right;
                // 维护 pre
                pre = pre.next;
                // 链表的下一个结点
                list = list.next;
            }
            // 链表维护
            list = head;
        }
        return root;
    }
}
```

