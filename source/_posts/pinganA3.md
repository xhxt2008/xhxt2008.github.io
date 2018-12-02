---
title: 平安产险2018极客挑战赛·初赛（三）
date: 2018-10-10 15:48:31
toc: true
tags: 
- 机器学习
- 数据分析
- 特征工程
- 竞赛
- 平安产险
categories: 技术
mathjax: true
---

## 特征工程

本次工作采用xgboost作为预测器，基于树模型的预测器对特征的质量相对敏感，因此是有必要做一些特征工程的。

### 删除极端值

我们选择两个重要程度较大特征，并且把他们在一个二维平面以散点打印了出来。红色蓝色分别表示，正例和负例。

```python
>>> cValues = pd.DataFrame(y_res)
>>> plt.figure(figsize=(15,15))
>>> cValues=cValues.replace({0:'b',1:'r'})
>>> plt.scatter(X_res[:,3],X_res[:,16],c=cValues.values[:,0],marker = '.')
```

![二维特征空间](http://ww1.sinaimg.cn/large/6b8ee255gy1fxkepu86iuj211y0zina4.jpg)
可以看出，在这个二维的特征空间里面，正负例几乎是重合的，这就很难只通过这两个特征做分类。而且，有一些点离群过于远了，这些异常点的存在会把大量有用信息压缩在一个很小的范围。我们认为应该予以删除这些点，获得更好的训练结果。

我们采用了基于knn的方法删除异常点，通过计算每个点最邻近的5个点的平均距离，删除最离群的0.3%的点。

```python
>>> from sklearn.neighbors import LocalOutlierFactor
>>> lof = LocalOutlierFactor(n_neighbors=5,contamination=0.003)
>>> y_pred = lof.fit_predict(X_train_select)

# 补上index
>>> index_mumberid = X_train_select.index
>>> y_predict=pd.Series(y_pred,index=index_mumberid)

# 保留对应的点
>>> ANOMALY_DATA = 1
>>> predicted_outlier_index = np.where(y_pred == ANOMALY_DATA)
>>> X_train_outlier = X_train_select.iloc[predicted_outlier_index]
```

```python
>>> plt.scatter(predicted_outlier[:,0],predicted_outlier[:,1],c=cValues1.values[:,0],marker = '.')
```

当然，这种基于knn的方法，在维度极高的情况下面临严重的维度爆炸的问题（效率及其低下）。在上一篇做了很多one-hot之后，维度已经变得高而稀疏。因此在做极端值删除之前，我们考虑先做特征选择。那么我们该留下多少特征呢？这又成为了一个新的问题。

### 确定被选择的特征数量

```python
>>> thresholds = np.sort(xgb_select.feature_importances_)
>>> for thresh in thresholds:
>>> 	# select features using threshold
>>> 	selection = SelectFromModel(xgb_select, threshold=thresh, prefit=True)
>>> 	select_X_train = selection.transform(X_train)
	# train model
>>> 	selection_model = XGBClassifier(scale_pos_weight= 100)
>>> 	selection_model.fit(select_X_train, y_train,eval_metric = f_beta_wrapper)
	# eval model
>>> 	select_X_train = selection.transform(X_train)
>>> 	y_pred = selection_model.predict(select_X_train)
>>> 	predictions = [round(value) for value in y_pred]
>>> 	f2 = fbeta_score(y_train, predictions,beta=2)
>>> 	print("Thresh=%.3f, n=%d, Accuracy: %.2f%%" % (thresh, select_X_train.shape[1], f2))

Thresh=0.000, n=51, Accuracy: 0.10%
Thresh=0.001, n=28, Accuracy: 0.10%
Thresh=0.001, n=28, Accuracy: 0.10%
Thresh=0.003, n=25, Accuracy: 0.10%
Thresh=0.004, n=24, Accuracy: 0.10%
Thresh=0.006, n=23, Accuracy: 0.10%
Thresh=0.007, n=21, Accuracy: 0.10%
Thresh=0.014, n=20, Accuracy: 0.10%
Thresh=0.017, n=19, Accuracy: 0.10%
Thresh=0.025, n=18, Accuracy: 0.10%
Thresh=0.025, n=18, Accuracy: 0.10%
Thresh=0.030, n=14, Accuracy: 0.09%
Thresh=0.030, n=14, Accuracy: 0.09%
...
```
xgboost里的feature_importances_方法得出的值，是根据这个特征在全部迭代中被使用的次数决定的。我们根据feature_importance的重要性值，对全部特征进行排序，然后逐个设置阈值，不断减少特征数量进行初步的训练和预测。直到精度开始下降（一定程度）。这时需要的特征就被选择出来了。这里一共有20个重要特征（总数是57个）。

### 特征选择

```python
>>> imp_values=xgb_select.feature_importances_
>>> fea_names=train_df.columns.values
>>> feature_importance=pd.Series(imp_values,index=fea_names)

>>> fea_imp_order=feature_importance.sort_values(ascending=False)
>>> fea_select =fea_imp_order.head(20)
>>> X_train_select = X_train.loc[:,fea_select.index]
```
这一步简单的取出了前20个重要特征。

### 欠采样
前面提到，数据类别不平衡高达200:1。在这么大的不平衡级别下，即使我们通过自定义分类器的metrics也无法获得好的分类效果。根据几次简单的测试，我们认为应该把比例调整到10:1左右，才能获得好的学习效果。

```python
>>> from imblearn.under_sampling import RandomUnderSampler
>>> from collections import Counter
>>> def ratio_multiplier(y):
>>>     multiplier = {0: 0.05, 1: 1}
>>>     target_stats = Counter(y)
>>>     for key, value in target_stats.items():
>>>         target_stats[key] = int(value * multiplier[key])
>>>     return target_stats
    
>>> rus = RandomUnderSampler(random_state=100,ratio=ratio_multiplier)#100
>>> X_res, y_res = rus.fit_sample(X_train_all, y_train_all)
```
解决类别不平衡的手段除了欠采样，还有过采样和生成过采样等等。我们尝试了这些方法，发现这么做对精度并没有太大提高，还反而拖累了学习的效率。

到这里我们就得到了一个相对干净的数据集，可以开始训练了。