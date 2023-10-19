# ✍ Segment Anything

## 1.introduce

### 1.1 效果

zero-shot(零样本)和few-shot(少样本)的[泛化能力](https://www.zhihu.com/search?q=%E6%B3%9B%E5%8C%96%E8%83%BD%E5%8A%9B\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22620355474%22%7D)

### 1.2 零样本的实现

通过提示输入，生成有效的mask

当提示是不确定的，能生成多个objects(比如衣服上的一个点，既可以表示衣服，也表示穿衣服的人)，如下图所示：提示可以是点，矩形框，文字，mask，或者是图像。

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

### 1.3模型结构

模型要支持灵活的提示，且要实时生成mask,对输出也是模糊的(比如表示衣服还是穿衣服的人)，设计结构如下：一个prompt encoder，对提示进行编码，image encoder对图像编码，生成embedding, 最后融合2个encoder，再接一个轻量的mask decoder，输出最后的mask。

\
\


<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>



### 1.4 数据支持

自然语言数据是通过在线获取；需要大量且多样化的[mask数据](https://www.zhihu.com/search?q=mask%E6%95%B0%E6%8D%AE\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22620355474%22%7D)

但是mask数据是不足的，替代方案就是建立一个“数据引擎”，分成3步：

人工辅助(帮助标注，类似交互式分割)，

半自动(通过提供提示,自动生成对象mask)，

全自动（通过规则[格网](https://www.zhihu.com/search?q=%E6%A0%BC%E7%BD%91\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22620355474%22%7D)作为提示，进行自动生成）。

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>



## 2.model

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### 2.1 image encoder <a href="#h_620355474_3" id="h_620355474_3"></a>



scalability and powerful pretraining method

用[MAE](https://openaccess.thecvf.com/content/CVPR2022/papers/He\_Masked\_Autoencoders\_Are\_Scalable\_Vision\_Learners\_CVPR\_2022\_paper.pdf) 和ViT

最低限度适应高分辨率的输入，该encoder在prompt encoder之前，对每张图像只运行一次。



相关项目整理：

[https://zhuanlan.zhihu.com/p/630529550](https://zhuanlan.zhihu.com/p/630529550)

{% embed url="https://browse.arxiv.org/pdf/2304.02643.pdf" fullWidth="false" %}
