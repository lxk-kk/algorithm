#### 【剑指offer_36_二叉搜索树与双向链表】

+ 题目描述

  ```text
  输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。
  ```

+ 规范

  ```java
  public class TreeNode {
      int val = 0;
      TreeNode left = null;
      TreeNode right = null;
  
      public TreeNode(int val) {
          this.val = val;
      }
  }
  ```

+ 解析

  ```java
  /*
  1、根据二叉搜索树的特性：左子树的值比结点值小，右子树的值比结点值大
  2、由题：将二叉树转换为 已排序的双向链表，即将该二叉搜索树进行中序遍历，并将遍历的邻接结点相互指向即可。
  */
  ```

+ 解决

  ```java
  /*
  【法1】
  将中序遍历的结点进行连接需知：（对于某结点A）
  1、A的前继=A的左子树的最大值（即：A的左子树中的最右结点=》A结点 为整棵左子树的后继 =》 结点A 将一直作为后继角色，深入递归到左子树，直到左子树的最右结点，两者邻接）
  2、A的后继=A的右子树的最小值（即：A的右子树中的最左结点=》A结点 为整棵右子树的前继 =》 结点A 将一直作为前继角色，深入递归到右子树，直到右子树的最左结点，两者邻接）
  3、对于结点A：如果存在左结点，则深入递归，否则，修改引用关系，使得遍历的相邻结点邻接！
  4、对于结点A：如果存在右节点，则深入递归，否则，修改引用关系，使得遍历的相邻结点邻接！
  
  【特殊处理】
  1、头结点和尾结点只需要连接一侧（即：头结点只需修改后继，尾结点只需修改前继）
  2、根结点是整棵树的中心，它将同时作为前继和后继角色，递归深入！
  */
  
  /*
  【法2】
  依旧借助中序遍历+node list
  1、递归深入左子树
  3、递归返回本结点，将本结点加入node list
  2、递归深入右子树
  
  借助一个 node list 记录已经排好序的链表，当无没有孩子结点时，将该结点加入到 node list 的左侧或者右侧！（以 结点A 为例）
  递归深入左子树：结点A 是整棵左子树的 后继：深入左子树后，将左子树结点依次加入 
  递归返回：将 结点A 加入 node list：作为左子树结点的后继，作为右子树结点的前继！
  递归深入右子树：结点A 是整棵右子树的 前继：深入右子树前
  */
  ```

+ 代码

  ```java
  // 【法1】
  public class Solution {
      TreeNode head;
      TreeNode end;
      public TreeNode Convert(TreeNode pRootOfTree) {
          head=pRootOfTree;
          end =pRootOfTree;
          if(head==null){
              return pRootOfTree;
          }
          // 找出双端队列的首结点：做特殊处理
          while(head.left!=null){
              head=head.left;
          }
          // 找出双端队列的尾结点：做特殊处理
          while(end.right!=null){
              end=end.right;
          }
          // 根结点做特殊处理
          convert(pRootOfTree,pRootOfTree,pRootOfTree);
          return head;
      }
      
      private void convert(TreeNode node,TreeNode last,TreeNode next){
          if(node.left==null){
              if(node != head){
                  // node 的 left
                  node.left=last;
                  last.right=node;
              }
          }else{
              // 深入左子树（以左节点为根）：本结点node作为其整棵左子树的后继，last：以本结点为根的子树的前继
              convert(node.left,last,node);
          }
          
          if(node.right==null){
              if(node != end){
                  // node 的 right
                  node.right=next;
                  next.left=node;
              }
          }else{
              // 深入右子树（以右节点为根）：本结点node作为其整棵右子树的前继，next：以本结点为根的子树的后继
              convert(node.right,node,next);
          }
      }
  }
```
  
  