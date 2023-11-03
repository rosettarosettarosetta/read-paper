---
description: A-T-T-E-N-T-I-O-N~         Attention is What I Want!
---

# ✍ Transformer

## 1.overview

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

各有6个block



## 2.整体结构

### 2.1Encoder  Decoder&#x20;



<figure><img src="../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>







#### 2.1.1 输入和处理

输入句子的 每一个单词的表示向量 X

x由[**词 Embedding**](broken-reference) 和**位置 Embedding** （Positional Encoding）相加得到。

<figure><img src="../.gitbook/assets/image (11) (1).png" alt=""><figcaption></figcaption></figure>

**词 Embedding:**d 表示 PE的维度&#x20;







**位置 Embedding** ：保存单词在序列中的相对或绝对位置。（训练/公式计算得到）

因为 Transformer 不采用 RNN 的结构，而是使用全局信息，不能利用单词的顺序信息，而这部分信息对于 NLP 来说非常重要。

<figure><img src="../.gitbook/assets/image (12) (1).png" alt=""><figcaption><p>位置编码交织了一条正弦曲线和一条余弦曲线，所有偶数索引都有正弦值，所有奇数索引都有余弦值。</p></figcaption></figure>

pos 表示单词在句子中的位置，d 表示 PE的维度 （编码向量的长度（与嵌入向量相同）和 i 是这个向量的索引值）

位置编码交织了一条正弦曲线和一条余弦曲线，所有偶数索引都有正弦值，所有奇数索引都有余弦值。

**ex:**

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9) (1) (1).png" alt=""><figcaption><p>40个单词的序列的编码值</p></figcaption></figure>

### 2.2Self-Attention（自注意力机制）

&#x20; Multi-Head Attention，是由多个 Self-Attention组成的

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>location</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Q(查询),K(键值),V(值)   矩阵&#x20;

#### &#x20;Q, K, V 的计算：

X, Q, K, V 的每一行都表示一个单词

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Self-Attention 的输出：

公式中计算矩阵Q和K每一行向量的内积，为了防止内积过大，因此除以 dk的平方根。Q乘以K的转置后，得到的矩阵行列数都为 n，n 为句子单词数，这个矩阵可以表示单词之间的 attention 强度。

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

####

<figure><img src="../.gitbook/assets/image (15) (1).png" alt=""><figcaption><p>1234 表示的是句子中的单词</p></figcaption></figure>

单词之间的attention强度

公式中计算矩阵Q和K每一行向量的内积，为了防止内积过大，因此除以 的平方根。Q乘以K的转置后，得到的矩阵行列数都为 n，n 为句子单词数



<figure><img src="../.gitbook/assets/image (16) (1).png" alt=""><figcaption></figcaption></figure>

使用 Softmax 计算     每一个单词对于其他单词的 attention 系数

每一行进行 Softmax，每一行的和都变为 1.



<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

得到 Softmax 矩阵之后可以和V相乘，得到最终的输出Z。\


<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

上图中 Softmax 矩阵的第 1 行表示单词 1 与其他所有单词的 attention 系数，最终单词 1 的输出 等于所有单词 i 的值 根据 attention 系数的比例加在一起得到

####





### 2.3Add & Norm     Feed Forward

<figure><img src="../.gitbook/assets/image (10) (1).png" alt=""><figcaption><p>结合A&#x26;M的位置看</p></figcaption></figure>

X表示 Multi-Head Attention 或者 Feed Forward 的输入

MultiHeadAttention(X) 和 FeedForward(X) 表示输出&#x20;



**Add**指 X+MultiHeadAttention(X)，是一种**残差网络**，通常用于解决多层网络训练的问题，可以让网络只关注当前差异的部分，用于_`防止网络退化`（在 ResNet 中经常用到）_



**Norm**指 Layer Normalization，Layer Normalization 会将每一层神经元的输入都转成均值方差都一样的，这样可以_`加快收敛`_。_（通常用于 RNN 结构）_





**Feed Forward** 是一个两层的全连接层，第一层的激活函数为 Relu，第二层不使用激活函数，![](<../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png>)



_（没错～MultiHeadAttention 和 FeedForward的输出输入维度是一样的）_





### 2.4 encoder 总览&#x20;

将得到的单词表示向量矩阵 (如上图所示，每一行是一个单词的表示 x) 传入 Encoder 中，经过 6 个 Encoder block 后可以得到句子所有单词的编码信息矩阵 C，如下图。单词向量矩阵用 Xn.d 表示， n 是句子中单词个数，d 是表示向量的维度 (论文中 d=512)。



&#x20;Encoder block 输出输出的维度一致

最后一个 Encoder block 输出的矩阵就是编码信息矩阵 C，这一矩阵后续会用到 Decoder 中。

<figure><img src="../.gitbook/assets/image (9) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### 2.5  Decoder

需要根据之前的翻译，求解当前最有可能的翻译

* 第一个 Multi-Head Attention 层采用了 Masked 操作。
* 第二个 Multi-Head Attention 层的K, V矩阵使用 Encoder 的编码信息矩阵C进行计算，而Q使用上一个 Decoder block 的输出计算。
* 最后有一个 Softmax 层计算下一个翻译单词的概率。

#### 2.5.1 masked  Multi-Head Attention&#x20;

防止第 i 个单词知道 i+1 个单词之后的信息 ,确保了顺序翻译

使用了类似 Teacher Forcing 的概念



\
这五步后面再看Transformer模型详解（图解最完整版） - 初识CV的文章 - 知乎 https://zhuanlan.zhihu.com/p/338817680

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>输入矩阵与 Mask 矩阵</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>





#### 2.5.2 第二个 Multi-Head Attention&#x20;

区别： Self-Attention 的 K, V不是使用 上一个 Decoder block 的输出计算的，而是使用 Encoder 的编码信息矩阵 C 计算的。

根据上一个 Decoder block 的输出 Z 计算 Q (如果是第一个 Decoder block 则使用输入矩阵 X 进行计算)



这样做的好处是在 Decoder 的时候，每一位单词都可以利用到 Encoder 所有单词的信息 (这些信息无需 Mask)。





#### 2.5.3 softmax

预测下一个单词



输出结构：

第一行只包含0的信息

第四行有0,1,2,3&#x20;

因为marsks

<figure><img src="../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>

#### 1.1.3 decoder 预测部分

将 Encoder 输出的编码信息矩阵 C传递到 Decoder 中，Decoder 依次会根据当前翻译过的单词 1\~ i 翻译下一个单词 i+1。

翻译到单词 i+1 的时候需要通过 **Mask** (掩盖) 操作遮盖住 i+1 之后的单词。



<figure><img src="../.gitbook/assets/image (10) (1) (1).png" alt=""><figcaption></figcaption></figure>



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

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

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



### [2.Word Embedding](transformer.md#2.1.1-shu-ru-he-chu-li)

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>飞得越高的数值越大</p></figcaption></figure>

可以运用于情感分析任务（如：大于等于0为正向情感、小于0为[负向情感](https://www.zhihu.com/search?q=%E8%B4%9F%E5%90%91%E6%83%85%E6%84%9F\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22434942001%22%7D)）

对句子中情感的值加权平均

词嵌入的过程=获取X的过程

\
最合乎直觉（intuition）的假设-[分布假设](https://www.zhihu.com/search?q=%E5%88%86%E5%B8%83%E5%81%87%E8%AE%BE\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22434942001%22%7D)：相似的词往往出现在同一环境中

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption><p>正向词一堆，<a href="https://www.zhihu.com/search?q=%E8%B4%9F%E5%90%91%E8%AF%8D&#x26;search_source=Entity&#x26;hybrid_search_source=Entity&#x26;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22434942001%22%7D">负向词</a>一堆，没有实意的词在一堆(is,to,by)</p></figcaption></figure>

\
3.[卷积](https://www.zhihu.com/search?q=%E5%8D%B7%E7%A7%AF\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22356155277%22%7D)具有天然的先天优势（inductive bias）：平移等价性（translation equivariance）和局部性（locality）。而transformer虽然不并具备这些优势，但是transformer的核心[self-attention](https://www.zhihu.com/search?q=self-attention\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22356155277%22%7D)的优势不像卷积那样有固定且有限的感受野，self-attention操作可以获得long-range信息（相比之下CNN要通过不断堆积Conv layers来获取更大的感受野），但训练的难度就比CNN要稍大一些。



## Reference:

[https://www.zhihu.com/search?type=content\&q=tarnsformer](https://www.zhihu.com/search?type=content\&q=tarnsformer)

{% file src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1).png" %}

{% file src="../.gitbook/assets/image (9) (1) (1) (1) (1).png" %}

{% embed url="https://arxiv.org/pdf/1706.03762.pdf" %}
