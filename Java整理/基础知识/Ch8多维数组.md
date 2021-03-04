# Ch8 Multidimensional Arrays

## 声明和初始化

##### 1.声明和创建方法

```java
datatype[][] refVar;//声明
refVar = new dataType[10][10];//创建
dataType[][] refVar=new dataType[10][10];//声明和创建

dataType refVar[][] = new dataType[10][10];//也合法，但少见

```

##### 2.length属性

Array.length=>对应于矩阵的行数；Array[0].length对应于矩阵的列数。

ArrayIndexOutOfBoundsException=>数组下标越界异常

锯齿状二维数组Ragged arrags：每行的元素数不一样

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216223417461.png" alt="image-20201216223417461" style="zoom:67%;" />

选B，创建数组时，①等号左边不能写具体的行数和列数；②new int后面的行数和列数至少要写清楚行数，列数每行可不一样；但是行数一定要确定。行数列数可以同时有。

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216223440116.png" alt="image-20201216223440116" style="zoom:67%;" />

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216224211731.png" alt="image-20201216224211731" style="zoom:80%;" />

##### 3.一体化的声明创建和初始化的方式

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20201216223221198.png" alt="image-20201216223221198" style="zoom:67%;" />

## 二维数组的处理processing

##### 1.按照输入值初始化

##### 2.打印数组

##### 3.求和

##### 4.按照列求和

##### 5.求最大值

##### 6.求最大值的最小下标

##### 7.随机交换random shuffling

## 扩展--N维数组









































