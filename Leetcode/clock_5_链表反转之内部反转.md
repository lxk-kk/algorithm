**题目描述**

```
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
```

**示例**

![](C:/Users/10652/Desktop/career/algorithm/Leetcode/image/链表反转之内部反转.jpg)

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]

输入：head = []
输出：[]

输入：head = [1]
输出：[1]
```

**解答**：反转过程分为三个部分：**结果链表、反转链表、待反转链表**

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
    public ListNode swapPairs(ListNode head) {
        if(head == null){
            return head;
        }
        // 结果链表
        ListNode rhead = null;
        ListNode rtail = null;

        // 当前已反转链表
        ListNode fhead = head;
        ListNode ftail = head;

        // 剩余未反转链表
        ListNode uhead = head.next;
        ListNode unext = null;
        int count = 1;

        while(uhead != null){
            // 避免出现循环引用
            ftail.next = null;

            // 反转链表
            while(uhead != null && count % 2 != 0){
                unext = uhead.next;
                uhead.next = fhead;
                fhead = uhead;
                uhead = unext;
                count++;
            }
            // 将已反转链表拼接到结果链表中
            if(rhead == null){
                rhead = fhead;
                rtail = ftail;
            } else {
                rtail.next = fhead;
                rtail = ftail;
            }
            fhead = null;
            ftail = null;
            if(uhead == null){
                break;
            }
            // 重置新已反转链表
            fhead = uhead;
            ftail = uhead;
            uhead = uhead.next;
            count++;
        }
        // 末尾处理
        if(fhead != null){
            if(rhead != null){
                rtail.next = fhead;
            } else {
                rhead = fhead;
            }
             ftail.next = null;
        }
        return rhead;
    }
}
```

