### Abstract

​	现有的视频生成都是主要结合visual 帧，然而audio信号则被忽略了。在这篇工作中我们集中于一个很少研究的文本引导语音视频生成的问题，并且提出语音视频生成器（SVG）。这是一个可以生成现实音频视频的网络。除此之外，我们提出了SVG-VQGAN来把**visual frames** 和 **audio melspectrograms**（光谱图） 转换成离散向量。svg-vqgan利用一种全新的超参对比学习学习方法，去适应内模态和外模态之间的一致性，并且改善离散化表征。利用对比学习和跨模态注意力去提取visual和audio的特征。然后基于transformer的解码器被用于text，visual，audio之间的特征建立架起桥梁生成音频视频。 **AudioSet-Cap** 这一 text-visual-audio数据集被用于训练。

### Introduction

​	拥有音频信号作为背景的视频，包含更加多的复杂信息和能让人类和机器更好地理解。对于T2SV生成最重要的三点：

① 为了更好地视频生成，需要如何建立跨模态之间的联系？

​	使用音频可以更好地去区分相似物体之间的区别

② 生成一致的visual frames和audio signals是很困难的，生成过程中需要对三模态之间进行建模

③ 没有配对的数据集，先前的工作都是集中于对于visual content的描述，省略了对于audio的描述，然而T2SV任务需要audio和text语义的一致性。

​	为了解决以上的问题，我们提出了SVG，SVG分成两步：==量化编码（quantized encoding）==和==音频光谱（audio spectrograms==） 

1. visual frames 和 audio spectrogram 被分别量化成tokens利用**SVG-Vector-Quantized GAN**（SVG-VQGAN)。为了获得更好的量化表征，我们提出了一种对比学习方法，这种方法可以采用模态间对比损失来建模跨模态之间的关联，并采样模态内对比损失作为正则化手段去防止提取特征偏离原始的模态。	我们分别从相同和不同的video clips中选择一个positive和negaive的样本，并且提出三个策略：① 基于filter的visual-audio相似性 ②text-guide negative samples selection ③ window-based positive samples selection。   像**天空**背景这种没有与audio有联系的visual 实体，跨模态注意力模块被用于去建立visual 和 audio 内容的local alignment，并且获取全局features来用作混合对比学习。
2. 一个**自回归Transformer解码器**被用于建模文本描述（text description）、视觉帧（visual frames）和音频信号（audio signals）之间在tokens的一致性。为了consider到visual 2 audio和 audio 2 visual 之间的注意力，我们提出了一种**形式的替代序列格式**（modality alternate sequence format），visual tokens和audio tokens都会被cat到每一帧和逐帧连接。

主要贡献：

① 第一份工作关注于一个全新的任务：text 2 sounding video generation。

② 提出一种SVG-VQGAN，跨模态注意力用于建立本地语义一致性和混合对比学习这两种方法来对内模态和外模态一致性进行建模

③ 一个人工标注的数据集，包含visual和audio的内容描述。



**个人理解**

实际上训练了一个vqgan样式网络。利用对比学习来建模audio和visual 之间的联系，并且是在一个自己构建的数据集上进行训练。但是代码没有开源，看了github上的demo，感觉效果一般。