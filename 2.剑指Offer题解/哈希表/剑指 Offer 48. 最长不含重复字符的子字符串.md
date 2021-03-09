# [剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

>请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。
>
>示例 1:
>
>输入: "abcabcbb"
>输出: 3 
>解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

利用HashSet不能存放有相同的元素的性质，以及双指针、滑动窗口等来进行维护，不断更新不重复子字符串的最大长度，即可。

~~~java
class Solution{
    public int lengthOfLongestSubstring(String s){
        int res=0,left=0,right=0;//结果和双指针
        HashSet<Character> set=new HashSet<>();//集合
        for(;right<s.length();right++){
            char c=s.charAt(right);//该下标上的字符
            while(set.contains(c)){//左指针移动至不存在重复的字符
                set.remove(s.charAt(left++));
            }
            set.add(c);
            if(set.size()>res) res=set.size();//更新最大值
        }
    }
}
~~~

