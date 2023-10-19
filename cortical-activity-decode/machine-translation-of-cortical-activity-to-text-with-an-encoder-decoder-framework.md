---
description: https://www.biorxiv.org/content/10.1101/708206v1.full.pdf
---

# Machine translation of cortical activity to text with an encoder-decoder framework

we train a recurrent neural network to map neural  signals directly to word sequences (sentences)

the network first encodes a 14 sentence-length sequence of neural activity into an abstract representation, and then decodes this 15 representation, word by word\
错句率7%（woc，真的那么神奇吗）\
\
循环神经网络，将神经信号直接映射到单词序列（句子）。

特别地，该网络首先将一段与句子长度相对应的神经活动序列编码为抽象表示，然后逐字将该表示解码为英语句子。后面用迁移学习



实验：训练数据包括一组约30-50个句子的多次口头重复，以及分布在周围沟语言皮层上的大约250个电极处的相应神经信号（electrocorticogram (ECoG)）。

多模态（又是他.....）



<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

{% embed url="https://www.biorxiv.org/content/10.1101/708206v1.full.pdf" %}
