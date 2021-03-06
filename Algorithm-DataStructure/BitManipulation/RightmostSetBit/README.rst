Position of rightmost set bit
=====================================================
找到整数最右边的被设为1的bit的位置。

思路
------------------------------------
首先我们要获取最右边的为1的bit。这里我们可以充分利用补码(即负数形式)。例如以int8_t为例，18和-18的二进制表达为::

    18:     0001 0010
    反码:   1110 1101
    -18:    1110 1110   // 补码，即反码再加1

补码是先将所有bit反转，再加上1。将所有bit反转后，原来的最右设置位变为0，其右边的0都变成了1。加上1后，最右设置位重新变成1，其右边bit重新变成0。将两个数进行 & 操作，即可得到最右设置位::

    18 & -18 = 0000 0010

怎么求该bit的位置呢？有多种方法。

- 可以用log2函数：log2(n & -n) + 1
- 可以设置一个查询表，例如{1 => 1, 2 => 2, 4 => 3, 8 => 4，...}

当然也可以写个循环右移，但问题是不需要这么多操作，一开始右移也可以找出最右设置位。
