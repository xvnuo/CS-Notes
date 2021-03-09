# [剑指 Offer 50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

>在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。
>
>示例:
>
>s = "abaccdeff"
>返回 "b"
>
>s = "" 
>返回 " "

用哈希表HashMap<>记录每个字符出现的次数，两次遍历字符串，分别初始化哈希表以及找到第一个只出现一次的字符，找不到则返回‘ ’。

思路比较简单，但是比较耗时耗空间。😟

~~~java
class Solution {
    public char firstUniqChar(String s) {
        HashMap<Character,Integer> map=new HashMap<>();//哈希
        for(char c: s.toCharArray()){//遍历字符串，初始化表
            map.put(c,map.getOrDefault(c,0)+1);
        }
        for(char c: s.toCharArray()){
            if(map.get(c)==1) return c;
        }
        return ' ';//什么都找不到
    }
}
~~~