---
layout: post
title: 在moderncv中插入footnote标签
categories:
- Latex
tags:
- moderncv
- footnote
- latex
---

找工作要做简历，用latex做pdf格式的，看了下比较成熟的CV模板，挑来挑去发现moderncv相对来说还是比较美观的，不过在制作过程中遇到不少琐碎的问题，比如字体，中文编码等，略吐槽下就是非母语为英文的真心是麻烦，编码问题不说外，字形美观程度也上不去,好在还能通过其它方式折个衷

对模板的需求，就是要插入footnote来说明一下简历的某些条目，这里差开一下主题，其实要说明的一些等级证书考试，不知道为什么这些考试总出现比通过线低1，2分的情况，往往这类型的分数与那些低分飘过的选手实力相差不多，甚至还会有逆转，可偏偏就是没有证！还得考，这样以来不就是在浪费生命，为别人贡献数据吗。99分真的与100分有这么大的区别吗？如果有那么这些区别真的对用人单位有那么多的帮助吗? 有鉴于不想做考试的炮灰的情况，我要用footnote来强调下这些考试的通过线，希望能遇到个明事例的开明老板:)

#####解决方案

> `usepackage{footmisc}` %引入新的功能包     
> `\footnotetext[num]`   %插入到制定待显示的位置 num换为需要的序号        
>  `footnotetext[num]{note contetnt}` %放入到带显示页码内容的下端，不需要嵌套到`section`内,num与上相应的编号一致   

#####效果图
![design sketch]({{site.IMG_PATH}}/picture.jpg)
