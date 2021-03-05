# [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

>请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。
>
>**示例 1：**
>
>```
>输入：s = "We are happy."
>输出："We%20are%20happy."
>```



## 方法1：Java库函数

使用java库函数replace()可以直接将原字符串中的空格替换成%20,非常方便。但是这是有违题目本意的做法。

~~~java
class Solution {
    public String replaceSpace(String s) {
        return s.replace(" ","%20");
    }
}
~~~



## 方法2：双指针

在字符串尾部填充字符，使得字符串的长度等于替换之后的长度：一个空格要替换成三个字符(%20)，所以遍历到一个空格时，可以在尾部填充两个空格。

让p1指向原字符串的末尾位置，p2指向字符串填充后的末尾位置。p1和p2从后往前遍历，当p1遍历到一个空格时，就需要令p2指向的位置依次填充02%，否则就填充上p1指向的字符。这样下来，在改变p2指向内容时，不会影响到p1原来字符串的内容。当p2遇到p1时或者sb被遍历到开头时，遍历完成，即可退出。

~~~java
public String replaceSpace(String s){
    StringBuffer sb=new StringBuffer(s);
    int p1=sb.length()-1;//指向尾部
    for(int i=0;i<=p1;i++){//遍历s，遇到空格则在尾部加两个空格
        if(sb.charAt(i)==' ') sb.append("  ");
    }
    int p2=s.length()-1;//当前长度
    while(p1>=0 && p2>p1){
        char c=sb.charAt(p1--);
        if(c==' '){//遇到空格则补上%20
            sb.setCharAt(p2--,'0');
            sb.setCharAt(p2--,'2');
            sb.setCharAt(p2--,'%');
        }else{//不是空格则直接往后拖
            sb.setCharAt(p2--,c);
        }
    }
    return sb.toString();//转换成字符串
    
}
~~~

