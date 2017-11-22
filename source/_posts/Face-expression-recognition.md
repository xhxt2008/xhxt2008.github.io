---
title: Paper survey about Face Expression Recognition
date: 2017-11-22 16:41:12
tags: 
  - 人脸表情识别
  - 人脸识别
  - 表情识别
  - 论文查找
  - 未翻译
  - 日文	
categories: 研究
---
表情認識（Face Expression Recognition）は、成熟した技術でした。表情認識確かに国籍認識と共通点が多かったです。
## まずは顔に表す感情の定義として。
普通六つの分類があります：楽しく、悲しく、怒る、驚く、嫌い、怖い。
	そして、表情認識のプロセスは、主に：
-	前の処理。サイズ、明るさ、仕草の平均化。
-	特徴の抽出。
-	分類。
<!-- more -->
## 特徴の抽出の方法にしては：
1.	幾何学的特徴、人の五官の位置を探して、それぞれの大きさ、距離、形状などを用いて、表情を識別します。
2.	PCAやICA。総体的に特徴を抽出方法
3.	LBP。局部の特徴を抽出。
4.	Gabor filters。画像信号を時空から頻度に変えて、特徴を抽出。（フェーリエやガウシャン変化を用いる）、よくANNやSVMと一緒に使われる。
5.	optical flow。動画で使う方法。「７」
6.	neural networkで特徴を選ぶ研究もあります。

## 分類にしては：
1.	Linear classifier「１」
2.	ANN
3.	SVM
4.	HMM(Hidden Markov Model)

## 今の主な研究方向：
1.	視覚と聴覚（audio-visual）を組み合わせして、表情の識別、音声認識の性能向上「３」。
2.	サンプルが少ない場合の識別「４」。
3.	表情認識を顔識別の性能向上での応用「５、６」。
4.	表情認識を使って、生徒が授業に集中しているのかどうかの研究「8」。（応用が新しい）
5.	アルゴリズム的な革新

## 基礎な表情認識は携帯アプリまであります。（Polygram）
 ![image](http://oonaavjvi.bkt.clouddn.com/%E5%9B%BE%E7%89%87%201.jpg)
Polygramは自撮りをシェアするアプリで、他の観客の写真を見る時の気分を認識できます。画像に示しているのは驚きの表情。

## 自分の考え：
1.	視覚と聴覚（audio-visual）を組み合わせだけではなく、体温や心拍を含めれば、認識の性能はさらにアップできますはずです。このような技術は、運転手状態管理に役に立てるかもしれません。
2.	片方の光源のジャミングに関してけれど。去年参加した連携大学の授業で、アイシン精機はとても完成度高いな技術を展示しました。
3.	新し応用については、今オンライン教育が盛んて、どうやって表情認識をオンライン教育に応用できるのは、役に立てます。オンラインテストも同じです。

## 後書き
表情認識は面白い研究方向です、今後の研究や就職に役に立てると思います。しかし、私はそれについて専門知識は不十分です、そして成熟した技術なため、本当に役に立てると方向を決めても、新しい方法を提案して、実現し難い場合もあります。
そこで、表情認識研究する同時に、前のテーマ：株の予測も同時にやると思います。新し成果や自分の考えがあれば、先生に報告します。

## 参考文献：
1.	J Anil, “Literature survey on face and face expression recognition”, 2016
2.	Yani Zhu, “Face expression recognition based on equable principal component analysis and linear regression classification”, 2016
3.	Ashish Tawari, “Face Expression Recognition by Cross Modal Data Association”, 2013
4.	Guodong Guo, “Learning from examples in the small sample case: face expression recognition”, 2005
5.	Ali Moeini, “Real-World and Rapid Face Recognition Toward Pose and Expression Variations via Feature Library Matrix”, 2015
6.	Mihai Gavrilescu, “Study on using individual differences in facial expressions for a face recognition system immune to spoofing attack”, 2016
7.	Chao-Kuei Hsieh, “An Optical Flow-Based Approach to Robust Face Recognition Under Expression Variations”, 2010
8.	Jacob Whitehill, “he Faces of Engagement: Automatic Recognition of Student Engagementfrom Facial Expressions”, 2014



