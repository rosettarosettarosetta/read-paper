---
description: '2018'
---

# Mean Teachers

**CL**

Mean Teachers是在模型的权重上。且该算法的核心思想是将模型分为教师和学生，老师用来生成学生学习的目标，学生用老师提供的目标来进行学习，而老师模型的权重是通过学生[模型时间](https://www.zhihu.com/search?q=%E6%A8%A1%E5%9E%8B%E6%97%B6%E9%97%B4\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22577959642%22%7D)记忆的加权平均得到，同时该算法也是**基于一致性正则化**



默认输入数据添加一个微小的扰动噪声时，模型的预测结果不会发生变化。

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

reference:

1\.&#x20;

{% embed url="https://zhuanlan.zhihu.com/p/577959642" %}

2\.



{% embed url="https://arxiv.org/pdf/1703.01780.pdf" %}

\
