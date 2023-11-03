---
description: cnn,rnn,transformer
---

# ✍ long-term dependency

### 长依赖问题

## CNN



用深度来换取远距离依赖



由卷积层和[池化层](https://www.zhihu.com/search?q=%E6%B1%A0%E5%8C%96%E5%B1%82\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%222839550750%22%7D)组成，它们是由权重共享的卷积核（或滤波器）和[非线性激活函数](https://www.zhihu.com/search?q=%E9%9D%9E%E7%BA%BF%E6%80%A7%E6%BF%80%E6%B4%BB%E5%87%BD%E6%95%B0\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%222839550750%22%7D)（如ReLU）构成的

#### Advantage：语料里的一些文本片段对输出类别有很强指示性

#### shorts:过程太间接了，因为信息在网络中实际传播了太多层。究竟哪些信息被保留，哪些被丢弃了，弄不清楚。

{% file src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1).png" %}

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

##

## RNN

Word Embedding:每一个单词转变为一个向量

首先处理文本的第一个输入单词“I”。首先通过查表得到单词“I”的向量表示E(I)。然后将E(I)和一个初始的 h0输入给模型（蓝色框）。 h一般被称为“隐状态（hidden state）”，

h0因为是第一个隐状态，所以它的值通常要么全为0，要么赋为随机值。

模型在第一步得到输入h0和E(I)，并通过函数g 来计算输出h1=g(h0,E(I))



\
RNN 在循环过程中，每个词按顺序输入，因此隐含地知道每个词的位置。

因为后一个词的计算需要用到前一个词的输出结果，所以理论上任何两个词的[依赖RNN](https://www.zhihu.com/search?q=%E4%BE%9D%E8%B5%96RNN\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2979260071%7D)都能捕捉到。

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### advantage：同时可以对前后词之间依赖建模（=CNN+pool）

#### shorts:容易出现梯度消失或梯度爆炸,所以对长依赖

#### pro modle :LSTM、GRU



##

{% file src="../.gitbook/assets/image (6) (1) (1) (1) (1).png" %}

##

## Transformer:

#### advantage：长距离（一个句子中各个词之间的相似结构信息加到模型里）

自注意力层（经过编码器或解码器）和前馈[全连接层](https://www.zhihu.com/search?q=%E5%85%A8%E8%BF%9E%E6%8E%A5%E5%B1%82\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%222839550750%22%7D)组成

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>





{% file src="../.gitbook/assets/image (7) (1) (1) (1) (1).png" %}

## References：

{% embed url="https://zhuanlan.zhihu.com/p/615000678" %}

{% embed url="https://www.zhihu.com/question/580810624/answer/2979260071" %}

{% embed url="https://zhuanlan.zhihu.com/p/264749298" %}
