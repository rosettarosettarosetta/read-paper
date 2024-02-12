---
description: 2017 3000引用
---

# 👁 Mean Teachers

**CL**

算法的核心是将模型分为教师和学生，老师用来生成学生学习的目标，学生用老师提供的目标来进行学习，老师模型的权重是通过学生[模型时间](https://www.zhihu.com/search?q=%E6%A8%A1%E5%9E%8B%E6%97%B6%E9%97%B4\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22577959642%22%7D)记忆的加权平均得到，同时该算法也是**基于一致性正则化（**consistency**）**



默认输入数据添加一个微小的扰动噪声时，模型的预测结果不会发生变化。

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (2).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

reference:

1\.&#x20;

{% embed url="https://zhuanlan.zhihu.com/p/577959642" %}

2\.





## ABSTRACT:

1.temporal ensembing 时间集成 的重要性



由于对于未标记的示例，分类成本未定义，仅凭噪声正则化本身无法帮助半监督学习。为了克服这个问题，模型在有噪声和无噪声的情况下评估每个数据点，然后应用两个预测之间的一致性成本。在这种情况下，模型扮演双重角色，既是教师又是学生。作为学生，它像以前一样学习；作为教师，它生成目标，然后自己作为学生使用这些目标进行学习。

由于模型本身生成目标，它们很可能是不正确的。如果给予生成的目标太多权重，不一致性的成本就会超过错误分类的成本，从而阻止学习新的特征。



目标是在不进行额外训练的情况下从学生模型中构建一个更好的教师模型。



作为第一步，考虑到模型的softmax输出通常在训练数据之外无法提供准确的预测。这可以通过在推断时向模型添加噪声来部分缓解，因此，一个带有噪声的教师可以产生更准确的目标。Laine和Aila \[13]将该方法命名为Π模型；我们将使用这个名称以及他们的版本作为我们实验的基础。



Π模型它对每个训练样本维护一个**指数移动平均（EMA）**预测。在每个训练步骤中，该小批量中所有示例的EMA预测基于新的预测进行更新。因此，每个示例的EMA预测由模型的当前版本和之前评估相同示例的早期版本组成的集合形成。这种集成提高了预测的质量，并且使用它们作为教师预测可以改善结果。然而，由于每个目标每个时期只更新一次，学到的信息以较慢的速度纳入训练过程中。数据集越大，更新的跨度越长，在在线学习的情况下，Temporal Ensembling如何使用尚不清楚。（可以周期性地多次评估所有目标超过一个时期，但保持评估跨度恒定将需要每个时期进行O(n^2)次评估，其中n是训练样本的数量。）



&#x20;the teacher model is an average of consecutive student models

&#x20;teacher model uses the EMA weights of the student model.





### the consistency cost J（一致性成本）

<figure><img src="../../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

&#x20;&#x20;

（ distance between the prediction of the student model (with weights θ and noise η) and the prediction of the teacher model (with weights θ 0 and noise η 0 ).）







Π 模型、Temporal Ensembling 和 Mean teacher 之间的区别在于教师预测是如何生成的。

Π 模型使用 θ“ = θ，而 Temporal Ensembling 则通过对连续预测的加权平均来近似 f(x, θ0, η0)。在这里，我们将训练步骤 t 时的 θ”t 定义为连续 θ 权重的 EMA（指数移动平均）：

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

α is a smoothing coefficient hyperparameter （平滑系数超参数）



这三种算法之间的另一个区别在于 Π 模型对 θ'进行训练，而 Temporal Ensembling 和 Mean Teacher 则将其视为优化过程中的常数。我们可以通过使用随机梯度下降在每个训练步骤中对噪声 η、η0 进行采样来近似计算一致性成本函数 J。在大多数实验中，我们使用均方误差（MSE）作为一致性成本。



学生模型和教师模型都在其计算过程中应用噪声（η，η0）来评估输入。学生模型的softmax输出与独热标签使用分类成本进行比较，并与教师输出使用一致性成本进行比较。在使用梯度下降更新学生模型的权重之后，教师模型的权重被更新为学生模型权重的指数移动平均值。两个模型的输出都可以用于预测，但在训练结束时，教师的预测更有可能是正确的。使用未标记示例的训练步骤类似，只是不会应用分类成本。





## Extended explanation： <a href="#user-content-extended-explanation" id="user-content-extended-explanation"></a>

1.temporal ensembing ：the targets change only once per epoch ，so it is  unwieldy when learning large datasets



{% embed url="https://arxiv.org/abs/1703.01780" %}

{% embed url="https://arxiv.org/pdf/1703.01780.pdf" %}

\
