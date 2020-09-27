#### 【剑指offer_38_字符串的排列】

+ 题目描述

  ```text
  输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。
  ```

+ 解析

  ```java
  /*
  求 n 个字符的所有全排列（字符可能重复）+按照字典序将所有全排列排序
  
  1、固定一个字符为字符串的首位，排列剩下子字符串
  2、将剩下子字符串视为一个新字符串，使用 步骤1 的思路对该子字符串进行排列
  3、当 以一个字符为首位的字符串排列完成以后，从剩下的字符中再选取一个字符作为首位，重复步骤1 和 步骤2
  
  由上述步骤=》满足递归条件
  */
  ```

+ 解决

  ```java
  /*
  【法1】：递归法
  
  以 a b c 为例：
  1、首先 a 为首位( i==j )：即保持原位为首位：
  	=> 递归深入排列以a为首的排列
  2、其次 b 为首位( string[i] != string[j] )：交换 a 、b 的位置，使得 b 为首位：
  	=> 递归深入排列以b为首的排列，完成后a、b复位
  3、最后 c 为首位( string[i] != string[j] )：交换 a 、c 的位置，使得 c 为首位：
  	=> 递归深入排列以c为首的排列，完成后a、e复位
  由上可见，要完成 a、b、c的轮询，就需要设置循环，当交换首位位置后，需要将首位位置复位，便于下一个字符交换首位位置！
  */
  
  
  /*
  【法2】：字典排序法
  (如：12345最小~54321最大)
  1、先将原字符串中的字符按照从小到大排列：获取全排列中值（ASCII）最小的字符串，作为当前串！（如：12345）
  2、获取当前串邻接的下一个串（字典序在当前串之后的邻接字符串），将获取到的下一个串作为当前串！
  3、重复步骤2，直到字典序的末尾！（如：54321）
  
  【获取字典序下一个的邻接串】
  由一般而言，字典序相邻的字符串，有公共的 前缀！
  	例1："12345" 邻接 "12354" ，共有前缀为 "123"！
  	例2："15432" 邻接 "21345" ，共有前缀 为空 ！
  由上述：只需要改变 共有前缀的后缀：
  1、从末尾开始，寻找 str[i]>str[i-1] 的 位置i 与 位置i-1
  	如上：
  		例1：str[i]为5 str[i-1]为4
  		例2：str[i]为5 str[i-1]为1
  2、从 i 开始（包括位置 i ），找出最后一个 值大于 str[i-1] 的位置，记为 位置m
  	如上：
  		例1：m=i 
  		例2：str[m]为2
  3、交换 m 和 i-1 的位置
  	如上：
  		例1："12345" => "12354" 	交换后 str[i-1]=5 str[i]=4 str[m]=4
  		例2："15432" => "25431" 	交换后 str[i-1]=1 str[i]=5 str[m]=2
   4、从 i 开始（包括位置 i），逆置 位置i 及其 之后的字符串序列
   	如上：
   		例1 逆置"4" ：12354" => "12354"
   		例2 逆置"5431" ：25431" => "21345"
   【判断字典序末尾】
   不存在 str[i] > str[i-1]	如：54321
  */
  ```

+ 代码

  ```java
  import java.util.ArrayList;
  import java.util.*;
  public class Solution {
      
      private ArrayList<String> lists;
      private char[] string;
      
      public ArrayList<String> Permutation(String str) {
          lists=new ArrayList<>(16);
          string=str.toCharArray();
          if(string.length==0){
              return lists;
          }
          // 全排列
          order(0);
          // 排序
          lists.sort(Solution :: compare);
          return lists;
          // 法0：
          /*
          1、固定第一个数，排列后面的数，后面的数作为整体
          2、后面的整体中，再固定第一个数，排列后面的数
          3、依次类推！
          */
      }
      private static int compare(String a,String b){
          return a.compareTo(b);
      }
      
      private void order(int i){
          if(i==string.length-1){
              lists.add(new String(string));
              return;
          }
          // 首位轮询设置
          for(int j=i;j<string.length;++j){
              // i==j 表示原位为首位
              // string[i]==string[j] 表示其他位为首位
              if(i==j || string[i] != string[j]){
                  // 交换位置，递归深入
                  /*
                  交换位置（轮询首位），首位固定，递归深入排列后面的部分
                  */
                  swap(string,i,j);
                  /*
                  注意：递归深入排列，是从首位的后一位开始！
                  */
                  order(i+1);
                  /*
                  当一个首位轮询排列完成后，需要轮询其他字母为首位，此时需要将字符串复位
                  */
                  swap(string,i,j);
              }
              // 隐含 i!=j && string[i]==string[j] 的情况，不做处理，表示去重
          }
      }
      private void swap(char[] a,int i,int j){
          if(i==j){
              return;
          }
          char e=a[i];
          a[i]=a[j];
          a[j]=e;
      }
  }
  ```

  