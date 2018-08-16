---
title: 平安产险2018极客挑战赛·初赛（一）
date: 2018-08-11 9:31:53
toc: true
tags: 
- 机器学习
- 数据分析
- 竞赛
- 平安产险
categories: 技术
mathjax: true
---

今年四五月份参加的平安的机器学习挑战赛获了二等奖奖，本来想忙完这阵好好吧比赛的经过分享一下的，但是做完比赛之后很快又忙于毕业设计。在细节淡忘之前想要好好把做的东西整理记录一下。

## 初赛赛题解析

[题目](http://pingancx.zhaopin.com/)非常简单，就是利用借款人的身份信息，借款记录，信用评级等信息，预测这个借款人是否会欺诈的而分类问题。提供了test集，train集，特征的说明文件和提交样品sample。参赛队伍有近一个月的时间建立模型，训练模型，提交结果。一个队伍最多有3个人，最多提交五次，取最高分作为成绩。

### 特征的理解

由于初赛时间充裕，我们可以逐个（64个）对特征进行分析，进而选择最好的处理方法。首先为了便于理解我们翻译了主办方提供的特征说明[文档](https://github.com/xhxt2008/LoanPrediction/blob/master/files/DataDictionary_cn.xlsx)。其中acc_now_deling是我们的Label，下面的都是提供的特征。有些特征的理解需要一些金融知识，我们这些外行只有强行看懂了。
![特征说明](http://oonaavjvi.bkt.clouddn.com/pinanA001.png)

### 特征的分析

光看懂文档当然是不够了，我们还调用Python的各种包对特征数据进行了初步的分析。

#### Label
首先对Y值进行分析，可以看出，正负类高度不平衡，达到了200:1的水平。这会对预测的结果造成深刻影响。我们后面会说一些我们解决这个问题的方法。
![Y值](http://oonaavjvi.bkt.clouddn.com/pinanA002.png)
![Y值正负例比较](http://oonaavjvi.bkt.clouddn.com/pinanA008.png)

#### 年份对贷款量对影响
![年份对贷款量对影响](http://oonaavjvi.bkt.clouddn.com/pinanA003.png)

#### loan_status对Y值对影响
![loan_status对Y值对影响](http://oonaavjvi.bkt.clouddn.com/pinanA004.png)

#### 各个特征相对于Y值的协方差
![协方差](http://oonaavjvi.bkt.clouddn.com/pinanA005.png)

协方差衡量了每个特征与Y值的互相惯性，协方差的绝对值越小，特征越不重要。不过这种方法没有考虑到特征的组合，不能完全依照这个。

#### 每个特征值NULL的数量
![NULL的数量](http://oonaavjvi.bkt.clouddn.com/pinanA006.png)

原则是NULL数量超过80%弃用这个特征，只有少量NULL的会填补特征，视具体情况补充均值，最大最小值，或者其他值。特殊情况会删除整条数据。

#### 独热编码
![NULL的数量](http://oonaavjvi.bkt.clouddn.com/pinanA007.png)

像这样的非数值类型变量要数值化，具体方法有二值化和独热编码。

经过对题目给的所有数据对分析之后，下一步就是对数据的预处理了。