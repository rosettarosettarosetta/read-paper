# supervision



## 自我训练

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>



## **联合培训**

协同训练基于两个数据_视图_训练两个单独的分类器。

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

因此，以下是简单来说协同训练的工作原理。

首先，在少量标记数据的帮助下为每个视图训练一个单独的分类器（模型）。

&#x20;然后添加更大的未标记数据池以接收伪标签。&#x20;

分类器使用具有最高置信度的伪标签相互训练。如果第一个分类器自信地预测数据样本的真实标签，而另一个分类器做出预测错误，则具有由第一个分类器分配的自信伪标签的数据更新第二个分类器，反之亦然。&#x20;

最后一步涉及组合来自两个更新分类器的预测以获得一个分类结果。&#x20;

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

## **SSL**
