---
title: Paper Survey about Do They All Look the Same Deciphering Chinese, Japanese and Koreans by Fine-Grained Deep Learning.
date: 2017-11-30 15:07:45
toc: true
tags: 
- 表情识别
- 人脸表情识别
- 人脸识别
- 神经网络
- 未翻译
- 英文
categories: 研究
---
## Abstract 
This study is about what extend Chinese, Japanese and Korean faces can be classified and which facial attributes offer the most important cues. First, we propose a novel way of obtaining large numbers of facial images with nationality labels. Then we train state-of-the-art neural networks with these labeled images. We are able to achieve an accuracy of 75.03% in the classification task, with chances being 33.33% and human accuracy 38.89% . Further, we train multiple facial attribute classifiers to identify the most distinctive features for each group. 

## Introduction 
- China, Japan and Korea are three of the world’s largest economies  with a large population. And they look very similar. 
- Some suggest that the main differences derive from mannerism and fashion. There also exist quite a few websites that put up Asian face classification chall-enges, which prove the difficulty of distinguishing them.

- Randomly shuffled 
![image](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkenzn2mtj20b6056t9c.jpg)
- Grouped
![image](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkenzn2mtj20b6056t9c.jpg)

## Related Literature 
- (Farfade, Saberian, and Li 2015; Levi and Hassner 2015; Fu, He, and Hou 2014; Wang, Li, and Luo 2016) have both achieved very high accuracy in face detection, gender and race classification.
- (Liu et al. 2015), which has achieved state-of-the-art perfor-mance, trains two networks, the first one for face localization and the second for attribute classification. Our work follows (Liu et al. 2015), and uses the same dataset as theirs, except that (1) for simplicity we use OpenCV to locate faces and (2) for ac- curacy we train a separate neural network for each attribute. 

## Data Collection and Pre-processing 
- We have two main data sources: Twitter and the CelebA dataset. We derive from Twitter the labeled Chinese, Japanese and Korean images, which are later used as input to the Resnet. We use CelebA to train the facial attribute classifiers. These classifiers are then used to classify the labeled Twitter images. 
- Twitter Images 
![image](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkenzsb6pj20e802u74j.jpg)
- CelebA Images, 
The CelebA dataset contains 202,599 images taken from ten thousand individuals. In this work, we follow Liu.’s practice in dividing the training, developing, and testing dataset. We use OpenCV to locate faces in each subset and eventually have 148,829 images for training, 18,255 images for development, and 18,374 images for testing.

## Experiments
### We conduct two experiments. 
- In the first experiment, we use the labeled Twitter images to fine-tune the Resnet and investigate to what extent Chinese, Japanese and Koreans can be classified. 
- In our second experiment, we train 40 facial attribute classifiers and examine which attributes contain the most important cues in distinguishing the three groups. 

### Face Classification 
- We split our dataset into development set, validation set, and test set ( 8: 1: 1) and experiment with different architectures from shallow networks (3-5 layers) to the 16-layer VGG and 50-layer ResNet. In our experiments, all networks would converge (Figure 4), but we observe that accuracy of 75.03% with Resnet shows the best result. 
![image](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkenzljehj209905x3ym.jpg)
- In Table 2, we report the confusion matrix for the testing images. Note that all the three peoples look equally “confusing” to the computer: the off-diagonal elements are roughly equal. The result we achieve answers in a definitive manner that Chinese, Japanese and Koreans are distinguishable. But it also suggests that this is a challenging task, which leads to our experiment on facial attribute classificatio
![image](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkeo0e64hj20ew05tq3z.jpg)

### Attribute Classification 
We construct a separate neural network for each of the 40 attributes in the CelebA dataset. The neural nets all share the same structure.
![image](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkeo5yjo8j20fo07vtac.jpg)

### Female
![image](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkenzugmwj20e1070mxp.jpg)
### Male
![image](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkeo0ajj0j20e1070wf0.jpg)

In Figures, we report the percentage of individuals that possess the corresponding facial attributes.
1. Bangs are most popular among Japanese and least popular among Chinese. 
2. Japanese smile the most and Chinese the least. 
3. Japanese have the most eyebags, followed by Koreans. 
4. Chinese are the most likely to have bushy eyebrows. 
5. Koreans are the mostly likely to have black hair and Japanese are the least likely. 

## Discussion
While our paper focuses on country comparisons, we also briefly summarize some of the significant findings on cross- country gender differentials that are either cultural or social in nature. 
- females tend to smile more than males 
- men are twice more likely to be wearing glasses than women 

## Limitations 
Our work is built on the assumption that Twitter users, celebrity followers in particular, are representative of the demographics of the entire population. This assumption may not exactly hold as various demographic dimensions such as gender and age are skewed in Twitter (Mislove et al. 2011). Nonetheless, we believe the direction of our estimates will remain consistent, as several of our findings are confirmed by social stereotypes and research on other regions. Also, this concern could be alleviated to some extent by examining several other celebrities.

## Conclusion 
- In this paper, we have demonstrated that Chinese, Japanese and Koreans do look different. By assembling a large data set of labeled images and experimenting with different neural network architectures, we have achieved a remarkable accuracy 75.03%, almost twice as high as the human average accuracy. 
- We have also examined 40 facial attributes of the three populations in an effort to identify the important cues that assist classification. Our study has shown that Chinese, Japanese and Koreans do differ in several dimensions but overall are very similar. 
- Our work, which complements existing APIs such as Microsoft Cognitive Services and Face++, could find wide applications in tourism, e-commerce, social media marketing, criminal justice and even counter-terrorism.



