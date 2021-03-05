# [剑指 Offer 11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

>把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  
>
>**示例 1：**
>
>```
>输入：[3,4,5,1,2]
>输出：1
>```

## 方法1：暴力遍历

如果旋转后最小值不在数组开头，可以遍历数组，找到第一个nums[i]<nums[i-1]的nums[i]，即为最小值。否则整个数组仍然是有序的，最小值就是数组开头元素。

~~~java
class Solution {
    public int minArray(int[] numbers) {
        for(int i=1;i<numbers.length;i++){//直接遍历
            if(numbers[i]<numbers[i-1]) return numbers[i];
        }
        return numbers[0];
    }
}
~~~

## 方法2：二分法

不同于普通的有序数组找特定值，旋转数组中关键在于确定对半分得到的两个子数组哪个是旋转数组，哪个是非递减数组(有序、递增)。整个数组的最小值一定在有序部分的最左侧边界上。已知非递减数组的第一个元素一定小于等于最后一个元素。可以通过修改二分法来求解：

~~~java
public int minArray(int[] numbers){
    int l=0,r=numbers.length-1;//左右指针
    while(l<r){
        int mid=(r-l)/2+l;//中间位置
        //只要右边比中间大，右边一定是有序数组
        if(numbers[r]>numbers[mid]) r=mid;
        //右边比中间小，如4，1，2，3的情况，最小值在1处,left++
        else if(numbers[r]<numbers[mid]) l=mid+1;
        else r--;//去重，如2，2，2，1，2的情况
    }
    return numbers[l];
}
~~~

