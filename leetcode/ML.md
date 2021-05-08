![](https://upload-images.jianshu.io/upload_images/18339009-06d5ce952a0eb80b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/18339009-fde0401c30936d0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/18339009-ef63be85b387ddf2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/18339009-bab7bcbd5d47f0aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](https://upload-images.jianshu.io/upload_images/18339009-0b715b33ee892114.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



# 激活函数
需要激活函数来引入非线性因素，使得神经网络可以任意逼近任何非线性函数。

![](https://upload-images.jianshu.io/upload_images/18339009-06a800d974c5405f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 范数
![](https://upload-images.jianshu.io/upload_images/15873283-f0bb69bddf469915.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





# 什么是最小二乘法？

最小二乘的核心思想---通过**最小化误差的平方和**，使得拟合对象无限接近目标对象.


# 常见的距离度量公式有那些？

1)Minkowski(闵可夫斯基距离)
 --- P=1:曼哈顿距离
--- P=2:欧式距离
--- P无穷:切比雪夫距离

2) 夹角余弦相似度
3) KL距离(相对熵)
4) 杰卡德相似度系数(集合)
5) Pearson相关系数ρ(距离=  1-ρ)






# 8.缺失值处理
删除：缺失数据太多

不处理：针对类似XGBoost等树模型

插值：统计量：均值、众数、中位数、

补全：感知压缩补全、最邻近补全、矩阵补全、建模预测、多重插补

分箱：（缺失值一个箱）
# 异常值处理

箱线图(没有对数据作任何限制性要求)
3-$\sigma$(Sigma)(符合正态分布）
BOX-COX转换（处理有偏分布）
长尾截断
聚类、k近邻、One Class SVM、Isolation Forest

>关于高势集特征model，也就是类别中取值个数非常多的， 一般可以使用聚类的方式，然后独热
# 数据分桶
等频分桶：区间的边界值要经过选择,使得每个区间包含大致相等的实例数量。
等距分桶：从最小值到最大值之间,均分为 N 等份；
Best-KS分桶：类似利用基尼指数进行二分类；
卡方分桶：自底向上的(即基于合并的)数据离散化方法。它依赖于卡方检验：具有最小卡方值的相邻区间合并在一起,直到满足确定的停止准则。
>为什么要做数据分桶呢？
离散后**稀疏向量内积乘法运算速度更快**，计算结果也方便存储，容易扩展；
离散后的特征对异常值**更具鲁棒性**，如 age>30 为 1 否则为 0，对于年龄为 200 的也不会对模型造成很大的干扰；
LR 属于广义线性模型，表达能力有限，经过离散化后，每个变量有单独的权重，这相当于**引入了非线性**，能够提升模型的表达能力，加大拟合；
离散后特征可以进行**特征交叉**，提升表达能力，由 M+N 个变量编程 M*N 个变量，进一步引入非线形，提升了表达能力；

# 特征选择
### 过滤式（filter）：
概述：按照发散性或者相关性对各个特征进行评分，设定阈值或者待选择阈值的个数，选择特征。然后再训练学习器。

方法： Relief、方差选择、相关系数、卡方检验、互信息
>数值型特征，方差很小的特征可不要
分类特征,取值个数高度偏斜的那种可以先去掉

>连续数据，正态分布，线性关系，用pearson相关系数是最恰当，当然用spearman相关系数也可以，效率没有pearson相关系数高。上述任一条件不满足，就用spearman相关系数，不能用pearson相关系数。

>两个定序测量数据（顺序变量）之间也用spearman相关系数，不能用pearson相关系数。
Pearson相关系数的一个明显缺陷是，作为特征排序机制，他只对线性关系敏感。如果关系是非线性的，即便两个变量具有一一对应的关系，Pearson相关性也可能会接近0。

>卡方检验一般是检查**离散变量与离散变量的相关性**，当然离散变量的相关性**信息增益**和**信息增益比**也是不错的选择（可以通过决策树模型来评估来看）

>互信息，利用互信息从信息熵的角度分析相关性

###  包裹式（wrapper）：

概述：直接把最终将要使用的学习器的性能作为特征子集的评价准则，根据目标函数（通常是预测效果评分），每次选择若干特征，或者排除若干特征。

方法：LVM（Las Vegas Wrapper）递归特征消除算法, 基于机器学习模型的特征排序

优缺点：
从最终学习器的性能来看，包裹式比过滤式更好；
计算开销通常比过滤式特征选择要大得多。

### 嵌入式（embedding）：

概述：结合过滤式和包裹式，学习器训练过程中自动进行了特征选择。先使用某些机器学习的算法和模型进行训练，得到各个特征的权值系数，根据系数从大到小选择特征。

方法：LR+L1、决策树、lasso回归

使用L1正则化的模型建叫做Lasso回归（权值稀疏），使用L2正则化的模型叫做岭回归（权值小）。




# 学习率
α的取值要合适，太小太慢，太大震荡。
选取α的经验，从……0.001—>0.01—>0.1—>1…
![image.png](https://upload-images.jianshu.io/upload_images/15873283-dd6aba8f1823b2ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
参数的更新公式为：
$𝒘_{𝒏+𝟏} = 𝒘_𝒏 − 𝒍𝒆𝒂𝒓𝒏𝒊𝒏𝒈\_𝒓𝒂𝒕𝒆𝛁$
######指数衰减学习率：
学习率随着训练轮数变化而动态更新
学习率计算公式如下：$$decayed\_learning\_rate=LEARNING\_RATE\_BASE*decay\_rate^{(global\_step/decay\_steps)}$$


# 什么是判别式模型和生成式模型
监督学习的任务是学习一个模型，对给定的输入预测相应的输出，这个模型的一般形式为一个**决策函数**或一个**条件概率分布**（后验概率）：
$Y=f(X)/P(Y|X)) $
**决策函数**：输入 X 返回 Y；其中 Y 与一个**阈值**比较，然后根据比较结果判定 X 的类别
**条件概率分布**：输入 X 返回 **X 属于每个类别的概率**；将其中概率最大的作为 X 所属的类别

假设我们有训练数据$(X,Y)$，$X$是属性集合，$Y$是类别标记。这时来了一个新的样本$x$，我们想要预测它的类别$y$,我们最终的目的是求得最大的条件概率$P(y|x)$作为新样本的分类。

## 判别式模型 (Discriminative Model)：
直接学习决策函数或者条件概率分布，直观来说，学习的是类别之间的最优分隔面，反映的是不同类数据之间的差异，无法反映训练数据本身的特性，反映的是不同类数据之间的差异。
常见判别模型有：线性回归、决策树、支持向量机SVM、k近邻、神经网络等；
## 生成式模型 (Generative Model)：
一般会对每一个类建立一个模型，有多少个类别，就建立多少个模型。比如说类别标签有｛猫，狗，猪｝，那首先根据猫的特征学习出一个猫的模型，再根据狗的特征学习出狗的模型，之后分别计算新样本$x$ 跟三个类别的联合概率 $P(y，x)$ ，然后根据贝叶斯公式：  
$ P(y|x) = \frac{P(x, y)}{P(x)}$
分别计算$P(y|x)$，选择三类中最大的$P(y|x)$作为样本的分类。
常见生成式模型有：**隐马尔可夫模型HMM、朴素贝叶斯模型、高斯混合模型GMM、LDA等；**
- 不管是生成式模型还是判别式模型，它们~~最终的判断依据都是条件概率$P(y|x)$~~，但是**生成式模型先计算了联合概率$P(y，x)$，再由贝叶斯公式计算得到条件概率。**因此，生成式模型可以体现更多数据本身的分布信息，其普适性更广。判别式模型更直接，目标性更强。

- 判别模型简单，准确率更高，不能反映训练数据本身的特性
- 由~~生成式模型可以产生判别式模型~~，但是由判别式模式没法形成生成式模型，~~当存在“隐变量”时，只能使用生成模型~~（隐变量：当我们找不到引起某一现象的原因时，就把这个在起作用，但无法确定的因素，叫“隐变量)

# 决策树的常用算法有那些，这些算法有什么区别？
常用的有:ID3,C4.5,CART三种算法

区别:
1) 纯度量化指标不同.ID3-->信息增益,C4.5-->信息增益率,CART-->基尼系数
2) 数据处理能力不同. ID3-->离散数据,C4.5,CART--->连续数据离散化,剪枝操作
3)要求的树的类型不同.ID3和C4.5可以是多叉树,CART只能是二叉树.


# rf和lr对比 ， 其他的算法懂嘛

# 归一化有用吗



（扒，谈到了3中分析的角度，分别适用于什么情况）

# 数据不同类型怎么处理？

# 数据波动大怎么处理？

# lr加正则项？

# lr和线性回归的异同

# lr的优劣（为什么在金融领域常用）

# hivesql是否了解

# 如何衡量分类结果的合理性？

# 淘宝可以改进的需求？

# 双十一活动效果分析的核心指标？提供补贴或者活动形式的建议？

# sql题：连续两次下单时间间隔小于一天的用户count

# 会用Excel？最常用什么函数？

# 介绍svm，介绍xgb

# 如何建模的，如何做的策略

# 从统计谈到了机器学习，从机器学习聊到了nlp的attention

# 讲一下核方法

# 讲一下svm推倒



#

# 一道sql题，关于用户多次发起订单的情况取最近的一次以及相关的信息

# rank() 和 dense rank（）区别

# 一个场景关于用户是否会愿意使用拼车这个功能，选用什么方法做预测哪些用户会使用拼车

# 关于上面方法的使用的优劣讨论

# 成交额下降怎么分析

# AB测试，统计学相关 ,假设检验

#

# 决策树和随机森林比较

# 关于剪枝

# 一个场景关于成单率的分析，成单率低是由什么原因导致的

# 如何量化一些指标做分析

# 网易严选8.8数据分析

1、严选为提升销售额和用户体验开通花呗免息活动

（1）如何设置分期门槛（如满多少免息）、期数（如三期、六期等），说说思路和原因。

（2）如何评估活动的效果，考虑哪些指标。

2、（1）云音乐推出年卡活动，两种方式：年卡买一年送一年；年卡买一年送两年的会员权益。如何评价哪种方式好。

（2）云音乐和严选开展联合会员活动，买黑胶vip送严选pro会员，如何评价活动的成本效益。

二、SQL（30*2）

1、商品表goods(id,name,weight)，交易表（id,goods_id,count）。求销售数量>20，重量<50的商品id，重量，销售总数。

2、云音乐：关注表（user_id,follower_id）,音乐喜爱表（user_id,music_id），音乐表(music_id,music_name), 为user_id=1的用户推荐其关注者喜爱的音乐名称（不重复），不要推荐自己本身喜欢的，音乐按id升序排列。

[复制代码](#)

<pre spellcheck="false" class="md-fences md-end-block ty-contain-cm modeLoaded" lang="" cid="n102" mdtype="fences" style="box-sizing: border-box; overflow: visible; font-family: var(--monospace); font-size: 0.9em; display: block; break-inside: avoid; text-align: left; white-space: normal; background-image: inherit; background-position: inherit; background-size: inherit; background-repeat: inherit; background-attachment: inherit; background-origin: inherit; background-clip: inherit; background-color: rgb(248, 248, 248); position: relative !important; border: 1px solid rgb(231, 234, 237); border-radius: 3px; padding: 8px 4px 6px; margin-bottom: 15px; margin-top: 15px; width: inherit; color: rgb(51, 51, 51); font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">`select distinct c.id,c.name,c.weight,c.total from``(select b.id,b.name,b.weight,sum(count) over (partition by id) as total from ``(select g.id,g.name,g.weight,t.count from goods g left join trans t on g.id = t.goods_id)``as b) as c where c.weight < 50 and c.total > 20`</pre>

[复制代码](#)

<pre spellcheck="false" class="md-fences md-end-block ty-contain-cm modeLoaded" lang="" cid="n104" mdtype="fences" style="box-sizing: border-box; overflow: visible; font-family: var(--monospace); font-size: 0.9em; display: block; break-inside: avoid; text-align: left; white-space: normal; background-image: inherit; background-position: inherit; background-size: inherit; background-repeat: inherit; background-attachment: inherit; background-origin: inherit; background-clip: inherit; background-color: rgb(248, 248, 248); position: relative !important; border: 1px solid rgb(231, 234, 237); border-radius: 3px; padding: 8px 4px 6px; margin-bottom: 15px; margin-top: 15px; width: inherit; color: rgb(51, 51, 51); font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;">`select music_name from``(select f.user_id,f.follwer_id,mm.music_id,nn.id,nn.music_name from follow f ``left join music_likes1 mm on f.follwer_id = mm.user_id LEFT JOIN ``music nn on nn.id = mm.music_id where f.user_id = 1 ) as p ``where music_id not in (select music_id from music_likes1 where user_id = 1) ``order by music_id asc`</pre>

\1\. 面试官自我介绍

\2\. 面试官介绍总的面试结构：一共四次

\3\. 面试开始：（问题顺序我可能记的不清楚了）

① 自我介绍

② 面试官追问：想知道我本科和研究生GPA、专业排名、自己认为在横向中的位置

③ 追问：简历中的某次竞赛获奖经历（介绍竞赛项目内容、含金量、分工、评委评价、其他同届获奖作品介绍）

④ 追问：有没有其他专利、学术论文发表

⑤ 具体聊研究生期间的研究方向和项目经历。（项目的复盘，项目架构和逻辑，深挖数据来源、分析过程中研究方法的选择标准、项目中的局限性、反思和优化空间）【这部分占时最长】

⑥ 实习经历+过程中遇到的困难，如果类似问题再发生，会如何去做？

⑧ 提问：认为很有成就的事情。

⑨ 提问：说一件一直在坚持的事情。

\4\. 面试主要部分结束，轮到我提问，我问了如下两个问题：

① 面试官对我今天的表现，有什么评价。（这部分面试官讲了很多，表示同期面试的背景很强的人很多，我从简历上并不是很占优势；并指出我讲自己简历的过程中需要注意逻辑，之后ta还给用高德产品给我做了示范。）

② 岗位的具体工作，今后可能负责到的产品设计和优化的场景。

# 解释Markov Chain（简历写了学过随机过程）

# SQL各种join的区别（感觉很多面试关于SQL可能都会问）

# 如何评判模型指标，除了准确率，

因为简历有Kaggle机器学习竞赛的经历，所以问说这个的主题是什么，然后为什么选择这些模型

# 解释XGBoost相对于GBDT有什么优点

# 如何处理缺失值

# 实习)监控指标包括哪一些，为什么监控指标要这样设置，如何进行监控；

# 成本和营销费用怎么关联；

# 深拷贝和浅拷贝的区别

可变对象：可以修改的对象，包括列表、字典、集合

在浅拷贝时，拷贝出来的**新对象的地址和原对象是不一样**的，但是新对象里面的**可变元素（如列表）的地址和原对象里的可变元素的地址是相同的**，也就是说浅拷贝它拷贝的是浅层次的数据结构（不可变元素），对象里的可变元素作为深层次的数据结构并没有被拷贝到新地址里面去，而是和原对象里的可变元素指向同一个地址，所以在新对象或原对象里对这个可变元素做修改时，两个对象是同时改变的，但是深拷贝不会这样

赋值： 值相等，地址相等

copy浅拷贝：值相等，地址不相等

deepcopy深拷贝：值相等，地址不相等

# 列表和元组的区别

1.  列表是动态数组，它们可变且可以重设长度（改变其内部元素的个数）。

2.  元组是静态数组，它们不可变，且其内部数据一旦创建便无法改变。 但是我们可以将两个元组合并成一个新元组。

3.  元组缓存于Python运行时环境，这意味着我们每次使用元组时无须访问内核去分配内存。

    > Python是一门垃圾收集语言，这意味着当一个变量不再被使用时，Python会将该变量使用的内存释放回操作系统，以供其他程序（变量）使用。然而，对于长度为1~20的元组，即使它们不在被使用，它们的空间也不会立刻还给系统，而是留待未来使用。这意味着当未来需要一个同样大小的新的元组时，我们不再需要向操作系统申请一块内存来存放数据，因为我们已经有了预留的空间。

工作表，制作一个表  仪表盘，放多个表  故事，ppt

甘特图,可视化进度

[图片上传失败...(image-e2d2b5-1602807317823)] 

MODE()众数

quartile分位数

stdev 标准差（standard deviation）variance方差

![img](https://upload-images.jianshu.io/upload_images/18339009-e94b1c23eb15233a?imageMogr2/auto-orient/strip%7CimageView2/2/w/673/format/webp) 

异常值检测：

![image](https://upload-images.jianshu.io/upload_images/18339009-2f06030e973d5a3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
