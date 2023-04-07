## GAN网络介绍

含义

- 以随机噪声为输入
- 生成以假乱真的数据

算法：

![image-20230406161310142](C:\Users\矿大物联网\AppData\Roaming\Typora\typora-user-images\image-20230406161310142.png)

- 生成对抗网的小批量随机梯度下降训练

- 通过提升其随机梯度来更新鉴别器

- 通过降低其随机梯度更新生成器

  本项目采用WCGAN,在DCGAN架构中，鉴别器D是卷积神经网络(CNN)，它应用大量滤波器从图像中提取各种特征。鉴别器网络将被训练以区分原始图像和生成的图像。卷积的过程如下图所示

  | Layer       | Shape                   | Activation |
  | ----------- | ----------------------- | ---------- |
  | input       | batch size, 3, 64, 64   |            |
  | convolution | batch size, 64, 32, 32  | LRelu      |
  | convolution | batch size, 128, 16, 16 | LRelu      |
  | convolution | batch size, 256, 8, 8   | LRelu      |
  | convolution | batch size, 512, 4, 4   | LRelu      |
  | dense       | batch size, 64, 32, 32  | Sigmoid    |

生成器g被训练为生成图像以欺骗鉴别器，它被训练为从随机输入生成图像。在DCGAN架构中，生成器由上采样输入的卷积网络表示。目标是处理小的输入，并使输出大于输入。它的工作原理是将输入扩展到中间为零然后对这个扩展区域进行卷积。在这个区域上的卷积将导致下一层的输入更大。上采样的过程如下图所示

上采样过程有很多名称:全卷积、网络内上采样、分数步进卷积、反卷积或转置卷积。生成器的网络结构由

| Layer         | Shape                                             | Activation |
| ------------- | ------------------------------------------------- | ---------- |
| input         | batch size, 100 (Noise from uniform distribution) |            |
| reshape layer | batch size, 100, 1, 1                             | Relu       |
| deconvolution | batch size, 512, 4, 4                             | Relu       |
| deconvolution | batch size, 256, 8, 8                             | Relu       |
| deconvolution | batch size, 128, 16, 16                           | Relu       |
| deconvolution | batch size, 64, 32, 32                            | Relu       |
| deconvolution | batch size, 3, 64, 64                             | Tanh       |

### Hyperparameter of DCGAN

The hyperparameter for DCGAN architecture is given in the table below:

| Hyperparameter                                               |
| ------------------------------------------------------------ |
| Mini-batch size of 64                                        |
| Weight initialize from normal distribution with std = 0.02   |
| LRelu slope = 0.2                                            |
| Adam Optimizer with learning rate = 0.0002 and momentum = 0.5 |

一点理论知识：

http://t.csdn.cn/am8lP