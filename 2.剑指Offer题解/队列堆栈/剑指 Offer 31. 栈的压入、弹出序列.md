# [剑指 Offer 31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

>输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。
>
>示例 1：
>
>~~~
>输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
>输出：true
>解释：我们可以按以下顺序执行：
>push(1), push(2), push(3), push(4), pop() -> 4,
>push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
>~~~

就是考察栈的后进先出的结构特性的原理题目。设置栈stack来模拟，首先按照压入顺序pushed一一压入元素，每次都要检查栈顶元素是否和当前遍历到的popped元素相同，相同则按照相同的方式弹出，观察是否能得到完整的该弹出序列，即index==popped.length。

~~~java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        if(pushed.length!=popped.length) return false;//长度不一致
        Stack<Integer> stack=new Stack<>();//模拟栈
        int index=0;
        for(int n: pushed){
            stack.push(n);//按照压入顺序逐个压入栈中模拟
            //若当前栈顶元素和遍历到的popped元素相同，则弹出
            while(index<popped.length && !stack.empty() && satck.peek()==popped[index]){
                stack.pop();
                index++;
            }
        }
        //判断能不能按照popped流程走完即可
        return index==popped.length;
    }
}
~~~