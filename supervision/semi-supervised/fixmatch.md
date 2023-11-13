---
description: 一致性正则化和伪标签方法的结合，一致性正则化时使用独立的弱增强和强增强。
---

# FixMatch

_（什么是独立的弱增强和强增强）_

## 分析、

#### 核心：

* 一致性正则和伪标签方法的，简单组合
* 无标签模型预测，与UDA一样，采用[RandAugment\[](fixmatch.md#3.randaugment)3]进行强增强



#### 增强的区别：

* “弱”增强（翻转、缩放）
* “强”增强（CutOut、CTAugment、RandAugment),

\




<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

## 论文摘要

将一个弱增强的图像（顶部）输入到模型中以获得预测结果（红色框）。**当模型对任何类别分配的概率高于一个阈值（虚线）时，将预测结果转换为一个（**[**独热**](fixmatch.md#user-content-extended-explanation-1)**伪标签（**a one-hot pseudo-label**），**只有当模型预测高于阈值时，才会保留为**伪标签（**相当于，在前期训练阶段中，无标签样本损失可能一直是为0的**）**。

然后，我们计算模型对同一图像进行**强增强**后的版本的预测结果（底部）。

通过**交叉熵损失**，训练模型使其在强增强版本上的预测结果与伪标签相匹配。



人工标签是基于一个**弱增强**的无标签图像产生的（例如，只使用翻转和平移数据增强），当模型输入同一图像的强增强版本时，这个弱增强的无标签图像被用作目标。



对于一个具有L个类别的分类问题，令X = (xb, pb) : b ∈ (1, . . . , B)表示一个包含B个标记示例的批次，其中xb是训练示例，pb是独热标签。令U = ub : b ∈ (1, . . . , µB)表示一个包含µB个无标签示例的批次，其中µ是一个超参数，用于确定X和U的相对大小。

pm(y | x)：模型对输入x产生的预测类别分布。

H(p, q)：两个概率分布p和q之间的交叉熵。

强增强和弱增强：分别用A(·)和α(·)表示。

α and Pm 是随机函数

#### 损失函数：

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

对这个想法的扩展：

在α的位置使用对抗性变换

在一次pm调用中使用运行平均或过去模型预测

在平方`2`损失的位置使用交叉熵损失，使用更强的数据增强形式

以及将一致性正则化作为较大的自监督学习（SSL）流程的组成部分。



#### 伪标签函数：

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

仅保留最大类别（q̂b = arg max(qb)）概率高于预定义阈值的人工标签，τ是阈值

使用**硬标签**使得伪标签与熵最小化密切相关，其中鼓励模型在无标签数据上具有低熵（即高置信度）的预测。















\


{% embed url="https://arxiv.org/abs/2001.07685" %}
\\
{% endembed %}

[https://github.com/google-research/fixmatch](https://github.com/google-research/fixmatch)

## [Extended explanation：](https://github.com/rosettarosettarosetta/read-paper/blob/main/sam/segment-anything.md#extended-explanation) <a href="#user-content-extended-explanation" id="user-content-extended-explanation"></a>

### 1.独热（one hot） <a href="#user-content-extended-explanation" id="user-content-extended-explanation"></a>

离散特征的编码分为两种情况：

1、离散特征的取值之间没有大小的意义，比如color：\[red,blue],那么就使用one-hot编码

2、离散特征的取值有大小的意义，比如size:\[X,XL,XXL],那么就使用数值的映射{X:1,XL:2,XXL:3}



其方法是使用N位状态寄存器来对N个状态进行编码



优点：独热编码解决了分类器不好处理属性数据的问题，在一定程度上也起到了扩充特征的作用。它的值只有0和1，不同的类型存储在垂直的空间。



ex:

猫：\[1, 0, 0] 狗：\[0, 1, 0] 鸟：\[0, 0, 1]

{% embed url="https://zhuanlan.zhihu.com/p/134495345" %}

### 2.交叉熵损失函数

#### 二分类

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

#### &#x20;多分类

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

\- ![M](https://www.zhihu.com/equation?tex=M\&consumer=ZHI\_MENG) ——类别的数量\
\- ![y\_{ic}](https://www.zhihu.com/equation?tex=y\_%7Bic%7D\&consumer=ZHI\_MENG) ——符号函数（ ![0](https://www.zhihu.com/equation?tex=0\&consumer=ZHI\_MENG) 或 ![1](https://www.zhihu.com/equation?tex=1\&consumer=ZHI\_MENG) ），如果样本 ![i](https://www.zhihu.com/equation?tex=i\&consumer=ZHI\_MENG) 的真实类别等于 ![c](https://www.zhihu.com/equation?tex=c\&consumer=ZHI\_MENG) 取 ![1](https://www.zhihu.com/equation?tex=1\&consumer=ZHI\_MENG) ，否则取 ![0](https://www.zhihu.com/equation?tex=0\&consumer=ZHI\_MENG)\
\- ![p\_{ic}](https://www.zhihu.com/equation?tex=p\_%7Bic%7D\&consumer=ZHI\_MENG) ——观测样本 ![i](https://www.zhihu.com/equation?tex=i\&consumer=ZHI\_MENG) 属于类别 ![c](https://www.zhihu.com/equation?tex=c\&consumer=ZHI\_MENG) 的预测概率

{% embed url="https://www.zhihu.com/tardis/zm/art/35709485?source_id=1003" %}
2
{% endembed %}

### 3.RandAugment

是一种数据增强方法

RandAugment通过在图像上应用一系列随机的图像变换来增强数据。这些变换包括旋转、缩放、裁剪、翻转、颜色变换等。具体的变换和其参数是通过随机选择生成的，以增加数据的多样性。

RandAugment的一个关键优点是，它可以通过控制变换的数量和强度来_平衡数据增强和模型的容量_。

### 4.硬标签

是指将样本的标签表示为确定的类别值，通常是一个整数或类别标识符。



vs：**软标签**（Soft Labels）是指将样本的标签表示为概率分布或连续值，用于表示模型对不同类别的置信度或概率。

## REFERENCE：

1.离散特征的编码分为两种情况：

.离散特征的取值之间没有大小的意义，比如color：\[red,blue],那么就使用one-hot编码

.离散特征的取值有大小的意义，比如size:\[X,XL,XXL],那么就使用数值的映射

\


{% embed url="https://www.zhihu.com/question/457133996/answer/2672409953" %}



