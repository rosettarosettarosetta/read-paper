# FixMatch



FixMatch是SSL的两种方法的组合：**一致性正则**和**伪标签**



对于无标签的样本，`FixMatch`：

* FixMatch首先对弱增强的无标签图像，预测伪标签\

*
  * 对于给定的图像，只有当模型产生高于阈值的预测时，才会保留作为伪标签

\


* 再对同一图像的强增强版本，预测出分类概率\

*
  * 通过交叉熵损失衡量，强弱二者的预测的一致性

\
FixMatch的核心：

* 一致性正则和伪标签方法的，简单组合
* 无标签模型预测，与UDA一样，采用RandAugment\[3]进行强增强



<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

将一个弱增强的图像（顶部）输入到模型中以获得预测结果（红色框）。**当模型对任何类别分配的概率高于一个阈值（虚线）时，将预测结果转换为一个独热伪标签（**a one-hot pseudo-label**）**。然后，我们计算模型对同一图像进行强增强后的版本的预测结果（底部）。通过**交叉熵损失**，训练模型使其在强增强版本上的预测结果与伪标签相匹配。



人工标签是基于一个**弱增强**的无标签图像产生的（例如，只使用翻转和平移数据增强），当模型输入同一图像的强增强版本时，这个弱增强的无标签图像被用作目标。



利用一张无标签样本，分别进行：

* “弱”增强（翻转、缩放）
* “强”增强（CutOut、CTAugment、RandAugment),



* 然后，通过model得到预测标签
* 并，通过标准交叉熵损失计算损失

\


上述“弱“增强方式预测过程，需要设定一个阈值大于阈值的才计算loss，小于的就不计算

相当于，在前期训练阶段中，无标签样本损失可能一直是为0的

\


{% embed url="https://arxiv.org/abs/2001.07685" %}
\\
{% endembed %}

[https://github.com/google-research/fixmatch](https://github.com/google-research/fixmatch)

## REFERENCE：

1\.

{% embed url="https://www.zhihu.com/question/457133996/answer/2672409953" %}



