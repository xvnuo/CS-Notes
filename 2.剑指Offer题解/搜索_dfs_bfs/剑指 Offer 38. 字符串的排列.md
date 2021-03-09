# [剑指 Offer 38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

>输入一个字符串，打印出该字符串中字符的所有排列。你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。
>
>示例:
>
>输入：s = "abc"
>输出：["abc","acb","bac","bca","cab","cba"]

就是很普通的组合问题，首先将字符串排序，用visited标记每个字符是否被访问过并添加到结果字符串中过，然后用深度优先搜索，每次搜索都从头到尾遍历原字符串s，如果该字符没有被访问过，就放到结果中去，然后继续深度优先搜索，即可得到结果。这类题做的还是不熟练www

~~~java
class Solution {
    List<String> res=new ArrayList<>();//存放结果
    public String[] permutation(String s) {
        char[] ch=s.toCharArray();//转换成字符数组
        Arrays.sort(ch);
        boolean[] visited=new boolean[ch.length];//标记
        dfs(ch,visited,"");
        //将结果列表转换成结果数组并返回
        String[] arr=new String[res.size()];
        for(int i=0;i<res.size();i++) arr[i]=res.get(i);
        return arr;
    }
    public void dfs(char[] ch,boolean[] visited,String tmp){
        if(tmp.length()==ch.length) res.add(tmp);//组合完成
        for(int i=0;i<ch.length;i++){//遍历ch，如果遇到没有访问过的就放到结果中
            if(visited[i]) continue;
            if(i>0 && ch[i]==ch[i-1] && visited[i-1]) continue;
            tmp+=ch[i];
            visited[i]=true;
            dfs(ch,visited,tmp);//继续深度优先搜索
            visited[i]=false;
            tmp=tmp.substring(0,tmp.length()-1);
        }
    }
}
~~~