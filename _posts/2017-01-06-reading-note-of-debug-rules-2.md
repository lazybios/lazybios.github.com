---
layout: post
title: "《调试九法》读书笔记(二) [完]"
date: "2017-01-06"
---

![debuger-notes]({{site.IMG_PATH}}/debuger-notes.jpg)

### 保持审计跟踪
+ 把你的操作、操作的顺序和结果全部记录下来
+ 要知道，任何细节都可能是重要的
+ 把事件关联到一起 

**记下你的每步操作、顺序和结果**，保持审记跟踪。在检查某问题时，要记下你所做的事、做事的顺序，以及发生的结果。每次都要完成这些记录。你是在检测测试步骤，就像检测软硬件一样。必须清楚每一个步骤和每步执行的结果，以此确定在调试时应重点关注哪一步。

当你去看心理医生时，医生会尝试提取你生活的“审计跟踪记录”(发生了什么事情，你对这些事有何感想)，以便查明你是否有心理障碍。想象一下，如果你能把浓缩的详细简历提供给医生，将会节省多少诊费!

**好记性不如烂笔头**，在细节方面，永远都不要相信你的记忆，而要把它写下来。如果你相信你的记忆，将会制造很多麻烦。你会忘掉一些你认为不重要的细节，当然，这些细节将会被证明是非常重要的。你会忘掉一些在你看来不重要的细节，而这些细节对于后来解决另一个不同问题的人可能很重要。除了口头表述以外，你无法将信息传递给别人，而这会浪费所有人的时间。你无法准确地记住事情是如何发生的、发生的顺序以及事件之间有何关联，所有这些都是非常重要的信息。

把事情记下来。最好用计算机来记录，这样可以进行备份，并把它附加到bug报告后面，这样就很容易发送给其他人，甚至可以用自动分析工具来过滤它。把你做的事情和结果记录下来。保存调试日志和跟踪记录，并且注明相关的事件和影响(日志本身不会记录这些内容)。把你的推理和修复操作以及其他内容全部记录下来。

### 检查插头
+ 从头开始
+ 尝试质疑你的假设
+ 对调试工具进行测试

永远不要相信自己的假设，特别是当这些假设在一些无法解释的问题中是核心因素的时候。应该问自己一个古老的、看似愚蠢的问题:“插头插上了吗?”虽然这个问题看上去很愚蠢，但它经常发生。你可能费尽周折检查调制解调器软件为什么不工作了，事实证明你只是把电话线踢掉了。

### 从外界获取全新观点
+ 征求别人的意见
+ 尝试咨询专业人士获取专业意见
+ 听取别人的经验
+ 放下面子
+ 报告症状，而不是讲你的理论
+ 你提出的问题不必十分肯定

要想重新理清一个案子的头绪，最好的方法就是把它讲给别人听。

向别人寻求帮助至少有3个原因(还不算把整个问题甩给别人):获得全新观点、专业知识和经验。而且，人们通常很愿意帮忙，因为这给了他们一个证明自己聪明的机会。

我们按照自己老一套的思路是很难看清全局的。我们都是普通人，对任何事情都有偏见，包括对bug隐藏在哪里的看法。这些偏见可能导致我们无法看清实际情况。而其他人则会从一个无偏见的角度来看问题(实际上他们只是有另一种不同的偏见)，这可能会给我们很大的启发，帮助找到新的方法。

事实上，有时向别人解释问题也会使你有全新的认识，之后你自己就解决了问题。对事实进行组织的过程迫使你跳出你原来的思维模式。
> 小黄鸭调试法

**有时候系统的某个部分看起来可能很神秘，这时我们不必到学校学上一年，而可以咨询专家来了解需要快速掌握哪些知识。但一定要找一位真正懂得你的问题的专家，如果他只是向你讲述一些晦涩难懂的时髦理论，那么他可能只是一个向你吹嘘技术的“江湖郎中”，而不会提供帮助。如果他告诉你这需要花费30个小时，还要为你准备一份报告，那么他是一位顾问，也许可以为你提供帮助，但你需要付费。**

你可能经验不足，但你周围可能有人以前见过你遇到的情况，当你向他们快速描述事情的经过后，他们会准确地告诉你出了什么问题。

专家难求，同样，具有某一特定领域经验的人可能也很难找到，因此
需要高昂的费用，但这笔钱是值得的。

有一位先生在一家大型工厂做设备维护工作，他工作了很多年，直到退休。他走之后，工厂有段时间运转得很好，直到有一天一台机器突然停止工作了，尽管新的维护人员尝试了各种各样的办法，它还是纹丝不动。他们打电话给这位退休的老员工，请他帮忙。他说他可以修好它，但需要10 000美元。工厂方面很着急，于是答应了他。

他带着工具箱来到工厂，走到机器旁，然后打开工具箱，拿出一把锤子，在机器的一侧敲了一下。机器开始运转了。他把锤子收起来，合上工具箱，然后索要他的10 000美元。工厂主很生气:“用锤子敲一下就值10 000美元吗?”“不，”他纠正他们，“敲一下只收10美元。知道在哪里敲击收9990美元。”

有些系统提供了故障维修指南，它收集了很多故障检修人员在修理某一系统时积累的经验。如果你正在使用的系统有这样一本指南，当你遇到“某个部件坏了”这样的问题，而非设计问题时，就可以求助于这本指南。只需在症状表中找到 你的问题，就能够快速地修复它。
> 当使用的开源软件出了问题，可以试一试到issue中搜索一下

你可能害怕寻求帮助，你认为这是无能的表现。但事实恰恰相反，这只是表明你急于修复bug。如果你获取了正确的见解、专业知识和经验，将会更快地修复问题。这并不会暴露你的弱点，如果说有什么的话，也只是说明你明智地选择了帮助。

在向别人描述问题的时候，一定要记住一件事:报告症状，而不要讲你的理论。之所以要从别人那里获得全新的观点，就是因为你的理论起不到任何作用。如果你找了一个人，把你的理论告诉他，那么也会把他拉到你原来的思维定式中。同时，你很有可能会把一些需要让他知道的关键细节隐藏起来了，因为你自己有偏见，认为这些细节不重要。因此一定要注意这一点。**当寻求帮助时，描述发生的事情，描述你看到的一切。如果有可能，还要把条件描述清楚。告诉别人什么事情是间歇发生的，什么事情不是。但不要告诉他你认为问题的原因是什么。**
> 提问的智慧

让他提出自己的观点。他们的观点可能与你的观点相符，也可能全然不同，而这正是你想要的。我曾经见过许多这样的错误，人们向别人寻求帮助，但那个人立即就被原来的无用的理论给“污染”了。(如果你的理论有什么用的话，就不需要找人帮忙了。)。

假设你后背的下方疼痛，你去看医生，医生想听你说说你感觉如何，而不想听你说你在因特网上查询了一番，断定自己得的是脊背的肿瘤。汽车修理工想知道你的汽车发动机在寒冷的早上无法启动，而不想听你说你认为这部通用公司的汽车是在星期一生产的，那位还没完全醒酒的装配工人不知把哪个传动装置拧得太紧了。

有一些地方属于不好判断的“灰色地带”。有时你可能注意到一些数据看起来很别扭，像是错误的，或者与问题有某种关系，但你不确定为什么会这样。这些地方是值得提出来的，事实是你发现了一些出乎意料或不理解的事情。它可能与问题无关，但至少是有用的信息。

### 如果你不修复Bug，它将依然存在
+ 查证问题确实已被修复
+ 查证确实是你的修复措施解决了问题 
+ 要知道，bug从来不会自己消失 
+ 从根本上解决问题
+ 对过程进行修复

当危险已经离你很近时，拒绝承认它并不是勇敢的表现，而是愚蠢。

如果你不修复它，它不会自动修复。每个人都希望看到bug消失。“看起来它不会再出问题了。”“这个问题发生了几次，但后来不知道发生了什么，它不再失败了。”当然，逻辑上的推断是“或许它不会再发生了。”但事实上它仍会发生。

不要只是擦掉地上的油，而要纠正设计机器的方式。

-完-

### 参考引用

+ [《调试九法——软硬件错误的排查之道》](https://book.douban.com/subject/5376270)