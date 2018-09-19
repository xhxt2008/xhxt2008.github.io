---
title: 平安产险2018极客挑战赛·初赛（一）
date: 2018-09-19 14:56:00
toc: true
tags: 
- 机器学习
- 数据分析
- 竞赛
- 平安产险
categories: 技术
mathjax: true
---
## 初赛数据预处理

### 概览
 经过上一阶段对数据分析，我们大体上根据情况的不同，有以下四种处理方式：
 1. **删除特征**：null值过多的特征
 2. **填充特征的null值**：有一些null值，可以依据一些规则，使填充值的影响最小。如填充平均值，众数，奇异值等等。
 3. **二值化**：对于只有两种状体啊的特征。如：male，female。
 4. **独热编码**：对于有多种状态的特征，如：地区信息
 5. **特殊处理方法**：依据具体情况
 
### 删除特征


```
'''
for delete: grade,zip_code,inq_last_12m,total_cu_tl,open_acc_6m,open_il_12m,open_il_24m,open_il_6m,total_bal_il,
open_rv_12m,open_rv_24m,max_bal_bc,all_util,inq_fi,policy_code,emp_title,emp_title,desc,title,zip_code,dti_joint,
annual_inc_joint,varification_status_joint,il_uti,mths_since_last_record,maddr_state,
issue_d,earliest_cr_line
新增：loan_status,addr_state,application_type,pymnt_plan
'''
>>> feature_delete = {'grade','zip_code','inq_last_12m','total_cu_tl','open_acc_6m','open_il_12m','open_il_24m',
                  'open_il_6m','total_bal_il','open_rv_24m','open_rv_12m','max_bal_bc','all_util','inq_fi',
                 'emp_title','policy_code','desc','title','dti_joint','annual_inc_joint','addr_state','mths_since_rcnt_il',
                 'verification_status_joint','il_util','mths_since_last_record','issue_d','earliest_cr_line',
                  'loan_status','addr_state','application_type','pymnt_plan'}
>>> data = data.drop(feature_delete,axis=1)
```
在上一篇博客中，我们分析了数据集中的空值情况。以上的特征由于各种原因我们予以删除的。除了null值过多的特征意外，比方说'zip_code'邮政编码，过于细化，而且跟地理信息重复。两个特征如果相关性过高（协方差大于0.95），我们也会删除到只剩一个。另外如果我们把某个特征转化成别的信息之后，我们也会删除原特征。

### 填充null值


```
'''
少量null值的处理，用均值替代：emp_length, revol_util, annual_inc, total_acc, earliest_cr_line, tot_cur_bal, tot_coll_amt
total_rev_hi_lim, revol_util, pub_rec, collections_12_mths_ex_med
减少：emp_length
'''
>>> feature_fill_null = {'revol_util','annual_inc','total_acc','earliest_cr_line_year','earliest_cr_line_month','tot_cur_bal',
                    'tot_coll_amt','total_rev_hi_lim','revol_util','pub_rec','collections_12_mths_ex_med'}
>>> def fill_null(feature_fill_null,data):
>>>     for items in feature_fill_null:
>>>         try:
>>>             time.strptime(data[items].mode()[0],"%b-%Y")
>>>             data[items] = data[items].fillna(data[items].mode()[0]) #日期的空值用众数替代
>>>             print (data[items].mode()[0])
>>>         except:
>>>             data[items] = data[items].fillna(data[items].mean()) #其他空值用均值替代
>>>             print (data[items].mean())
>>>     return data

>>> fill_null(feature_fill_null,data)
```

填充null值这里有两种
（或三种）情况。
- 如果特征是日期型变量，我们取其中最多的日期作为填充值，以减小null值的冲击。
- 如果特征值是数值类型，如雇佣时长，总贷款额等。我们计算正常值的平均数作为填充值。
- 如果null值有特殊的意义（我们想象逾期未补款时间，null为没有逾期或者没有借款，区别于0），我们应该赋予null一个异常值，比如说一个较大的数。

### 二值化

```python
#for binarilization：pymnt_plan,term, application_type, initial_list_status
#减少：pymnt_plan，application_type  增加：loan_condition,region
>>> feature_for_binary = {'term','initial_list_status','loan_condition','region'}
>>> def binary(feature_for_binary,data):
>>>     for items in feature_for_binary: 
>>>         mapping = {label:idx for idx,label in enumerate(set(data[items]))}  
>>>         data[items] = data[items].map(mapping) 
>>>     return data
    
>>> binary(feature_for_binary,data)
```
二值化很好理解，经过分析，我们认为上面的两个属于只有两个取值的离散型变量。

### 独热编码


```python
#for get_dummies: home_ownership, verification_status, loan_status,purpose
#减少：loan_status

>>> feature_for_dummy = {'home_ownership','verification_status','purpose'}
>>> def multi_get_dummies(feature_for_dummy,data):
>>>     for items in feature_for_dummy: 
>>>         data = data.join(pd.get_dummies(data[items],prefix=items))
>>>         data.pop(items)
>>>     return data    

>>> data = multi_get_dummies(feature_for_dummy,data)
```
对于离散多类型变量，我们为了取消类型之间的线性相关性，不能直接对他们编号。需要使用独热编码对不同类型进行编号。
值得注意的是，独热编码在特征类型很多的情况下会显著增加特征的维度，造成特征过于稀疏不利于学习。因此对于特征种类过多的我们进行了特殊处理，后面会详细介绍。

### 特殊的处理

#### 信用评级sub_grade和工作年限emp_length

```python
>>> sub_grade_mapping = {'A1':0,'A2':1,'A3':2,'A4':3,'A5':4,
                     'B1':5,'B2':6,'B3':7,'B4':8,'B5':9,
                     'C1':10,'C2':11,'C3':12,'C4':13,'C5':14,
                     'D1':15,'D2':16,'D3':17,'D4':18,'D5':19,
                     'E1':20,'E2':21,'E3':22,'E4':23,'E5':24,
                     'F1':25,'F2':26,'F3':27,'F4':28,'F5':29,
                     'G1':30,'G2':31,'G3':32,'G4':33,'G5':34}

>>> data.sub_grade = data.sub_grade.map(sub_grade_mapping)
```

```python
>>> emp_length_mapping = {'10+ years':10,'9 years':9,'8 years':8,'7 years':7,'6 years':6,
                       '5 years':5,'4 years':4,'3 years':3,'2 years':2,'1 year':1,'< 1 year':0.5,'n/a':0}
>>> data.emp_length = data.emp_length.map(emp_length_mapping) 
```
不同的信用评级和工作年限是有线性关系的，为了准确学出这种关系，我们把这两个字段映射成对应的数字。
再对null值的处理上，信用评级我们采用平均值填充，但是对于工作年限，我们认为null可能是没有工作，我们填充0来表示。

#### 地址所在的州addr_state
```python
# 地域划分addr_state->region
>>> west = ['CA', 'OR', 'UT','WA', 'CO', 'NV', 'AK', 'MT', 'HI', 'WY', 'ID']
>>> south_west = ['AZ', 'TX', 'NM', 'OK']
>>> south_east = ['GA', 'NC', 'VA', 'FL', 'KY', 'SC', 'LA', 'AL', 'WV', 'DC', 'AR', 'DE', 'MS', 'TN' ]
>>> mid_west = ['IL', 'MO', 'MN', 'OH', 'WI', 'KS', 'MI', 'SD', 'IA', 'NE', 'IN', 'ND']
>>> north_east = ['CT', 'NY', 'PA', 'NJ', 'RI','MA', 'MD', 'VT', 'NH', 'ME']

>>> data['region'] = np.nan

>>> def finding_regions(state):
>>>     if state in west:
>>>         return 'West'
>>>     elif state in south_west:
>>>         return 'SouthWest'
>>>     elif state in south_east:
>>>         return 'SouthEast'
>>>     elif state in mid_west:
>>>         return 'MidWest'
>>>     elif state in north_east:
>>>         return 'NorthEast'
    
>>> data['region'] = data['addr_state'].apply(finding_regions)
```
美国有50个州，如果对州这个字段进行独热编码将会多出50个特征。这显然是不划算的，因此，我们把50个州根据美国的地理特点划分成东北，西北，东南，西南，西部五个区域，然后在进行独热编码。

#### 日期类特征issue_d，earliest_cr_line
```python
>>> data.issue_d = pd.to_datetime(data['issue_d'])
>>> data.earliest_cr_line = pd.to_datetime(data['earliest_cr_line'])

>>> data['issue_d_year']=data.issue_d.dt.year
>>> data['issue_d_month'] = data.issue_d.dt.month
>>> data['earliest_cr_line_year']=data.earliest_cr_line.dt.year
>>> data['earliest_cr_line_month'] = data.earliest_cr_line.dt.month
```
通过上一篇的分析我们发现，贷款量跟年份是有关系的，因此我们把原本的年-月信息处理分开成两个特征，分别记录年月。这样我们的模型就能学到贷款状态与年份，和月份的信息。

#### mths_since_last_major_derog

这个字段记录了上次违约之后过去的月数，有大量（约70%）的null值，我们最初把这个特征删除了，怎么训练精度都很难提高，这里要提出来注意一下。
这里都null值更多指的是压根没有违约，所以不应该赋值为0，那将表示刚刚违约了。我们应当将null赋一个较大的数值，或者如果使用基于决策树都模型，也可能直接留着null值不用处理。