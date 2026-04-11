---
title: "DGA检测介绍"
date: 2021-04-19T15:02:11+08:00
lastmod: 2021-04-23T14:08:13+08:00
categories: [大数据与网络安全]
tags: [DGA, 僵尸网络, 域名生成算法]
draft: false
math: true
summary: "DGA(Domain Generation Algorithm)是一种用于生成大量伪随机域名的算法"
---

## 什么是DGA

DGA(Domain Generation Algorithm)是一种用于生成大量伪随机域名的算法。通过一系列的数值运算，该算法将数值直接或者间接的转化为字符，并通过一定的规则将这些字符组合起来形成域名。

依据域名生成方式的不同，现有的DGA域名可分为4个大类：基于算术的DGA，基于哈希的DGA，基于单词表的DGA，基于置换的DGA：

1. 基于算术的DGA通过一系列数值运算得到一组值，然后将这些值直接（ASCII编码）或者间接（字符表的偏移量）的转化为字符。这种生成方式计算消耗小，随机性足够强，是当前大部分DGA采用的方式。
2. 基于哈希的DGA则是使用十六进制表示的哈希值生成域名，常用的是SHA256和MD5两种哈希方式，但是此类域名计算量较大。
3. 基于单词表的DGA则是从一个或多个单词表中选择单词拼接成域名，这样的域名通过模拟人类生成域名的方式具备更强的迷惑性。
4. 基于置换的域名则是在若干个初始域名的基础上，通过多轮置换生成多个新的域名，在现有的43中DGA中仅有VolatileCedar一个家族采用种方式。DGA域名生成过程如下图所示。

![域名生成算法示意图](https://img-blog.csdnimg.cn/20210419152147791.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO,size_16,color_FFFFFF,t_70#pic_center)

## 为什么要检测DGA

### 僵尸网络的危害

随着智能家居、智能工控等物联网设备的爆炸增长，新型僵尸网络的规模和危害也不断扩大，僵尸网络仍是当前大型网络攻击的重要平台和源头。botmaster为了能有效控制整个网络，防止安全人员或其他攻击者检测并劫持网络，通常会采用一定技术方法隐藏网络的通讯方式。

早期僵尸网络多采用硬编码方式将CC服务器地址写在僵尸程序中，僵尸主机通过对硬编码在程序中的C&C域名或IP地址进行轮询请求来探测可用的命令控制服务器，但此方法可使用的域名或IP数量有限且无法动态修改，因此防御人员只需要通过逆向获取域名或IP列表后添加入黑名单中即可实现简单高效的防御。

为了解决上述问题，攻击者提出了基于DGA算法的Domain-Flux机制来对抗关闭。C&C地址不再采用硬编码的固定域名或IP，而是根据DGA算法动态生成的、不断变化的域名。通过这种机制，攻击者可以通过周期性的变更所注册的DGA域名子集来隐藏C&C主机IP地址，僵尸主机通过遍历DGA域名全集总能找到当前正在使用的C&C域名，而防御人员不仅无法通过逆向工程直接获取真实C&C地址，同时为了确保能有效切断僵尸网络的通讯需要屏蔽全部DGA域名，时间和技术成本急速上升。因为Domain-Flux机制抗干扰性强，实现方式简单，其已经成为现代僵尸网络通讯机制的主流发展方向。

依据文献可知，43种僵尸网络中已有23种采用Domain-Flux作为其通讯方式，该机制完整应用流程如图2.3所示。

![Domain-Flux示意图](https://img-blog.csdnimg.cn/20210419152146863.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO,size_16,color_FFFFFF,t_70#pic_center)

## 现有的DGA检测方法及相关论文

### 基于域名关联特征的检测

基于域名关联特征的检测主要是结合域名的注册记录、结构特征、DNS解析记录等多维度的数据进行关联分析，以此判断该域名是否被用于Domain-Flux通讯。此类方法由于综合了多方信息，模型从多领域进行分析判断，因此都有较高的准确率和较低的误报率，但缺点在于模型训练和检测过程都需要对多领域、多维度的域名相关信息进行长期的采集、分析与融合，因此这类检测方法消耗资源大、检测实时性低，只适用于历史域名记录的溯源检测。

- 李青山等人提出一种基于域名访问活跃度特征的DGA域名检测算法。该模型通过分析DGA域名与正常域名在子域名数量、域名活跃度等特征方面的差异实现了不依赖于字符特征的检测。
- Samuel Schüppen等人提出了一种基于随机森林和支持向量机的分类模型FANCI。他们从NXDomain记录及正常域名中提取域名的结构特征、字符统计特征、语言学特征和响应信息等多维数据来训练模型。FANCI模型在实验数据集上的准确率为99.93%，在真实流量数据集上的最高误报率仅为0.064%。
- Ryan R. Curtin等人通过定义smashword分数来衡量DGA域名所含单词与英文单词的相似度。他们利用smashword评分并结合域名注册信息训练出了基于RNN网络(recurrent neural networks)的DGA域名检测模型，该模型对于最难识别的matsnu_dga家族的检测准确率为95%，误报率为0.5%。

### 基于域名自身特征的检测

基于域名自身特征的检测是通过挖掘域名自身包含的深层信息来区分正常域名与DGA域名。由于DGA域名多采用特定算法大量生成，因此域名在长度、字符频率、语义关联性等方面与人工命名的域名有不少差异，通提取域名中的这些特征可有效实现对域名的分类。基于域名自身特征的检测方法不需要依赖额外的信息辅助判断域名的类别，因此检测模型消耗资源少、识别速度快，适合部署为实时检测系统，但受限于当前所用技术方法，此类模型的准确性和可解释性仍需要不断提升。

- Jonathan Woodbridg首次将LSTM（Long Short-Term Memory）网络应用于DGA域名检测领域。Woodbridge将训练数据预处理后输入词嵌入层进行向量化表示，然后利用深层的LSTM网络来拟合DGA域名和正常域名的特征，最后通过sigmoid层输出样本为正常域名的概率。该模型在DGA域名二分类任务中有较好的表现，AUC值达到了0.9993，但其无法有效检测基于词表组合类型的DGA域名家族。
- Joewie J. Koh等人针对Jonathan Woodbridge模型的缺陷，提出了一种基于预训练词嵌入层的上下文敏感的DGA域名检测模型。由于基于词表组合类型的DGA域名只是随机从词表中选取单词来组成域名，故所选单词间的语义相关性较低，而正常域名为了方便记忆和传播，所用单词通常具备较强的语义关联，故通过使用上下文敏感的词嵌入层可以为模型提供语义层面的特征。
- Luhui Yang等人则将词嵌入和词性分析引入模型构建中，提出了基于语义分析的，针对基于词表组合类型的DGA域名检测模型。他们在特征抽取中通过加入单词以及词性的分布有效的提升了模型的效果。Bin Yu等人则直接从真实DNS流量中采集域名数据，并提出了一种利用CNN网络(Convolutional Neural Network)对域名进行特征提取和分类的模型charCNN。
- Binay Dahal等人则提出了一种基于自动编码器的DGA域名分类模型。该模型通过在特征提取阶段引入自动编码器来实现特征提取，由此得到的模型可以有效的应对不同DGA域名家族间特征不同的问题。陈立皇等人则提出了一种基于注意力机制的DGA域名检测算法ATT-GRU。
- Daniel S. Berman首次将一维胶囊网络引入DGA域名检测，在降低模型复杂度的同时，保持了同深度卷积神经网络同样的检测效果。Duc Tran等人则针对DGA检测中类别不均衡的问题，提出了一种基于LSTM的数据平衡算法LSTM.MI。

## 现有检测方法的局限

### 1. 未考虑对未知DGA家族的检测能力

模型只对训练集所用DGA家族表现性能较好，无法准确识别训练集外的DGA家族。

### 2. 模型的训练过程强依赖于DGA数据的获取

由于所提第一点问题，因此对于新型的DGA家族，现有模型必须首先获取新型DGA家族的域名数据，然后再重新训练模型使其具备识别能力。存在的问题是：首先，DGA域名数据的获取是困难的、滞后的；其次，DGA域名有效时间短，变化种类多。

## 所做工作

引入Positive-Confidence框架理论，通过对训练过程中损失函数的修正，将负样本相关变量通过正样本及其可信度表示，使得模型训练过程中只需要拟合正常域名特征，有效增强了模型泛化性能，使其具备发现新型DGA家族的能力，同时模型训练不直接依赖于DGA数据，缩短模型训练周期。

![流程示意图](https://img-blog.csdnimg.cn/20210423122919390.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO,size_16,color_FFFFFF,t_70#pic_center)

### Positive-Confidence框架

针对数据集极度不均衡情景下负样本太少，模型训练不可靠的问题，Takashi Ishida等人在2018年NIPS会议上提出了一种改进的经验风险最小化框架。

**论文：[Binary Classification from Positive-Confidence Data](https://dl.acm.org/doi/10.5555/3327345.3327492)**

#### 理论分析

在Positive-Confidence框架中，假设样本数据为 $d$ 维向量 $x \in R^d$，样本类别标签为 ${-1,1}$，数据服从未知分布 $p(x,y)$。现在要训练一个分类模型 $g(x)$，使得经验损失 $R(g)$ 最小化，其定义为：

$$R(g)=E_{p(x,y)}[l(yg(x))]$$

其中 $l(yg(x))$ 是损失函数，$E_{p(x,y)}$ 表示基于 $p(x,y)$ 的期望。将上式展开有：

$$R(g)=\sum_{y=\pm1}\int l(yg(x))p(x|y)p(y)dx$$

其中 $p(x|y)$ 表示随机变量 $x$ 基于 $y$ 的分布。若用 $\pi_-$ 表示 $p(y=-1)$，$E_-$ 表示 $E_{p(x|y=-1)}$，则上式可以表示为

$$R(g)=\int l(g(x))p(x|y=+1)p(y=+1)dx+\int l(-g(x))p(x|y=-1)p(y=-1)dx$$

$$=\pi_+E_+[l(g(x))]+\pi_-E_-[l(-g(x))]$$

由于存在以下变换：

$$\pi_+p(x|y=+1)+\pi_-p(x|y=-1)=p(x)=\frac{\pi_+p(x|y=+1)}{r(x)}$$

其中 $r(x)=p(y=+1|x)$，若 $p(y=+1|x)\neq0$ 对于分布为 $p(x)$ 的全部 $x$ 都成立，则将上式变形可以得到：

$$\pi_-p(x|y=-1)=\pi_+p(x|y=+1)(\frac{1-r(x)}{r(x)})$$

最终可以将优化目标转化为：

$$\min_g\sum_{i=1}^{n}[l(g(x_i))+\frac{1-r_i}{r_i}l(-g(x_i))]$$

### LSTM网络

LSTM (Long Short-Term Memory)是Hochreiter和Schmidhuber设计发展的一种深度循环神经网络。LSTM的提出主要是为了解决普通循环神经网络学习长序列过程中的梯度消失和梯度爆炸问题。LSTM通过在内部引入门控结构来控制数据的传输状态，使其在训练过程中不是一味的记录全部的时序数据，而是模仿人类的记忆的方式，有选择性的保留需要长时间存储的信息，忘记不重要的信息，因而在各类序列任务中有优异的表现。

![LSTM网络结构](https://img-blog.csdnimg.cn/20210423123241590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO,size_16,color_FFFFFF,t_70#pic_center)

LSTM中神经元学习原理流程如下：

- 第一步是决定那些信息需要从神经元状态中丢弃，由"遗忘门"来决定。
- 第二步是决定那些新的信息需要保存在神经元状态中，由"输入门"来决定。
- 第三步需要将旧神经元状态更新到当前时刻神经元状态中。
- 第四步决定当前时刻神经元的输出信息。

### ProfDGA模型结构及超参数

以LSTM网络为基础模型，以Positive-Confidence理论为训练框架，构建了针对DGA域名的检测模型ProfDGA。

**模型超参数：**

| 参数 | 值 |
| :--- | :--- |
| embedding_dim | 128 |
| lstm_hiden_size | 128 |
| batch_size | 512 |
| max_epoch | 10 |
| lr | 0.01 |

### 实验结果

![同型测试结果](https://img-blog.csdnimg.cn/20210423133209210.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO,size_16,color_FFFFFF,t_70#pic_center)

![异形测试结果](https://img-blog.csdnimg.cn/20210423133209358.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dxY19DU0RO,size_16,color_FFFFFF,t_70#pic_center)
