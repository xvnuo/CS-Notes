# [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

>输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。
>
>示例 1：
>
>输入：arr = [3,2,1], k = 2
>输出：[1,2] 或者 [2,1]

#### 方法一：Java库函数Arrays.sort()

直接调用Java中的Arrays.sort()函数进行排序，然后返回下标在0-k-1的数组元素。

~~~java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        int[] res=new int[k];
        Arrays.sort(arr);//数组排序
        for(int i=0;i<k;i++) res[i]=arr[i];//复制结果数组
        return res;
    }
}
~~~

#### 方法二：堆排序/优先队列

使用Java中的优先队列实现并维护一个大跟堆的数据结构，保持堆的大小为k，然后遍历数组中的数字，遍历时做以下判断：

- 若目前堆的大小小于k，将当前数字放入堆中
- 否则判断当前数字与大根堆堆顶元素的大小关系，堆顶元素是当前堆内元素的最大值，所以，如果当前数字比大根堆堆顶还大则跳过这个数字，否则，则可以先poll掉该堆顶，再把该数字放入堆中。

~~~java
class Solution{
    public int[] getLeastNumbers(int[] arr,int k){
        if(k==0 || arr.length==0) return new int[0];//空
        Queue<Integer> pq;//创建大根堆
        pq=new PriorityQueue<>((v1,v2)->v2-v1);
        for(int num: arr){//遍历并维护大顶堆
            if(pq.size()<k) pq.offer(num);//根个数不足K,放入
            else if(num<pq.peek()){
                pq.poll();//弹出堆中的最大值，即栈顶
                pq.offer(num);//入堆
            }
        }
        //遍历结束后，堆中元素就是数组的最小的k个元素
        int[] res=new int[pq.size()];
        int index=0;
        for(int num: pq) res[index++]=num;
        return res;
    }
}
~~~

#### 方法三：快排

使用快排，每次paertition之后得到的j的前面的所有元素都比j小，后面的所有元素都比j大，通过递归不断进行快排操作，从而找到最小的k个元素。

每次quickSort快排切分1次，返回j，使得j左侧的元素都比j处的元素值小，j右侧的元素都比j处的元素值大，最终找到排序后下标为j的元素，如果j恰好等于k就返回j以及j左边所有的数。

~~~java
class Solution{
    public int[] getLeastNumbers(int[] arr,int k){
        if(k==0 || arr.length==0) return new int[0];//空
        return quickSort(arr,0,arr.length-1,k-1);//快排
    }
    public int[] quickSort(int[] nums,int left,int right,int k){
        int j=partition(nums,left,right);
        if(j==k) return Arrays.copyOf(nums,j+1);
        else if(j>k) return quickSort(nums,left,j-1,k);
        else return quickSort(nums,j+1,right,k);
    }
    public int partition(int[] nums,int left,int right){
        int v=nums[left];
        int i=left,j=right+1;
        while(true){
            //分别找到左侧第一个大于v和右侧第一个小于v的元素，交换
            while(++i<=right && nums[i]<v);
            while(--j>=left && nums[j]>v);
            if(i>=j) break;
            int tmp=nums[i];//交换值
            nums[i]=nums[j];
            nums[j]=tmp;
        }
        nums[left]=nums[j];//交换j和left处元素值
        nums[j]=v;
        return j;//此时j左侧元素都小于他，右侧都大于他
    }
}
~~~