#### 【剑指offer_61_扑克牌顺子】

##### 题目描述

```text
LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。
```

##### 解决思路

+ 思想：值最大为13，可使用位图法记录除0以外的数字，重复直接返回 false！遍历完数组，则数组内的数字，已经在位图中标记，同时记录了数组中 0 的个数！
+  接下来就是遍历 位图，找到第一个 1 所在位，并开始连续往后遍历4次，记录值为 1 的总数！
+  此时 0的个数+1 的个数若等于5，则说明用零代替，数组有序！否则无序！

```java
public class Solution {
    public boolean isContinuous(int [] numbers) {
        int mark=0;
        int zero=0;
        if(numbers == null || numbers.length==0){
            return false;
        }
        if(numbers.length==1 && judge(numbers[0]) ){
            return true;
        }
        // 遍历数组：记录 0 的个数，并在 mark 中标记 相应位，若有重复，则返回 false
        for(int i=0;i<numbers.length;++i){
            if(!judge(numbers[i])){
                return false;
            }
            if(numbers[i] == 0){
                zero++;
            }else if((mark & (1<<numbers[i]))==1){
                return false;
            }else{
                mark |= (1<<numbers[i]);
            }
        }
        int count=13;
        // 右移置位：清除第一个 1 前面的 0 位，并且最多只需要 遍历 13 次（1~13）
        while(((mark>>>=1)&1)==0 && (--count)>0){}
        if(count==0){
            // 不能5张牌全部都是0
            return false;
        }
        // 下面的任务：连续遍历5次：记录 1 的个数
        // 经过 上述 while ，说明首位已经是 1，计数一次
        int countOne=1;
        int k=numbers.length;
        do{
            // 右移置位：若为1则计数
            if(((mark>>>=1)&1)==1){
                countOne++;
            }
        }while(--k > 1);
        // 返回 0 的个数 + 1 的个数 与 数组长度做比较！
        return (zero+countOne)==numbers.length;
    }
    // 判断 牌面数字是否合理！
    private boolean judge(int num){
        return num>=0 && num<=13;
    }
}
```