---
description: 一致性正则化和伪标签方法的结合，一致性正则化时使用独立的弱增强和强增强。
---

# 👁 FixMatch

_（什么是独立的弱增强和强增强）_

## 分析、

#### 核心：

* 一致性正则和伪标签方法的，简单组合
* 无标签模型预测，与UDA一样，采用[RandAugment\[](fixmatch.md#3.randaugment)3]进行强增强

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

## 论文摘要

将一个弱增强的图像（顶部）输入到模型中以获得预测结果（红色框）。**当**[<mark style="color:yellow;">**模型**</mark>](fixmatch.md#1.what-does-two-models-mean-is-one-be-made-from-the-paper)**对任何类别分配的概率高于一个阈值（虚线）时，将预测结果转换为一个（**[**独热**](fixmatch.md#user-content-extended-explanation-1)**伪标签（**a one-hot pseudo-label**），**只有当模型预测高于阈值时，才会保留为**伪标签（**相当于，在前期训练阶段中，无标签样本损失可能一直是为0的**）**。

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

这指的是使用“**硬”标签**（即模型输出的arg max）并仅保留最大类别概率高于预定义阈值的人工标签

仅保留最大类别（q̂b = arg max(qb)，qd为伪标签）概率高于预定义阈值的人工标签，τ是阈值

使用[**硬标签**](fixmatch.md#4.-ying-biao-qian)使得伪标签与熵最小化密切相关，其中鼓励模型在无标签数据上具有低熵（即高置信度）的预测。（有点不懂了）



#### 损失函数：

损失函数包括2个交叉熵函数：有标签数据的监督损失<img src="../../.gitbook/assets/image (44).png" alt="" data-size="line">(应用于标注的数据)和无监督损失<img src="../../.gitbook/assets/image (45).png" alt="" data-size="line">

<img src="../../.gitbook/assets/image (48).png" alt="" data-size="line">只是应用于弱增强有标签样本的标准交叉熵损失：



<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

FixMatch为每个无标签样本计算一个人工标签，然后在标准的交叉熵损失中使用该标签。

为了获得**人工标签**，我们首先计算模型在给定无标签图像的弱增强版本上的预测类别分布：qb = pm(y | α(ub)）。然后，我们使用q̂b = arg max(qb)作为伪标签，但我们强制将交叉熵损失施加在模型对ub的强**增强版本的输出上**：



<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

与上上个式子相似，区别在于人工标签是基于弱增强图像计算的，并且损失是针对强增强图像的模型输出进行施加的。



FixMatch所最小化的损失函数简单地表示为`s + λu`u，其中λu是一个固定的标量超参数，表示无标签损失的相对权重。在现代SSL算法中，通常会在训练过程中增加无标签损失项（λu）的权重\[51, 24, 4, 3, 36]。然而，我们发现对于FixMatch来说这是不必要的，



### 增强的区别：

* “弱”增强（翻转、缩放）

50%的概率在所有数据集（除了SVHN）上随机水平翻转图像，并且在垂直和水平方向上随机平移图像，最多不超过12.5%。

* “强”增强（CutOut、CTAugment、RandAugment，AutoAugment变体),

基于AutoAugment的RandAugment和CTAugment

AutoAugment使用强化学习来找到一种由Python Imaging Library中的变换组成的增强策略。然而，这需要使用有标签数据来学习增强策略，在有限的有标签数据可用的SSL设置中使用时存在问题。

RandAugment和CTAugment两者**不需要预先使用有标签数据学习增强策略**，而是对每个样本随机选择变换。

对于RandAugment，控制所有失真程度的幅度是从预定义的范围中随机采样的（还使用了具有随机幅度的RandAugment进行了UDA），而CTAugment则在运行时动态学习每个变换的幅度。



### 训练与参数

我们使用模型参数的指数移动平均来报告最终性能。

对于学习率调度，我们使用余弦学习率衰减\[28]，将学习率设置为<img src="../../.gitbook/assets/image (53).png" alt="" data-size="line">，其中η是初始学习率，k是当前训练步骤，K是总训练步骤数。



### 扩展：

ReMixMatch中的[**增强锚定**](fixmatch.md#5.-zeng-qiang-mao-ding)（对于每个无标签样本，使用M个强增强进行一致性正则化）和**分布对齐**（鼓励模型预测具有与有标签集相同的类别分布）可以直接应用于FixMatch。此外，可以将FixMatch中的强增强替换为与模态无关的增强策略，例如MixUp \[59]或对抗扰动\[33]。



FixMatch与两种最近的方法最相似，它们分别是无监督数据增强（UDA）\[54]和ReMixMatch \[3]

{% embed url="https://arxiv.org/abs/2001.07685" %}
\\
{% endembed %}

[https://github.com/google-research/fixmatch](https://github.com/google-research/fixmatch)

## &#x20;<a href="#user-content-extended-explanation" id="user-content-extended-explanation"></a>

## &#x20;<a href="#user-content-extended-explanation" id="user-content-extended-explanation"></a>

## Extended explanation： <a href="#user-content-extended-explanation" id="user-content-extended-explanation"></a>

### 1.独热（one hot） <a href="#user-content-extended-explanation" id="user-content-extended-explanation"></a>

离散特征的编码分为两种情况：

1、离散特征的取值之间没有大小的意义，比如color：\[red,blue],那么就使用one-hot编码

2、离散特征的取值有大小的意义，比如size:\[X,XL,XXL],那么就使用数值的映射{X:1,XL:2,XXL:3}



其方法是使用N位状态寄存器来对N个状态进行编码



优点：独热编码解决了分类器不好处理属性数据的问题，在一定程度上也起到了扩充特征的作用。它的值只有0和1，不同的类型存储在垂直的空间。



ex:

猫：\[1, 0, 0]&#x20;

狗：\[0, 1, 0]&#x20;

鸟：\[0, 0, 1]

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



### 5.增强锚定

对于每个无标签样本，增强锚定使用多个强增强方法生成多个增强版本的样本。然后，这些增强版本的样本被输入到模型中进行预测。一致性正则化的思想是，如果模型在不同增强版本上产生了相似的预测结果，那么这些预测结果应该是可靠的。

## REFERENCE：

1.离散特征的编码分为两种情况：

.离散特征的取值之间没有大小的意义，比如color：\[red,blue],那么就使用one-hot编码

.离散特征的取值有大小的意义，比如size:\[X,XL,XXL],那么就使用数值的映射



## Experiments experiment

作者也实验了在一致性正则化中，强弱数据增强策略不同的组合对结果影响。强增强+强增强的组合会导致模型发散。将弱增强替换为无增强则会导致模型过拟合。使用弱增强+弱增强则会导致训练失败（这里笔者难以理解内在的原因）。

\


## question:

### 1.what does two models mean?is one be made from the paper?&#x20;

gpt的答案：

在FixMatch模型中，获取模型的预测结果通常是通过调用模型的推理（inference）过程来实现的。具体步骤如下：

1. 准备输入数据：将待预测的图像数据进行预处理，包括图像大小调整、归一化等操作，以符合模型的输入要求。
2. 调用模型的推理过程：将预处理后的图像数据输入到已经训练好的FixMatch模型中。
3. 获取预测结果：模型会对输入的图像进行前向传播，计算出每个类别的概率分布。可以通过获取概率最高的类别或者根据需要获取其他信息，如置信度等。
4. 解释和使用预测结果：根据具体的应用场景，可以将预测结果用于分类、目标检测、图像生成等任务。

### 2.when have the loss ,how to imporve the model? 

{% embed url="https://www.zhihu.com/question/457133996/answer/2672409953" %}

{% embed url="https://zhuanlan.zhihu.com/p/387276231" %}

