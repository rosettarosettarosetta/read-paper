---
description: 2017 3000引用
---

# 👁 Mean Teachers

**CL**

Mean Teachers是在模型的权重上。且该算法的核心思想是将模型分为教师和学生，老师用来生成学生学习的目标，学生用老师提供的目标来进行学习，而老师模型的权重是通过学生[模型时间](https://www.zhihu.com/search?q=%E6%A8%A1%E5%9E%8B%E6%97%B6%E9%97%B4\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22577959642%22%7D)记忆的加权平均得到，同时该算法也是**基于一致性正则化**



默认输入数据添加一个微小的扰动噪声时，模型的预测结果不会发生变化。

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

reference:

1\.&#x20;

{% embed url="https://zhuanlan.zhihu.com/p/577959642" %}

2\.





## ABSTRACT:

1.temporal ensembing 时间集成 的重要性



由于对于未标记的示例，分类成本未定义，仅凭噪声正则化本身无法帮助半监督学习。为了克服这个问题，Γ模型\[21]在有噪声和无噪声的情况下评估每个数据点，然后应用两个预测之间的一致性成本。在这种情况下，模型扮演双重角色，既是教师又是学生。作为学生，它像以前一样学习；作为教师，它生成目标，然后自己作为学生使用这些目标进行学习。由于模型本身生成目标，它们很可能是不正确的。如果给予生成的目标太多权重，不一致性的成本就会超过错误分类的成本，从而阻止学习新的特征。



因此，我们的目标是在不进行额外训练的情况下从学生模型中构建一个更好的教师模型。



## Extended explanation： <a href="#user-content-extended-explanation" id="user-content-extended-explanation"></a>

1.temporal ensembing ：the targets change only once per epoch ，so it is  unwieldy when learning large datasets



{% embed url="https://arxiv.org/abs/1703.01780" %}

{% embed url="https://arxiv.org/pdf/1703.01780.pdf" %}

\
