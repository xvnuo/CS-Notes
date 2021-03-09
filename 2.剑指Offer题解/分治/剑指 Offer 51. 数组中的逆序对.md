# [剑指 Offer 51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

>在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。
>
> 示例 1:
>
>输入: [7,5,6,4]
>输出: 5

非常典型的归并排序，可以继续看315（和本题一样）,327,493, 做完这三道相信归并排序的理解会更加深入。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210309221539701.png" alt="image-20210309221539701" style="zoom:80%;" />

![image-20210309221856748](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210309221856748.png)

基于这种思路，可以得到如下代码：

~~~java
class Solution{
    public int reversePairs(int[] nums){
        if(nums==null || nums.length<=1) return 0;//无逆序对
        return mergeSort(nums,0,nums.length-1);//分治法找
    }
    public int mergeSort(int[] nums,int left,int right){
        if(left>=right) return 0;//无
        int mid=(right-left)/2+left;//中间值
        return merge(nums,left,mid,right)
      +mergeSort(nums,left,mid)+mergeSort(nums,mid+1,right);
    }
    public int merge(int[] nums,int left,int mid,int right){
        int res=new int[right-left+1];//将left~right内排好序
        int i=left,j=mid+1,k=0,count=0;
        while(i<=mid && j<=right){
            if(nums[i]>nums[j]) count+=mid-i+1;
            res[k++]=(nums[i]<=nums[j]?nums[i++]:nums[j++]);
        }
        while(i<=mid) res[k++]=nums[i++];
        while(j<=right) res[k++]=nums[j++];
        for(int m=0;m<res.length;m++) nums[left+m]=res[m];
        return count;
    }
}
~~~