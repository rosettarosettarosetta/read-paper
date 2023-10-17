---
description: A-T-T-E-N-T-I-O-N~         Attention is What I Want!
---

# ✍ Transformer

## 1.overview



## 2.整体结构

### 2.1Encoder  Decoder :

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>都包含 6 个 block</p></figcaption></figure>





#### 2.1.1 输入和处理

输入句子的 每一个单词的表示向量 X

x由**词 Embedding** 和**位置 Embedding** （Positional Encoding）相加得到。

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

**词 Embedding:**d 表示 PE的维度&#x20;



**位置 Embedding** ：保存单词在序列中的相对或绝对位置。（训练/公式计算得到）

因为 Transformer 不采用 RNN 的结构，而是使用全局信息，不能利用单词的顺序信息，而这部分信息对于 NLP 来说非常重要。

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption><p>位置编码交织了一条正弦曲线和一条余弦曲线，所有偶数索引都有正弦值，所有奇数索引都有余弦值。</p></figcaption></figure>

pos 表示单词在句子中的位置，d 表示 PE的维度 （编码向量的长度（与嵌入向量相同）和 i 是这个向量的索引值）

位置编码交织了一条正弦曲线和一条余弦曲线，所有偶数索引都有正弦值，所有奇数索引都有余弦值。

**ex:**

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>40个单词的序列的编码值</p></figcaption></figure>

### 2.2Self-Attention（自注意力机制）

&#x20; Multi-Head Attention，是由多个 Self-Attention组成的

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>location</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Q(查询),K(键值),V(值)   矩阵&#x20;

#### &#x20;Q, K, V 的计算：

X, Q, K, V 的每一行都表示一个单词

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Self-Attention 的输出：

公式中计算矩阵Q和K每一行向量的内积，为了防止内积过大，因此除以 dk的平方根。Q乘以K的转置后，得到的矩阵行列数都为 n，n 为句子单词数，这个矩阵可以表示单词之间的 attention 强度。

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

####

#### 2.2.2Add & Norm



Add 表示残差连接 (Residual Connection) 用于防止网络退化

Norm 表示 Layer Normalization，用于对每一层的激活值进行归一化

#### 2.1.2&#x20;

将得到的单词表示向量矩阵 (如上图所示，每一行是一个单词的表示 x) 传入 Encoder 中，经过 6 个 Encoder block 后可以得到句子所有单词的编码信息矩阵 C，如下图。单词向量矩阵用 Xn.d 表示， n 是句子中单词个数，d 是表示向量的维度 (论文中 d=512)。



每一个 Encoder block 输出的矩阵维度与输入完全一致。

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

#### 1.1.3 decoder 预测部分

将 Encoder 输出的编码信息矩阵 C传递到 Decoder 中，Decoder 依次会根据当前翻译过的单词 1\~ i 翻译下一个单词 i+1。

翻译到单词 i+1 的时候需要通过 **Mask** (掩盖) 操作遮盖住 i+1 之后的单词。



<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>



### 1.2Transformer 的输入



#### 1.2.1 单词 Embedding

单词的 Embedding 有很多种方式可以获取，例如可以采用 Word2Vec、Glove 等算法预训练得到，也可以在 Transformer 中训练得到。



#### 1.2.2 位置 Embedding





仅仅用了注意力机制而不是循环或者卷积

第一个只依赖于自注意力

point : multi-headed self-attention层





时序模型现在用的最多的是rnn(一个词一个词的分析)

transofrmer类似于多头的注意力，约等于多输出通道





使用编码器解码器架构

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

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



## Extended explanation：

### [1.PE](transformer.md#1.1.1-shu-ru-he-chu-li)

2i 表示偶数的维度，2i+1 表示奇数维度 (即 2i≤d, 2i+1≤d)。使用这种公式计算 PE 有以下的好处。

* 使 PE 能够适应比训练集里面所有句子更长的句子，假设训练集里面最长的句子是有 20 个单词，突然来了一个长度为 21 的句子，则使用公式计算的方法可以计算出第 21 位的 Embedding。
* 可以让模型容易地计算出相对位置，对于固定长度的间距 k，PE(pos+k) 可以用 PE(pos) 计算得到。因为 Sin(A+B) = Sin(A)Cos(B) + Cos(A)Sin(B), Cos(A+B) = Cos(A)Cos(B) - Sin(A)Sin(B)。

\




## Reference:

[https://www.zhihu.com/search?type=content\&q=tarnsformer](https://www.zhihu.com/search?type=content\&q=tarnsformer)

{% file src="../.gitbook/assets/image (3) (1).png" %}

{% file src="../.gitbook/assets/image (9) (1).png" %}

{% embed url="https://arxiv.org/pdf/1706.03762.pdf" %}
