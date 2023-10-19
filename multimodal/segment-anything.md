# ✍ Segment Anything

## 1.introduce

### 1.1 效果

zero-shot(零样本)和few-shot(少样本)的[泛化能力](https://www.zhihu.com/search?q=%E6%B3%9B%E5%8C%96%E8%83%BD%E5%8A%9B\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22620355474%22%7D)

交互性---快速

### 1.2 零样本的实现-提示工程prompt engineering

提示的形式不同（点、框、语言）

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

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### 2.1 image encoder（？） <a href="#h_620355474_3" id="h_620355474_3"></a>



scalability and powerful pretraining method

用[MAE](https://openaccess.thecvf.com/content/CVPR2022/papers/He\_Masked\_Autoencoders\_Are\_Scalable\_Vision\_Learners\_CVPR\_2022\_paper.pdf) 和[ViT](segment-anything.md#1.vit)

最低限度适应高分辨率的输入，该encoder在prompt encoder之前，对每张图像只运行一次。

图像进行降采样：图像编码器的输出是输入图像的16倍降采样的嵌入。

_“Motivated by scalability and access to strong pre-training, we use an MAE \[47] pre-trained Vision Transformer (ViT) \[33] with minimal adaptations to process high resolution inputs, specifically a ViT-H/16 with 14×14 windowed attention and four equally-spaced global attention blocks, following \[62].”_

vit:视觉编码器

### 2.2 prompt encoder

分成2类：稀疏的(点，box，文本)，稠密的（mask）

稀疏提示被映射为256维的向量嵌入 ，一个点被表示为其位置的位置编码与两个学习到的嵌入之和，这两个嵌入指示该点是在前景还是背景中（_one of two learned embeddings that indicate if the point is either in the foreground or background._）

* point:映射到256维的向量，包含代表点位置的 positional encoding，加2个代表该点是前景/背景的可学习的embedding。
* box:用一个[embedding](https://www.zhihu.com/search?q=embedding\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22620355474%22%7D)对表示（1）可学习的embedding代表左上角（2）可学习的embedding代表右下角
* 文本：通过CLIP模型进行文本编码
* mask:用输入图像1/4分辨率的mask，然后用(2,2)卷积核，[stride-2](https://www.zhihu.com/search?q=stride-2\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22620355474%22%7D)输出channel为4和16，再用(1,1)卷积核将channel升到256. mask 和iamge embedding通过element-wise相乘(逐元素相乘，可以理解成mask的feature对image的feature进行加权)

### 2.3 mask decoder &#x20;

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

* 在prompt embeddings中插入一个可学习的token，用于[docoder](https://www.zhihu.com/search?q=docoder\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22620355474%22%7D)的输出。

（1）prompt toekns+output tokens进行self attn,

（2）用得到的token和image embedding进行 cross attn（token作为Q）

（3）point-wise MLP 更新token

（4）用image embedding和（3）的token进行cross atten（image embedding作为Q）

重复上述步骤2次，再将attn再通过残差进行连接，最终输出masks和iou scores。

为了解决输出模糊性问题(一个提示可能生成多个mask，比如衣服上的一个点，既可以表示衣服，也表示穿衣服的人)，预测输出多个masks(发现\*\*整体，部分，子部分\*\*已经足够描述mask)，在训练过程中,只回传最小的[loss](https://www.zhihu.com/search?q=loss\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22620355474%22%7D)，为了对mask进行排序，增加一个小的head预测mask和目标的iou。

当输入多个提示时，生成的mask会比较接近，为了减少loss退化和确保获取明确的mask，此时只预测一个mask（作为第4个预测mask，只有多个提示时才预测，当单个提示时不用）





### 2.4 [模型训练](https://www.zhihu.com/search?q=%E6%A8%A1%E5%9E%8B%E8%AE%AD%E7%BB%83\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22620355474%22%7D) <a href="#h_620355474_6" id="h_620355474_6"></a>

训练时模拟交互分割的过程，从目标mask中随机选取前景点或者box，点是从[gt mask](https://www.zhihu.com/search?q=gt%20mask\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22620355474%22%7D)选取，box增加长边10%的噪声，最大20像素。

在第一次prompt预测mask之后，后续是从预测mask和gt mask有差异的区域采样点，如果新生成的点是FN，则作为前景，如果是FP，则作为背景。同时，将预测的mask（unthresholded mask logits代替二值化的mask,不过滤阈值，默认为0），作为prompt作为迭代。

训练过程中，发现用8个采样点比较合适(对比16个，没有明显增益)，为了鼓励模型从mask中获益，其中2个迭代不用新采样的点，总共11个迭代，1一个是初始化的prompt输入，然后是8个上述迭代，再加2个不重新采样点的迭代（这样可以refine mask）。由于mask decoder比较轻，所以可以进行更多次的迭代。

\
\


3.data engine（数据引擎）\
\



* 辅助人工标注

通过SAM基于浏览器的交互式分割工具，通过“brush”和"eraser"工具，进行标注。模型可以实时输出mask，建议标注者优先标记他们命名的对象，按图层顺序标记，如果一个mask标记超过30s，先处理下一张。

SAM先用公开数据集训练，然后再用新增的标注mask训练。随着数据越多，image-encoder的能力越强，retrained了6次。随着模型改进，每个mask平均标注时间从34s到14s，平均每张图像mask从22增加到44个。在这个过程中，从12万图像中，收集了430万个mask。

* 半自动

增加mask的多样性，首先检测出可信的mask，然后用预测mask填充图像，让标注者标注未标记的mask。为了检测可信的mask,先用第一步的mask训练了一个类别一样的box检测器。半自动过程中，从18万张图像中生成了590万个mask。用新收集的数据，重新训练模型，平均标注时间又回到了34s,因为新的mask都是比较有难度的。每张图像上mask从44增加到72。

* 全自动

利用前2步，得到的大量的和多样性的mask，结合模型可以根据不明确的输入也能输出有效的mask（参考mask encoder）,对图像生成（32,32）个格网点，每个点预测一系列mask，如果一个点落在部分、子部分上，模型返回部分、子部分和整体的object。同时，通过预测的iou筛选 _confident_(可信的mask),选取一个_stable_的mask(稳定的mask,在相似的mask中，概率阈值在 0.5-δ和 0.5-δ之间)；最后，通过nms过滤_confident_和_stable_中重复的mask。

为了提高mask比较小的，还通过放大图像进行crop，处理多个mask覆盖的情况。

在1100万数据集上，生成了11亿高质量的mask。

## Extended explanation：

### 1.[ViT](segment-anything.md#h\_620355474\_3) :

机制：ViT模型只是用了transformer的Encoder来提取特征

将图像分为无数小块（patches）

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

## reference:

MAE：

https://www.zhihu.com/question/498364604/answer/2219725892

相关项目整理：

{% embed url="https://zhuanlan.zhihu.com/p/630529550" %}

{% embed url="https://browse.arxiv.org/pdf/2304.02643.pdf" fullWidth="false" %}



