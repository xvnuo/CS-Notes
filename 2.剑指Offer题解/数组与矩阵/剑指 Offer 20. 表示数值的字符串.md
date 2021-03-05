# [剑指 Offer 20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

>请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100"、"5e2"、"-123"、"3.1416"、"-1E-16"、"0123"都表示数值，但"12e"、"1a3.14"、"1.2.3"、"+-5"及"12e+5.4"都不是。

这道题做的十分艰难，一定要非常详细的把各种可能的错误情况都罗列进去！

~~~java
class Solution {
    public boolean isNumber(String s) {
        s=s.trim();//去掉左右的空格
        if(s==null || s.length()==0) return false;//空
        int i=0;
        char[] ch=s.toCharArray();//字符串转换成字符数组
        boolean numFlag=false, dotFlag=false,eFlag=false;
        while(i<ch.length){
            if(ch[i]=='+' || ch[i]=='-'){//正负号只能出现在最前面或者E后面
                if(i!=0 && !isE(ch[i-1])) return false;   
                if(i==ch.length-1) return false; 
            }
            else if(isE(ch[i])){
                if(eFlag) return false;
                if(!numFlag) return false;
                eFlag=true;
                if(i==ch.length-1) return false;//e后面不能没有数字
            }
            else if(ch[i]=='.'){
                if(!numFlag && i==ch.length-1) return false;
                if(dotFlag) return false;//小数点只能出现一次
                if(eFlag) return false;//E后面不能出现小数点
                dotFlag=true;
            }
            else if(isDigit(ch[i])){
                numFlag=true;
            }
            else return false;
            i++;
        }
        return true;

    }
    public boolean isDigit(char c){//判断是否是数字字符
        return c>='0' && c<='9';
    }
    public boolean isE(char c){//判断是否是E
        return c=='e' || c=='E';
    }
}
~~~

