**题目描述**

```
给定仅有小写字母组成的字符串数组 A，返回列表中的每个字符串中都显示的全部字符（包括重复字符）组成的列表。例如，如果一个字符在每个字符串中出现 3 次，但不是 4 次，则需要在最终答案中包含该字符 3 次。

你可以按任意顺序返回答案。
```

**示例**

```
输入：["bella","label","roller"]
输出：["e","l","l"]

输入：["cool","lock","cook"]
输出：["c","o"]
```

**解答**：遍历每个字符串中的字符，每遍历完一个字符串，都各个字符出现的最小次数。

```java
class Solution {
    public List<String> commonChars(String[] A) {
        int[] letterRecord = new int[26];
        int[] letterTemp = new int[26];
        boolean first = true;
        for(String str : A){
            for(int i = 0; i < str.length(); ++i){
                int idx = str.charAt(i) - 'a';
                letterTemp[idx]++;
            }
            for(int i = 0; i < 26; ++i){
                if(letterRecord[i] > letterTemp[i] || first){
                    letterRecord[i] = letterTemp[i];
                }
                letterTemp[i] = 0;
            }
            first = false;
        }
        List<String> result = new ArrayList<>();
        for(int i = 0; i < 26; ++i){
            if(letterRecord[i] > 0){ // 简单的优化
                char ch = (char)('a' + i);
                String a = String.valueOf(ch); // 如果使用 ""+ch 拼接成字符串，则会更耗时，更耗空间！
                while(letterRecord[i]-- > 0){
                    result.add(a);
                }
            }
        }
        return result;
    }
}
```

