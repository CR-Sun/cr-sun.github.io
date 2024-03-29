---
layout:     post
title:      Neural Operator -- Graph Kernel Network for Partial Differential Equations
subtitle:   论文记录，个人学习用，侵删
date:       2023-3-11
author:     Chengrui
header-img: img/gnn.jpg
catalog: true
tags:
    - 神经网络
    - 论文笔记
    - GNN
---

# Neural Operator: Graph Kernel Network for Partial Differential Equations

### 1. Introduction

为了发展神经网络，需要新的思路，将传统神经网络从有限维欧几里得空间映射到类别或另一个有限维欧几里得空间中的***思想***扩展到**<u>*实值函数定义的子集所组成的输入和输出空间*</u>**中。

#### 1.1 Literature Review

1. 挑战：

   1. 有些PDE很难获取（底层基本原理未知）
   2. 有些PDE是非线形系统，求解或者数值计算非常复杂

2. 思路：

   1. 将求解器部分替换为一个神经网络：
      1. 缺点：
         1. 有网格依赖性，并且需要对架构进行修改以适应不同分辨率和离散化的K（dimension of in/output），以便实现一致的误差
         2. 这种方法受到训练数据的离散化大小和几何形状的限制，因此无法在域中查询新点的解。
   2. 将方程的解替换为一个神经网络：
      1. 优点：
         1. 没有了网格依赖性，因为方程的解是定义在物理空间上的
      2. 缺点：
         1. 参数依赖性是以网格相关的方式考虑的。实际上，对于任何具有新系数函数a的新方程，都需要训练一个新的神经网络$F_a$。
         2. 这种方法类似于传统方法，如有限元方法，用神经网络空间替换局部有限维基函数张开的线性空间。这种方法遭受与传统方法相同的计算问题：需要针对每个新参数解决优化问题。
         3. 这种方法仅限于已知基础PDE的情况；纯数据驱动的学习函数空间之间的映射是不可能的。
   3. 本文的方法最接近***简约基方法***（***reduced basis method***）
      1. 优点：
         1. 没有网络依赖性
         2. 方程参数的变化仅仅需要网络的一次前馈传递
         3. 不需要底层PDE的知识，数据可以来自实验数据或者计算机仿真

3. 图神经网络（GNN）

   1. 图卷积，边卷积，注意力机制，图池化
   2. GNN也被应用于物理现象的建模，如分子和刚体系统，因为这些问题具有自然的图形解释：粒子是节点，相互作用是边。
   3. 编码器-解码器模型，在潜空间中构建图
      1. 缺点：
         1. 模型使用最近邻结构，当网格大小增加时，无法捕捉非局部依赖性。
      2. 对比：
         1. 我们直接在输出函数的空间域上构建图形，节点位于空间域上。通过消息传递，我们能够直接学习网络的内核，从而逼近PDE解。当查询新位置时，我们只需将新节点添加到我们的空间图中并将其连接到现有节点，通过利用Nyström扩展算子的能力来避免插值误差。
      3. 问题：
         1. 有限维参数依赖的无限维空间之间的映射。（如何实现？）

   #### 1.2 Contributions

   ​	我们引入了神经算子的概念，并通过图内核网络进行实例化，这是一种新颖的深度神经网络方法，用于学习定义在 Rd 有界开放子集上的函数的无限维空间之间的映射。

   - 优点：
     1. 与现有方法不同，我们的方法可以明显地学习函数空间之间的映射，并且在不同的估计和网格下具有不变性，
     2. 使用Nyström扩展将函数空间上的神经网络连接到任意可能不规则的网格上的GNN族。（怎么做？反向扩展？）
     3. 展示了神经算子方法具有与传统和深度学习方法相当的近似精度。
     4. 数值结果表明，神经算子只需在少量样本上进行训练即可推广到整个问题类。
     5. 展示了半监督学习的能力，即仅在少量点上学习数据，然后推广到整个域。
     6. 在一类椭圆偏微分方程的背景下得到了说明，这类方程在科学和工程中出现的问题中具有代表性。

### 2.Problem Setting

- 目标：从有限个输入-输出对中学习一个无限维空间上的非线性映射，
- 假设参数{a_n, u}^N独立同分布
- F†：A→U是（通常是）非线性映射,我们的目标是通过构建参数映射来建立F†的近似
- 构造cost function，问题变为最小化问题
- 网络输入是function to a PDE（激励） 输出是PDE的解，输入和输出都是定义在Rd上的实值函数
- 鉴于我们的输入和输出都是函数，为了数值地处理它们，我们假设我们只能逐点获取这些函数上的值
- 方法独立于离散化PK，因此是一种真正的函数空间方法；我们通过展示误差在K→∞时的不变性来在数值上验证这一点。这种性质非常理想，因为它允许在不同的网格几何和离散化大小之间转移解。

### 3.Graph Kernel Network

- 方法与网格离散化无关的理由：消息传递网络的邻域选取依赖的是物理空间上的距离而不是节点个数，也就是说邻域包含的节点数会随着离散化参数K的增大而增大（问题：对于小尺度问题，如果物理空间的距离选取不合适，会导致网络的全局信息传递，可能不符合实际，换句话说，如何确定物理空间的距离是一个问题）

- Nystrom Extension

  随机选取点从而简化矩阵的维数，简化学习过程，可以理解为对每个随机子矩阵计算特征值

  最大的问题：不理解对于PDE作出的优化，感觉只是把激励函数和解函数上的数据直接拿来学习，跟PDE没有关系，带来的问题：（解释性？，没有解决实质问题，对于没有PDE的问题，只能靠实验获取数据，样本量难以保证，对于复杂的非线性问题，数据成本？可解释性？复杂时变系统？）













