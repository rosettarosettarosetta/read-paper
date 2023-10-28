# supervision



## 自我训练

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>



## **联合培训**

协同训练基于两个数据_视图_训练两个单独的分类器。

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

因此，以下是简单来说协同训练的工作原理。

首先，在少量标记数据的帮助下为每个视图训练一个单独的分类器（模型）。

&#x20;然后添加更大的未标记数据池以接收伪标签。&#x20;

分类器使用具有最高置信度的伪标签相互训练。如果第一个分类器自信地预测数据样本的真实标签，而另一个分类器做出预测错误，则具有由第一个分类器分配的自信伪标签的数据更新第二个分类器，反之亦然。&#x20;

最后一步涉及组合来自两个更新分类器的预测以获得一个分类结果。&#x20;

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

## **SSL**

运行 SSL 的一种流行方法是以图形的形式表示标记和未标记的数据，然后应用[标签传播算法](https://pages.cs.wisc.edu/\~jerryzhu/pub/CMU-CALD-02-107.pdf)。它通过整个数据网络传播人造注释

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

如果查看图表，将看到一个数据点网络，其中大部分没有标记，带有四个携带标签（两个红点和两个绿点代表不同的类别）。任务是在整个网络中传播这些彩色标签。一种方法是选择第 4 点，然后计算通过网络从 4 到每个彩色节点的所有不同路径。如果你这样做，你会发现有 5 条步道通向红点，只有 4 条步道通向绿色点。由此，我们可以假设第 4 点属于红色类别。然后您将对图表上的每个点重复此过程。&#x20;
