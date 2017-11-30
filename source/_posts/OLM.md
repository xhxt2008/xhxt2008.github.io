---
title: Paper Survey about Mining Opinion Leaders in Big Social Network 
date: 2017-11-30 14:14:21
tags:
  - 未翻译
  - 社交网络
  - 意见领袖
  - 数据挖掘
  - 聚类
  - 英文
categories: 研究
---
## Abstract
 By identifying the opinion leaders, companies or governments can manipulate the selling or guiding public opinion, respectively. Additionally, detecting the influential comments is able to understand the source and trend of public opinion formation. However, mining opinion leaders in a huge social network is a challenge task because of the complexity of graph processing and leadership analysis. In this study, a novel algorithm, OLMiner, is proposed to efficiently find the opinion leaders from a huge social network.
<!-- more -->
## Introduction
### Opinion leaders
In a social network, the opinion leader means the influential person who may be an expert in a specific domain and have lots of people following his/her comments or ideas. opinion leaders are usually the information generator and message senders who familiar with the media by a secondary transmission. 
### Past research: utilize the characteristic analysis to find opinion leaders.
- the number of posted articles
- the number of articles replied by others
- the number of followers

### Limitation
- Influence overlapping problem: The discovered opinion leaders may have many common followers, hence, the influence only can affect a small set of people. 
- Time-consuming: The discovered opinion leaders may have many common followers  the analysis of network structure is time-consuming, especially for large social graphs.

###  Opinion Leader Miner(OLMiner)
1. OLMiner utilizes the post-and-follow relationships among individuals to quickly construct a social network. The time of user writing a post is also considered.
2. To tackle the influence overlapping problem, OLMiner proposes a clustering algorithm to effectively and efficiently discover the community structure of social network.
3. OLMiner utilizes K-mean clustering algorithm to exam the leadership quality Of each node and effective filtering out unpromising nodes.
4. OLMiner checks the size of input data to decide the execution environment, running on single computer or cloud environment.

## Related Works
### Opinion Analysis And Mining 
- Opinion Observer system: An opinion Observer system could analyze and compare the consumer opinions among competing products. Opinion Observer also uses language pattern mining method to extract product features from the Pros and Cons in a specific type of reviews. CopeOpi is an opinion mining system.
- Thesauruses: When mining opinion or analyzing sentiment, many thesauruses are generally utilized for measuring the orientation or the property of comment, such as WordNet and General Inquire for English lexicon, HowNet and NTUSD for Chinese lexicon. 

###  Opinion Leader Mining 
- Opinion leaders are domain-sensitive
- There are many papers modified PageRank algorithm to find opinion leaders, such as OpinionRank, LeaderRank and Dynamic OpinionRank. 

## Proposed Algorithm: OLMiner
![image](http://oonaavjvi.bkt.clouddn.com/OLM1.png)
1. We first check the size of input data to decide running on a single computer or cloud environment. 
2. Then, OLMiner utilizes the post-and-follow relationships among individuals to quickly construct a social network. When calculating the similarity (weight) of edge, we also consider the average time of posted article to tune the similarities among individuals. 
3. Then, for detecting opinion leaders, we propose an efficient algorithm to find out community structure and extract qualified community. 
4. Based on the discovered communities, we use clustering method to significantly shrink the candidate size of opinion leaders and avoid the influence overlapping problem. 
5. Finally, OLMiner selects the k best-quality users from the candidate set according to the score of measuring function. 

### Algorithm 1: OLMiner (A, U, k)
> Input: A: set of all articles published in a social website,
U: set of all users in a social website,
k: desired number of opinion leaders
Output: OL: a set of opinion leaders
01. OL  ; G  ; Cs  ; Candi_set;
02. Granularity_Checking (G);
03. G  Network_Construction (U, A);
04. Cs Community_Detection (G, k);
05. Candi_set  Candidate_Generation (Cs, k);
06. OL  Leader_Selection (Candi_set, k);
07. output OL;

### 1. granularity checking
check the size of input data to decide running on a single computer or cloud environment. 
### 2. Social Network Construction
- We segment 24 hours into four sections:
![image](http://oonaavjvi.bkt.clouddn.com/OLM02.png)


- Given a set of articles A and a set of users U, to build a social network **G(V, E, W)**.
- If users u, v∈U have reply-article-relation in A, an edge (u, v) exists in E.
- Set of adjacent nodes of a node u is defined as **adj(u)**.
- The similarity **sim(u, v)** is defined as the common adjacent users of u and v, 
![image](http://oonaavjvi.bkt.clouddn.com/OLM03.png)
- The weight **w(u, v)** of edge (u, v) in E.
![image](http://oonaavjvi.bkt.clouddn.com/OLM04.png)

### 3. Community Structure Detection
![image](http://oonaavjvi.bkt.clouddn.com/OLM05.jpg)
1. Firstly, capture the community structure of social network to reduce the execution time for finding opinion leaders. 
2. Then, we use 2nd-stage clustering to analyze the characteristics of uses and shrink the size of the candidate set.
3. Finally, we pick k users have better leadership quality from candidate set as opinion leaders. 
#### Modularity gain 
- Given a social network G = (V, E, W) and its clustering result C = {c1, c2, …, cp}, the modularity function is defined as:
![image](http://oonaavjvi.bkt.clouddn.com/OLM06.jpg)
- OLMiner utilizes the modularity gain as the terminated criteria.
#### Significant Community
- The set of significant community Cs is defined as follows:
![image](http://oonaavjvi.bkt.clouddn.com/OLM07.jpg)

### 4. Opinion Leader Candidate Generation
![image](http://oonaavjvi.bkt.clouddn.com/OLM08.jpg)
Different from others, we use kmean clustering to build the candidate set, the kmean clustering can effectively shrink the size of candidate set.
- Candidate Generation Algorithm(kmean)

> Input: Cs = {c1, c2,..., cn}: significant communities, k: user-specified number of opinion leaders 
Output: Candi_set: a candidate set 
1. Candi_set;
2. for each ci ∈ Cs do 
3. randomly select k nodes in ci as centroids s1,..., sk; 
4. CKi = { cki1, cki2,..., ckik} set each s1,..., sk as a cluster in CKi; 
5. while true do // kmean clustering 
6. for each u ∈ ci do 
7. calculate the distance to s1,..., sk; // based on four characteristics in Table 1 
8. if distance(sj) is shortest 
9. ckij ckij ∪ u; 
10. re-calculate the centroids of each cluster in CKi; // re-calculate each sj in ckij 
11. if no more node changes cluster 
12. break; 
13. …

- Given a clustering result CKi = {cki1, cki2, ..., ckin} in a significant community ci, the score of ckij is evaluated as, 
![image](http://oonaavjvi.bkt.clouddn.com/OLM09.jpg)

### 5. Opinion Leader Selection
- First, we use the total number of nodes in significant communities to decide how many opinion leaders are allocated to each significant community.
![image](http://oonaavjvi.bkt.clouddn.com/OLM10.jpg)
- Then, we first sort the clusters in decreasing order based on the score of cluster (line 4, Algorithm 5). Then, OLMiner picks the nodes in high-score cluster until we reach the number of desired opinion leaders k

## EXPERIMENTAL RESULTS
![image](http://oonaavjvi.bkt.clouddn.com/OLM11.jpg)
- The content of dataset include the subjects of article, article content, author, reply content, and the time of publish or reply articles.
- We compare OLMiner with att_clustering which use a clustering method only based on the characteristics of individuals to find opinion leaders. 
- we use the influence spread as metric
![image](http://oonaavjvi.bkt.clouddn.com/OLM12.jpg)
- The result of OLMiner is better than att_clustering.
![image](http://oonaavjvi.bkt.clouddn.com/OLM13.jpg)![image](http://oonaavjvi.bkt.clouddn.com/OLM14.jpg)
By our observation, the influence of att_clustering increase slowdown when n is greater than 130. We can clearly point out that influence overlapping problem is serious in this criterion. The steady increase of influence of OLMiner indicates that the proposed method can effectively reduce the impact of influence overlapping problems and discover opinion leaders with better qualities. 

## CONCLUSION
Recently, opinion leader discovery has drawn much attention due to its widespread applicability. In this paper, we develop a novel algorithm, OLMiner, which integrates network structure and leadership quality analysis methods to efficiently find opinion leaders in a large social network. We utilize the two-stage clustering methods to significantly reduce the impact of influence overlapping problem in social network. OLMiner also utilizes the sentiment analysis to distinguish the opinion trend of discovered opinion leaders. Finally, to mention the practicability, we perform the proposed algorithm on real datasets. The experimental results show that OLMiner could detect more qualified opinion leaders under different criteria and effectively solve the influence overlapping problem.











