---
description: CNN+医学图像分割的SAM
---

# Ladder Fine-tuning approach for SAM integrating complementary network

## [https://arxiv.org/pdf/2306.12737.pdf](https://arxiv.org/pdf/2306.12737.pdf)

## Segment Anything Model (SAM)（图像分割）医学图像与CNN结合

难点：lack of training samples due to privacy concerns

sloution： ensure their optimal utilization(~~这也叫答案？~~)

data set:various medical imaging modality such as X-rays, CT scans, MRI scans, or ultrasound images



### SAM

include:image encoder, prompt encoder and mask decoder



#### mask decoder

掩码解码器：掩码解码器是一个生成模型的组件，用于生成目标图像或以图像为基础进行图像编辑的掩码。它接收图像编码器和提示编码器的输出，并生成一个掩码，指示生成图像中的目标对象或要编辑的区域。掩码解码器可以是一个生成对抗网络（GAN）或变分自动编码器（VAE），它使用编码器的输出来生成与输入提示相关的图像。

##

{% embed url="https://arxiv.org/pdf/2306.12737.pdf" %}

{% file src="../.gitbook/assets/中文Ladder Fine-tuning approach for SAM integrating complementary network.pdf" %}
