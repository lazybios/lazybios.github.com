---
layout: post
title: 践行数据结构--简单选择排序
categrises:
- 数据结构/算法
tags:
- Selection Sort
- 选择排序
---
次关键字的比较，从n-i+1个记录中选择出最优记录，并和第i个记录进行交换(其中n是记录个数，i是交换的趟数1<=i<=n，最后一个数不需要比较，就可以知道其正确放置的位置)**其排序时间复杂度，无论情况好坏每趟都要进行n-i次比较，故n组数需要比较的次数为n(n-1)/2次，对于交换最好情况是0次，即按顺序排方式，最坏的情况是n-1次，即当所要求排序方式的逆序排放时，在算法的时间复杂度的计算中，常常考虑的是最差情况，故比较次数和交换次数相加，最后其复杂度是O(n**2)

下面是一组简单排序编排的舞蹈，可以用来帮助理解排序过程:)
<iframe height=498 width=510 src="http://player.youku.com/embed/XMjU4NTY5NTcy" frameborder=0 allowfullscreen></iframe>

####实现代码
pyton和c语言版本
<script src="https://gist.github.com/lazybios/e48ec835ffd852ff9349.js"></script>

