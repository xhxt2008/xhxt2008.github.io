---
title: sk-learn学习笔记，简单的knn分类和数据可视化实践
date: 2017-10-06 13:54:32
tags: 
	- 机器学习
	- 数据可视化
	- 数据挖掘
	- knn
categories: 技术
---

## 官方示例
 ``` Python
>>>import numpy as np
>>>from sklearn import datasets
>>>iris = datasets.load_iris()
>>>iris_X = iris.data   #iris是一组关于花蕊四个性状的数据集
>>>iris_y = iris.target     #这些花蕊相对均匀的分为三个类别
>>>np.unique(iris_y)
array([0, 1, 2])
```
读出sk-learn自带的数据集

``` Python
# Split iris data in train and test data
# A random permutation, to split the data randomly
>>>np.random.seed(0)
>>>indices = np.random.permutation(len(iris_X))
>>>iris_X_train = iris_X[indices[:-10]]
>>>iris_y_train = iris_y[indices[:-10]]
>>>iris_X_test  = iris_X[indices[-10:]]
>>>iris_y_test  = iris_y[indices[-10:]]
>>>len(iris_X_train)
140
```
先把数据打乱，再把形状数据和类别数据分别切成训练集和测试集。事实上iris数据集共有150条数据，我们取140条作为训练集，10条作为测试集。

``` Python
# Create and fit a nearest-neighbor classifier
>>>from sklearn.neighbors import KNeighborsClassifier
>>>knn = KNeighborsClassifier()
>>>knn.fit(iris_X_train, iris_y_train) 
>>>knn.predict(iris_X_test)
array([1, 2, 1, 0, 0, 0, 2, 1, 2, 0])
```
就这么轻巧的预测结果就出来了，简单的令人发指。
<!-- more -->

## 一些思考
``` Python
>>>import matplotlib.pyplot as plt  
>>>plt.plot(iris_X_train)
>>>plt.show()
```
![image](http://oonaavjvi.bkt.clouddn.com/sk001.jpg)

我想直观的看一下这组数据究竟长什么样，直接plot的话结果是这样的，于是我想让他在坐标系里显示，为此，需要去掉一个维度。


``` Python
#把数组转化为dataFrame
>>>import pandas as pd
>>>import matplotlib.pyplot as plt
>>>iris_X_train_1 = pd.DataFrame(iris_X_train)
>>>iris_Y_train_1 = pd.DataFrame(iris_y_train)
>>>iris_X_test_1 = pd.DataFrame(iris_X_test)
>>>iris_Y_test_1 = pd.DataFrame(iris_y_test)
>>>iris_X_train_1.head()

```
![image](http://oonaavjvi.bkt.clouddn.com/WX20171016-044038@2x.png)

转换成DataFrame格式其实最主要是为了执行删除列的操作。


``` Python
>>>iris_X_train_2 = iris_X_train_1.drop([3],axis=1) #除掉一个维度
>>>iris_X_test_2 = iris_X_test_1.drop([3],axis=1)
>>>iris_X_train_2.iloc[:,0].values #取其中一列并且转化成array
```
使用drop和iloc函数分别对数组进行删除列和提取列的操作。顺便把df变回array加上.value就行了。


``` Python
cValue = iris_Y_train_1
cValue.replace({0:'r',1:'g',2:'b'}).values #花蕊的3种类型“0，1，2”用“r，g，b”表示，便于上色
```
X是花蕊的性状，Y是花蕊的分类。


``` Python
>>>from mpl_toolkits.mplot3d import Axes3D  
>>>fig = plt.figure()
>>>ax = fig.add_subplot(111, projection='3d') 
>>>ax.scatter(iris_X_train_2.iloc[:,0].values,iris_X_train_2.iloc[:,1].values,iris_X_train_2.iloc[:,2].values,c=cValue.values)  
>>>plt.show() 
```
![image](http://oonaavjvi.bkt.clouddn.com/sk002.jpg)

这个好像被叫做特征空间都东西就出来了。我们可以看见被分类为三种不同类型的花蕊，他们的特征的空间。他们的边缘有一些交错，这可能会带来误差。


``` Python
>>>knn = KNeighborsClassifier()
>>>knn.fit(iris_X_train_2, iris_Y_train_1) 
>>>predict = knn.predict(iris_X_test_2)
```
这里重复之前的步骤，直接调用knn的包，把预测结果赋给predict。

``` Python
#计算预测精度的函数
def accuricy(test_y,predict):
    same=0
    for i in range(0,len(test_y)):
        a = test_y[i]
        b = predict[i]
        if a==b:
 #       print(test_y[i])
            same+=1
    return same*100 / len(test_y)
```
一个简单的计算学习精度的函数。

```
>>>accuricy(iris_y_test,predict)
90
```
这里我们可以发现即使这组数据去掉第四个特征，也能达到同样的学习性能（90%）。通过比较，我发现在去掉第一个特征的情况下，能达到100%的精度。
这里我是手动的删掉某一个特征，好像有一些算法可以科学的降低维度，如我最近在论文里读到的二元主成分分析，这也是我今后要学习的。








