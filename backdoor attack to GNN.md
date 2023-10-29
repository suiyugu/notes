### Unnoticeable Backdoor Attacks on Graph Neural Networks

#### GNN后门攻击一般步骤

1. 触发器＋伪造标签 附加在训练数据
2. 感染的图数据上训练GNN
3. 后门GNN模型对添加触发器的测试图做出误导的预测

#### 本文提出的UGBA攻击方法：

- 低成本攻击预算
- 隐蔽性

### 证明UGBA有效性实验

#### NETTACK

对抗性算法，有限攻击预算内迭代修改图结构（连边、节点特征），降低模型的分类精度（即输出标签）。

不攻击模型 而是数据 但NETTACK将脏数据交给了模型来训练 攻击效果大打折扣

###### 后门 注重操控模型

核心思想：把感染了触发器子图的数据交给模型训练，让训练后的模型把附带了触发器子图的节点或者图预测为攻击者想要的分类

###### 现有不足：

- 需要大量的节点都附加上触发器 也会导致触发器容易被检测
- 现实数据一般遵循同质性假设 触发器易被识别和销毁

#### SBA   随机生成子图作为触发器

[^SBA]: Zaixi Zhang, Jinyuan Jia, Binghui Wang, and Neil Zhenqiang Gong. 2021. Backdoor attacks to graph neural networks. In SACMAT. 15–26.

##### SBA-Gene 生成图

##### SBA-Samp 平滑分类器

![image-20231017234043436](https://gitee.com/sygu66/img/raw/master/image-20231017234043436.png)

#### GTA     基于触发器生成器的后门攻击

[^GTA]: Zhaohan Xi, Ren Pang, Shouling Ji, and Ting Wang. 2021. Graph backdoor. InUSENIX Security. 1523–1540.



#### 实验验证上述三种现存后门攻击方式在两方面局限性

###### SBA-Gene SBA-Samp GTA

##### Size of Poisoned Nodes

在大型数据集OGB-arxiv(169343个节点)上验证

目标模型：SAGE

攻击成功率指标：ASR（成功将具有后门触发器的样本误分类为攻击者所期望的目标类别的比率

$ASR=\frac{成功攻击样本数}{总攻击样本数}$

![image-20231018144529665](https://gitee.com/sygu66/img/raw/master/image-20231018144529665.png)

感染节点预算不足的情况下ASR都较差

原因：

1. SBA-Gen SGA-Samp触发器固定
2. GTA自适应触发器，但感染节点随机，**未充分利用**感染节点



#### Detection of Triggers

评估在固定**感染节点数2400**时，数据集**OGB-arxiv**上两种防御方法下三种后门攻击的效果

[^Prune]: 剪掉低余弦相似度相连节点
[^Prune+LD]: 上述基础上丢弃低余弦相似度节点标签

其中防御设定支剪的余弦相似度阈值为10%

评估指标：

- attack success rate ASR
- clean accuracy  评估模型受到攻击后是否仍然能够准确分类正常数据

![image-20231018144558851](https://gitee.com/sygu66/img/raw/master/image-20231018144558851.png)

ASR显著下降，模型精度几乎无损失

原因：

触发器子图节点特征与原本干净图的特征不同，违背原本的同质相似性。易被支剪掉



### UGBA

###### 创新：

- 基于聚类的感染节点选择算法，对于特定模式的一种节点只选择感染一次/少数次，减少了攻击的预算（要中毒的节点数）

  感染最具代表性节点

- 自适应触发器 令触发器内部节点、触发器与感染节点间都满足同质相似性

考虑成本，分为两个步骤：

![image-20231018001418192](https://gitee.com/sygu66/img/raw/master/image-20231018001418192.png)

#### Poisoned Node Selector $f_p$

感染具有代表性的节点能够使其他相似节点也能成功被后门攻击

- **考虑邻居节点的结构属性**：直接对节点进行聚类没有考虑图拓扑结构，所以通过GCN编码器获取节点经过前向传递后的特征聚类。

  K-means算法对特征集合分类，越靠近聚类质心的节点更具备代表性

- **考虑感染节点的度**：感染节点的节点度高会导致模型预测性能下降，影响的不只是性能，还有触发器的隐蔽性

![image-20231018170044051](https://gitee.com/sygu66/img/raw/master/image-20231018170044051.png)

$\lambda$为控制度对节点选择的影响

#### Adaptive Trigger Generator $f_g$

为了生成与附加节点类似的自适应触发器，自适应触发器生成器𝑓𝑔将节点的节点特征作为输入，采用MLP同时生成节点𝑣的节点特征和触发器结构

![image-20231022132933813](https://gitee.com/sygu66/img/raw/master/image-20231022132933813.png)

$x_i$：节点特征   $W_f$特征学习参数  $W_a$结构学习参数

$X_i^g$：触发节点的综合特征   $A_i^g$：生成的触发器的邻接矩阵

生成的触发器：$g_i=(X_i^g,A_i^g)$

优化触发器生成器

##### Unnoticeable Loss $L_c$

这种后门攻击的关键思想是：

- 确保中毒节点或测试节点𝑣与余弦相似度高的触发节点**相连**
- 在所生成的触发器𝑔**中**，所连接的触发器节点也应具有较高的相似性

![image-20231022134755324](https://gitee.com/sygu66/img/raw/master/image-20231022134755324.png)

$T$是相似度阈值

当两个相连节点的余弦相似度低于阈值，低于阈值的差值就会被加到损失上

##### Bi- level optimization

双级优化攻击模型

在后门攻击有效时同时不影响正常标签的检测



![image-20231022135427426](https://gitee.com/sygu66/img/raw/master/image-20231022135427426.png)

![image-20231022135444946](https://gitee.com/sygu66/img/raw/master/image-20231022135444946.png)

#### UGBA有效性实验

**数据集**：Cora  Pubmed  Flickr  OGB-arxiv

###### 指标：$ASR|Clean Accuracy(\%)$ 

**后门攻击方法对比**：GTA  SBA-Samp  SBA-Gen

![image-20231018152554312](https://gitee.com/sygu66/img/raw/master/image-20231018152554312.png)



**多种图神经网络架构下的攻击效果对比**：GCN  SAGE  GAT

验证UGBA在不同GNN模型中的可转移性



**验证感染节点选择算法的有效性**

限制感染节点个数，比较后门攻击方法在相同实验设置（数据集、实验重复次数、触发器节点数限制等）下的攻击效果

###### 指标：$ASR(\%)$

![image-20231018153503192](https://gitee.com/sygu66/img/raw/master/image-20231018153503192.png)





##### 验证隐蔽性约束和节点选择算法对抵抗后门攻击防御方法的有效性（消融实验

[^UGBA\CS]: 都删掉
[^UGBA\C]: 删掉隐蔽性约束
[^UGBA\S]: 删掉节点选择算法

![image-20231018153921319](https://gitee.com/sygu66/img/raw/master/image-20231018153921319.png)

原因：经过隐蔽性约束后节点相似性高



计算触发边和干净边的边缘相似性，用余弦相似度评估

还是验证了相对GTA提出隐蔽性约束后边缘相似性的提高

![image-20231018154351273](https://gitee.com/sygu66/img/raw/master/image-20231018154351273.png)



#### 参数影响研究

![image-20231018155446673](https://gitee.com/sygu66/img/raw/master/image-20231018155446673.png)

超参数$\beta$,$T$对UGBA性能的影响，超参数控制训练触发生成器时**Unnoticeable Loss** **$L_c$**的权重以及使用的相似分数的阈值。

$\beta$：{0, 50, 100, 150, 200}

$T$：{0, 0.2, 0.4, 0.6, 0.8, 1} and {0.6, 0.7, 0.8, 0.9, 1}

针对Prune+LD防御策略的攻击成功率(ASR）

Pubmed中，相似性阈值𝑇需要大于0.2;而OGB-arxiv要求𝑇大于0.8。这是因为与Pubmed相比，OGB-arxiv中的边显示出更高的相似性得分，为了避免被检测到，需要更高的相似性阈值𝑇

（某种程度上又证明了隐蔽性约束的有效？