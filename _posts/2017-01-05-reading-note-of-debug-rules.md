---
layout: post
title: "《调试九法》读书笔记(一) "
date: "2017-01-05"
---

有可能在你 和你的所有竞争对手的设计中都有一些很难查找的bug，谁能够最快地修复它们，谁就占据了优势。当你快速找到bug时，不仅 能够更快地为客户提供更高质量的产品，而且也能够更早下班回家，与家人一起享受美好的时光。

不要把“显然”和“容易”混淆在一起，有些显然的规则遵守起来并不总是那么容易。

调试通常是查明为什么一个设计没有按计划工作，而故障检修通常是在已知设计没有问题的情况下，查明一件产品出了什么问题——可能是某个文件被删除了，某条线断了或者是某个元件出了问题。

医生给病人看病就相当于“故障检修”，医生永远没有调试的机会了。因为上帝已经完成了调试的任务，他花了一天时间设计了人类并造出原型，又在同一天把他的产品投放到人间，看起来上帝好像很赶时间!我想我们可以原谅他优先处理重要的方面，而给我们留下了像“拇指外翻 5”和“男性规律性脱发”这样的小bug。

### 理解系统
+ 阅读手册
+ 仔细阅读每个细节
+ 掌握基础知识
+ 了解工作流程
+ 了解辅助工具
+ 不要盲目记忆，而要学会查阅具体细节

理解系统的基本方法就是阅读手册。我们应该首先阅读手册，而不是等到所有办法都不管用之后才去读它。当你买来一件东西时，手册告诉你怎么操作它，以及它是用来干什么的。我们需要一页一页读完手册并理解它，以便用它来完成我们需要做的工作。

理解了你自己的系统后，还会获得一个额外的好处。当你找到bug时，必须在不破坏其他地方的前提下修复它们。理解系统行 为是不破坏系统的第一步。

#### 逐字逐句阅读整个手册
人们在调试的时候，通常都不会彻底地阅读系统手册。他们采取跳读的方式，查看他们认为重要的一些章节，但问题的线索可能就隐藏在被略过的那些章节中。是的，我就是凌晨1点的时候找到了线索，最后终于发现了进料阀控制器的bug。

编程指南和API可能非常厚，但你必须深入挖掘它，查找你认为有问题的函数。

当你尝试寻找bug时，必须知道要查找的路线。开始时，你需要猜测在哪里把系统分隔开，以便隔离问题，这种猜测完全取决于你对系统功能划分的了解。你至少要大体上知道所有的模块和接口都是做什么的。

不要猜测，而要查阅手册。芯片制造商或软件工具的开发人员已经把详细信息写到手册中，而你不应该盲目相信自己的记忆。

### 重现失败
+ 从头开始制造失败
+ 查找不受你控制的条件
+ 记录每件事，并找到间歇性bug的特征
+ 不要过于相信统计数据
+ 要认识到「那」是可能发生的
+ 永远不要丢掉你的调试工具

关键是在发生失败的时候要看到它。这是很多调试的典型问题——你看不到问题是如何发生的，因为你方便观察的时候，它并没有发生。这并不是说它只发生在深夜(即使你最终是在深夜时发现了它)，它可能是每7次中只发生1次，或者是Charlie测试它的时候，碰巧发生了一次。

设法正常使用，并观察它是如何出错的。当然，测试本来就应该是这样的，但这里的重
要之处是在错误第一次出现之后，能够使它再现。通常，认真记录测试过程可以作为补充，但你必须认识到仅有一次错误是不够的。

仔细观察你做了什么，然后再做一次，并且记下你做的每个步骤。然后，按照你自己所写的步骤去做，确定这样做确实可以重现错误。


我房屋的一扇窗户漏雨，但只是在倾盆大雨而且刮东南风的时候才会漏。我想赶在下一次暴风雨到来之前把它修好，于是我架了把梯子，拿着喷水的水管向窗户喷水，看看它到底是怎么漏雨的。这使我准确地看到了漏雨的地方，原来是墙缝那里有一道空隙，我把这个空隙塞满，并再次喷水检查，发现即使在这么高的水压下，它也不再漏水了。

查找现有bug已经够我们忙的了，不要再制造新的bug。利用工具来观察发生了什么错误，但不要因此改变程序的运行机理，因为可能正是这个机理导致了错误。

记住，这并不意味着不能用自动化过程来引发失败，也不意味着在这个过程中不能采用一些起到放大效果的措施。自动测试能够使间歇性的问题更快发生，例如电视游戏的例子。放大效果可以使得细微的问题更明显，例如在修窗户的例子中，我可以用喷水管来找到漏雨的窗户，而不用等待偶尔才有的暴风雨来检查。这两种技术都有助于引发失败，而不是模拟失败的机理。所做的改变应该是一些高层次的改变，只影响错误发生的频率，而不影响错误的发生方式。

你必须能够看到失败。如果它不是每次都发生，那么就必须忽略掉不发生的时候，而在它每次发生时观察它。关键是在每次运行的时候捕捉相关信息，以便在发生失败之后查看这些数据。方法就是让系统在运行的时候尽可能多地输出信息，并把它们记录到“调试日志”文件中。

如果你曾经与工程师们打过交道(与他们一起工作过足够长的时间)，那么一定听他们说过“那不可能发生”。测试人员或现场技术人员报告了一个问题，而工程师则摇摇头，思考一会儿，然后说:“那不可能发生。”
> 画面感好强！


有时，一种测试工具可以在其他的调试场合重复使用。当你设计它的时候，应该考虑到这一点，并且使它易于维护和升级。这意味着要采用好的工程技术，并实现文档化，等等。把它加入到源代码控制系统中，并构建到你的系统中，以便随时可以使用。不要只把它当做一次性的工具来编码，扔掉它可能是个错误。


有时，一个工具非常有用，你实际上可以把它当做产品来卖。很多公司发现所开发的工具比产品还要畅销，于是开始转而销售这种工具。
> 像是在预言Slack


### 不要想，而要看
+ 观察失败
+ 查看细节
+ 植入插装工具(这里的插装即打断点、记日志，信号灯等手段)
+ 不要害怕深入研究
+ 注意海森堡效应
+ 猜测只是为了确定搜索的重点

在没有事实作为参考以前妄下结论是个很大的错误。主观臆断的人总是为了套用理论而扭曲事实，而不是用理论来解释事实。

亲眼看到底层的失败是非常重要的。如果你猜测失败是如何发生的，那常常会修复一些根本不是bug的问题。这样的修复不仅不会解决问题，而且还会浪费时间和金钱，甚至会破坏其他地方。请记住，不要这样做。

“不要想，而要看”，这是我最常跟工程师们说的一句话，比其他任何调试建议说得都要多。有时，当某位工程师提出了一个表面看来非常好的想法，但进一步研究发现它实际上根本算不上好想法的时候，我们就会开玩笑地说:“看，他是一位思想家。”所有工程师都是思想家。他们喜欢思考，这是一件有趣的事情，而且肯定胜过体力劳动，这也正是我们成为工程师的首要原因。虽然我们有各种各样好的设想，但发生问题的原因更加多样化，即使是最有想象力的工程师也无法想象出来。那么，为什 么我们认为能够通过思考来找到问题呢?因为我们是工程师，因为想比看要简单得多。

我曾经有位同事，他非常聪明，也对自己的逻辑思考和理解产品的能力非常自信。当他听说有bug时，总是会说:“我敢打赌，它就是‘这样这样’的一个问题。”我总是告诉他我跟他打赌。我们从来没有用钱来做赌注，这对我来说可真糟糕，因为几乎每次都是我赢。虽然他很聪明，也了解系统，但他并没有看到失败，因此没有足够的信息来查明原因。
> 怎么感觉身边这样的选手还不少呢，你有没有中枪？

当你的错误猜测一一被否定后，你精疲力尽，但你仍然必须找到bug。你需要做的工作量仍然跟先前一样多，唯一的不同就是你的时间变少了。这很糟糕，除非你认为“越早掉队，你就越有充裕的时间去追赶”。因此，为了帮助你在思考之前先进行观察。


却去修复了一个完全没有发生错误的地方。这样，你不仅没有修复问题，而且还可能改变了时序，因此把问题隐藏起来了，这会使你误认为已修复问题。更糟的是，你可能会破坏其他地方。即使在最好的情况下，这也会导致时间和经济上的损失，就像一个(打不好高尔夫球的)人去买一套新的球杆，而不是请职业高尔夫球手帮他分析击球的姿势。新的球杆不会帮助他纠正总是把球打偏的问题，而高尔夫球的课程很便宜。他仍会打出很多次双柏忌2，最后不得不放弃尝试，去参加高尔夫球课程。

一定要亲眼看到实际错误是如何发生的。观察往往比猜测能够更快地找到问题。因为猜测虽然看起来是捷径，但这条捷径并不会带你找到问题的根源。

在停下来思考问题之前，对细节的观察应该到什么程度才合适呢?简单的答案是:“一直观察，直到把问题的原因锁定在几种 可能性之内。

经验可以起到帮助作用，就像理解系统会起到帮助作用一样。当你作出了错误的假设并沿着它追查问题时，经验会告诉你在特定情况下追查到什么程度就应该停止了。经验会告诉你什么时候问题的原因已经被锁定到一个很小的范围内。你会知道怎样做一个好的调试人员。评判标准不是多快地提出一个猜测，也不是猜测得有多好，而是尽可能少地按错误的猜测行动。


海森堡是量子物理学的开拓者之一。他致力于研究质量和体积极小 的原子内的粒子，他发现你要么测量一个粒子的位置，要么测量它向哪个位置运动，但这二者当中有一个测量得越精确，另一个就越测不准。无法得到准确测量的原因是探针成为了系统的一部分。
> 在引入新工具之前，要确保工具对系统本身是没有污染的。

“不要想，而要看”并不意味着不能做任何猜想。事实上猜测是好事，特别是当你理解了系统之后。你的猜测可能很接近事实，但猜测只是为了确定搜索的重点。

因此，不要过分相信你的猜测，它们往往偏离了方向，并且把你引入歧途。如果事实表明，经过仔细的插装仍然无法确定你的猜测是否正确，那么就到了退回并再次猜测的时候了。


### 分而治之
+ 通过逐次逼近缩小搜索范围
+ 从有问题的一端开始搜索
+ 修复已知bug，bug之间会互相保护，互相隐藏
+ 首先消除噪声干扰，排除不必要的干扰

当你排除了所有的不可能，不管留下了什么，也不管看起来多么不可思议，那必定都是事实。

你可能已经注意到，在查找问题时，“分而治之”实际上是第一条需要使用的原则。事实上，在查找问题时它也是唯一需要应用 的规则。所有其他规则都只是帮助你遵循这条规则。分而治之是调试的核心。

如果修复某个问题对其他的问题有影响，一定要首先修复它之后再测试其他的问题。如果修复了一个问题后将会引发新的问题，那么你可以尽早发现，并有更多时间处理新的问题。

### 一次只改一个地方
+ 隔离关键因素
+ 与正常情况进行比较
+ 确定自从上一次正常工作以来你改变了什么地方

社会科学家和医学研究人员通常利用兄弟、姐妹(有时是双胞胎)来分离他们正在研究的因素。他们可能观察一些分开生活的双胞胎，由于双胞胎的基因是完全相同的，因此任何差别都来自环境因素。同理，他们可能观察被领养的孩子，把家庭环境作为常量，并假设任何差别都来自基因。当牙医寻找你口腔中对冷物过敏的牙齿时，他会使用一种类似于步枪的仪器喷出冷空气流，一次检查一颗牙齿，而不会在你的嘴里放一个冰块。过去，人们会用一长串灯泡来装饰圣诞树，如果有一只灯泡坏了，整串灯泡都不会亮。这时，你一次只会更换一只灯泡，直到整串灯泡重新亮起来。如果你把所有灯泡都换掉，将会花很多钱，而换下来的灯泡中只有一个是坏的，其他都是好的。

这就是科学方法。为了看到一个变量的影响，科学家尝试控制可能影响结果的所有其他变量。

在很多情况下，你可能想改变系统的不同部分，以便看看它们是否对问题有影响。这往往是一个危险的信号，说明你正在猜测，而不是使用插装工具来观察正在发生什么。你正在改变条件，而不是捕捉错误的自然发生。这可能会把最初的错误隐藏起来，而且引起更多错误。

在核潜艇中，动力装置控制台前面有一个黄铜杆。当发生某种状况而启动警报时，工程师们必须双手抓住黄铜杆，并一直保持，直到看清所有仪表和指示器，明白发生了什么事情。这样做是为了帮助他们克服开始“修复”问题的冲动(急着按开关和打开阀门)。这些快速的修复举动会扰乱系统的自动修复功能，由于快速设置新的条件而隐藏了原来的问题，而且还有可能引发真正的大灾难。更有效的方法是记住要做的事情(“抓住黄铜杆”)，而不是记住不要去做什么事情(“不要动仪表盘”)。因此 要抓住黄铜杆!

有时，改变测试序列或一些操作参数可以使问题更加有规律地出现，这有助于观察错误，而且可能会帮助我们找到问题的线索。但我们仍然应该一次只改变一个地方，以便判断哪个参数有影响。如果做了一个改变后看上去没有什么效果，应立即把它改回来。

-待续-

### 参考引用
+ [《调试九法——软硬件错误的排查之道》](https://book.douban.com/subject/5376270)

