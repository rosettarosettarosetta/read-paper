# ✅ semi-supervised

## 综述：

注意：**Semi-supervised clustering**，



在深度学习环境中，采用经典的SSL框架并发展新的SSL方法非常重要，这导致了深度半监督学习（DSSL）。

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

我们将DSSL分类为五类，即**生成方法、一致性正则化方法（CL）、基于图的方法、伪标记方法(PL)和混合方法**



**对抗训练：**

对抗训练（Adversarial Training）是一种通过引入对抗性示例或对抗性信号来训练模型的方法。它主要用于增强模型的鲁棒性和抗干扰能力。

在对抗训练中，通常存在两个模型：生成模型和判别模型。生成模型旨在生成具有对抗性特征的样本，以尽可能地欺骗判别模型。判别模型则被训练用于区分真实样本和生成样本。这两个模型通过交替训练进行对抗优化。

{% embed url="https://arxiv.org/pdf/2103.00550.pdf" %}
