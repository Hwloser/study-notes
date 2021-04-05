## 方法一

### 初等函数

#### 思路

我们使用变量来跟踪进位，并从包含最低有效位的表头开始模拟逐位相加的过程。

![](2_add_two_numbers.svg)

算法

就像你在纸上计算两个数字和那样，我们首先从最低有效位也就是列表 l1 和 l2 的表头开始相加。由于每位数字都应当处于 0...9 的范围内，我们计算两个数字的和时可能会出现 “溢出”。例如，5 + 7 = 125+7=12。在这种情况下，我们会将当前位的数值设置为 22，并将进位 carry=1 带入下一次迭代。进位 carry 必定是 0 或 1，这是因为两个数字相加（考虑到进位）可能出现的最大和为 9 + 9 + 1 = 19。

伪代码如下：

* 将节点初始化为返回列表的哑结点。
* 将进位 carry 初始化为 0.
* 将 p 和 q 分别初始化为列表 l1 和 l2 的头部
* 遍历列表 l1 和 l2 直至到达他们的尾端

1. 将 x 设为节点 p 的值。如果 p 已经达到 l1 的末尾，则，将其值设置为 0.
2. 将 y 设置为 q 的值。如果 q 已经达到 l2 的末尾，则将其值设置为 0。
3. 设置为 sum = x + y + carrry;
4. 更新进位的值，carry = sum/10;
5. 创建一个数值为（sum mod 10）的新结点，并将其设置为当前结点的下一个结点，然后将当前几点前进到下一个结点。
6. 同时，将 p 和 q 前进到下一个结点

* 检查 carry = 1 是否成立，如果成立，则向返回列表追加一个含有数字 1 的新结点。
* 返回 哑结点的下一个结点。

**结果：** >
```
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1,q = l2,curr = dummyHead;
    int carry = 0;
    while(p != null || q != null){
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if(p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummuHead.next;
}
```

**复杂度分析**

* 时间复杂度：O(max(m,n)),假设 m,n 分别为 l1,l2的长度，上面的算法最多重复 max(m,n)次

*空间复杂度：O(\max(m, n))， 新列表的长度最多为 max(m,n) + 1。