## 排序算法

### 冒泡排序

基本思想：两个数比较大小，较大的数下沉，较小的数上浮。

过程：从下往上(数组从后往前)两两比较，如果相邻的两个数，第二个数大就交换位置(下沉)。重复过程，每次过程结束都会确定第i位为最小值

### 选择排序

基本思想：在长度为n的无序数组中，第一次便利n-1个数，找到最小的数值与第一个元素交换；

过程：第i个值与剩余的每一个值[i,length-1]比较(i从0开始且不包含最后一位)，将两值中小的值放在第i位。

### 插入排序(链表结构)

基本思想：假设将前n-1个数已经排序好了，现将第n个数插入到有序数列中。

过程：从头开始将第2个元素开始，将i个元素插入到i之前的链表中

### 希尔排序

基本思想：
在要排序的一组数中，根据某一增量分为若干子序列，并对子序列分别进行插入排序。
然后将增量逐渐减少，并重复上述过程。直至增量为1，此时数据序列基本有序，最后进行插入排序。

### 快速排序

基本思想：(分治法)

先从数列中去除一个数作为key值；

将比这个数小的数全部放在它的左边，大于或等于它的数全部放在它的右边

对左右两个小数列重复第二步，直至各区间只有一个数。