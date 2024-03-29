

# 决策树

整个决策树的学习过程就是一个递归地选择最优特征，并根据该特征对数据集进行划分，使得各个样本都得到一个最好的分类的过程。
![决策树模型由内部结点和叶子节点构成，内部节点表示特征或者属性，叶子节点表示类标签](https://upload-images.jianshu.io/upload_images/18339009-14654252382d2fbf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**包括三个过程：特征选择、决策树生成和决策树剪枝 。**


## 特征选择
构建决策树的过程中**特征要怎么选择**？(纯度）
答：当然我们是想去找一个分类效果或者说区分度较大的特征把数据集进行分开。那么这个**分类效果或者区分度怎么去衡量**？  （信息增益）

![](https://upload-images.jianshu.io/upload_images/18339009-79098dc6bcfea36d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 纯度
可以把决策树的构造过程理解成为寻找纯净划分的过程。数学上，我们可以用纯度来表示，**纯度换一种方式来解释就是让目标变量的分歧最小**。
>集合1：6次都去打篮球
>集合2：4次去打篮球，2次不去打篮球
>集合3：3次去打篮球，3次不去打篮球
>按照纯度指标：集合1 > 集合2 > 集合3。因为集合1的分歧最小，集合3的分歧最大。

### 信息熵

信息熵表示了**信息的不确定度**，理解起来就是衡量一组样本的混乱程度，样本越混乱，越不容易做出决定。
![p(i|t) 代表了节点 t 为分类 i 的概率](https://upload-images.jianshu.io/upload_images/18339009-5f9b85356b571f32.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>集合1：5次去打篮球，1次不去打篮球
>集合2：3次去打篮球，3次不去打篮球
>在集合 1 中，假设：类别 1 为“打篮球”，即次数为 5；类别 2 为“不打篮球”，即次数为 1。那么节点划分为类别 1 的概率是 5/6，为类别 2 的概率是 1/6，带入上述信息熵公式可以计算得出：
>![](https://upload-images.jianshu.io/upload_images/18339009-955d7dec499b1dc6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>集合 2 中，也是一共 6 次决策，其中类别 1 中“打篮球”的次数是 3，类别 2“不打篮球”的次数也是 3，那么信息熵为多少呢？我们可以计算得出：
>![信息熵越大，纯度越低](https://upload-images.jianshu.io/upload_images/18339009-bb3ddc760bbe1582.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**用特征对样本进行划分之后，会让混乱程度减小，就是熵变小，纯度变高**

>一开始，比如3次打篮球，3次不打，没法做判断，但是如果用刮风这个特征来划分一下，相当于有了一个条件，这时候，可能刮风的条件下，有2次不打篮球，1次打篮球， 这不就纯度提高了，有利于做出决策了，不去打。

### 信息增益
**得知特征 A** 的信息而使得样本集合**不确定性减少的程度**。计算公式如下：
![即划分之前的信息熵与给定一个特征a划分样本之后的信息熵之差](https://upload-images.jianshu.io/upload_images/18339009-2aff1cfa8a91da67.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![](https://upload-images.jianshu.io/upload_images/18339009-3144ec446d0f759c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>先算信息熵 根据信息熵的公式，7个样本，4个否，3个是，则信息熵
>![](https://upload-images.jianshu.io/upload_images/18339009-7568894c56bdc616.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>分别**计算每个特征的条件熵，就是划分之后的混乱度**。首先将天气作为属性的划分，会有三个叶子节点 D1、D2 和 D3，分别对应的是晴天、阴天和小雨。我们用 + 代表去打篮球，- 代表不去打篮球。那么第一条记录，晴天不去打篮球，可以记为 1-，于是我们可以用下面的方式来记录 D1，D2，D3：
>![](https://upload-images.jianshu.io/upload_images/18339009-68c44fdd5cac2eec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>分别计算三个叶子节点的信息熵如下：
>![](https://upload-images.jianshu.io/upload_images/18339009-89ab611436eceef9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>计算特征的信息增益 还是拿天气这个特征，最后的信息增益就是：
>![](https://upload-images.jianshu.io/upload_images/18339009-021605213da4beb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>根据上面的这个思路，我们就可以分别计算湿度，温度，刮风的信息增益如下：
>Gain(D，天气) = 0.02
>Gain(D , 温度)=0.128（这个最大）
>Gain(D , 湿度)=0.020
>Gain(D , 刮风)=0.020
>可以看出来，温度作为属性的信息增益最大，所以，先以这个为节点，划分样本。
>![](https://upload-images.jianshu.io/upload_images/18339009-e8b8543e6e6eba22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>然后我们要将上图中第一个叶节点，也就是 D1={1-,2-,3+,4+}进一步进行分裂，往下划分，计算其不同属性（天气、湿度、刮风）作为节点的信息增益，可以得到：
>Gain(D , 湿度)=1
>Gain(D , 天气)=1
>Gain(D , 刮风)=0.3115
### 基尼系数
当基尼系数越小的时候，说明样本之间的差异性小，**不确定程度**低。

分类的过程本身是一个不确定度降低的过程，即纯度的提升过程。


## 决策树生成
**解成为寻找纯净划分的过程**。构建的时候并不是选择所有的特征进行划分，因为那样的话，每 个叶子节点只对应了一个实例， 这样的结果就是对训练集拟合的非常好，但是泛化能力比较差，容易造成过拟合。所以**过拟合问题怎么解决**？我们应该**分到什么程度的时候就停止**，不继续分下去了？   

及早停止增长的主要方法有：
- 节点内数据样本低于某一阈值； 
- 所有节点特征都已分裂；
- 节点划分前准确率比划分后准确率高。

## 决策树的剪枝
假如我们真的生成了一棵决策树了，因为我们是根据训练集进行生成的，事先也不知道它的泛化能力怎么样，**还是过拟合怎么办呢**？ 

### 预剪枝（Pre-Pruning）
预剪枝不仅可以**降低过拟合的风险而且还可以减少训练时间**，但另一方面它是**基于“贪心”策略，会带来欠拟合风险**。

- 预剪枝是在决策树构造时就进行剪枝。方法是在构造的过程中对节点进行评估，**如果对某个节点进行划分，在验证集中不能带来准确性的提升**，那么对这个节点进行划分就没有意义，这时就会**把当前节点作为叶节点**，不对其进行划分。



### 后剪枝（Post-Pruning）。

**后剪枝决策树的欠拟合风险很小**，泛化性能往往优于预剪枝决策树。**但同时其训练时间会大的多**。

- 后剪枝就是在生成决策树之后再进行剪枝，通常会从决策树的叶节点开始，**逐层向上**对每个节点进行评估。如果**剪掉这个节点子树，与保留该节点子树在分类准确性上差别不大或者准确性的提升**，那么就可以把该节点子树进行剪枝。**用这个节点子树的叶子节点来替代该节点，类标记为这个节点子树中最频繁的那个类**。


# ID3（信息增益）

ID3 算法是建立在奥卡姆剃刀（用较少的东西，同样可以做好事情）的基础上：越是小型的决策树越优于大的决策树。

##  思想

从信息论的知识中我们知道：**信息熵越大，从而样本纯度越低**
以**信息增益来度量特征选择**，选择信息增益最大的特征进行分裂。

算法采用**自顶向下的贪婪搜索**遍历可能的决策树空间（C4.5 也是贪婪搜索）。 其大致步骤为：

1.  初始化特征集合和数据集合；
2.  计算数据集合信息熵和所有特征的条件熵，选择信息增益最大的特征作为当前决策节点；
3.  更新数据集合和特征集合（删除上一步使用的特征，并按照特征值来划分不同分支的数据集合）；
4.  重复 2，3 两步，若子集值包含单一特征，则为分支叶子节点。

## 缺点

*   ID3**没有剪枝策略**，容易过拟合；
*   信息增益准则**对可取值数目较多的特征有所偏好**，类似“编号”的特征其信息增益接近于 1；
*   只能用于处理 **离散分布的特征**；
*   **没有考虑缺失值**。

>假设我们把样本编号也作为一种属性的话，那么有多少样本，就会对应多少个分支，每一个分支只有一个实例，这时候每一个分支上Entropy(Di)=0，没有混乱度，显然这时候Gain(D,编号) = Entropy(D) - 0 。显然是最大的，那么按照ID3算法的话，会选择这个编号当做第一个分裂点。
# C4.5（信息增益率）

C4.5 算法最大的特点是**克服了 ID3 对特征数目的偏重**这一缺点，引入**信息增益率**来作为分类标准。
## 思想

C4.5 相对于 ID3 的缺点对应有以下改进方式：

*   引入**悲观剪枝策略进行后剪枝**；
*   引入**信息增益率作为划分标准**；
*   将**连续特征离散化**
*   对**缺失值的处理**
## 划分
![](https://upload-images.jianshu.io/upload_images/18339009-996cc9dd2e7056ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![信息增益g(D,A)与训练数据集D在特征A的划分下数据集本身的一个混乱程度(熵）](https://upload-images.jianshu.io/upload_images/18339009-195f71c64f005a58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>![](https://upload-images.jianshu.io/upload_images/18339009-d41c3640a09e69ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>$H_A(D) = - \frac{1}{7} \log \frac{1}{7} * 7 = -\log \frac{1}{7}$ 也就是说**类别越多，混乱程度越大，这时候信息增益比也会减小**
>![](https://upload-images.jianshu.io/upload_images/18339009-dd420252adbc1345.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>当属性有很多值的时候，相当于被划分成了许多份，虽然**信息增益变大了，但是对于 C4.5 来说，属性熵也会变大**，所以整体的信息增益率并不大。




## 悲观剪枝方法
后剪枝技术中的一种，**用递归的方式从下往上针对每一个非叶子节点，评估用一个最佳叶子节点去代替这课子树是否有益。如果剪枝后与剪枝前相比其错误率是保持或者下降，则这棵子树就可以被替换掉**。C4.5 通过训练数据集上的错误分类数量来估算未知样本上的错误率。



## 离散化处理连续属性
C4.5 可以处理连续属性的情况，对连续的属性进行离散化的处理。比如打篮球存在的“湿度”属性，不按照“高、中”划分，而是按照湿度值进行计算，那么湿度取什么值都有可能。该怎么选择这个阈值呢，**C4.5 选择具有最高信息增益的划分所对应的阈值**。

假设 n 个样本的连续特征 A 有 m 个取值，C4.5 将其排序并取相邻两样本值的平均数共 m-1 个划分点，分别计算以该划分点作为二元分类点时的信息增益，并选择信息增益最大的点作为该连续特征的二元离散分类点；

## 缺失值的处理
### 问题一：在特征值缺失的情况下进行划分特征的选择？（即如何计算特征的信息增益率）
C4.5 的做法是：对于具有缺失值特征，**用没有缺失的样本子集所占比重来折算**
>![](https://upload-images.jianshu.io/upload_images/18339009-b0c80c60ec007b2f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>我们不考虑缺失的数值，可以得到温度 D={2-,3+,4+,5-,6+,7-}。
>温度 = 高：D1={2-,3+,4+} ；温度 = 中：D2={6+,7-}；温度 = 低：D3={5-} 。这里 + 号代表打篮球，- 号代表不打篮球。
>针对将属性选择为温度的信息增为：Gain(D′, 温度)=Ent(D′)-0.792=1.0-0.792=0.208 
>属性熵 =1.459
>信息增益率 Gain_ratio(D′, 温度)=0.208/1.459=0.1426
>D′的样本个数为 6，而 D 的样本个数为 7，所以所占权重比例为 6/7，所以 Gain(D′，温度) 所占权重比例为 6/7
>所以：Gain_ratio(D, 温度)=6/7*0.1426=0.122。
>这样即使在温度属性的数值有缺失的情况下，我们依然可以计算信息增益，并对属性进行选择。

### 问题二：选定该划分特征，对于缺失该特征值的样本如何处理？（即到底把这个样本划分到哪个结点里）
C4.5 的做法是：将样本同时划分到所有子节点，不过要调整样本的权重值，其实也就是以不同概率划分到不同节点中。这样，如果再往下划分属性，对于该样本来说，算条件熵的时候，得考虑上他本身的一个权重了。

##  缺点

*   **剪枝策略可以再优化**；
*   C4.5 用的是**多叉树，用二叉树效率更高**；
*   C4.5 **只能用于分类**；
*   C4.5 使用的**熵模型拥有大量耗时的对数运算，连续值还有排序运算**；
*   C4.5 在构造树的过程中，对数值属性值需要按照其大小进行排序，从中选择一个分割点，所以只适合于能够驻留于内存的数据集，当训练集大得无法在内存容纳时，程序无法运行。

# CART(Classification And Regression Tree)（Gini 系数）

ID3 和 C4.5 虽然在对训练样本集的学习中可以尽可能多地挖掘信息，但是其生成的决策树分支、规模都比较大，**CART 算法的二分法可以简化决策树的规模，提高生成决策树的效率**。

##  思想

CART 包含的基本过程有分裂，剪枝和树选择。

*   **分裂：**分裂过程是一个**二叉递归划分过程**，其输入和预测特征既可以是连续型的也可以是离散型的，CART 没有停止准则，会一直生长下去；
*   **剪枝：**采用**代价复杂度剪枝**，从最大树开始，每次**选择训练数据熵对整体性能贡献最小的那个分裂节点作为下一个剪枝对象**，直到只剩下根节点。CART 会产生一系列嵌套的剪枝树，需要从中选出一颗最优的决策树；
*   **树选择：**用**单独的测试集评估每棵剪枝树的预测性能**（也可以用交叉验证）。

CART 在 C4.5 的基础上进行了很多提升。

*   C4.5 为多叉树，运算速度慢，CART 为**二叉树，运算速度快**；
*   C4.5 只能分类，CART **既可以分类也可以回归**；
*   CART 使用**Gini 系数作为变量的不纯度量，减少了大量的对数运算**；
*   CART 采用**代理测试来估计缺失值，而 C4.5 以不同概率划分到不同节点中**；
*   CART 采用**“基于代价复杂度剪枝”方法进行剪枝，而 C4.5 采用悲观剪枝方法**。


熵模型拥有大量耗时的对数运算，基尼指数在简化模型的同时还保留了熵模型的优点。**基尼指数代表了数据集的不纯度，基尼系数越小，不纯度越低，特征越好。这和信息增益（率）正好相反**。
**偏向于特征值较多的特征，类似信息增益**

![其中 k 代表类别](https://upload-images.jianshu.io/upload_images/18339009-fd030b5c3872bac1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


此外，当 CART 为二分类，其表达式为：

![](https://upload-images.jianshu.io/upload_images/18339009-e96e434d5fcddb1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/18339009-1a13bd9c671d2ef2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以看到在平方运算和二分类的情况下，其运算更加简单。当然其性能也与熵模型非常接近。那么问题来了：基尼指数与熵模型性能接近，但到底与熵模型的差距有多大呢？

![](https://upload-images.jianshu.io/upload_images/18339009-2634d1e7ae9b91d7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 剪枝策略
采用一种“**基于代价复杂度的剪枝**”方法进行后剪枝，这种方法会**生成一系列树，每个树都是通过将前面的树的某个或某些子树替换成一个叶节点而得到的**，这一系列树中的**最后一棵树仅含一个用来预测类别的叶节点**。然后用一种成本复杂度的度量准则来判断哪棵子树应该被一个预测类别值的叶节点所代替。这种方法需要使用一个单独的测试数据集来评估所有的树，根据它们在测试数据集熵的分类性能选出最佳的树。
![](https://upload-images.jianshu.io/upload_images/18339009-3dd929662c36baff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 类别不平衡

CART 的一大优势在于：无论训练数据集有多失衡，它都可以将其自动消除不需要建模人员采取其他操作。
CART 使用了一种先验机制，其作用相当于对类别进行加权。这种先验机制嵌入于 CART 算法判断分裂优劣的运算里，在 CART 默认的分类模式中，总是要计算每个节点关于根节点的类别频率的比值，这就相当于对数据自动重加权，对类别进行均衡。
对于一个二分类问题，节点 node 被分成类别 1 当且仅当：
![](https://upload-images.jianshu.io/upload_images/18339009-d4b8f4e693c4b901.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
比如二分类，根节点属于 1 类和 0 类的分别有 20 和 80 个。在子节点上有 30 个样本，其中属于 1 类和 0 类的分别是 10 和 20 个。如果 10/20>20/80，该节点就属于 1 类。
通过这种计算方式就无需管理数据真实的类别分布。假设有 K 个目标类别，就可以确保根节点中每个类别的概率都是 1/K。这种默认的模式被称为“先验相等”。
先验设置和加权不同之处在于先验不影响每个节点中的各类别样本的数量或者份额。先验影响的是每个节点的类别赋值和树生长过程中分裂的选择。


## CART回归树
CART 回归树划分数据集的过程和分类树的过程是一样的，只是回归树得到的预测结果是连续值，而且评判“不纯度”的指标不同。

在 CART 分类树中采用的是基尼系数作为标准，那么在 CART 回归树中，如何评价“不纯度”呢？实际上我们要**根据样本的混乱程度，也就是样本的离散程度来评价“不纯度”**。

样本的离散程度具体的计算方式是，先计算所有样本的均值，然后计算每个样本值到均值的差值。我们假设 x 为样本的个体，均值为 u。为了统计样本的离散程度，我们可以取差值的绝对值，或者方差。

其中差值的绝对值为样本值减去样本均值的绝对值：
![](https://upload-images.jianshu.io/upload_images/18339009-4048782839ee1252.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

方差为每个样本值减去样本均值的平方和除以样本个数：
![](https://upload-images.jianshu.io/upload_images/18339009-7f1d0d9476d5e3b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 连续值处理
所以这两种节点划分的标准，分别对应着两种目标函数最优化的标准，即用最小绝对偏差（LAD），或者使用最小二乘偏差（LSD）。这两种方式都可以让我们找到节点划分的方法，通常使用最小二乘偏差的情况更常见一些。

![](https://upload-images.jianshu.io/upload_images/18339009-f2f441d849017359.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###  预测方式
对于决策树建立后做预测的方式，上面讲到了 CART 分类树采用叶子节点里概率最大的类别作为当前节点的预测类别。而回归树输出不是类别，它采用的是用最终叶子的均值或者中位数来预测输出结果。

#  ID3、C4.5 和 CART 三者之间的差异。
- 划分标准的差异：ID3 使用信息增益偏向特征值多的特征，C4.5 使用信息增益率克服信息增益的缺点，偏向于特征值小的特征，CART 使用基尼指数克服 C4.5 需要求 log 的巨大计算量，偏向于特征值较多的特征。
- 使用场景的差异：ID3 和 C4.5 都只能用于分类问题，CART 可以用于分类和回归问题；ID3 和 C4.5 是多叉树，速度较慢，CART 是二叉树，计算速度很快；
- 样本数据的差异：ID3 只能处理离散数据且缺失值敏感，C4.5 和 CART 可以处理连续性数据且有多种方式处理缺失值；从样本量考虑的话，小样本建议 C4.5、大样本建议 CART。C4.5 处理过程中需对数据集进行多次扫描排序，处理成本耗时较高，而 CART 本身是一种大样本的统计方法，小样本处理下泛化误差较大 ；
- 样本特征的差异：ID3 和 C4.5 层级之间只使用一次特征，CART 可多次重复使用特征；
- 剪枝策略的差异：ID3 没有剪枝策略，C4.5 是通过悲观剪枝策略来修正树的准确性，而 CART 是通过代价复杂度剪枝。
```
# encoding=utf-8
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_boston
from sklearn.metrics import r2_score,mean_absolute_error,mean_squared_error
from sklearn.tree import DecisionTreeRegressor
# 准备数据集
boston=load_boston()
# 探索数据
print(boston.feature_names)
# 获取特征集和房价
features = boston.data
prices = boston.target
# 随机抽取33%的数据作为测试集，其余为训练集
train_features, test_features, train_price, test_price = train_test_split(features, prices, test_size=0.33)
# 创建CART回归树
dtr=DecisionTreeRegressor()
# 拟合构造CART回归树
dtr.fit(train_features, train_price)
# 预测测试集中的房价
predict_price = dtr.predict(test_features)
# 测试集的结果评价
print('回归树二乘偏差均值:', mean_squared_error(test_price, predict_price))
print('回归树绝对值偏差均值:', mean_absolute_error(test_price, predict_price))
```

## 缺失值处理

上文说到，模型对于缺失值的处理会分为两个子问题：

1.  如何在特征值缺失的情况下进行划分特征的选择？
CART 一开始严格要求分裂特征评估时只能使用在该特征上没有缺失值的那部分数据，在后续版本中，CART 算法使用了一种惩罚机制来抑制提升值，从而反映出缺失值的影响（例如，如果一个特征在节点的 20% 的记录是缺失的，那么这个特征就会减少 20% 或者其他数值）。


2.  选定该划分特征，模型对于缺失该特征值的样本该进行怎样处理？
CART 算法的机制是为树的每个节点都找到代理分裂器，无论在训练数据上得到的树是否有缺失值都会这样做。在代理分裂器中，特征的分值必须超过默认规则的性能才有资格作为代理（即代理就是代替缺失值特征作为划分特征的特征），当 CART 树中遇到缺失值时，这个实例划分到左边还是右边是决定于其排名最高的代理，如果这个代理的值也缺失了，那么就使用排名第二的代理，以此类推，如果所有代理值都缺失，那么默认规则就是把样本划分到较大的那个子节点。代理分裂器可以确保无缺失训练数据上得到的树可以用来处理包含确实值的新数据。

##  剪枝策略

采用一种“基于代价复杂度的剪枝”方法进行后剪枝，这种方法会生成一系列树，每个树都是通过将前面的树的某个或某些子树替换成一个叶节点而得到的，这一系列树中的最后一棵树仅含一个用来预测类别的叶节点。然后用一种成本复杂度的度量准则来判断哪棵子树应该被一个预测类别值的叶节点所代替。这种方法需要使用一个单独的测试数据集来评估所有的树，根据它们在测试数据集熵的分类性能选出最佳的树。

我们来看具体看一下代价复杂度剪枝算法：
![](https://upload-images.jianshu.io/upload_images/18339009-cccb83d565d02511.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##  类别不平衡

CART 的一大优势在于：**无论训练数据集有多失衡，它都可以将其自动消除**不需要建模人员采取其他操作。

CART 使用了一种先验机制，其作用相当于对类别进行加权。这种先验机制嵌入于 CART 算法判断分裂优劣的运算里，在 CART 默认的分类模式中，总是要计算每个节点关于根节点的类别频率的比值，这就相当于对数据自动重加权，对类别进行均衡。

对于一个二分类问题，节点 node 被分成类别 1 当且仅当：

![](https://upload-images.jianshu.io/upload_images/18339009-86c45148051c6c61.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

比如二分类，根节点属于 1 类和 0 类的分别有 20 和 80 个。在子节点上有 30 个样本，其中属于 1 类和 0 类的分别是 10 和 20 个。如果 10/20>20/80，该节点就属于 1 类。

通过这种计算方式就无需管理数据真实的类别分布。假设有 K 个目标类别，就可以确保根节点中每个类别的概率都是 1/K。这种默认的模式被称为“先验相等”。

先验设置和加权不同之处在于先验不影响每个节点中的各类别样本的数量或者份额。先验影响的是每个节点的类别赋值和树生长过程中分裂的选择。

##  回归树

CART（Classification and Regression Tree，分类回归树），从名字就可以看出其**不仅可以用于分类，也可以应用于回归**。其回归树的建立算法上与分类树部分相似，这里简单介绍下不同之处。

#### 连续值处理

![](https://upload-images.jianshu.io/upload_images/18339009-88fe67ba88a34b70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 预测方式

对于决策树建立后做预测的方式，上面讲到了 CART 分类树采用叶子节点里概率最大的类别作为当前节点的预测类别。而回归树输出不是类别，它采用的是用最终叶子的均值或者中位数来预测输出结果。

# 总结

最后通过总结的方式对比下 ID3、C4.5 和 CART 三者之间的差异。

*   **划分标准的差异：**ID3 使用信息增益偏向特征值多的特征，C4.5 使用信息增益率克服信息增益的缺点，偏向于特征值小的特征，CART 使用基尼指数克服 C4.5 需要求 log 的巨大计算量，偏向于特征值较多的特征。
*   **使用场景的差异：**ID3 和 C4.5 都只能用于分类问题，CART 可以用于分类和回归问题；ID3 和 C4.5 是多叉树，速度较慢，CART 是二叉树，计算速度很快；
*   **样本数据的差异：**ID3 只能处理离散数据且缺失值敏感，C4.5 和 CART 可以处理连续性数据且有多种方式处理缺失值；从样本量考虑的话，小样本建议 C4.5、大样本建议 CART。C4.5 处理过程中需对数据集进行多次扫描排序，处理成本耗时较高，而 CART 本身是一种大样本的统计方法，小样本处理下泛化误差较大 ；
*   **样本特征的差异：**ID3 和 C4.5 层级之间只使用一次特征，CART 可多次重复使用特征；
*   **剪枝策略的差异：**ID3 没有剪枝策略，C4.5 是通过悲观剪枝策略来修正树的准确性，而 CART 是通过代价复杂度剪枝。


# 优缺点
优点：
- **算法简单，模型具有很强的解释性**
- 可以用于分类和回归问题

缺点：
- 决策树模型**很容易出现过拟合现象**，且不同的训练数据集构建的模型相差也很大。实际项目中，我们往往不是单独使用决策树模型，为了避免决策树的过拟合，需对决策树结合集成算法使用，如bagging算法和boosting算法。

## 在调参时的注意点有：
1）当**样本少数量但是样本特征非常多**的时候，决策树很容易过拟合，一般来说，样本数比特征数多一些会比较容易建立健壮的模型

2）如果**样本数量少但是样本特征非常多**，在拟合决策树模型前，推荐先做维度规约，比如主成分分析（PCA），特征选择（Losso）或者独立成分分析（ICA）。这样特征的维度会大大减小。再来拟合决策树模型效果会好。

3）推荐多用决策树的可视化（下节会讲），同时先限制决策树的深度（比如最多3层），这样可以先观察下生成的决策树里数据的初步拟合情况，然后再决定是否要增加深度。

4）在训练模型前，注意观察样本的类别情况（主要指分类树），**如果类别分布非常不均匀，就要考虑用class_weight来限制模型过于偏向样本多的类别。**

5）决策树的数组使用的是numpy的float32类型，如果训练数据不是这样的格式，算法会先做copy再运行。

6）如果**输入的样本矩阵是稀疏的**，推荐在拟合前调用csc_matrix稀疏化，在预测前调用csr_matrix稀疏化。


# sklearn参数  
|                                                  | DecisionTreeClassifier                                       | DecisionTreeRegressor                                        |
| ------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 特征选择标准criterion                            | 可"gini"或者"entropy"，默认的"gini"就可以，即CART算法。除非你更喜欢类似ID3, C4.5的最优特征选择方法。 | 可以使用"mse"或者"mae"推荐使用默认的"mse"。一般来说"mse"比"mae"更加精确。 |
| 特征划分点选择标准splitter                       | 可"best"或者"random"。前者在特征的所有划分点中找出最优的划分点。后者是随机的在部分划分点中找局部最优的划分点。默认的"best"适合样本量不大的时候，而**如果样本数据量非常大，此时决策树构建推荐"random" ** |                                                              |
| 划分时考虑的最大特征数max_features               | 默认是"None",意味着划分时考虑所有的特征数；如果是"log2"意味着划分时最多考虑$log_2N$个特征；如果是"sqrt"或者"auto"意味着划分时最多考虑$\sqrt{N}$个特征。如果是整数，代表考虑的特征绝对数。如果是浮点数，代表考虑特征百分比，即考虑（百分比xN）取整后的特征数。其中N为样本总特征数。一般来说，如果样本特征数不多，比如小于50，我们用默认的"None"就可以了，**如果特征数非常多，我们可以灵活使用刚才描述的其他取值来控制划分时考虑的最大特征数**，以控制决策树的生成时间。 |                                                              |
| 决策树最大深max_depth                            | 默认可以不输入，如果不输入的话，决策树在建立子树的时候不会限制子树的深度。一般来说，数据少或者特征少的时候可以不管这个值。**如果模型样本量多，特征也多的情况下，推荐限制这个最大深度**，具体的取值取决于数据的分布。常用的可以取值10-100之间。 |                                                              |
| 内部节点再划分所需最小样本数min_samples_split    | 这个值限制了子树继续划分的条件，如果某节点的样本数少于min_samples_split，则不会继续再尝试选择最优特征来进行划分。 默认是2.如果样本量不大，不需要管这个值。**如果样本量数量级非常大，则推荐增大这个值。**我之前的一个项目例子，有大概10万样本，建立决策树时，我选择了min_samples_split=10。可以作为参考。 |                                                              |
| 叶子节点最少样本数min_samples_leaf               | 这个值限制了叶子节点最少的样本数，如果某叶子节点数目小于样本数，则会和兄弟节点一起被剪枝。 默认是1,可以输入最少的样本数的整数，或者最少样本数占样本总数的百分比。如果样本量不大，不需要管这个值。**如果样本量数量级非常大，则推荐增大这个值。**之前的10万样本项目使用min_samples_leaf的值为5，仅供参考。 |                                                              |
| 叶子节点最小的样本权重和min_weight_fraction_leaf | 这个值限制了叶子节点所有样本权重和的最小值，如果小于这个值，则会和兄弟节点一起被剪枝。 默认是0，就是不考虑权重问题。一般来说，**如果我们有较多样本有缺失值，或者分类树样本的分布类别偏差很大，就会引入样本权重，这时我们就要注意这个值了。** |                                                              |
| 最大叶子节点数max_leaf_nodes                     | 通过限制最大叶子节点数，可以防止过拟合，默认是"None”，即不限制最大的叶子节点数。如果加了限制，算法会建立在最大叶子节点数内最优的决策树。如果特征不多，可以不考虑这个值，**但是如果特征分成多的话，可以加以限制，**具体的值可以通过交叉验证得到。 |                                                              |
| 类别权重class_weight                             | 指定样本各类别的的权重，主要是为了**防止训练集某些类别的样本过多，导致训练的决策树过于偏向这些类别。**这里可以自己指定各个样本的权重，或者用“balanced”，如果使用“balanced”，则算法会自己计算权重，样本量少的类别所对应的样本权重会高。当然，如果你的样本类别分布没有明显的偏倚，则可以不管这个参数，选择默认的"None" | 不适用于回归树                                               |
| 节点划分最小不纯度min_impurity_split             | 这个值限制了决策树的增长，如果某节点的不纯度(基尼系数，信息增益，均方差，绝对差)小于这个阈值，则该节点不再生成子节点。即为叶子节点 。 |                                                              |
| 数据是否预排序presort                            | 这个值是布尔值，默认是False不排序。一般来说，如果样本量少或者限制了一个深度很小的决策树，设置为true可以让划分点选择更加快，决策树建立的更加快。如果样本量太大的话，反而没有什么好处。问题是样本量少的时候，我速度本来就不慢。所以这个值一般懒得理它就可以了。 |                                                              |




```python
### 导入需要用到的python库
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from sklearn.datasets import load_iris

iris = load_iris()
y = iris.target
X = iris.data
print(y.shape, X.shape)
### 将数据集拆分为训练集和测试集
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=0)

### 特征缩放
from sklearn.preprocessing import StandardScaler
sc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.transform(X_test)

### 对测试集进行决策树分类拟合
from sklearn.tree import DecisionTreeClassifier
classifier = DecisionTreeClassifier(criterion='entropy', random_state=0)
classifier.fit(X_train, y_train)

### 预测测试集的结果
y_pred = classifier.predict(X_test)

print(y_test, y_pred)
```
[数据](https://github.com/luwill/machine-learning-code-writing/blob/master/id3/example_data.csv)

# numpy
计算熵值。
```
# 计算熵值。
def entropy(ele):
    '''
    input: A list contain categorical value.
    output: Entropy value.
    entropy = - sum(p * log(p)), p is a prob value.
    '''
    # 计算列表值的概率分布yes 1/2 no 1/2
    probs = [ele.count(i)/len(ele) for i in set(ele)]
    # 计算熵值
    entropy = -sum([prob*log(prob, 2) for prob in probs])
    return entropy
print(set(df['play'].tolist()))
entropy(df['play'].tolist())
```

根据特征和特征值进行数据划分的方法：
```
# 根据特征和特征值进行数据划分的方法：
def split_dataframe(data, col):
    '''
    input: dataframe, 根据那一列分.
    output: a dict of splited dataframe.
    '''
    # 这列中唯一的值
    unique_values = data[col].unique()
    # 装dataframe的字典，键是上面唯一的值
    result_dict = {elem : pd.DataFrame for elem in unique_values}
    # 根据列值分dataframe
    for key in result_dict.keys():
        # 各行如果这列等于该键则添加
        result_dict[key] = data[:][data[col] == key]
    return result_dict
split_example = split_dataframe(df, 'temp')
split_example
```

根据熵计算公式和数据集划分方法计算信息增益来选择最佳特征
```
# 根据熵计算公式和数据集划分方法计算信息增益来选择最佳特征
def choose_best_col(df, label):
    '''
    input: datafram, 目标特征
    output: max infomation gain, best column, 
            splited dataframe dict based on best column.
    '''
    # 目标特征的信息熵
    entropy_D = entropy(df[label].tolist())
    # 除了目标特征的列表
    cols = [col for col in df.columns if col not in [label]]
    # ['humility', 'outlook', 'temp', 'windy']
    # 初始化最大信息增益、最佳列和最佳分割dict
    max_value, best_col = -999, None
    max_splited = None
    # 根据不同的列拆分数据
    for col in cols:
        splited_set = split_dataframe(df, col)
        # {'high':    humility   outlook play  temp  windy
                # 0      high     sunny   no   hot  False
                # 1      high     sunny   no   hot   True
                # 2      high  overcast  yes   hot  False
                # 3      high     rainy  yes  mild  False
                # 7      high     sunny   no  mild  False
                # 11     high  overcast  yes  mild   True
                # 13     high     rainy   no  mild   True, 
        # 'normal':    humility   outlook play  temp  windy
                # 4    normal     rainy  yes  cool  False
                # 5    normal     rainy   no  cool   True
                # 6    normal  overcast  yes  cool   True
                # 8    normal     sunny  yes  cool  False
                # 9    normal     rainy  yes  mild  False
                # 10   normal     sunny  yes  mild   True
                # 12   normal  overcast  yes   hot  False}
        entropy_DA = 0
        for subset_col, subset in splited_set.items():
            # 计算拆分后的dataframe中目的特征的熵
            entropy_Di = entropy(subset[label].tolist())
            # 计算当前特征的熵。加入某个特征之后的条件熵。
            entropy_DA += len(subset)/len(df) * entropy_Di
        # 计算当前特征的信息增益
        info_gain = entropy_D - entropy_DA
        
        if info_gain > max_value:
            max_value, best_col = info_gain, col
            max_splited = splited_set
    return max_value, best_col, max_splited
    
choose_best_col(df, 'play')
```
根据上述代码和示例数据集构造一个ID3决策树：
```
class ID3Tree:
    # define a Node class
    class Node:
        def __init__(self, name):
            self.name = name
            self.connections = {}

        def connect(self, label, node):
            self.connections[label] = node
            
    def __init__(self, data, label):
        self.columns = data.columns
        self.data = data
        self.label = label
        self.root = self.Node("Root")
    
    # print tree method
    def print_tree(self, node, tabs):
        print(tabs + node.name)
        for connection, child_node in node.connections.items():
            print(tabs + "\t" + "(" + connection + ")")
            self.print_tree(child_node, tabs + "\t\t")
    
    def construct_tree(self):
        self.construct(self.root, "", self.data, self.columns)
    
    # construct tree
    def construct(self, parent_node, parent_connection_label, input_data, columns):
        max_value, best_col, max_splited = choose_best_col(input_data[columns], self.label)
        
        if not best_col:
            node = self.Node(input_data[self.label].iloc[0])
            parent_node.connect(parent_connection_label, node)
            return

        node = self.Node(best_col)
        parent_node.connect(parent_connection_label, node)
        
        new_columns = [col for col in columns if col != best_col]
        
        # Recursively constructing decision trees
        for splited_value, splited_data in max_splited.items():
            self.construct(node, splited_value, splited_data, new_columns)
            
tree1 = ID3Tree(df, 'play')
tree1.construct_tree()
tree1.print_tree(tree1.root, "")
```
```
import numpy as np
import matplotlib.pyplot as plt
from sklearn import datasets

# 加载葡萄酒的数据集
wine = datasets.load_wine()

# 为了方便可视化，只选取 2 个特征
X = wine.data[:, [0, 6]]
y = wine.target

# 绘制散点图
plt.scatter(X[y==0, 0], X[y==0, 1])
plt.scatter(X[y==1, 0], X[y==1, 1])
plt.scatter(X[y==2, 0], X[y==2, 1])
plt.show()
from sklearn.model_selection import train_test_split
from sklearn import tree

# 拆分为训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)

# 调用决策树分类算法
dtc = tree.DecisionTreeClassifier(max_depth=2)
dtc.fit(X_train, y_train)

# 算法评分
print('训练得分：', dtc.score(X_train, y_train))
print('测试得分：', dtc.score(X_test, y_test))


from matplotlib.colors import ListedColormap

# 定义绘制决策边界的函数
def plot_decision_boundary(model, axis):
    x0, x1 = np.meshgrid(
        np.linspace(axis[0], axis[1], int((axis[1]-axis[0])*100)).reshape(-1,1),
        np.linspace(axis[2], axis[3], int((axis[3]-axis[2])*100)).reshape(-1,1))
    X_new = np.c_[x0.ravel(), x1.ravel()]
    y_predict = model.predict(X_new)
    zz = y_predict.reshape(x0.shape)
    custom_cmap = ListedColormap(['#EF9A9A','#FFF59D','#90CAF9'])
    plt.contourf(x0, x1, zz, cmap=custom_cmap)
# 绘制决策边界
plot_decision_boundary(dtc, axis=[11, 15, 0, 6])
plt.scatter(X[y==0, 0], X[y==0, 1])
plt.scatter(X[y==1, 0], X[y==1, 1])
plt.scatter(X[y==2, 0], X[y==2, 1])
plt.show()
```
![决策边界](https://upload-images.jianshu.io/upload_images/18339009-3e6d3760aaf7fc86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
# 导入相关库，需要先安装 graphviz 和 pydotplus，并在电脑中 Graphviz 软件
import pydotplus
from sklearn.tree import export_graphviz
from IPython.display import Image
from io import StringIO
# 将对象写入内存中
dot_data = StringIO()
# 生成决策树结构
tree.export_graphviz(dtc, class_names=wine.target_names,
                     feature_names=[wine.feature_names[0], wine.feature_names[6]],
                     rounded=True, filled=True, out_file = dot_data)
# 生成树形图并展示出来
graph = pydotplus.graph_from_dot_data(dot_data.getvalue())
Image(graph.create_png())
```
![决策树可视化](https://upload-images.jianshu.io/upload_images/18339009-0203e5ed9fe4d043.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
