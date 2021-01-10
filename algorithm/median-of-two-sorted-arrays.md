## 方法一：

### 递归法

为了解决这个问题，我们需要理解“中位数的作用是什么”在统计中，中位数被用来：

	将一个集合划分为两个长度相同的子集，其中一个子集中的元素总是大于另一个自己中的元素

将nums1和nums2各划分为{将A,B划分}

Left_nums1:		nums1[0]~nums1[i-1]
Left_nums2:		nums2[0]~nums2[j-1]

Right_nums1:	nums1[i]~nums2[m]
Right_nums2:	nums2[j]~nums2[n]

------>

将Left_nums1和Left_nums2放入一个集合Left_part中，另一个也是放入Right_part。

如果我们可以确定：

```
1. len(letf_part) = len(Right_part)
2. max(left_part) <= min(RIght_part)
```

那么，我们已经将{A,B}中的所有元素划分为相同长度的两个部分，且其中一部分中的元素总是大于另一部分中的元素。

那么：

median = ( max(left_part) + min(Right_part) ) / 2

要确保这两个条件，我们只需要保证：

```
1. i+j = m-i + n-j (或：m-i + n-j +1)	如果 n >= m,只需要使 i = 0~m,j = (m+n+1)/2 - i
2. B[j-1] <= A[j] 以及 A[i-1] <= B[j]
```