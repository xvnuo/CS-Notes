# [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

>输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。 
>
>示例 1：
>
>输入: "the sky is blue"
>输出: "blue is sky the"
>
>示例 2：
>
>输入: "  hello world!  "
>输出: "world! hello"
>解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。

字符串相关的 处理方法。可以使用StringBuilder等来进行操作。

~~~java
class Solution {
    public String reverseWords(String s) {
        String[] strs=s.trim().split(" ");//拆分
        StringBuilder res=new StringBuilder();
        for(int i=strs.length-1;i>=0;i--){
            String str=strs[i];
            //逐个append到res上面去
            if(!str.equals(" ") && !str.equals("")){
                res.append(str);
                res.append(" ");
            }
        }
        //转换成字符串并删除后面的空格
        return res.toString().trim();
    }
}
~~~

