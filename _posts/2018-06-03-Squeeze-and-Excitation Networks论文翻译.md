摘要

卷积神经网络建立在卷积运算的基础上，通过融合局部感受野内的空间信息和通道信息来提取信息特征。为了提高网络的表示能力，许多现有的工作已经显示出增强空间编码的好处。在这项工作中，我们专注于通道，并提出了一种新颖的架构单元，我们称之为“Squeeze-and-Excitation”（SE）块，通过显式地建模通道之间的相互依赖关系，自适应地重新校准通道式的特征响应。通过将这些块堆叠在一起，我们证明了我们可以构建SENet架构，在具有挑战性的数据集中可以进行泛化地非常好。关键的是，我们发现SE块以微小的计算成本为现有的最先进的深层架构产生了显著的性能改进。SENets是我们ILSVRC 2017分类提交的基础，它赢得了第一名，并将top-5错误率显著减少到2.251%，相对于2016年的获胜成绩取得了∼25%
的相对改进。

1.引言

卷积神经网络（CNNs）已被证明是解决各种视觉任务的有效模型[19,23,29,41]。对于每个卷积层，沿着输入通道学习一组滤波器来表达局部空间连接模式。换句话说，期望卷积滤波器通过融合空间信息和信道信息进行信息组合，而受限于局部感受野。通过叠加一系列非线性和下采样交织的卷积层，CNN能够捕获具有全局感受野的分层模式作为强大的图像描述。最近的工作已经证明，网络的性能可以通过显式地嵌入学习机制来改善，这种学习机制有助于捕捉空间相关性而不需要额外的监督。Inception架构推广了一种这样的方法[14,39]，这表明网络可以通过在其模块中嵌入多尺度处理来取得有竞争力的准确度。最近的工作在寻找更好地模型空间依赖[1,27]，结合空间注意力[17]。

与这些方法相反，通过引入新的架构单元，我们称之为“Squeeze-and-Excitation” (SE)块，我们研究了架构设计的一个不同方向——通道关系。我们的目标是通过显式地建模卷积特征通道之间的相互依赖性来提高网络的表示能力。为了达到这个目的，我们提出了一种机制，使网络能够执行特征重新校准，通过这种机制可以学习使用全局信息来选择性地强调信息特征并抑制不太有用的特征。

SE构建块的基本结构如图1所示。对于任何给定的变换$$\mathbf{F}_{tr} : \mathbf{X} \rightarrow \mathbf{U},$$$$\mathbf{X} \in \mathbb{R}^{W’ \times H’ \times C’},$$$$ \mathbf{U} \in \mathbb{R}^{W \times H \times C} $$

(例如卷积或一组卷积)，我们可以构造一个相应的SE块来执行特征重新校准，如下所示。特征U首先通过squeeze操作，该操作跨越空间维度W×H聚合特征映射来产生通道描述符。这个描述符嵌入了通道特征响应的全局分布，使来自网络全局感受野的信息能够被其较低层利用。这之后是一个excitation操作，其中通过基于通道依赖性的自门机制为每个通道学习特定采样的激活，控制每个通道的激励。然后特征映射U被重新加权以生成SE块的输出，然后可以将其直接输入到随后的层中。

> ![Squeeze-and-Excitation](/img/picture/3232548-f0e4dc6cd95a89dd.png)    
>
> ​                                                      图1. Squeeze-and-Excitation块                                                	    

