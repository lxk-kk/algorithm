**题目描述**

```
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
```

**示例**

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

**解答**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    // 法2：合并计算
    public ListNode addTwoNumbers(ListNode l1, ListNode l2){
        ListNode head = null;
        ListNode tail = head;
        int c = 0;
        int num = 0;
        while(l1 != null || l2 != null || c > 0){
            int a = l1 == null ? 0 : l1.val;
            int b = l2 == null ? 0 : l2.val;
            num = a + b + c;
            c = num / 10;
            num %= 10;

            ListNode node = new ListNode(num);
            if(head == null){
                head = node;
            } else {
                tail.next = node;
            }
            tail = node;

            l1 = l1 == null ? null : l1.next;
            l2 = l2 == null ? null : l2.next;
        }
        return head;
    }
    // 法1：分开计算
    public ListNode addTwoNumbers_old(ListNode l1, ListNode l2) {
        ListNode head = new ListNode();
        ListNode tail = head;
        int c = 0;
        while(l1 != null && l2 != null){
            c = createNode(l1.val + l2.val + c, tail);
            l1 = l1.next;
            l2 = l2.next;
            tail = tail.next;
        }
        while(l1 != null){
            c = createNode(l1.val + c, tail);
            l1 = l1.next;
            tail = tail.next;
        }
        while(l2 != null){
            c = createNode(l2.val + c, tail);
            l2 = l2.next;
            tail = tail.next;
        }
        if(c != 0){
            c = createNode(c, tail);
        }
        head = head.next;
        return head;
    }
    private int createNode(int num, ListNode tail){
        int c = num / 10;
        num %= 10;
        ListNode node = new ListNode(num);
        tail.next = node;
        return c;
    }
}
```

