# [剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

>输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。
>
> **示例 1:**
>
>```
>输入: [10,2]
>输出: "102"
>```

这个题目利用Java的lambda表达式可以很方便的解决掉：

- 构建字符串列表，将nums中数字通过String.value(n)一一放入列表中
- 对字符列表进行排序，构造比较器compareTo,指明比较条件(o1+o2).compareTo(o2+o1)，即两个字符串前后结合还是后前结合，按照更小的方式来进行排
- 这样以后，该有序列表从小到大排成的一个字符串对应的数字就是最小的，通过String.join("", res)可以快速实现。

~~~java
class Solution {
    public String minNumber(int[] nums) {
        List<String> res=new ArrayList<>();
        for(int num: nums){//一一放入字符串列表中
            res.add(String.valueOf(num));
        }
        //利用compareTo比较器对列表进行排序
        res.sort((o1,o2)->(o1+o2).compareTo(o2+o1));
        return String.join("",list);//按照顺序转换成字符串
    }
}
~~~