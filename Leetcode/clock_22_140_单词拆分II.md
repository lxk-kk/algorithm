**题目描述**：给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。返回所有这些可能的句子。

**示例**

```
输入:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
输出:
[
  "cats and dog",
  "cat sand dog"
]

输入:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
输出:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
解释: 注意你可以重复使用字典中的单词。

输入:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
输出:
[]
```

**提示**：

```
分隔时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。
```

**解答**：

+ **暴力 DFS（深度优先遍历）** -- **超时**

+ **DFS + 记忆优化** -- **超时**

  + 记忆：某位置处可生成的单词

+ **DFS + 记忆优化 + 减枝** -- **AC**

  + 记忆：某位置处可构成的单词

  + 减枝：记录某位置处构成的单词，能否成功使得后续字符串都被完整的划分

    + 若：某位置处不能构成单词，则记录
    + 若：某位置处构成了单词，但后续子串不能完整划分句子，则记录

    下一次遍历到该位置时，判断是否存在减枝记录，如果存在，则直接返回，否则可递归深入

**题解**

+ 解 1：**暴力 DFS** -- **超时**

  ```java
  class Solution {
      // 单词集
      HashSet<String> set;
      List<String> result;
      int len;
      String str;
      public List<String> wordBreak(String s, List<String> wordDict) {
          set = new HashSet<>(wordDict);
          result = new ArrayList<>();
          str = s;
          len = s.length();
          recursion(new StringBuilder(), 0);
          return result;
      }
      // 暴力 DFS
      private void recursion(StringBuilder builder, int idx){
          // 如果到达字符串末尾，说明成功划分单词
          if(idx == len){
              result.add(builder.toString());
              return;
          }
          if(idx > 0){
              builder.append(" ");   
          }
          int size = builder.length();
          // 从当前位置起，重新遍历到一个新单词，并继续深入，若无法构成一个单词，则结束当前的 DFS
          for(int i = idx; i < len; ++i){
              String temp = str.substring(idx, i + 1);
              if(set.contains(temp)){
                  builder.append(temp);
                  // dfs
                  recursion(builder, i + 1);
                  builder.delete(size, builder.length());
              }
          }
      }
  }
  ```

+ 解 2：**DFS + 记忆优化** -- **超时**

  ```java
  class Solution {
      // 单词集
      HashSet<String> set;
      // 记忆集：位置 -> 所构成单词的列表 映射
      HashMap<Integer, List<String>> map;
      List<String> result;
      int len;
      String str;
      public List<String> wordBreak(String s, List<String> wordDict) {
          set = new HashSet<>(wordDict);
          result = new ArrayList<>();
          str = s;
          len = s.length();
          map = new HashMap<>();
          recursion(new StringBuilder(), 0);
          return result;
      }
      private void recursion(StringBuilder builder, int idx){
          // 字符串末尾
          if(idx == len){
              result.add(builder.toString());
              return;
          }
          if(idx > 0){
              builder.append(" ");   
          }
          int size = builder.length();
          // 记忆集判断当前位置是否已经 遍历过
          if(map.containsKey(idx)){
              List<String> arr = map.get(idx);
              // 当前位置遍历过，但是无法构成单词
              if(arr == null){
                  return;
              }
              // 启用记忆集
              for(String temp : arr){
                  recursion(builder.append(temp), idx + temp.length());
                  builder.delete(size, builder.length());
              }
              return;
          }
          // 当前 idx 未遍历过
          boolean judge = false;
          for(int i = idx; i < len; ++i){
              String temp = str.substring(idx, i + 1);
              
              // 字串是否构成单词
              if(set.contains(temp)){
                  judge = true;
                  
                  // 第一次遍历 idx位置时，可能会有多种单词的构成可能
                  if(map.containsKey(idx)){
                      map.get(idx).add(temp);
                  } else {
                      List<String> arr = new ArrayList<>();
                      arr.add(temp);
                      map.put(idx, arr);
                  }
  
                  builder.append(temp);
                  // dfs 递归深入
                  recursion(builder, i + 1);
                  builder.delete(size, builder.length());
              }
          }
          // 如果当前位置未能构成单词，则做空记忆
          if(!judge){
              map.put(idx, null);
          }
      }
  }
  ```

+ 解 2：**DFS + 记忆优化 + 减枝** -- **AC**

  ```java
  class Solution {
      // 单词集
      HashSet<String> set;
      // 位置 -> 构成的单词列表 map
      HashMap<Integer, List<String>> map;
      // 减枝记忆集（某位置是否需要减枝）
      HashSet<Integer> record;
      List<String> result;
      int len;
      String str;
      public List<String> wordBreak(String s, List<String> wordDict) {
          set = new HashSet<>(wordDict);
          result = new ArrayList<>();
          str = s;
          len = s.length();
          map = new HashMap<>();
          record = new HashSet<>();
          recursion(new StringBuilder(), 0);
          return result;
      }
      // 返回值 boolean：是否能够划分成句子
      private boolean recursion(StringBuilder builder, int idx){
          // 字符串末尾
          if(idx == len){
              result.add(builder.toString().trim());
              return true;
          }
          // 减枝
          if(record.contains(idx)){
              return false;
          }
          
          builder.append(" ");
          int size = builder.length();
          // 当前位置是否遍历过
          if(map.containsKey(idx)){
              // 当前位置可构成单词，并且后续子串可以完整划分句子（与 解2 中的 map 有所不同）
              // 此处的 map 在构建元素时,就保证 key 代表的位置可构成单词，并且后续的子串能够完整划分句子
              for(String temp : map.get(idx)){
                  recursion(builder.append(temp), idx + temp.length());
                  builder.delete(size, builder.length());
              }
              return true;
          }
          // 当前位置未遍历过
          boolean canRecord = false;
          for(int i = idx; i < len; ++i){
              String temp = str.substring(idx, i + 1);
              // 成功构成单词
              if(set.contains(temp)){
                  builder.append(temp);
                  // dfs 深入，根据结果判断后续子串是否能够完整划分句子
                  if(recursion(builder, i + 1)){
                      // 可成功划分句子，当前位置不需要减枝记录
                      canRecord = true;
                      // 将当前 位置->单词 做记录
                      // 此处保证：只有当前位置可以构成单词，并且后续子串能够为完整划分句子时才会被记录
                      // 否则，不会被记录
                      if(map.containsKey(idx)){
                          map.get(idx).add(temp);
                      } else {
                          List<String> arr = new ArrayList<>();
                          arr.add(temp);
                          map.put(idx, arr);
                      }
                  }
                  builder.delete(size, builder.length());
              }
          }
          // 若当前位置不能构成单词
          // 若当前位置可以构成单词，但后续子串不能完整的划分句子
          // 则，都会被减枝
          if(!canRecord){
              record.add(idx);
          }
          return canRecord;
      }
  }
  ```

  