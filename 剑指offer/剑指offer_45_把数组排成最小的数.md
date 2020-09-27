#### 【剑指offer_45_把数组排成最小的数】

```java
import java.util.ArrayList;
import java.util.*;

public class Solution {
    // 解法 1：直接比较字符串！
    private static int compare(Integer a, Integer b) {
        // 下面这段两行：简单粗暴，简直精辟！
        String stra = a+""+b;
        String strb = b+""+a;
        return stra.compareTo(strb);
    }
    
    // 解法 2：一个字符一个字符的比较！
    private static int compare_1(Integer a, Integer b) {
        String stra = String.valueOf(a);
        String strb = String.valueOf(b);
        return Solution.temp(stra, strb, 0, 0,true);

    }
    
    // 解法2（附属）
    private static int temp(String stra, String strb, int sta, int stb,boolean judge){
        for (int i = sta, j = stb; i < stra.length(); ++i, ++j) {
            char cha = stra.charAt(i);
            char chb = strb.charAt(j);
            if (cha > chb) {
                if(judge){
                    return 1;
                }
                return -1;
            }
            if (cha < chb) {
                if(judge){
                    return -1;
                }
                return 1;
            }
            if (strb.length() == j + 1) {
                j = stb - 1;
            }
        }
        // stra 已经遍历完
        if ((stra.length()-sta) == (strb.length()-stb) || (stra.length()-sta) % (strb.length()-stb) == 0) {
            return 0;
        }
        // stra .length()<strb.length || stra.length() < strb.length*n;
        if (stra.length() < strb.length()) {
            judge=judge?false:true;
            return Solution.temp(strb, stra, stra.length(), 0,judge);
        }
        if (stra.length() > strb.length()) {
            judge=judge?false:true;
            return Solution.temp(strb, stra, 0, stra.length() - strb.length(),judge);
        }
        return 0;
    }

    public String PrintMinNumber(int[] numbers) {
        Integer[] num = new Integer[numbers.length];
        for (int i = 0; i < numbers.length; ++i) {
            num[i] = numbers[i];
        }
        // 注意 sort 方法的参数：不能是基本类型，因为 Comparator 函数式接口中使用的是泛型！所以需要将 int 类型的数组转换为 Integer 类型的数组，或者 List<Integer>
        Arrays.sort(num, Solution::compare);
        // Arrays.sort(num, Solution::compare_1);
        StringBuilder builder = new StringBuilder();
        for (Integer aNum : num) {
            builder.append(aNum);
        }
        return builder.toString();
    }
}
```

