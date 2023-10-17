---
description: cnn,rnn,transformer
---

# long-term dependency problem

### 长依赖问题

### CNN

用深度来换取远距离依赖

{% file src="../.gitbook/assets/image (5).png" %}

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

## RNN

[词嵌入](https://www.zhihu.com/search?q=%E8%AF%8D%E5%B5%8C%E5%85%A5\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2979260071%7D)（Word Embedding）:每一个单词转变为一个向量

首先处理文本的第一个输入单词“I”。首先通过查表得到单词“I”的向量表示E(I)。然后将E(I)和一个初始的 h0h\_0h\_0输入给模型（蓝色框）。 hhh一般被称为“隐状态（hidden state）”， h0h\_0h\_0因为是第一个隐状态，所以它的值通常要么全为0，要么赋为随机值。

模型在第一步得到输入h0h\_0h\_0 和E(I)，并通过函数ggg 来计算输出h1=g(h0,E(I))h\_1=g(h\_0,E(I))h\_1=g(h\_0,E(I))。



#### shorts:容易出现梯度消失或梯度爆炸

#### pro modle :LSTM、GRU

## References：

[https://www.zhihu.com/collection/916759096](https://www.zhihu.com/collection/916759096)
