---
description: https://www.zhihu.com/search?type=content&q=tarnsformer
---

# ✍ Transformer

### Encoder  Decoder :

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p>都包含 6 个 block</p></figcaption></figure>



#### 1.

获取输入句子的每一个单词的表示向量 X，X由单词的 Embedding（Embedding就是从原始数据提取出来的Feature） 和单词位置的 Embedding 相加得到。

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>











仅仅用了注意力机制而不是循环或者卷积

第一个只依赖于自注意力

point : multi-headed self-attention层





时序模型现在用的最多的是rnn(一个词一个词的分析)

transofrmer类似于多头的注意力，约等于多输出通道





使用编码器解码器架构

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

#### 1.inputs

#### 2.tokenization

1. 分割文本：将输入文本划分为不同的单词、子词或字符。通常，这个步骤使用空格或标点符号来进行分割。
2. 构建词汇表：根据分割后的文本构建一个词汇表或字典，其中包含模型需要处理的所有单词、子词或字符。词汇表中的每个单词、子词或字符都会分配一个唯一的标识符，称为"token ID"。
3. 编码输入序列：将分割后的文本转换为模型可以处理的数值表示。每个单词、子词或字符都会用其对应的token ID进行编码。
4. 添加特殊标记：为了提供额外的语义信息，通常会在输入序列的开头和结尾添加特殊的起始和结束标记。这些标记有助于Transformer模型理解序列的开始和结束。
5. 限制序列长度：为了满足模型输入的要求，通常会对输入序列的长度进行限制。超过限制长度的部分可能需要被截断或者进行其他处理。
6. 填充序列：为了确保输入序列具有相同的长度，可能需要在较短的序列末尾添加特殊的填充标记。

#### 3.input embedding

输入序列中的每个token转换为其对应的向量表示的过程





{% file src="../.gitbook/assets/image (3).png" %}

{% file src="../.gitbook/assets/image (9).png" %}

{% embed url="https://arxiv.org/pdf/1706.03762.pdf" %}
