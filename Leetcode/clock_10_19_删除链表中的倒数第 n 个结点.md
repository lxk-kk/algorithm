**题目描述**：  给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。 

**示例**

```
示例：
给定一个链表: 1->2->3->4->5, 和 n = 2.
当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

**注意**

```
说明：给定的 n 保证是有效的。
进阶：你能尝试使用一趟扫描实现吗？
```

**解答**

+ 解 1：两次遍历
  1. 遍历得到链表总长度
  2. 遍历得到倒数第 N+1 个结点
+ 解 2：双指针，一次遍历
  + 指针 1：倒数 N 个结点链表的前继
  + 指针 2：末尾结点

**题解**

+ **题解 1：双指针**

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
  // 法 2(时间优化)：一次遍历，双指针（双指针内的结点数量保持在 N 个）
  public ListNode removeNthFromEnd(ListNode head, int n) {
      if(head == null){
          return head;
      }
      // 前驱指针：末尾 N 个结点链表的前驱
      ListNode pre = null;
      // 后继指针：末尾结点
      ListNode tail = head;
      // 两个指针中的结点数量
      int count = 0;
      // 遍历一次，维护 前驱、后继指针
      while(tail != null){
          count++;
          if(count > n){
              if(pre == null){
                  pre = head;
              } else {
                  pre = pre.next;
              }
          }
          tail = tail.next;
      }
      // 遍历完成：前驱指针即为倒数第 N+1 个结点
      if(pre == null){
          head = head.next;
      } else {
          pre.next = pre.next.next;
      }
      return head;
  }
  ```

+ **题解 1：两次遍历**

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
  // 法 1：遍历两次：第一次获得总的结点数量，第二次找到待删除的结点
  public ListNode removeNthFromEnd_1(ListNode head, int n) {
      ListNode node = head;
      int count = 0;
      while(node != null){
          count++;
          node = node.next;
      }
      count -= n;
      ListNode pre = null;
      node = head;
      while(count-- > 0){
          pre = node;
          node = node.next;
      }
      if(pre == null){
          head = head.next;
      } else {
          pre.next = node.next;
      }
      return head;
  }
  ```