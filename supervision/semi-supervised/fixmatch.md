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



\




{% embed url="https://arxiv.org/abs/2001.07685" %}
\\
{% endembed %}

[https://github.com/google-research/fixmatch](https://github.com/google-research/fixmatch)
