# Zero Shot

**可见类 & 不可见类**(seen classes & unseen classes)：在ZSL问题中，特征空间(feature space)包含一些带标签的训练实例，这些实例所涵盖的的类别就称为可见类(seen classes)；同时，特征空间中还包含一些不带标签的测试实例，这些实例所属的类别称为不可见类(unseen classes)。&#x20;



假设我们的模型已经认识了马，老虎、熊猫，现在需要该模型学习如何识别 斑马，那么我们需要告诉模型一个提示prompt/attribute，怎样的对象才是斑马，但是并不能直接让模型看见斑马。所以模型需要知道的信息是马的样本、老虎的样本、熊猫的样本、不同样本的标签，以及关于前三种动物和斑马的描述，这样经过训练模型便可识别斑马这种未见过的类别&#x20;



握自动标注能力，在 SA-1B 中，仅仅进行了 **0.012%**（120, 000 张） 的专家标注，就已经具备优秀的全自动分割水平（99.1% 的标注由 SAM 自动生成），其他模型需要 10% 甚至更多的标注量才能达到类似的水平，印证了 SAM 充分挖掘了数量有限的监督数据集，大大降低了监督算法对数据量的依赖，从而大大降低了监督算法过拟合的可能。



在图文模态算法的近期发展进程中，一直围绕着一个关键词，即「n to n」，也就是我们常说的「模态对齐」。让属于特定 1 个词组的 n 种图像对齐到该单词，同时，让属于特定 1 个图像的 n 种词组对齐到该图片，是 Clip 十分擅长举一反三的关键。后续的 ALBEF 工作进一步优化了模态对齐的解决方案。另外，自从 CNN➕Pooling 的架构带飞了基于空间信息的 CNN 算法以来，我们都意识到不同抽象尺度对于学习算法的重要性，只有不断下采样池化，才能强迫模型站在多种尺度上观察空间信息特征，增广空间数据学习的视角。**Segment Anything 正是 n to n 与多尺度抽象的实践者，以 n to n 为本，以多尺度抽象为基**。

SAM 利用 Transformer 的 Cross attention 机制，促使图像的特征图创新性地与点模态、定位框模态、掩码模态（不是最终预测的掩码哦）和文本模态相互作用。值得注意的是，这 4 种模态的实例对于它所属的目标图像块并不是唯一确定的，本质上是在鼓励模型关注 n to n 的关系。







**SAM 利用 Transformer 的 Cross attention 机制**

reference:

{% embed url="https://cloud.tencent.com/developer/article/2314851" %}

{% embed url="https://blog.csdn.net/weixin_54338498/article/details/130054278?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169848203516800213074406%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=169848203516800213074406&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-15-130054278-null-null.142^v96^pc_search_result_base6&utm_term=%E7%9B%91%E7%9D%A3%20sam&spm=1018.2226.3001.4187" %}

