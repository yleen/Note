# 在线电视剧的受众竞争力预测和分析 | KDD论文解读
>简介： 目前，网络视频平台的主要流量来自于热门电视剧，而平台的核心收益就是在这些流量上进行广告投放。通过准确预估剧目流量可以优化广告投放效果从而提高收益。但是，仅仅预测流量还不足以回答更深层次的问题。例如，平台未来要采购哪些剧目？这不仅要考虑剧目带来的流量，还要考虑平台内剧目的竞争关系，以避免造成热度内耗问题。所以，本文通过竞争力问题定义、算法设计以及实验对比，在剧目受众竞争力问题上进行了初步探索。

*作者：张鹏，刘传仁，宁克锋，祝文祥，张宇*


目前，网络视频平台的主要流量来自于热门电视剧，而平台的核心收益就是在这些流量上进行广告投放。通过准确预估剧目流量可以优化广告投放效果从而提高收益。但是，仅仅预测流量还不足以回答更深层次的问题。例如，平台未来要采购哪些剧目？这不仅要考虑剧目带来的流量，还要考虑平台内剧目的竞争关系，以避免造成热度内耗问题。所以，本文通过竞争力问题定义、算法设计以及实验对比，在剧目受众竞争力问题上进行了初步探索。
问题定义：
目前学术界并没有定义过剧目之间的竞争力，我们在调研过竞争力相关的文章后提出了一种剧目竞争力的定义。首先我们通过统计用户的观看次数，然后计算出两两剧目之间对用户观看次数的[相对占有量](#相对占有量)，最后对所有用户取平均作为最终的竞争力。


我们以周为单位计算得到竞争力，进一步可以构建成为竞争网络图，图的节点为剧目，边是竞争力，这张竞争网络图是动态的，随着时间推移不断变化，而我们要预测的是未来网络图中每条边的数值，也就是剧目之间的竞争关系。下图是动态竞争网络的示意图，在已知T-2、T-1、T时刻的竞争网络，要预测T+1时刻的竞争网络。值得注意的是，动态竞争网络中的剧目不是一成不变的，旧剧往往在大结局之后一段时间会消失，而新剧在首播时会出现。

![该图是个有向完全图](https://i.loli.net/2020/12/08/yw9nhm6tQ45ZRzc.png)

**算法设计：**

针对上述问题，我们结合深度神经网络和[知识库系统](#知识库)设计了一种动态深度网络分解框架，并命名为
Dynamic Deep Network Factorization (DDNF)。该框架可充分融合剧目的静态和动态特征以及竞争网络中的时序动态模式，优化剧目在动态竞争网络中的隐含表征，并用其有效预测未来的受众竞争力。框架共分为三个部分：第一部分是时序模块（Temporal Latent Factors），该模块利用[张量分解](#张量分解)从竞争网络中学习到剧目的时序隐含表征；第二部分是深度静态特征模块（Deep Embedding of Static Features），该模块利用知识库（KB）和深度神经网络（DNN）从静态特征中抽取剧目的关系和属性信息，静态特征主要包括了剧目的简介、题材、制作人员关系等；第三部分是循环动态特征模块（Recurrent Embedding of Dynamic Features），该模块利用长短期记忆网络（LSTM）从剧目的动态特征中抽取剧目的动态变化信息，动态特征包括了剧目每天的观看次数、点赞人数、更新状态等。最终将三个模块组合在一起，形成端到端的动态深度网络分解框架，示意图如下，绿色框表示时序模块，黄色框表示深度静态特征模块，紫色框表示循环动态特征模块。

![image.png](https://i.loli.net/2020/12/08/JqXwrsWnQlp8L2u.png)

**实验对比：**

我们利用某大型网络视频平台的历史数据，构建了剧目竞争力数据集，并利用该数据集进行实验。数据集包括了电视剧与综艺两个数据集，时间跨度为一年。同时，我们选取了经典矩阵分解算法PMF、时序矩阵分解算法BTMF[fully Bayesian treatment mode]、考虑额外信息的矩阵分解算法HBMFSI以及兼具时序和额外信息的ETF进行了对比，实验结果表明，我们的算法DDNF在两个数据集上都取得了最好的效果，同时，发现对于新剧的竞争力预估，DDNF表现更加突出。


![image.png](https://i.loli.net/2020/12/08/KO249rGdaUnMcoC.png)

**总结：**

针对网络电视剧目，本文首次提出了受众竞争力的建模和动态预测问题。论文首先通过挖掘剧目的观看记录构建一系列动态的竞争网络，然后结合深度神经网络和知识库系统设计了动态深度网络分解框架。该框架可以融合剧目的静态和动态特征以及竞争网络中的时序动态模式，优化剧目在动态竞争网络中的隐含表征，并用其有效预测未来的受众竞争力。通过预测剧目之间的竞争力刻画剧目的受众，对于视频平台的广告售卖、剧目采购计划、以及与其它平台的合作和竞争等决策任务。

参考：

https://developer.aliyun.com/article/771162
https://www.aminer.cn/conf/kdd2020/videos/5f03f3b611dc830562232035





##### 知识库

知识库功能是支撑机器人运行的主要功能，其中包括众多子功能，完善知识库能够提升机器人问答准确率，利用好知识卡片、知识关联、知识链接等功能能够更好的支撑系统运行，提高机器人效率，降低错误率。

##### 相对占有量
本剧目用户占有率与竞争剧目的用户占有率之比

##### 张量分解
https://zhuanlan.zhihu.com/p/24798389

在实际应用中，这里的评分矩阵往往是一个稀疏矩阵，即很多位置上的元素是空缺的，或者说根本不存在。试想一下，如果有10000个用户，同时存在10000部电影，如果我们需要构造一个评分矩阵，难道每个用户都要把每部电影都看一遍才知道用户的偏好吗？其实不是，我们只需要知道每个用户仅有的一些评分就可以利用矩阵分解来估计用户的偏好，并最终推荐用户可能喜欢的电影。

https://lumingdong.cn/recommendation-algorithm-based-on-matrix-decomposition.html#%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF

PMF:In the context of movie recommendation, PMF [19] mod-els the user-movie preference matrix as a product of twolower-rank user and movie matrices. PMF is a classic matrixfactorization method and offers a probabilistic foundationfor regularization with Gaussian noise.BTMF:Also in the movie recommendation context, BTMF [31]extends the conventional matrix factorization method byassuming user preferences change over time, and modelsthe dynamics with a transition matrix for user latent factorsbetween consecutive time slices.HBMFSI:To incorporate additional information for matrixfactorization, HBMFSI [23] places Gaussian-Wishart priorson mean vectors and precision matrices of latent factors,where the prior distribution mean is regressed on additionalinformation in a Bayesian approach.ETF:Using a similar problem formulation with our audiencecompetition networks, ETF [32] not only considers tempo-ral dynamics but also uses additional information to learndynamic latent factors for predicting future tensor slices





Thanks for watching this video for our paper prediction and profiling of audience competition for online television series. 
感谢收看我们的论文《在线电视剧观众竞争预测与分析》
![image.png](https://i.loli.net/2020/12/10/GDx3HjimJo94Iya.png)
Understanding the target audience for popular television series is valuable for online video platform to manage advertising sales and purchase video copyrights. 
了解热门电视剧的目标受众，对于网络视频平台管理广告销售和购买视频版权具有重要的价值。
Existing studies in this domain generally focus on using data mining and machine learning techniques to predict the popularity of television series. 
目前这一领域的研究主要集中在使用数据挖掘和机器学习技术来预测电视剧的受欢迎程度。
However, knowing only the popularity of television series may limit our ability to answer more in depth questions and develop more intelligent applications. 
但是，仅了解电视剧的普及程度不足以解决更深入的问题并开发更智能的应用程序
![image.png](https://i.loli.net/2020/12/10/hbtcvUIMwPZ6iQe.png)
For example, which television series should the platform purchase in the future? Such tests should consider not only the popularity of the new television series, but also the audience competition with other television series on the platform.     
例如，平台将来应该购买哪部电视剧？ 这种预测不仅要考虑新电视剧的普及程度，还要考虑平台上其他电视剧的观众竞争
![image.png](https://i.loli.net/2020/12/10/OlwpIrQDHkhiBNa.png)
To study the audience competition of television series, first we define the competitiveness between television series is a network based on historical audience behaviors. 
为了研究电视剧观众竞争，首先我们定义电视连续剧之间的竞争性是一个基于观众历史行为的网络。
Here this figure provides an intuitive demonstration of the audience competition networks. 
此图直观地展示了观众竞争网络。
The network edges between the television series indicate the competition relationships.
电视剧之间的连线表示了竞争关系
The arrows indicate the competition directions, and thicknesses indicate the degrees of the competition. 
方向表示竞争关系，厚度表示竞争权重
A research problem is to predict the future competition networks, given a sequence of historical competition networks. 
本研究课题是通过连续的历史竞争来预测未来的竞争网络。
![image.png](https://i.loli.net/2020/12/10/kQ6UhRcSFgBuq7I.png)
![image.png](https://i.loli.net/2020/12/10/jMkS5iG3KNsBae6.png)
To predict the competition networks, we develop the dynamic network factorization framework. 
为了预测竞争网络，我们提出了动态网络框架
Specifically we use temporal leading factors depending on static features and recurrent embedded in a dynamic feature to join the model the dynamic competition networks for online television series. 
具体地说，我们使用依赖于静态特征和周期性嵌入动态特征的时间主导因素来加入在线电视连续剧的动态竞争网络模型。
![image.png](https://i.loli.net/2020/12/10/cayontrV1kLZeph.png)
Our approach can support many interesting applications. 
我们的方法可以支持许多有趣的应用程序。
For instance, this figure shows the audience profiling of a new television series based on the predicted competition network. 
例如，此图显示了用我们的预测方法来预测新电视连续剧的观众概况
The prediction by our approach is very close to the observation. 
通过我们的方法进行的预测非常接近观察结果。

具体来说，我们可以根据竞争模式决定新电视剧的发行时间和更新策略。 如果预测一个新节目对现有节目来说太竞争了，我们可以推迟或错开发布时间，以确保平台的整体受欢迎程度

More details and results can be found in this paper. 
Thanks for watching.




##### recurrent embedded
recurrent embedded
循环嵌入??



网络视频平台的主要流量来自于热门电视剧，而平台的核心收益就是在这些流量上进行广告投放。
平台未来要采购哪些剧目？这不仅要考虑剧目带来的流量，还要考虑平台内剧目的竞争关系，以避免造成热度内耗问题。

目前这一领域的研究主要集中在使用数据挖掘和机器学习技术来预测电视剧的受欢迎程度。

为了研究电视剧观众竞争，我们在调研过竞争力相关的文章后提出了一种剧目竞争力的定义
首先我们通过统计用户的观看次数，然后计算出两两剧目之间对用户观看次数的相对占有量，最后对所有用户取平均作为最终的竞争力。
图的节点为剧目，边是竞争力，这张竞争网络图是动态的，随着时间推移不断变化，而我们要预测的是未来网络图中每条边的数值，也就是剧目之间的竞争关系。下图是动态竞争网络的示意图，
在已知T-2、T-1、T时刻的竞争网络，要预测T+1时刻的竞争网络。值得注意的是，动态竞争网络中的剧目不是一成不变的，
旧剧往往在大结局之后一段时间会消失，而新剧在首播时会出现。

本研究课题是通过连续的历史竞争来预测未来的竞争网络。
为了预测竞争网络，我们提出了动态网络框架
针对上述问题，我们结合深度神经网络和[知识库系统](#知识库)设计了一种动态深度网络分解框架，并命名为Dynamic Deep Network Factorization (DDNF)。该框架可充分融合剧目的静态和动态特征以及竞争网络中的时序动态模式，优化剧目在动态竞争网络中的隐含表征，并用其有效预测未来的受众竞争力。框架共分为三个部分：第一部分是时序模（Temporal Latent Factors），该模块利用[张量分解](#张量分解)从竞争网络中学习到剧目的时序隐含表征；第二部分是深度静态特征模块（Deep Embedding of Static Features），该模块利用知识库（KB）和深度神经网络（DNN）从静态特征中抽取剧目的关系和属性信息，静态特征主要包括了剧目的简介、题材、制作人员关系等；第三部分是循环动态特征模块（Recurrent Embedding of Dynamic Features），该模块利用长短期记忆网络（LSTM）从剧目的动态特征中抽取剧目的动态变化信息，动态特征包括了剧目每天的观看次数、点赞人数、更新状态等。最终将三个模块组合在一起，形成端到端的动态深度网络分解框架，示意图如下，绿色框表示时序模块，黄色框表示深度静态特征模块，紫色框表示循环动态特征模块。