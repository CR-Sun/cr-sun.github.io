---
layout:     post
title:      Mesh Convolutional Neural Networks for Wall Shear Stress Estimation in 3D Artery Models
subtitle:   论文记录，个人学习用，侵删
date:       2023-3-26
author:     Chengrui
header-img: img/gnn.jpg
catalog: true
tags:
    - 神经网络
    - 论文笔记
    - 血液动力学
---



## Mesh Convolutional Neural Networks for Wall Shear Stress Estimation in 3D Artery Models


1. Authors: Julian Suk, Pim de Haan, Phillip Lippe, Christoph Brune, and Jelmer M. Wolterink

2. Affiliation: Department of Applied Mathematics & Technical Medical Centre, University of Twente, Enschede, The Netherlands

3. Keywords: coronary blood flow, geometric deep learning, computational fluid dynamics, surrogate modelling

4. Urls: arXiv:2109.04797v1 [cs.LG] 10 Sep 2021

5. Summary: 

- (1): 该文章的研究背景是使用深度学习来快速估计在表面网格上的壁面剪切应力，结合人体内的血流动力学和涡的理论，深度学习方法对血管疾病的防治有着重要的意义； 
- (2): 过去的方法都需要手动处理表面网格的重参数化来匹配卷积神经网络架构，存在一些问题，如复杂性和耗时性，因此本文提出使用网格卷积神经网络，直接运行在与CFD使用的有限元表面网格上；这一方法得到了验证，并且能够准确预测在表面网格上的3D WSS向量； 
- (3): 该文章的研究方法是使用网格卷积神经网络直接处理CFD使用的有限元表面网格，并对合成冠状动脉模型的两个数据集进行训练，并得出良好的结果； 
- (4): 该方法的运行时间小于5秒，持续地达到一个标准化的平均误差不超过1.6％，并高达90.5％的中位数估计准确度，证明了使用网格卷积神经网络在动脉模型中进行血流动力学参数估计的可行性。
7. Methods:

- (1): 本文的核心方法是采用网格卷积神经网络来直接处理血管表面的有限元网格，实现对血管壁面剪切应力（WSS）的快速估计；

- (2): 文章采用了编码解码架构的U-Net来保证估计结果是全局的，其中编码器和解码器依次包含三个不同的池化层，每层包含两个卷积层和一个跳跃连接；

- (3): 为保证输入特征对于网格平移具有不变性，对于每个顶点，本文采用局部球形坐标系的平均值作为特征，并且采用外积形式特征向量，使得其对于曲面旋转有等变性；

- (4): 为了保证特征具有等变性，本文采用了每个点在坐标系上的极角作为规范点，并且通过代数不变量的方式表示局部坐标系，使得模型具有整体旋转等变性；

- (5): 本文采用了三种不同的网格卷积算法，分别为GraphSAGE、FeaSt、和gauge-equivariant mesh convolution，通过实验比较证明了gauge-equivariant mesh convolution算法的优越性；

- (6): 本文使用了两个数据集进行模型训练和验证，分别为single arteries数据集和autoML数据集，其中single arteries数据集包含2000个合成的冠状动脉模型，autoML数据集包含15个真实血管模型。





8. Conclusion:

- (1): 本文提出的基于网格卷积神经网络的血管壁面剪切应力（WSS）估计方法在血流动力学领域具有重要的应用价值。

- (2): 创新点：文章创新地采用网格卷积神经网络直接处理血管表面的有限元网格，避免了手动处理表面网格的繁琐步骤；性能：实验结果表明，该方法能够准确预测在表面网格上的3D WSS向量，并且运行时间小于5秒，持续地达到一个标准化的平均误差不超过1.6％，以及90.5％的中位数估计准确度；工作量：文章提出的方法不需要手动处理表面网格的重参数化，使得模型训练和预测的工作量减少了很多。

- (3): 因此，本文提出的方法为动脉模型中进行血流动力学参数估计提供了一条新的思路和方法，对于提高相关疾病的诊断和治疗具有很大的帮助。



