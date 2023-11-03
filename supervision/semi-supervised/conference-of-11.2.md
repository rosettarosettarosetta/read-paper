# ✍ conference of 11.2

### 两个方法:PL,CL

### Curriculum Labeling (CL)：

利用一致性，如teacher/student输出一致、数据增强后预测结果应一致

通过在每个自学习周期之前重新启动模型参数来避免[概念漂移](https://www.zhihu.com/search?q=%E6%A6%82%E5%BF%B5%E6%BC%82%E7%A7%BB\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22530860794%22%7D)



半监督学习的一个必要条件是输入空间上潜在的边际数据分布 p(x) 包含[后验分布](https://www.zhihu.com/search?q=%E5%90%8E%E9%AA%8C%E5%88%86%E5%B8%83\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22433751531%22%7D) p(y|x) 的信息.

p(x)为输入样本的分布      p(y|x) 表示在给定某个样本的情况下，得到其所属某个类的概率（后演概率）

我们可以使用未标记的数据来获得关于 p(x) 的信息，从而获得关于 p(y|x) 的信息。



主要思想：对于同一样本经过不同的变换或扰动后，网络对变换（扰动）前后的输出应是相似的。

基于假设：_smoothness assumption：对于x_,x′∈X 并且非常的接近，那么他们所对应的标签 y,y′ 是相同的。

_Low-density assumption（略，感觉无关）_一个好的决策边界应该尽可能通过这种样本稀疏的区域（[低密度区域](https://www.zhihu.com/search?q=%E4%BD%8E%E5%AF%86%E5%BA%A6%E5%8C%BA%E5%9F%9F\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22433751531%22%7D)），这样才能更好地区分不同类别的样本 （详见reference1）

### PL：伪标签

利用已有全监督模型生成伪标签

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

伪标签使用带有 Dropout 的微调阶段，可以将预训练的网络以有监督的方式同时使用标记和未标记的数据进行训练。

流程：

* 将有标签部分数据分为两份：train\_set&[validation\_set](https://www.zhihu.com/search?q=validation\_set\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22116065447%22%7D)，并训练出最优的model1
* 用model1对未知标签数据(test\_set)进行预测，给出伪标签结果pseudo-labeled
* 将train\_set中抽取一部分做新的validation\_set，把[剩余部分](https://www.zhihu.com/search?q=%E5%89%A9%E4%BD%99%E9%83%A8%E5%88%86\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22116065447%22%7D)与[pseudo-labeled](https://www.zhihu.com/search?q=pseudo-labeled\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22116065447%22%7D)部分融合作为新的train\_set，训练出最优的model2
* 再用model2对未知标签数据(test\_set)进行预测，得到最终的final result label





FixMatch





## REFERENCR:

1. CL 基础讲解（有一些进阶的论文部分没看）

{% embed url="https://zhuanlan.zhihu.com/p/433751531" %}

2\.

：





## ABSTRACT：

1.Semi-Supervised Semantic Segmentation with Cross Pseudo Supervision（CVPR2021）

[交叉伪监督](https://www.zhihu.com/search?q=%E4%BA%A4%E5%8F%89%E4%BC%AA%E7%9B%91%E7%9D%A3\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22433751531%22%7D)（一种新的、简单的网络扰动一致性[正则化方法](https://www.zhihu.com/search?q=%E6%AD%A3%E5%88%99%E5%8C%96%E6%96%B9%E6%B3%95\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22433751531%22%7D)）&#x20;

交叉伪监督两个目的：鼓励两个[扰动网络](https://www.zhihu.com/search?q=%E6%89%B0%E5%8A%A8%E7%BD%91%E7%BB%9C\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22433751531%22%7D)对同一输入图像的预测高度相似（Consistency learning），并使用带有伪标签的未标记数据扩展训练数据（self-training）。

\
\


