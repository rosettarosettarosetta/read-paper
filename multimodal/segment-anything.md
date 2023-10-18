# ✍ Segment Anything

模型结构应该是什么样？

模型要支持灵活的提示，且要实时生成mask,对输出也是模糊的(比如表示衣服还是穿衣服的人)，设计结构如下：一个prompt encoder，对提示进行编码，image encoder对图像编码，生成embedding, 最后融合2个encoder，再接一个轻量的mask decoder，输出最后的mask。

\
\


<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>



{% embed url="https://browse.arxiv.org/pdf/2304.02643.pdf" fullWidth="false" %}
