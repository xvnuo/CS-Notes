# [剑指 Offer 19. 正则表达式匹配](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/)

>请实现一个函数用来匹配包含'. '和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但与"aa.a"和"ab*a"均不匹配。
>
>**示例 1:**
>
>```
>输入:
>s = "aa"
>p = "a"
>输出: false
>解释: "a" 无法匹配 "aa" 整个字符串。
>```

整个的感觉就是很神奇，递归的时候一定要把各种情况都讨论清楚：

- 如果pat遍历完成了，str还没有遍历完，说明不匹配
- 如果pat遍历完成了，且str也遍历完成了，说明完全匹配
- 用flag标记当前pat元素是否和str元素一样，或者当前pat元素是"."，则也可以匹配的上
- 同时判断pat的下一个元素
  - 如果下一个元素是“*”,当前元素能匹配上只需要递归判断(i+1, p)是否匹配即可
  - 如果下一个元素是“*”，若当前元素和str不能匹配上，只需判断(i, pi+2)能不能匹配上，因为✳对应的元素数量设置为0，即可跳过这个不匹配的元素
  - 如果下一个元素不是✳，需要判断当前元素是否和str匹配且之后的元素是否也匹配，即flag && Match(i+1, pi+1)

~~~java
class Solution {
    public boolean isMatch(String s, String p) {
        char[] str=s.toCharArray();
        char[] pat=p.toCharArray();
        return  Match(str,pat,0,0);
    }
    public boolean Match(char[] str,char[] pat,int i,int pi){
        //①pat遍历完成，同时str也被匹配完
        if(i==str.length && pi==pat.length) return true;
        //②pat遍历完成，但是str还没匹配完
        if(i!=str.length && pi==pat.length) return false;
        //③pat没有遍历完成：
        boolean flag=i<str.length && (pat[pi]==str[i] || pat[pi]=='.');
        if(pi<=pat.length-2 && pat[pi+1]=='*'){
            return Match(str,pat,i,pi+2) || (flag && Match(str,pat,i+1,pi));
        }
        return flag && Match(str,pat,i+1,pi+1);
    }
}
~~~