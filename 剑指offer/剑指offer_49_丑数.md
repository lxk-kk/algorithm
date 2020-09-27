#### 【剑指offer_49_丑数】

+ 题目描述

  ```text
  把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。
  ```

+ 解析

  ```java
  /*
  由题可知：丑数的定义：除1以外，其余丑数的质数因子只有 2、3、5！
  如何判断一个数为丑数！
  解法 1、将一个数 num 分解出 所有因数 2、3、5，剩下的数若为1，则表明是丑数，否则不是丑数！
  	优点 ：简单、易操作，占用空间少！
  	缺点 ：穷举整数，若为丑数，则记录，否则丢弃：会对非丑数的整数进行判断！耗时，性能低！
  	
  解法 2、对一个已存在的丑数，将其分别与 2、3、5 相乘，结果仍然为丑数！
  	优点：不需要判断不是丑数的整数，计算结果直接为丑数，性能高！
  	缺点：需要记录已存在的丑数，目的是推出当前已记录丑数的下一个丑数，所以占用空间！
  	
  	如何推出下一个丑数？
  	令丑数记录中，已存在的最大丑数为 maxU，则
          1）肯定在 数组中存在某个丑数 mid，使得 2*mid 正好大于 maxU，记这个数为 mid2
          2）肯定在 数组中存在某个丑数 mid，使得 3*mid 正好大于 maxU，记这个数为 mid3
          2）肯定在 数组中存在某个丑数 mid，使得 5*mid 正好大于 maxU，记这个数为 mid5
          由上述：当前已记录的丑数的下一个丑数，将会是 2*mid2、3*mid3、5*mid5 中最小的那个！找出这三个丑数中最小的丑数，将其加入 丑数记录数组（按从小到大排序），于此同时，更新相应的 mid2、mid3、mid5 即可找出下一个 邻接丑数！
  */
  ```

+ 代码

  ```java
  // 【 解法 1 】：时间换空间
  public class Solution {
      public int GetUglyNumber_Solution_1(int index) {
         if(index<=0){
             return 0;
         }
          if(index<=6){
              return index;
          }
          int count=6;
          int number=6;
          while(count<index){
              // 一个数一个数的判断！
              number++;
              if(judge(number)){
                  count++;
              }
          }
          return number;
      }
      // 判断是否为质数因数 只有 2、3、5：判断丑数！
      private boolean judge(int k){
          while(k%2==0) k/=2;
          while(k%3==0) k/=3;
          while(k%5==0) k/=5;
          if(k==1) return true;
          return false;
      }
  }
  ```

  ```java
  // 【 解法 2 】：空间换时间
  public class Solution {
      public int GetUglyNumber_Solution_2(int index) {
         if(index<=0){
             return 0;
         }
          if(index<=6){
              return index;
          }
          int[] ugly = new int[index+1];
          for(int i=0;i<6;++i){
              ugly[i]=i+1;
          }
          int mid2=3;
          int mid3=2;
          int mid5=1;
          int count=6;
          while(count<index){
              count++;
              // 计算下一个丑数的可能值！
              int end2=2*ugly[mid2];
              int end3=3*ugly[mid3];
              int end5=5*ugly[mid5];
              // 找出可能值中最小的数，即为下一个丑数！
              ugly[count-1]=Math.min(end2,Math.min(end3,end5));
              // 以下 if-else ：更新 mid2、mid3、mid5：为找出下一个 丑数做准备！
              if(ugly[count-1]==end2){
                  mid2++;
                  while(3*ugly[mid3]<=end2) mid3++;
                  while(5*ugly[mid5]<=end2) mid5++;
              }else if(ugly[count-1]==end3){
                  mid3++;
                  while(2*ugly[mid2]<=end3) mid2++;
                  while(5*ugly[mid5]<=end3) mid5++;
              }else{
                  mid5++;
                  while(3*ugly[mid3]<=end5) mid3++;
                  while(2*ugly[mid2]<=end5) mid2++;
              }
          }
          return ugly[index-1];
      }
  }
  ```

  