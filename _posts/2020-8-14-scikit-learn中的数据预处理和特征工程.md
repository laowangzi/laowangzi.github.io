---
published: true
title: scikit-learn中的数据预处理和特征工程
category: python
tags: 
  - scikitlearn
  - python
layout: post

---
##数据预处理（Preprocessing&Impute ）
数据预处理中所用类的使用和模型使用步骤相同。都是分三步：1.实例化 2.fit数据 3.调用需要的接口
###数据的无量纲化
在一些使用梯度下降法的算法中，数据范围过大会导致算法运行时间过长，因此需要数无量纲化将大范围数据放缩到0到1之间以提高收敛速度。
####数据归一化
在sklean中使用processing.minmaxscaler做数据归一化。实际公式是x=(x-xmin)/(xmax-xmin)
####数据标准化
公式为x'=(x-u)/o,目的是将数据变化为正态分布。使用StandardScaler.
       
    scaler=StandardScaler()
    scaler.fit(data)
    scaler.mean_     返回均值
    scaler.var_      返回方差 
    sca_data=scaler.transform(data)    通transform接口导出数据

####标准化与归一化的选择
大部分时间选择标准化。在不涉及距离的时候可以选择归一化。
##缺失值处理
sklearn中缺失值填充只有Impute.SimpleImputer这一个类。

    from sklearn.impute import SimpleImputer

    参数
    missing_values  告诉缺失值是什么，默认np.nan
    strategy        选择缺失值填充策略。可选'constant'(定值填充),'mean','median'(中值),'most_frequent'(众数，对数值型和字符型特征都可用)
    fill_value      搭配constant参数使用。同时适用于数值型和字符型。
    copy            默认为True,将创建新的填补过的数据副本。false为将缺失值填补到原矩阵
##编码与哑变量
###编码
很多模型处理的数值只能是数字，而收集到的数据有可能是文字，此时就需要进行编码。
####编码标签
Processing.LabelEncoder,用法与建立模型相同（三部曲）。在fit数据之后可以通过.class属性查看属性。transform结束后再赋值回去即可。
####特征标签
Processing.OrdinalEncoder.
###独热编码
preprocessing.OneHotEncoder用来编码数字无法准确表达的变量。比如‘红=1，黄=2，蓝=3’这样labelencoder编码**不合适**。独热编码就是一个比特代表一类，比如红黄蓝可用三位001来标识，这样避免出现偏序性影响算法效果。在转换后需要.toarry（）
###二值化和分段
####二值化
将特征值设置为0和1。preprocessing.Binarizer。在实例化时使用threshhold参数设定阈值。
####分段
preprocessing.KBinsDiscretizer。将连续变量划分为分类变量。
     
    n_bins     每个特征中分箱个数。会被应用于所有特征，默认值5.
    encode     编码方式，默认onehot
    strategy   箱宽策略：uniform(等宽),quantile（等样本数）,kmeans（聚类分箱）。默认quantile

##特征工程
包括特征提取，特征创造（通过特征之间相互运算创造新的特征）和特征选择。特征工程的第一步是理解业务！
###特征选择
特征太多时需要筛选特征，这个过程独立于机器学习过程。可使用过滤法。
####过滤Filter
#####方差过滤VarianceThreshold
通过特征本身方差来筛选特征。方差太小的特征被认为对区分样本贡献很小，优先删除。一般只会用方差为0的预处理一下，然后跟进使用其他更优的筛选方法来进一步筛选。

    from sklearn.feature_selection import VarianceThreshold   #注意来源
    之后的使用和前边一样，三部曲。不填threshold默认为0
    如果想让特征直接减半，可以将特征们方差的中位数设置为threshold。通过numpy中np.median(X.var().values)获得特征们的中位数（X.var()代表每一列的方差，取出来带索引，因此需要.values取值）
#####卡方过滤
    from sklearn.modelselection import SelectKBest
    from sklearn.modelselection import chi2
    x=SelectKBest(chi2,k=300)     #chi2代表告诉KBest，我使用的统计量是卡方。k=300代表我需要300个特征。
可以使用学习曲线来确定参数k。更好的也可以根据p值选择k

                                                                                                                                                                                