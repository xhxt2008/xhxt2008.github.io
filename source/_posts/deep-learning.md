---
title: 上帝归上帝，恺撒归凯撒，传统方法与深度学习之辨。
date: 2018-05-07 01:21:14
toc: false
tags: 
- 机器学习
- 深度学习
- 神经网络
- 动态规划
categories: 研究
---
<script type="text/javascript" language="javascript">   
function iFrameHeight() {   
var ifm= document.getElementById("iframepage");   
var subWeb = document.frames ? document.frames["iframepage"].document : ifm.contentDocument;   
if(ifm != null && subWeb != null) {
   ifm.height = subWeb.body.scrollHeight;
   ifm.width = subWeb.body.scrollWidth;
}   
}   
</script>
以**数据作为驱动**的机器学习，正在快速的在各个领域，替代传统的**基于规则**的方法。那么有没有某个领域目前还无法替代呢？
答案是肯定的。我们来看一个例子。
> 我的一个朋友做的是环境方面的研究，具体对象是使用微生物处理工厂排放的污水。这就需要对污水中的生态环境进行建模。我们假设特征（X值）是水的温度，PH值，以及各种元素的含量等等，目标（Y值）是某种微生物的活性。
<!-- more -->
![数据驱动的方法](http://oonaavjvi.bkt.clouddn.com/CVD02.png)

乍一看，这个例子完全可以用最时髦的机器学习的方法来做，比如说神经网络什么的。然而现状是：
- 对每一份污水对水样我们都要用十分昂贵对器材进行有限的分析，大概一次最多只能获得300条数据。
- 水中的微量元素有上千种，也就是变量，或者说输入的维度，或者特征，有上千个。

我们直观的感受，数据太少了，机器学习模型根本无法收敛。那这个问题通常是怎么处理的呢？

## 传统方法是怎么做的？
在各种机器学习论文中，被爆的体无完肤的传统方法到底是什么方法？可能现在都没有人感兴趣了。其实现在的“机器学习”这个概念里，除了深度学习，基本上都是和传统的最优化，运筹学相通的。比方说：决策树，模拟退火，蚁群算法，GA，EDA，PSO之类的。
传统最优化领域里面，有一个万金油叫做“动态规划（Dynamic programming）”，一些比较保守的老教授非常喜欢这个东西。我们看一下百度对他的定义。
> 动态规划(dynamic programming)是运筹学的一个分支，是求解决策过程(decision process)最优化的数学方法。20世纪50年代初美国数学家R.E.Bellman等人在研究多阶段决策过程(multistep decision process)的优化问题时，提出了著名的最优化原理(principle of optimality)，把多阶段过程转化为一系列单阶段问题，利用各阶段之间的关系，逐个求解，创立了解决这类过程优化问题的新方法——动态规划。

![动态规划](http://oonaavjvi.bkt.clouddn.com/CVD03.jpg)

动态规划就是典型的基于规则（Rule base）的，回到我们的例子中，污水里每一种元素是一个方块，连线可以表示他们之间的关系，有一个公式可以表示这种关系。比方说H+离子和PH值就是相关的。阶段可以表示谁和谁会先发生反应，谁和谁后发生反应。最后对于我们的研究对象，可以表示为一个目标函数（Target function），我们一般是要求他的最大值或者最小值。
朋友告诉我，即使温度或者PH值发生一丁点改变，反应的方程式或者顺序都会有很大变化。在这个例子中，这种传统的方法某种程度上更加接近科学的本质，因为这种生物化学的反应，在大自然中本来就是客观的，我们的研究只是去发现它。而用机器学习来做，把数据直接扔给黑箱更像是在偷鸡。

## 反思一下
我举这个例子并不是想说机器学习不好，只是想说它并不是万金油。那些在各种论文中被爆的体无完肤的“传统方法”之所以存在是有他的必要性的。只是因为研究对象不同，我们就要应用相对应的方法。
那机器学习，尤其是深度学习的优势在哪里呢？这几年深度学习之所以火了，很大的原因归功于它在**计算机视觉**和**自然语言处理**领域的应用。他们对应的传统方法是什么呢？
- 在计算机视觉领域，表情识别和人脸识别曾经都是使用基于规则的方法的，他们通过计算五官的大小，和他们之间的距离等等，来对人脸或者表情进行分类。京都大学情报学院在这方面做得非常优秀，他们使用一些力学模型对时系列的表情进行识别。我之前做的paper survey涉及到这方面，感兴趣可以读一下。[Paper survey about Timing-Based Facial Expression Recognition of Kyoto University by Prof. Kawashima](https://xhxt2008.github.io/2017/11/22/paper-survey-kyoto/)
- 在自然语言处理领域，词语预测，句子纠错，机器翻译等等曾经用的都是基于规则的方法。使用的方法包括形式语言自动机理论，用例翻译等等。和我们大学学过的一门叫做《编译原理》的课程有点类似，都是基于某种规则（语法）的。

在深度学习应用之前，传统方法在这两个领域的表现都不佳。我们人类的大脑可以快速的区分两张人脸，即使他们相差得很小。一张照片有若干个像素点构成，如何通过计算这些像素点来让计算机区分两张脸呢？我们相信规则是有的，但是很大程度上无法做到，因为过于复杂。
人脸尚且有五官之间距离等可以测算，那对象识别呢？比方说识别一张图片是猫还是狗。找到这种规则来区分几乎是不可想象的。

在自然语言处理领域，自然语言天生就不是形式语言（如编程语言），虽有语法，但是例外却很多。再加上现在的网络用语，不断有新的，奇怪的东西加入。基于用例的翻译用武之地并不太多。

## 那我们人脑又是如何实现的呢？
推荐莫烦的视频说得非常通俗易懂[科普: 人工神经网络 VS 生物神经网络
](https://www.bilibili.com/video/av15997699)。
<iframe src="//player.bilibili.com/player.html?aid=16938887&cid=27691025&page=1" id="iframepage" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" onload="iFrameHeight()" > </iframe>

简单说，生物神经网络是通过神经元细胞突触的链接完成电信号的传递，进行语言理解图像识别甚至思维的。神经元之间的连接可以通过后天的训练用进废退。人工神经网络充其量只能算是对生物神经网络一种粗糙的模仿。

有人说人工神经网络是一种基于传统数理模型的东西。实际上它也确实是基于数理模型的。不过我想换个角度解读它，**人工神经网络是仿生的**。它之所以有效，是因为它在一定程度上模仿了生物的神经系统，因此，他才会在像视觉，语言理解等方面接近或者达到人类的水平。

所以我们大胆假设，人工神经网络在一些人类本身并不擅长，或者存在固定的客观规则，或者规则并不是太复杂的情况下。将会是不合适或者不经济的。

说了这么多，作为结论我想说。上帝的归上帝，恺撒的归凯撒，没有不好的学习算法，只有不合适的学习算法。

## 参考文献
- [科普: 人工神经网络 VS 生物神经网络](https://www.bilibili.com/video/av15997699)
- [京都大学大学院情報学研究科 知能情報学専攻](http://vision.kuee.kyoto-u.ac.jp/~hiroaki/index-j.html)
- [北九州市立大学国際環境工学研究科](https://www.kitakyu-u.ac.jp/subject/graduate/env/)

---

<p style="border-left-color:#008000; border-left-style: solid; border-left-width: 5px; background-color:#fafafa; padding-left:5px;"><span style="color: #808080; font-size: 12px;">本文原始链接：[简单易懂，什么是VC维](https://xhxt2008.github.io/2018/05/11/VC-dimension/)    
作者：[Tsukiyo](https://xhxt2008.github.io/)    
本文基于 [署名-非商业性使用-禁止演绎 3.0 中国大陆许可协议 (CC BY-NC-ND 3.0 CN)](https://creativecommons.org/licenses/by-nc-nd/3.0/cn/deed.zh) 发布，欢迎转载，但是必须保留本文的署名[Tsukiyo](https://xhxt2008.github.io/)及链接。如果要用于商业目的，请联系作者。
</span></p>