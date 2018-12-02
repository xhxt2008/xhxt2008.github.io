---
title: Combining multiple feature selection methods for stock prediction Union, intersection, and multi-intersection approaches 
date: 2017-12-21 19:07:05
toc: true
tags: 
- 特征选择
- 特征工程
- 未翻译
- 英文
categories: 研究
---

## Abstract
To effectively predict stock price for investors is a very important research problem. In literature, data mining techniques have been applied to stock (market) prediction. Feature selection, a pre-processing step of data mining, aims at filtering out unrepresentative variables from a given dataset for effective prediction. As using different feature selection methods will lead to different features selected and thus affect the prediction performance, the purpose of this paper is to combine multiple feature selection methods to identify more representative variables for better prediction. 

## Introduction
Stock investments are a very popular investment activity around the world. In literature, some basic important factors, such as financial ratios, technical indexes, and macroeconomic indexes have been proved as the important factors of affecting stocks' rise and fall. However, different studies select their factors (i.e. input variables) differently for their prediction models [3]. That is, the opinion of the important factors for stock prediction is somewhat different in related work since there is no exact answer to the question of what are the most representative variables. On the other hand, it is the fact that using different input variables can make the same prediction model performs differently. Therefore, constructing the optimal stock prediction model for investors is very challenging. 
**Feature selection** can be used to filter out redundant and/or irrelevant features from a chosen dataset resulting in more representative features for better prediction performances.
That is, the chosen feature selection method is supposed to select usable features for stock prediction. However, using different feature selection methods is likely to produce different results Therefore, if we could apply a number of different feature selection methods and then combine the selection results, we can not only understand the most important and representative variables that all the feature selection methods ‘agree’, but also further improve prediction performances over using one single feature selection methods. 

## Related work 
![Related work](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkepwajznj215o0gy4d1.jpg)

## Literature review 
### Stock price theory 
- Stock prices mean the actual transaction price through the buyers and sellers in the market. Stock prices are determined by the laws of supply and demand. In theory, whether the price of a stock is high or low, it is decided by the buyers and sellers' transactions in the open market. When supply and demand change, the stock price must be changed. 
- Fama‘s Efficient Market Hypothesis supposes that the investment activity is a “Fair-Game Market”. It means all information has disclosed in the stock market, and reflects on stock prices. According to the difference of disclosed information, there are three kinds of Efficient Market Hypothesis：
    - The Weak Form Efficient Market 
    - The Semi-strong Form Efficient Market
    - The Strong Form Efficient Market 
 
### Stock price analysis methods 
- **Fundamental analysis**: Fundamental analysis believes that every stock has its intrinsic value. If the share prices lower than the intrinsic value, it means the stock is undervalued. In addition, economic factors also belong to this category. 
- **Technical analysis**. Technical analysis, also known as “charting”, has been a part of financial practice for many decades. It studies the historical price and volume movement of a stock by using charts as the primary tool to forecast future price movements. This theory believes that the trends and patterns of an investment instrument's price, volume, breadth, and the trading activities reflect most of the relevant market information that a decision maker can utilize to determine its value. 

### Feature selection
In many research problems, such as pattern recognition, it is important to choose a group of set of attributions with more prediction information. That is, if the number of irrelevant or redundant features is reduced drastically, the running time of a learning algorithm is also reduced. Moreover, a more general concept can be yielded. Performing feature selection can lead to many potential benefits, which are facilitating data visualization and data understanding, reducing the measurement and storage requirements, reducing training and utilization times, defying the curse of dimensionality to improve prediction performances, etc. 

### Principal Component Analysis (PCA) 
Principal Component Analysis (PCA) is a multivariate statistical technique. It aims at reducing the dimensionality of a dataset with a large number of interrelated variables. In particular, it extracts a small set of factors or components that are constituted of highly correlated elements, while retaining their original characters. After performing PCA, the uncorrelated variables which are called components, will replace the original variables. 
![Principal Component Analysis](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkenyuw81j20ts0dhjsr.jpg)

### GA-SVM
The main idea of Genetic Algorithms (GA) is from Darwin's theory of evolution from natural selection in the survival of the fittest. GA attempts to computationally mimic the processes by which natural selection operates.
![GA-SVM](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkenzjpnjj20fr0c8aaw.jpg)

### Classification and Regression Trees (CART) 
The Classification and Regression Trees (CART) is a statistical technique that can select from a large number of explanatory variables those that are most important in determining the response variable to be explained. The decision trees produced by CART are strictly binary, containing exactly two branches for each decision tree. The root node t is separated into two samples based on some condition. The samples that fit the condition will be separated into the left nodes (tl), and the others will be separated into the right nodes (tr). 
In particular, a decision tree is based on the entropy theory that the attribute (or feature) with the highest information gain (or greatest entropy reduction) is chosen as the test attribute for the non-leaf node 
$g\left ( Y,A \right )=H\left ( Y \right )-H\left ( Y|A \right )$

Related work only applies one chosen feature selection method to filter out irrelevant variables. This motivates us to collect all relevant variables used for stock prediction in literature and then combining multiple feature selection methods to identify more representative variables for improving prediction performances. 

## Experimental design 
### The first experimental stage
![The first experimental stage](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkenz2fsrj208o0a4gmb.jpg)
### The second experimental stage 
![The second experimental stage](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkeo0f4ppj20a60br0tm.jpg)

## Results 
### Single feature selection methods 
![Single method](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkeodedmdj20q709fjva.jpg)
![Single method](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkeoerrm1j20q308kn0g.jpg)
Figures show the rate of prediction accuracy of the four different MLP models based on the one quarter and other-quarter based testing datasets respectively. As we can see, the results are slightly different if different testing datasets are considered. 
We can find the models with feature selection are obviously better. And They have similar effects
### Multiple feature selection methods 
![Multiple method](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkeoepkscj20q50acdk9.jpg)
![Multiple method](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkeoczzt6j20q709ntd0.jpg)

The results indicate that the intersection between PCA and GA outperforms the other combination approaches over the one quarter based testing dataset. On the other hand, combining multiple feature selection methods by the multi-intersection approach performs the best based on the other-quarter based testing dataset. However, the rates of prediction accuracy by both combination approaches over the two testing datasets do not have a big difference, i.e. less than 0.3%. 

## Conclusion 
- This paper compares three different feature selection methods, i.e. Principal Component Analysis (PCA), Genetic Algorithms (GA), and decision trees (CART) and combines them based on union, intersection, and multi- intersection approaches to examine their prediction accuracy and errors. 
- The experimental results show that combining multiple feature selection methods can provide better prediction performances than using single feature selection methods. In particular, the intersection between PCA and GA and the multi-intersection of PCA, GA, and CART perform the best, which provide the highest rate of prediction accuracy and the lowest error rate of predicting stocks' rise. 
- Moreover, these two combined approaches select 14 and 17 important variables respectively from the 85 original variables, which filter out many unrepresentative variables. These variables can be used for practical investment decisions.

### Reference
- Qing-Guo Wang, “Linear, Adaptive and Nonlinear Trading Models for Singapore Stock Market with Random Forests”, 2012
 

